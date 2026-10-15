# Exercise 6 — Publish MCP Servers as a Product and Subscribe via Developer Hub

With three MCP Servers deployed across Exercises 2, 3, and 4, the onboarding agent now has governed access to employee data, HR policies, and purchase requisition creation. But deployment alone is not enough — for an AI agent to discover, authenticate against, and consume these servers, they need to be published as a **Product** in the **Developer Hub** and need to be **Subscribed**.

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

1. From your Integration Package `IN261_0XX_TechEd_2026_Package` (replace XX with your assigned participant number), click the **grid icon (⠿)** in the top-right navigation bar.

   <br>![Click the grid icon from the Integration Package](/exercises/ex6/images/Navigate_to_DeveloperHub.png)

2. In the menu that appears, click **Developer Hub**.

   <br>![Click Developer Hub from the grid menu](/exercises/ex6/images/Navigate_to_DeveloperHub2.png)

### Step 2: Developer Hub Home

You will land on the **Developer Hub** home page — the centralized catalog for APIs, Events, and MCP Servers.

Notice that a product **`IN261 Employee Onboarding`** is already visible in the catalog. This is a **solution product** pre-configured by the session instructors — it contains the same four MCP Servers bundled and published as a reference. In the steps below, you will create your own participant-specific product (`IN261_0XX Employee Onboarding`) in exactly the same way.

   <br>![Developer Hub home page — solution product already visible in catalog](/exercises/ex6/images/DeveloperHub_HomePage.png)

### Step 3: Open Admin Center → Content

1. In the Developer Hub top navigation bar, click **Admin Center**.
2. From the dropdown, select **Content**.

   <br>![Admin Center — select Content](/exercises/ex6/images/CreateProduct.png)

### Step 4: Select the Business System

1. On the **Manage Content** page, the **Business Systems** tab is active by default.
2. Click on the business system **`workshop01b`** to open it.

   <br>![Manage Content — Business Systems list](/exercises/ex6/images/CreateProduct2.png)

### Step 5: Select the AI Artifacts

1. Click the **AI Artifacts** tab. You will see all MCP Servers deployed on this business system.

2. Use the **Search by Name** field to filter by your participant number (e.g. `IN261_050`). This narrows the list to your three MCP Servers.

3. Select all three by checking the checkbox next to each:
   - `IN261_0XX_Create_PR_S4Hana_MCP` (replace XX with your assigned participant number)
   - `IN261_0XX_Employee_Lookup_MCP` (replace XX with your assigned participant number)
   - `IN261_0XX_HR_Policy_Lookup_MCP` (replace XX with your assigned participant number)

   <br>![AI Artifacts — filter by participant number and select your three MCP Servers](/exercises/ex6/images/CreateProduct3.png)

4. Clear the search and search for **`ServiceCloud`**. Select `IN261_Create_SC_ServiceCloud_MCP` to add it to your selection.

   <br>![AI Artifacts — select the ServiceCloud MCP Server](/exercises/ex6/images/CreateProduct4.png)

5. Clear the search field. You should now see **4 Selected AI artifacts**. Click **Create Product**.

   <br>![AI Artifacts — 4 selected, click Create Product](/exercises/ex6/images/CreatePrdouct5.png)

### Step 6: Fill in Product Details and Publish

Fill in the product details:

| Field | Value |
|---|---|
| Name | `IN261_0XX Employee Onboarding` (replace XX with your assigned participant number) |
| ID | `IN261_0XX_Employee_Onboarding` (replace XX with your assigned participant number) |
| Short Text | `The IN261_0XX Employee Onboarding MCP Artifacts are deployed as a product.` (replace XX with your assigned participant number) |
| Description | `The IN261_0XX Employee Onboarding MCP Artifacts are deployed as a product.` (replace XX with your assigned participant number)|

Click **Publish**.

   <br>![Create Product dialog — fill in details and click Publish](/exercises/ex6/images/CreateProduct6.png)

### Step 7: Acknowledge the Confirmation

An **Information** dialog confirms that your publish request has been submitted. Click **Close**.

   <br>![Information dialog — request submitted](/exercises/ex6/images/CreateProduct7.png)

> [!NOTE]
> Publishing triggers an AI-assisted content creation process in the background. You can monitor progress in **Admin Center → Scheduled Requests**.

### Step 8: Monitor via Scheduled Requests

1. Click **Admin Center** and select **Scheduled Requests**.

   <br>![Admin Center — Scheduled Requests](/exercises/ex6/images/CreateProduct8.png)

