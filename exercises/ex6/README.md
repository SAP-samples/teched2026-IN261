# Exercise 6 — Publish Deployed MCP Servers as a Product and Subscribe to the AI Agent in Developer Hub
In this exercise, you will publish your four deployed MCP Servers as a product in the Developer Hub and add your product to the existing AI Agent subscription — making your MCP tools available to the Employee Onboarding AI Agent.

⏱ **Estimated time: 15 minutes**

## Overview
With four MCP Servers deployed across Exercises 2, 3, 4, and 5, the employee onboarding AI agent now has governed access to employee data, HR policies, purchase requisition creation, and support case creation. But deployment alone is not enough — for an AI agent to discover and call these tools, they need to be published as a **Product** in the **Developer Hub** and linked to the **AI Agent subscription**.

The **Developer Hub** is the API and Events catalog built into SAP Integration Suite. It is where APIs, Events and MCP tools are published, discovered, and subscribed to — giving organizations a governed, self-service way to expose enterprise capabilities to consumers, including AI agents.

In this exercise, you will publish all four MCP Servers as a single product, then add your product to the existing **IN261 Employee Onboarding AI Agent** subscription. The subscription was pre-created by the instructor — by adding your product to it, you authorize the agent to authenticate and call your specific MCP tools.

By the end, your four deployed MCP Servers are bundled as a governed product in the Developer Hub and the AI Agent is authorized to call them — ready for Exercise 7.

## Exercise 6.1 — Publish your Deployed MCP Servers as a Product
In this exercise, you select all your four deployed MCP Servers, and publish them as a single product in Developer Hub.
1. From your Integration Package **`IN261_0**_TechEd_2026_Package`** *(replace ** with your assigned participant number)*, click the **grid icon (⠿)** in the top-right navigation bar.

   ![Click the grid icon from the Integration Package](images/ex61-1.png)
   
2. In the menu that appears, click **`Developer Hub`**.

   ![Click Developer Hub from the grid menu](images/ex61-2.png)

3. You will land on the **`Developer Hub`** home page.
   > **Hint:** A product **`IN261 Employee Onboarding`** is already visible in the catalog. This is a **solution product** pre-configured by the session instructors — it also contains the  four solution MCP Servers bundled and published as a reference. In the steps below, you will create your own participant-specific product in exactly the same way.

   ![Developer Hub home page — solution product already visible in catalog](images/ex61-3.png)

4. In the Developer Hub top navigation bar, click **`Admin Center`** and select **`Content`** from the dropdown menu options.

   ![Admin Center — select Content](images/ex61-4.png)

5. On the **`Manage Content`** page, the **`Business Systems`** tab is active by default.

6. Click on the business system matching your assigned tenant — **workshop01a** if your participant number is **001–020**, or **workshop01b** if your participant number is **021–040**.

   ![Manage Content — Business Systems list](images/ex61-6.png)

7. Click the **`AI Artifacts`** tab. You will see all MCP Servers from all the participants deployed on this business system.

8. Use the **`Search by Name`** field to filter by your participant number i.e. <b>`IN261_0**`</b> *(replace ** with your assigned participant number e.g. `IN261_050`)*. This narrows the list to your four MCP Servers.

9. Select all your four deployed MCP servers by checking the checkbox next to each entry:
   - **`IN261_0**_Employee_Lookup_MCP`** *(replace ** with your assigned participant number)*
   - **`IN261_0**_HR_Policy_Lookup_MCP`** *(replace ** with your assigned participant number)*
   - **`IN261_0**_Create_PR_S4Hana_MCP`** *(replace ** with your assigned participant number)*
   - **`IN261_0**_Create_SupportCase_MCP`** *(replace ** with your assigned participant number)*

   ![AI Artifacts — filter by participant number](images/ex61-7.png)

10. Verify you have **4 Selected AI artifacts**, then click **`Create Product`**.

    ![AI Artifacts — 4 selected, click Create Product](/images/ex61-8.png)

