# Exercise 1 — Employee Onboarding AI Agent

In this exercise you will download, configure and run the Employee Onboarding AI Agent on your laptop. The agent connects to SAP Advanced Event Mesh, receives a new hire event from SuccessFactors, and orchestrates the full onboarding process through SAP Integration Suite — with your own identity propagated to S/4HANA via Principal Propagation.

## What you will do

1. Download and unzip the agent
2. Install dependencies
3. Configure your personal settings
4. Test your IAS login
5. Run the agent and observe it process a new hire event

## Steps

### 1. Download and unzip

Download [employee-onboarding-ai-agent.zip](./employee-onboarding-ai-agent.zip) and unzip it on your laptop.

### 2. Install dependencies

Open a terminal in the unzipped folder and run:

```bash
npm install
```

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

All other values are pre-configured — do not change them.

### 4. Test your IAS login

```bash
node test-login.js
```

Your browser will open the SAP IAS login page. Log in with your SAP BTP credentials. You should see your name and email printed in the terminal.

### 5. Run the agent

```bash
node agent.js
```

Log in when the browser opens. The agent will connect to your AEM queue and wait for a new hire event. Your instructor will trigger the event — watch your terminal for the onboarding workflow to execute.

## Troubleshooting

**Port 3000 already in use**
```bash
netstat -ano | findstr :3000
taskkill /PID <pid from above> /F
```

**Browser does not open automatically**
Copy the URL printed in the terminal and open it manually in your browser.

**npm install fails**
Make sure you have Node.js v18 or higher installed: [nodejs.org](https://nodejs.org)

## Summary

You've now run an AI agent that uses your own SAP IAS identity to orchestrate employee onboarding across SAP systems via Principal Propagation.

Continue to - [Exercise 2](../ex2/README.md)
