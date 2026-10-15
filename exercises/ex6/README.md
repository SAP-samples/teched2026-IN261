# Exercise 6 — Publish MCP Servers as a Product and Subscribe via Developer Hub

With three MCP Servers deployed across Exercises 2, 3, and 4, the onboarding agent now has governed access to employee data, HR policies, and purchase requisition creation. But deployment alone is not enough — for an AI agent to discover, authenticate against, and consume these servers, they need to be published as a **Product** in the **Developer Hub**.

In this exercise, you will publish all three MCP Servers as a single product in the Developer Hub, create a subscription for your AI agent, and retrieve the OAuth credentials the agent needs to authenticate. By the end, your onboarding agent has everything it needs to connect and call all three MCP tools.

> [!NOTE]
> This exercise builds on the three MCP Servers deployed in [Exercise 2](../ex2/README.md), [Exercise 3](../ex3/README.md), and [Exercise 4](../ex4/README.md). Ensure all three show **Runtime Status: STARTED** before proceeding.

## What is the Developer Hub?

The **Developer Hub** is the API catalog built into SAP Integration Suite. It is where APIs and MCP tools are published, discovered, and subscribed to — giving organizations a governed, self-service way to expose enterprise capabilities to consumers, including AI agents.

When an MCP Server is published as a **Product** in the Developer Hub, it becomes:

- **Discoverable** — AI agents and developers can find it through the catalog
- **Subscribable** — Consumers request access by creating a subscription
- **Credentialed** — Each subscription generates a unique OAuth Client ID and Secret, scoped to that subscriber alone

This is how SAP Integration Suite ensures that every agent call is authenticated, authorized, and traceable back to a specific subscriber — not a shared service account.

## Exercise 6.1 — Publish MCP Servers as a Product

In this sub-exercise, you navigate to the Developer Hub, select all three deployed MCP Servers, and publish them as a single product.

### Step 1: Navigate to Developer Hub

1. From any screen in SAP Integration Suite, click the **grid icon (⠿)** in the top-right navigation bar.
2. In the menu, click **Developer Hub**.

   <br>![Navigate to Developer Hub](/exercises/ex6/images/Navigate_DeveloperHub.png)

### Step 2: Open Admin Center → Content

1. In the Developer Hub top navigation bar, click **Admin Center**.
2. From the dropdown, select **Content**.

   <br>![Admin Center — Content](/exercises/ex6/images/Admin_Center_Content.png)

### Step 3: Select the Business System

1. On the **Manage Content** page, click the **Business Systems** tab.
2. Click on your Integration Suite business system to open it.

   <br>![Business Systems list](/exercises/ex6/images/Business_Systems.png)

### Step 4: Select the AI Artifacts

1. Click the **AI Artifacts** tab. You will see all three MCP Servers deployed in this session.
2. Select all three MCP Servers for your participant number by checking the checkbox next to each:
   - `IN261_0XX_Employee_Lookup_MCP`
   - `IN261_0XX_HR_Policy_Lookup_MCP`
   - `IN261_0XX_Create_PR_S4Hana_MCP`

   > Replace **XX** with your assigned participant number.

3. Click **Create Product**.

   <br>![AI Artifacts — select all three MCP Servers and click Create Product](/exercises/ex6/images/AI_Artifacts.png)

### Step 5: Fill in Product Details and Publish

Fill in the product details:

| Field | Value |
|---|---|
| Name | `IN261_0XX_Onboarding_Agent_Product` (replace XX with your assigned participant number) |
| ID | `IN261_0XX_Onboarding_Agent_Product` (replace XX with your assigned participant number) |
| Short Text | `MCP Product bundling Employee Lookup, HR Policy Lookup, and Purchase Requisition MCP Servers for the onboarding agent` |
| Description | `MCP Product bundling Employee Lookup, HR Policy Lookup, and Purchase Requisition MCP Servers for the onboarding agent` |

Click **Publish**.

   <br>![Create Product dialog — fill in details and click Publish](/exercises/ex6/images/Create_Product.png)

> [!NOTE]
> Publishing triggers an AI-assisted content creation process in the background. Wait a moment for the product to appear in the Developer Hub catalog before proceeding to the next step.

### Step 6: Confirm the Product is Live

In the top navigation bar, click **Developer Hub** to return to the home page. Your product `IN261_0XX_Onboarding_Agent_Product` should now appear in the catalog.

   <br>![Product live in Developer Hub catalog](/exercises/ex6/images/Product_Published.png)

## Exercise 6.2 — Subscribe to the Product

In this sub-exercise, you subscribe to the published product, creating the agent subscription that will generate the OAuth credentials your AI agent needs.

### Step 1: Open the Product and Subscribe

1. Click on **`IN261_0XX_Onboarding_Agent_Product`** from the Developer Hub catalog.

   > Replace **XX** with your assigned participant number.

2. On the product detail page, click **Subscribe → Create New Subscription for Agent**.

   <br>![Product detail — Subscribe dropdown](/exercises/ex6/images/Subscribe.png)

### Step 2: Fill in Subscription Details

Fill in the subscription details:

| Field | Value |
|---|---|
| Name | `IN261_0XX_Onboarding_Agent_Subscription` (replace XX with your assigned participant number) |
| Short Text | `Subscription for the onboarding AI agent to access Employee Lookup, HR Policy, and Purchase Requisition MCP tools` |
| Description | `Subscription for the onboarding AI agent to access Employee Lookup, HR Policy, and Purchase Requisition MCP tools` |

Click **Create**.

   <br>![Create New Subscription for Agent](/exercises/ex6/images/Create_Subscription.png)

> [!NOTE]
> Subscription provisioning may take a short while. Proceed to Exercise 6.3 once the subscription appears under **My Workspace → Subscriptions → Agents**.

## Exercise 6.3 — Retrieve OAuth Credentials

In this sub-exercise, you retrieve the OAuth credentials generated for your subscription. These are what the onboarding AI agent will use to authenticate against all three MCP tools.

### Step 1: Open My Workspace

1. In the Developer Hub top navigation bar, click **My Workspace**.
2. On the Subscriptions page, click the **Agents** tab.
3. Click on **`IN261_0XX_Onboarding_Agent_Subscription`** to open it.

   > Replace **XX** with your assigned participant number.

   <br>![My Workspace — Agents subscriptions list](/exercises/ex6/images/My_Workspace.png)

### Step 2: Copy Your Credentials

On the subscription **Overview** page, locate the **Credentials** section. Copy and save all three values — your AI agent will need them in the next exercise:

| Credential | What it is |
|---|---|
| **Token URL** | OAuth token endpoint — used to exchange credentials for a bearer token before calling any MCP tool |
| **Key** | OAuth Client ID |
| **Secret** | OAuth Client Secret |

> [!NOTE]
> It may take a short while for the credentials to appear. If the Credentials section is not yet visible, refresh the page and try again.

   <br>![Subscription — Credentials section](/exercises/ex6/images/Credentials.png)

## Summary

You have now published all three MCP Servers as a governed product in the Developer Hub and retrieved the OAuth credentials for your onboarding AI agent.

- All three MCP tools — **Employee Lookup**, **HR Policy Lookup**, and **Purchase Requisition** — are bundled into a single discoverable product
- The Developer Hub manages access control — only subscribed agents with valid credentials can authenticate and invoke the tools
- The OAuth credentials (Token URL, Client ID, Client Secret) are what the AI agent uses to get a bearer token before calling any MCP tool

Continue to - [Exercise 7](../ex7/README.md)
