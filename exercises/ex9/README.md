# Exercise 9 — Observe, Approve, and Verify the AI-Orchestrated Onboarding Workflow

The new hire event has been received. Your agent is now working — calling MCP tools, retrieving enterprise data, grounding decisions in company policy, and preparing an onboarding plan for your review.

In this exercise, you will observe the agent orchestrate the full onboarding workflow in real time, approve the plan as the hiring manager, and verify the outcomes — a Purchase Requisition in SAP S/4HANA and a Support Case in SAP Service Cloud V2. You will then close the loop by reviewing the MCP tool call analytics in SAP Integration Suite.

> [!NOTE]
> This exercise continues directly from [Exercise 8](../ex8/README.md). Your agent terminal must be open and the new hire event must have been triggered before proceeding.

## Exercise 9.1 — Observe the Agent Orchestrate the Workflow

In this sub-exercise, you watch the agent process the new hire event step by step in the terminal — receiving the event, calling MCP tools, and building the full onboarding plan.

### Step 1: New Hire Event Received

The terminal confirms that the AEM message has arrived from SAP SuccessFactors. The employee ID is extracted from the event payload and the agent begins processing.

   <br>![Terminal — new hire event received from AEM](/exercises/ex9/images/NewHire_received.png)

### Step 2: Agent Calls MCP Tools

The agent makes three tool calls in sequence:

1. **`get_bestrun_hcm_employee`** (via Employee Lookup MCP) — fetches the new hire's personal details from SAP SuccessFactors
2. **`get_bestrun_hcm_employeeJob`** (via Employee Lookup MCP) — fetches job title, department, manager, business unit, and location
3. **`post_newhire_applicable_hr_policy`** (via HR Policy Lookup MCP) — sends the employee context to the RAG-grounded policy engine and retrieves the applicable equipment list, system access entitlements, and training requirements

All three calls are routed through the SAP Integration Suite MCP Gateway, authenticated with your identity token.

   <br>![Terminal — agent calling Employee Lookup and HR Policy MCP tools](/exercises/ex9/images/Calling_MCP_Tools.png)

### Step 3: Onboarding Plan Presented

The agent structures all retrieved data into a full onboarding plan and presents it in the terminal. The plan opens with a **New Hire Summary** and the **Equipment Provisioning** list — items that will be ordered via a Purchase Requisition in SAP S/4HANA.

   <br>![Terminal — new hire summary and equipment provisioning](/exercises/ex9/images/Onboarding_Plan.png)

Scrolling down, the plan continues with the **System Access** section — Day 1 auto-provisioned systems and additional systems that require a Service Cloud support case to be raised.

   <br>![Terminal — system access requirements](/exercises/ex9/images/Required_System_Access.png)

Finally, the **Training Schedule** lists mandatory courses by day 5, 10, and 30, plus role-specific training that also requires a Service Cloud support case.

   <br>![Terminal — training schedule](/exercises/ex9/images/Training_Schedule.png)

## Exercise 9.2 — Approve the Onboarding Plan

In this sub-exercise, you act as the hiring manager and approve the onboarding plan. This is the **human-in-the-loop** gate — the agent has paused and is waiting for your explicit response before taking any action in backend systems.

### Step 1: Review the Approval Prompt

After presenting the full plan, the agent addresses the manager directly and waits for a response. No backend action has been taken yet.

   <br>![Terminal — agent waiting for manager approval](/exercises/ex9/images/Human_In_The_Loop.png)

### Step 2: Approve

In the terminal, type the following and press **Enter**:

```
Approved
```

> [!NOTE]
> The agent will not proceed until you explicitly approve. If you type anything other than `Approved`, the agent will treat it as a rejection and stop the workflow.

## Exercise 9.3 — Verify the Results

After approval, the agent calls the remaining two MCP Servers in parallel — creating the Purchase Requisition in SAP S/4HANA and the Support Case in SAP Service Cloud V2.

### Step 1: Open the Purchase Requisition

The agent prints a direct link to the Purchase Requisition created in SAP S/4HANA. Click the link — it will open S/4HANA and log you in automatically via Principal Propagation, showing the PR created for the new hire's equipment.

### Step 2: Open the Support Case

The agent also prints a direct link to the Support Case created in SAP Service Cloud V2 for the new hire's IT system access and training enrollment. Click the link to verify the case was created with the correct details.

## Exercise 9.4 — View MCP Analytics in SAP Integration Suite

Every tool call made by the agent was routed through the SAP Integration Suite MCP Gateway — authenticated, authorized, logged, and traceable. In this sub-exercise, you view the analytics to see every MCP call the agent made during this workflow.

### Step 1: Navigate to Analytics

1. In SAP Integration Suite, navigate to **Analyze**.
2. Select **APIs** from the sub-menu.

### Step 2: Review MCP Tool Call Logs

You will see all MCP tool calls recorded during the onboarding workflow — the tool name, caller identity, response time, and status for each call. This is the governance and observability layer that makes every agent action auditable.

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
