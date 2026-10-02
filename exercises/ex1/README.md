# Exercise 1 — Employee Onboarding AI Agent

> **Disclaimer:** This exercise is designed for workshop and learning purposes only. The architecture, tools, and approach presented here are simplified to provide a realistic hands-on experience within the constraints of a workshop session. They do not represent SAP's recommended, suggested, or officially supported architecture for building AI agents or integrating SAP systems. Always refer to official SAP documentation and best practices for production implementations.

In this exercise you will download, configure and run the Employee Onboarding AI Agent on your laptop. The agent connects to SAP Advanced Event Mesh, receives a new hire event from SuccessFactors, and orchestrates the full onboarding process through SAP Integration Suite — with your own identity propagated to S/4HANA via Principal Propagation.

## Architecture

```
SuccessFactors (HiringComplete notification event — no payload details)
        │
        ▼
SAP Advanced Event Mesh
        │
        ▼  (your assigned queue)
Employee Onboarding AI Agent  ←── you run this on your laptop
        │
        ├── Tool 1: get_new_hire_details
        │     API composition via Integration Suite
        │     combines SuccessFactors Employee, Job, and Manager APIs
        │     returns full new hire profile in one call
        │
        ├── Tool 2: get_company_policy
        │     reads onboarding policy grounded via RAG
        │     backed by SAP AI Launchpad Orchestration flow
        │     returns role/location-specific equipment and access policies
        │
        │   ── Human-in-the-Loop ──────────────────────────────────────
        │     Agent presents onboarding plan to manager for approval
        │     workflow pauses until manager approves
        │   ─────────────────────────────────────────────────────────────
        │
        ├── Tool 3: create_purchase_requisition  (after manager approval)
        │     creates PR in SAP S/4HANA On-Premise
        │     your IAS identity propagated via Principal Propagation
        │     PR recorded under your name in S/4 audit log
        │
        └── Tool 4: create_service_cloud_ticket  (after manager approval)
              creates ticket in SAP Service Cloud
              covers both IT system access and training enrollment requests

        All tools exposed via SAP Integration Suite MCP Gateway
        which enforces authentication, audit logging, and PP
```

## What happens when you run it

1. Your browser opens → log in with your SAP IAS credentials
2. Agent confirms your identity in the terminal
3. Agent connects to your AEM queue and waits for a new hire event
4. New hire notification arrives → agent fetches full employee, job and manager details via API composition
5. Agent reads company policy via RAG-grounded AI Launchpad Orchestration flow
6. Agent presents onboarding plan → **manager approves** (human-in-the-loop)
7. After approval → Purchase Requisition created in S/4HANA On-Premise under your identity via PP
8. After approval → Service Cloud ticket created for IT access and training enrollment

## Steps

### 1. Download and unzip

Download [employee-onboarding-ai-agent.zip](./employee-onboarding-ai-agent.zip) and unzip it on your laptop.

### 2. Install dependencies

Open a terminal in the unzipped folder and run:

```bash
npm install
```

> Requires Node.js v18 or higher — [nodejs.org](https://nodejs.org)
> The agent runs Claude via SAP AI Core — no Anthropic account needed, access is pre-configured via the shared AI Core proxy.

### 3. Configure your settings

Copy the environment template:

```bash
cp .env.example .env
```

Open `.env` and fill in the values provided by your instructor:

| Variable | What to do |
|---|---|
| `AICORE_API_KEY` | Enter the value provided by instructor |
| `AEM_USERNAME` | Enter the value provided by instructor |
| `AEM_PASSWORD` | Enter the value provided by instructor |
| `AEM_QUEUE` | Change `XX` to your participant number e.g. `onboarding/participant-01` |

**Pre-configured for this workshop — do not change:**

| Variable | Value |
|---|---|
| `AICORE_BASE_URL` | Shared AI Core proxy |
| `AEM_HOST` | Shared AEM hostname |
| `AEM_VPN` | Shared AEM Message VPN |
| `MCP_GATEWAY_URL` | Shared Integration Suite MCP Gateway |
| `IAS_TENANT` | Shared IAS tenant |
| `OIDC_CLIENT_ID` | Shared OIDC client |
| `OIDC_REDIRECT_URI` | `http://localhost:3000/callback` |

### 4. Run the agent

```bash
node agent.js
```

Log in when the browser opens. The agent will connect to your AEM queue and wait for a new hire event. Your instructor will trigger the event — watch your terminal for the onboarding workflow to execute.

## Project Structure

| File | Purpose |
|---|---|
| `agent.js` | The agent itself — Claude agentic loop, entry point |
| `test-login.js` | Verify your IAS login works before running the full agent |
| `tools/ias-login.js` | IAS login flow — OIDC + PKCE, opens browser, returns token |
| `tools/aem-subscriber-amqp.js` | Connects to AEM queue via AMQP, receives new hire events |
| `tools/mcp-client.js` | Calls Integration Suite MCP Gateway with your identity token |
| `prompts/employee-onboarding.md` | Claude system prompt — defines what the agent does |
| `package.json` | Defines dependencies — needed for `npm install` |
| `package-lock.json` | Locks exact dependency versions for consistent installs |
| `.env.example` | Template — copy to `.env` and fill in your values |

## Troubleshooting

**Port 3000 already in use**
```bash
netstat -ano | findstr :3000
taskkill /PID <pid from above> /F
```

**Browser does not open automatically**
Copy the URL printed in the terminal and open it manually in your browser.

**Token exchange fails**
Make sure `http://localhost:3000/callback` is registered as a redirect URI in the IAS app settings.

**npm install fails**
Make sure you have Node.js v18 or higher installed: [nodejs.org](https://nodejs.org)

## Summary

You've now run an AI agent that uses your own SAP IAS identity to orchestrate employee onboarding across SAP systems via Principal Propagation.

Continue to - [Exercise 2](../ex2/README.md)