2. You will see your product `IN261_0XX Employee Onboarding` listed with status **In Progress**. Wait until the status changes to **Success** before proceeding.

   <br>![Scheduled Requests — In Progress](/exercises/ex6/images/CreateProduct9.png)

### Step 9: Confirm the Product is Live

Once the Scheduled Request shows **Success**, click **Developer Hub** in the top navigation bar to return to the home page. Your product `IN261_0XX Employee Onboarding` should now appear in the catalog.

   <br>![Developer Hub home — product live in catalog](/exercises/ex6/images/CreateProduct10.png)

## Exercise 6.2 — Subscribe to the Product

In this sub-exercise, you subscribe to the published product, creating the agent subscription that will generate the OAuth credentials your AI agent needs.

### Step 1: Open the Product and Subscribe

1. Click on **`IN261_0XX Employee Onboarding`** from the Developer Hub catalog.

   > Replace **XX** with your assigned participant number.

2. On the product detail page, click **Subscribe** (top right) and select **Create New Subscription for Agent** from the dropdown.

   <br>![Product detail — Subscribe dropdown](/exercises/ex6/images/SubscribeProduct1.png)

### Step 2: Fill in Subscription Details

In the **Create New Subscription for Agent** dialog, fill in the following details:

- **Name:** `IN261_0XX Employee Onboarding AI Agent` *(replace XX with your assigned participant number)*
- **Short Text:** `IN261_0XX Employee Onboarding AI Agent` *(replace XX with your assigned participant number)*
- **Description:** `IN261_0XX Employee Onboarding AI Agent` *(replace XX with your assigned participant number)*

For the **Client ID** field, the value is pre-populated based on your tenant. Copy the value that matches your workshop tenant and paste it in:

**workshop01a:**
```
d3230023-bd4b-46ae-901e-65acaeda9ab4
```

**workshop01b:**
```
65cd1664-3e34-42c7-abbe-e9af594be5cd
```

> [!IMPORTANT]
> The Client ID is already configured as an environment variable in your AI agent. It is important to use the **exact Client ID** matching your tenant — using the wrong value will cause authentication to fail.

Click **Create**.

   <br>![Create New Subscription for Agent dialog](/exercises/ex6/images/SubscribeProduct2.png)

### Step 3: Wait for Subscription Finalization

After clicking **Create**, you will be taken to the subscription detail page. A banner will appear:

> *"Your subscription is being finalized. Please refresh the page after some time."*

Click **Refresh** after a few seconds. Once the page reloads without the banner, your subscription is ready.

   <br>![Subscription being finalized](/exercises/ex6/images/subscribeProduct2.5.png)

## Exercise 6.3 — Retrieve OAuth Credentials

In this sub-exercise, you retrieve the OAuth credentials generated for your subscription. These are what the onboarding AI agent will use to authenticate against all MCP tools.

### Step 1: Open My Workspace

1. In the Developer Hub top navigation bar, click **My Workspace**.
2. On the Subscriptions page, click the **Agents** tab. You will see your newly created subscription `IN261_0XX Employee Onboarding AI Agent` listed with Type **AI**.
3. Click on the subscription name to open it.

   > Replace **XX** with your assigned participant number.

   <br>![My Workspace — Agents subscriptions list](/exercises/ex6/images/SubscribeProduct3.png)

### Step 2: Copy Your Credentials

On the subscription **Overview** page, note the **Wellknown URL** and locate the **Credentials** section. Copy and save these values — your AI agent will need them in the next exercise:

| Credential | What it is |
|---|---|
| **Wellknown URL** | OpenID Connect discovery endpoint — used to derive the OAuth token endpoint for authentication |
| **Key** | OAuth Client Secret — click the **eye icon** (top right of the Credentials section) to reveal the value, then click the **copy icon** next to it |

> [!NOTE]
> The **Client ID** is already configured as an environment variable in your AI agent — no need to store it separately. The **Wellknown URL** and **Key** (Client Secret) are the values you need to copy from here.

> [!NOTE]
> It may take a short while for the credentials to appear. If the Credentials section is not yet visible, refresh the page and try again.

   <br>![Subscription — Credentials section](/exercises/ex6/images/SubscribeProduct4.png)

## Summary

You have now published all three MCP Servers as a governed product in the Developer Hub and retrieved the OAuth credentials for your onboarding AI agent.

- All MCP tools are bundled into a single discoverable product **`IN261_0XX Employee Onboarding`** in the Developer Hub catalog
- The Developer Hub manages access control — only subscribed agents with valid credentials can authenticate and invoke the tools
- The OAuth credentials (Token URL, Client ID, Client Secret) are what the AI agent uses to get a bearer token before calling any MCP tool

Continue to - [Exercise 7](../ex7/README.md)
