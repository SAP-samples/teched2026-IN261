# Scenario Introduction

**Estimated time: 15 minutes**

## Business Scenario

A new employee has just accepted an offer and completed their profile in **SAP SuccessFactors**. From this moment, multiple teams across the organization need to act — simultaneously and without dropping the ball:

- **HR** needs to confirm the hire and ensure the right policies apply based on the employee's role and location
- **IT** needs to provision system access — some on Day 1 automatically, some requiring a formal request
- **Finance** needs to order the right equipment via a Purchase Requisition in SAP S/4HANA
- **The hiring manager** needs visibility and control — approving the onboarding plan before any action is taken

Traditionally, this involves manual handoffs, emails, spreadsheets, and tickets across disconnected systems. Steps get missed, approvals are delayed, and new hires arrive on Day 1 without the tools they need.

In this session, an **AI agent** handles the entire flow automatically — triggered by a real-time **NEWHIRE** event from SAP SuccessFactors, grounded in company HR policy, and **governed at every step** through the **SAP Integration Suite MCP Gateway**.

---

## The Challenge: AI Agents Need Governed Access to Enterprise Systems

AI agents are powerful — but connecting them directly to enterprise backends is not the answer. Direct connections bypass security policies, break audit trails, and create uncontrolled access paths that cannot be monitored or revoked.

The **Model Context Protocol (MCP)** provides a standard for AI agents to discover and call enterprise tools — but MCP alone does not solve governance. An agent calling an MCP Server still needs:

- **Authentication** — is the caller who they claim to be?
- **Authorization** — is this caller allowed to call this tool?
- **Audit logging** — what was called, by whom, when, and with what result?
- **Policy enforcement** — rate limits, access controls, data filtering
- **Analytics** — call volumes, response times, error rates

The **SAP Integration Suite MCP Gateway** provides all of this — sitting between the AI agent and every backend system, enforcing policy on every tool call without the agent or the backend needing to know.

---

## Scenario Architecture

The **Best Run Employee Onboarding AI Agent** is a Node.js agent powered by **Claude Sonnet via SAP AI Core**. It listens for **NEWHIRE** events from SAP SuccessFactors via **SAP Advanced Event Mesh** and orchestrates the full onboarding workflow across four MCP Servers — all governed through the **SAP Integration Suite MCP Gateway**.

### 🔁 AI Agentic WorkFlow