11. Fill in the product details:

      | Field | Value |
      |---|---|
      | Name |  **`IN261_0** Employee Onboarding`** *(replace ** with your assigned participant number)* |
      | ID | Auto Populated|
      | Short Text | **`The IN261_0** Employee Onboarding MCP Artifacts are deployed as a product.`** *(replace ** with your assigned participant number)* |
      | Description | **`The IN261_0** Employee Onboarding MCP Artifacts are deployed as a product.`** *(replace ** with your assigned participant number)*|

       Click **`Publish`**.

      ![Create Product dialog — fill in details and click Publish](images/ex61-9.png)

12. An **`Information`** dialog confirms your publish request has been submitted. Click **`Close`**.

    ![Information dialog — request submitted](images/ex61-10.png)

      > [!NOTE]
      > Publishing triggers an AI-assisted content creation process in the background. You can monitor progress in **Admin Center → Scheduled Requests**.

13. Click **`Admin Center`** and select **`Scheduled Requests`** from the dropdown menu options.

    ![Admin Center — Scheduled Requests](images/ex61-11.png)

14. You will see your product <b>`IN261_0** Employee Onboarding`</b> listed with status **`In Progress`**.  Wait until the status changes to **`Success`** before proceeding. Click **`Refresh`** to update the status.

    ![Scheduled Requests — In Progress](images/ex61-12.png)

15. Once the Scheduled Request shows **`Success`**, click **`Developer Hub`** in the top navigation bar to return to the home page. Your product **`IN261_0** Employee Onboarding`** should now appear in the catalog.

    ![Developer Hub home — product live in catalog](images/ex61-13.png)

## Exercise 6.2 — Add your Product to the Existing AI Agent Subscription
In this exercise, you add your published product to the existing AI Agent subscription — authorizing the employee onboarding agent to authenticate and call your MCP tools.

1. From the Developer Hub home page, click on **`IN261_0** Employee Onboarding`** *(replace ** with your assigned participant number)* to open it.

   ![Product detail page](images/ex62-1.png)

2. On the product detail page, click **`Subscribe`** (top right) and select **`Add To Existing Subscription for Agent`** from the dropdown.

   ![Subscribe — Add To Existing Subscription for Agent](images/ex62-2.png)
   
3. In the dialog, find and select **`IN261 Employee Onboarding AI Agent`** and Click **`Add`**.
   > Sometimes the entry may be disabled and you will not be able to add it — please call the instructor for support.

   ![Select existing subscription and click Add](images/ex62-3.png)

5. Your product is now linked to the existing AI Agent subscription and in **`AI Artifacts`** you see your four deployed MCP servers.

   ![Confirmation — product added to subscription](images/ex62-4.png)

6. And in **`Associated Subscriptions`** tab, you see the AI Agent subscription.
   > It may take a moment to reflect — reload the browser if the changes are not visible.

   ![Confirmation — product added to subscription](images/ex62-5.png)

> [!NOTE]
> Why add to an existing subscription instead of creating a new one?
> <br> The AI Agent authenticates to the Developer Hub using a single IAS (Identity Authentication Service) app registration — which means all participants share the same OAuth Client ID.
> <br> The Developer Hub enforces one Agent subscription per Client ID, so creating a new subscription is not possible. Instead, each participant adds their product to the one shared subscription — the agent's single identity gets access to all participant products this way.

## Summary
You have published your four MCP Servers as a governed product in the Developer Hub and added it to the existing AI Agent subscription.

- All four MCP tools are bundled into a single discoverable product **`IN261_0** Employee Onboarding`** in the Developer Hub catalog
- Adding your product to the existing subscription authorizes the agent to authenticate and call your specific MCP tools
- The agent credentials are pre-configured in the agent bundle — in Exercise 7, you only need to provide your MCP Server URLs

Continue to - [Exercise 7 — Download, Configure, and Run the Employee Onboarding AI Agent](../ex7/README.md)
