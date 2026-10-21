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

### Step 3: Agent Creates the Purchase Requisition and Support Case

Immediately after your approval, the agent calls the two remaining MCP tools:

1. **`post_newhire_equipments`** (via Create PR S/4HANA MCP) — creates a Purchase Requisition in SAP S/4HANA for the new hire's equipment and returns the PR number and a direct link
2. **`post_sap_c4c_api_v1_case_service_cases`** (via Create SC Service Cloud MCP) — creates a Support Case in SAP Service Cloud V2 for system access and training assignment and returns the case ID and a direct link

   <br>![Terminal — agent creating Purchase Requisition in SAP S/4HANA](/exercises/ex9/images/Approved_PR_Created.png)

   <br>![Terminal — agent creating Support Case in SAP Service Cloud V2](/exercises/ex9/images/Approved_SC_Created.png)

### Step 4: Review the Onboarding Summary

Once both actions complete, the agent prints a full **Onboarding Summary** in the terminal — a structured table for each outcome with the PR number, Case ID, and a direct link to each record.

   <br>![Terminal — Onboarding Summary with PR link and Service Cloud Case link](/exercises/ex9/images/Onboarding_Summary.png)

> [!IMPORTANT]
> The summary contains two clickable links — the **PR Link** for SAP S/4HANA and the **Case Link** for SAP Service Cloud V2. Hold **Ctrl** and click each link to open it in your browser. You will use these links in Exercise 9.3 to verify the results.

### Step 5: Confirm the Workflow is Complete

The agent closes with a final **Completed Steps** table confirming all five steps finished successfully.

   <br>![Terminal — all five steps completed](/exercises/ex9/images/Completed_Steps.png)

## Exercise 9.3 — Verify the Results

With both backend records created, you will now open each one directly from the links in the Onboarding Summary and verify the details.

### Step 1: Open the Purchase Requisition in SAP S/4HANA

1. In the terminal, **Ctrl+Click** the **PR Link** from the Onboarding Summary. Your browser will open the SAP S/4HANA login page.

2. Log in with your workshop credentials:
   - **User Name:** `IN261-0XX` *(replace XX with your participant number)*
   - **Password:** *will be shared by the instructors*

   Click **Continue**.

   <br>![SAP S/4HANA — login page](/exercises/ex9/images/S4HANA_Login.png)

3. After login, the Purchase Requisition opens directly. Verify the **General Information** tab — PR number, employee name, number of items, and total value.

   <br>![SAP S/4HANA — Purchase Requisition General Information](/exercises/ex9/images/PR_General_Summary.png)

4. Click the **Items** tab to see the individual equipment lines ordered for the new hire.

   <br>![SAP S/4HANA — Purchase Requisition Items](/exercises/ex9/images/PR_Items.png)

### Step 2: Open the Support Case in SAP Service Cloud V2

1. Return to the terminal and **Ctrl+Click** the **Case Link** from the Onboarding Summary. Your browser will open the SAP Service Cloud V2 login page.

2. Click **Sign in using SSO**.

   <br>![SAP Service Cloud V2 — login page, click Sign in using SSO](/exercises/ex9/images/ServiceCloud_SSO_Login.png)

3. On the IAS login page, enter your workshop credentials:
   - **User Name:** `IN261-0XX` *(replace XX with your participant number)*
   - **Password:** *will be shared by the instructors*

   Click **Continue**.

   <br>![SAP Service Cloud V2 — IAS SSO login](/exercises/ex9/images/ServiceCloud_IAS_Login.png)

4. The Support Case opens directly on the **Timeline** tab. Verify the case subject, status, and creation timestamp.

   <br>![SAP Service Cloud V2 — Case Timeline](/exercises/ex9/images/ServiceCloud_Case_Timeline.png)

5. Click the **General** tab to see the **Involved Parties** — the processor, reporter, service team, and employee linked to this case.

   <br>![SAP Service Cloud V2 — Case General (Involved Parties)](/exercises/ex9/images/ServiceCloud_Case_General.png)