```
New Hire joins Best Run company, details entered using SAP SuccessFactors
        │
        ▼
SAP Advanced Event Mesh (AEM)
        │
        ▼  new hire event triggers the agent
🤖 Best Run Employee Onboarding AI Agent  ──  Claude Sonnet via SAP AI Core
        │
        ├──────────────────────────────────────────────────────┐
        ▼                                                      ▼
👤 Employee Profile + Job Data                    📄 Best Run HR Onboarding Policy
   SAP API Composition                           RAG via SAP AI Launchpad
   (unified API over SuccessFactors)                 Orchestration
        │                                                      │
        └──────────────────┬───────────────────────────────────┘
                           │
                           ▼
              📋 Onboarding Plan Prepared
                           │
                           ▼
              ⏸️  Manager Approval  ← human-in-the-loop
                           │
           ┌───────────────┴───────────────┐
           ▼                               ▼
  🛒 Purchase Requisition         🎫 Support Case
     SAP S/4HANA                     SAP Service Cloud V2
     (equipment)                     (system access + training)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  All tool calls flow through SAP Integration Suite MCP Gateway
  Principal Propagation ensures end-to-end identity traceability
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
<img src="images/architecture.png" width=100% height=100%>

---

## The Agentic Workflow — Step by Step

### 1. New Hire Event Triggers the Agent

A new hire completes their profile in **SAP SuccessFactors**. SAP Integration Center constructs a **NEWHIRE** event payload and publishes it to a dynamically constructed topic in **SAP Advanced Event Mesh (AEM)**:

```
sap/sfsf/newhire/TechEd_IN261_0{employeeId}
```

Your personal AEM queue is subscribed to your dedicated topic — the agent receives the event and begins processing.

### 2. Agent Fetches the New Hire Profile

The agent calls the **Employee Lookup MCP Server** — backed by **SAP API Composition (Business Data Graph)**. A single MCP tool call returns the complete new hire profile: personal details, job title, department, business unit, manager, and location — unified from multiple SuccessFactors APIs into two clean entities.

This call uses **Principal Propagation** — your workshop identity flows from the agent through the MCP Gateway all the way to SAP SuccessFactors. The OData API Audit Log in SuccessFactors records the call under your user, not a shared service account.

### 3. Agent Retrieves Applicable HR Policies

The agent calls the **HR Policy Lookup MCP Server** — backed by a pre-configured Integration Flow that calls **SAP AI Launchpad Orchestration with Document Grounding**. The new hire's role and department are sent as a natural language policy query. The Document Grounding engine searches the indexed **BestRun HR Onboarding Policy** document and returns the most relevant policies — covering equipment provisioning, system access entitlements, and training requirements.

This is **Retrieval-Augmented Generation (RAG)** in action — the agent's decisions are grounded in real company policy documents, not model assumptions.

### 4. Onboarding Plan Prepared and Presented

The agent structures all retrieved data into a complete **Onboarding Plan** — covering:

- **Equipment Provisioning** — items to be ordered via a Purchase Requisition in SAP S/4HANA
- **System Access** — Day 1 auto-provisioned systems and additional systems requiring a Support Case
- **Training Schedule** — mandatory and role-specific training courses with enrollment timelines

The plan is presented in the terminal for the hiring manager's review.

### 5. Human-in-the-Loop: Manager Approval

The agent pauses and waits. **No backend system is touched until the hiring manager explicitly approves.**

This is the **human-in-the-loop** gate — a fundamental governance principle that ensures no automated action is taken without human consent. The manager reviews the full plan and types `Approved` to proceed.

### 6. Agent Creates the Backend Records

Immediately after approval, the agent calls two MCP Servers in parallel:

- **Create PR S/4HANA MCP** — calls a pre-configured Integration Flow that creates a **Purchase Requisition in SAP S/4HANA** for the new hire's equipment and returns the PR number and a direct link
- **Create Support Case MCP** — calls the **SAP Service Cloud V2 API** directly to create a **Support Case** for IT system access and training enrollment, returning the Case ID and a direct link

### 7. End-to-End Traceability

Every tool call is routed through the **SAP Integration Suite MCP Gateway** — authenticated, authorized, logged, and traceable. The **OData API Audit Log** in SAP SuccessFactors confirms Principal Propagation end to end. The **MCP Analytics dashboard** in SAP Integration Suite shows call volumes, response times, and error patterns across all MCP Servers.

---

## What You Build in This Session

Across nine exercises, you build every component of this architecture from scratch — then watch it run end to end:

| Exercise | What You Build |
|---|---|
| **Ex 1** | Personal AEM queue subscribed to your NEWHIRE topic |
| **Ex 2** | Employee Lookup MCP Server — backed by SAP API Composition with Principal Propagation |
| **Ex 3** | HR Policy Lookup MCP Server — backed by RAG via SAP AI Launchpad Orchestration |
| **Ex 4** | Purchase Requisition MCP Server — backed by SAP S/4HANA via Integration Flow |
| **Ex 5** | Support Case MCP Server — backed directly by SAP Service Cloud V2 API |
| **Ex 6** | Publish all four MCP Servers as a governed product in Developer Hub and subscribe to the AI Agent |
| **Ex 7** | Download, configure, and run the Employee Onboarding AI Agent |
| **Ex 8** | Trigger a NEWHIRE event to start the workflow |
| **Ex 9** | Observe, approve, and verify the full AI-orchestrated onboarding workflow |

---

## Key Concepts

| Concept | What It Means in This Session |
|---|---|
| **Model Context Protocol (MCP)** | Standard protocol for AI agents to discover and call enterprise tools at runtime |
| **SAP Integration Suite MCP Gateway** | Governed bridge between the AI agent and SAP backends — authentication, authorization, policy enforcement, and analytics in one place |
| **Event-Driven Architecture** | SAP Advanced Event Mesh delivers the NEWHIRE event in real time, triggering the agent automatically |
| **Principal Propagation** | Your logged-in identity flows through every tool call — SuccessFactors sees you, not a service account |
| **API Composition — SAP Business Data Graph** | Multiple SuccessFactors APIs unified into two clean entities — no context rot, no fragmented calls |
| **Retrieval-Augmented Generation (RAG)** | Agent decisions grounded in real BestRun HR policy documents via SAP AI Launchpad Orchestration |
| **Human-in-the-Loop** | Manager approval gate pauses the agent mid-workflow — no backend action without explicit consent |
| **Developer Hub** | API and MCP tool catalog in SAP Integration Suite — governed self-service discovery and subscription for AI agents |

---

## Summary
You should now be familiar with the session scenario — a realistic, production-grade AI agentic workflow that governs every agent action through the **SAP Integration Suite MCP Gateway**, propagates identity end to end, and keeps a human in control at every critical decision point.

Return to the [Exercises](../README.md#exercises) to start with the first exercise.
