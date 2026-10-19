# Exercise 9 — Observe, Approve, and Verify the AI-Orchestrated Onboarding Workflow

The new hire event has been received. Your agent is now working — calling MCP tools, retrieving enterprise data, grounding decisions in company policy, and preparing an onboarding plan for your review.

In this exercise, you will observe the agent orchestrate the full onboarding workflow in real time, approve the plan as the hiring manager, and verify the outcomes — a Purchase Requisition in SAP S/4HANA and a Support Case in SAP Service Cloud V2. You will then close the loop by reviewing the MCP tool call analytics in SAP Integration Suite.

> [!NOTE]
> This exercise continues directly from [Exercise 8](../ex8/README.md). Your agent terminal must be open and the new hire event must have been triggered before proceeding.

## Exercise 9.1 — Observe the Agent Orchestrate the Workflow

In this sub-exercise, you watch the agent process the new hire event step by step in the terminal — fetching employee data, retrieving HR policy, and preparing the onboarding plan.

### Step 1: Agent Fetches the New Hire Profile

After receiving the event, the agent calls the **Employee Lookup MCP** to retrieve the new hire's full profile from SAP SuccessFactors via API Composition — personal details, job information, manager, department, and location in a single tool call.

   <br>![Terminal — agent fetching new hire profile via Employee Lookup MCP]()

### Step 2: Agent Retrieves the Applicable HR Policy

The agent calls the **HR Policy Lookup MCP**, which queries the SAP AI Launchpad Orchestration Document Grounding pipeline. It returns the onboarding policy applicable to this hire — required equipment, system access entitlements, and mandatory training.

   <br>![Terminal — agent retrieving HR policy via RAG pipeline]()

### Step 3: Agent Prepares the Onboarding Plan

Using the employee profile and the HR policy, the agent prepares a structured onboarding plan and presents it in the terminal for manager review.

   <br>![Terminal — onboarding plan presented for approval]()

## Exercise 9.2 — Approve the Onboarding Plan

In this sub-exercise, you act as the hiring manager and review the onboarding plan. This is the **human-in-the-loop** gate — the agent pauses and waits for your explicit approval before taking any action in backend systems.

### Step 1: Review the Plan

Read through the onboarding plan presented in the terminal. It will include the recommended equipment, system access, and training schedule for the new hire.

### Step 2: Approve

In the terminal, type the following and press **Enter**:

```
approve
```

   <br>![Terminal — manager types approve]()

> [!NOTE]
> The agent will not proceed until you explicitly approve. If you type anything other than `approve`, the agent will treat it as a rejection and stop the workflow.

## Exercise 9.3 — Verify the Results

After approval, the agent calls the remaining two MCP Servers in parallel — creating the Purchase Requisition in SAP S/4HANA and the Support Case in SAP Service Cloud V2.

### Step 1: Open the Purchase Requisition

The agent prints a direct link to the Purchase Requisition created in SAP S/4HANA. Click the link — it will open S/4HANA and log you in automatically via Principal Propagation, showing the PR created for the new hire's equipment.

   <br>![Terminal — PR created, link printed]()

   <br>![SAP S/4HANA — Purchase Requisition opened]()

### Step 2: Open the Support Case

The agent also prints a direct link to the Support Case created in SAP Service Cloud V2 for the new hire's IT system access and training enrollment. Click the link to verify the case was created with the correct details.

   <br>![Terminal — Support Case created, link printed]()

   <br>![SAP Service Cloud V2 — Support Case opened]()

## Exercise 9.4 — View MCP Analytics in SAP Integration Suite

Every tool call made by the agent was routed through the SAP Integration Suite MCP Gateway — authenticated, authorized, logged, and traceable. In this sub-exercise, you view the analytics to see every MCP call the agent made during this workflow.

### Step 1: Navigate to Analytics

1. In SAP Integration Suite, navigate to **Analyze**.
2. Select **APIs** from the sub-menu.

   <br>![SAP Integration Suite — navigate to Analyze → APIs]()

### Step 2: Review MCP Tool Call Logs

You will see all MCP tool calls recorded during the onboarding workflow — the tool name, caller identity, response time, and status for each call. This is the governance and observability layer that makes every agent action auditable.

   <br>![Analytics — MCP tool call logs for the onboarding workflow]()

> [!NOTE]
> Principal Propagation ensures that each tool call log shows the identity of the authenticated user — not a shared service account. This is the audit trail that makes the agentic workflow enterprise-ready.

## Summary

You have now completed the full employee onboarding workflow — from a new hire event in SAP SuccessFactors to governed, traceable outcomes across multiple SAP systems.

- The agent fetched the new hire profile and applicable HR policy via **MCP tools governed by SAP Integration Suite**
- You approved the onboarding plan as the hiring manager — the **human-in-the-loop** gate ensured no automated action without consent
- A **Purchase Requisition** was created in SAP S/4HANA for the new hire's equipment
- A **Support Case** was created in SAP Service Cloud V2 for IT system access and training enrollment
- Every tool call was **authenticated, logged, and traceable** via SAP Integration Suite MCP Gateway analytics

Continue to - [Exercise 10](../ex10/README.md)