6. Click the **Changes** tab to see the **Document Flow** — confirming when the case was created and its current status.

   <br>![SAP Service Cloud V2 — Case Changes (Document Flow)](/exercises/ex9/images/ServiceCloud_Case_Changes.png)

## Exercise 9.4 — View MCP Analytics in SAP Integration Suite

Every tool call made by the agent was routed through the SAP Integration Suite MCP Gateway — authenticated, authorized, logged, and traceable. In this sub-exercise, you verify that your MCP Servers are running and explore the analytics dashboard to see call volumes, response times, and error patterns for your MCP tools.

### Step 1: Navigate to Monitor

1. In SAP Integration Suite, navigate to **Monitor → Integrations and APIs** in the left navigation.

   <br>![Integration Suite — Monitor → Integrations and APIs](/exercises/ex9/images/Monitor_Overview.png)

2. Under **Manage Integration Content**, you will see all deployed MCP Servers with their runtime status. Confirm that all your MCP Servers show **Started** and that the **Error** count is **0**.

### Step 2: Verify Your MCP Server Deployment

1. Click on **`IN261_0XX_Employee_Lookup_MCP`** (replace XX with your participant number) from the Integration Content list.

2. The detail panel on the right shows the **Endpoints** tab with your MCP Server URL, the **Status Details** confirming successful deployment, and the deployment metadata.

   <br>![Monitor — MCP Server detail with Endpoints and deployment status](/exercises/ex9/images/Monitor_MCP_Deployed.png)

### Step 3: Navigate to Analyze

1. In the left navigation, click **Analyze**.
2. Set the filters at the top of the page:
   - **Runtime:** `Integration Cell`
   - **Artifact Type:** `MCP Server`

   <br>![Analyze — Overview KPIs for MCP Servers](/exercises/ex9/images/Analyze_Overview_KPIs.png)

3. The **Overview** tab shows three KPIs for the selected period — **Total MCP Calls**, **MCP Response Time**, and **Total MCP Errors** — giving you a health snapshot of all MCP activity across your tenant.

### Step 4: Explore MCP Call Charts

Scroll down the Overview page to see the detailed charts.

The **MCP Server Calls** chart shows the daily call count over time — broken down by cumulative, failure, and success.

   <br>![Analyze Overview — MCP Server Calls daily chart](/exercises/ex9/images/Analyze_Daily_Call_Count.png)

Further down, **Top MCP Servers of the Week** shows which of your MCP Servers received the most calls, and **Top Subscriptions for Agents** shows the agent subscriptions driving that traffic.

   <br>![Analyze Overview — Top MCP Servers and Top Subscriptions for Agents](/exercises/ex9/images/Analyze_Top_MCP_Servers.png)

### Step 5: Review the Health Tab

Click the **Health** tab at the top of the Analyze page.

The Health view shows **MCP Server Errors by Tool vs Resource** and the **MCP Policy Errors** heatmap — breaking down which MCP Servers experienced policy-level failures, how many, and when.

   <br>![Analyze — Health tab with MCP Policy Errors heatmap](/exercises/ex9/images/Analyze_Health_Tab.png)

> [!NOTE]
> The analytics dashboard covers all MCP Servers across your tenant — not just the ones called during this workflow. This is the governance and observability layer that makes every agent action auditable and enterprise-ready.

## Summary

You have now completed the full employee onboarding workflow — from a new hire event in SAP SuccessFactors to governed, traceable outcomes across multiple SAP systems.

- The agent fetched the new hire profile and applicable HR policy via **MCP tools governed by SAP Integration Suite**
- You approved the onboarding plan as the hiring manager — the **human-in-the-loop** gate ensured no automated action without consent
- A **Purchase Requisition** was created in SAP S/4HANA for the new hire's equipment
- A **Support Case** was created in SAP Service Cloud V2 for IT system access and training enrollment
- Every tool call was **authenticated, logged, and traceable** via SAP Integration Suite MCP Gateway analytics

Continue to - [Exercise 10](../ex10/README.md)
