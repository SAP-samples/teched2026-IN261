# Exercise 8 — Trigger a New Hire Event
In this exercise, you will trigger a new hire event that your agent is waiting for. Choose the route that works best for you — both result in a **NEWHIRE** event arriving in your AEM queue and your agent starting the onboarding workflow.

⏱ **Estimated time: 10 minutes**

## Overview
Your agent is running and subscribed to your AEM queue — waiting for a **NEWHIRE** event from SAP SuccessFactors. In this exercise, you trigger that event.

There are two routes to choose from:

- **Route 1: Add a New Employee in SAP SuccessFactors** — triggers a real **NEWHIRE** event through the standard SuccessFactors new hire flow. This is the production-realistic path.
- **Route 2: Publish the Event directly from the AEM Try Me! Tool** — uses the AEM Try Me feature to publish a pre-defined **NEWHIRE** event payload directly to your queue.  This is the fallback path if you want a faster trigger, or if SAP SuccessFactors is unavailable or has some issues.

By the end, a **NEWHIRE** event has been published to your AEM queue and your agent has received it — the onboarding workflow has begun.

> [!NOTE]
> Ensure your agent from [Exercise 7](../ex7/README.md) is still running in the terminal before proceeding. If the terminal was closed, re-run **`node agent.js`** from the **`IN261_employee_onboarding_ai_agent`** folder.

## Exercise 8.1 — Choose your Route and Trigger the Event

<details>
<summary><strong>Route 1 — Add a New Employee in SAP SuccessFactors</strong></summary>
...steps...
</details>

<details>
<summary><strong>Route 2 —  Publish the Event directly from the AEM Try Me! Tool</strong></summary>

1. Log on to [AEM](https://url.sap/hu5r93) tenant using the user ID and password that is already provided by the instructors.
   For login, use the participant number that is assigned to you.
   ![Log on](images/ex82-1.png)

2. Click **`Cluster Manager`** in the left navigation menu on the landing page.

   ![Cluster Manager](images/ex82-2.png)
   
3. In the **`Cluster Manager: Services`** screen, click on the **`TechEd2026-IN261`** tile.
   >**HINT:** If you cannot see the tile, uncheck the **`Only show my services`** checkbox.

	![Cluster](images/ex82-3.png)

4. Switch to the **`Manage`** tab.
	
	![Manage](images/ex82-4.png)

5. Click on the **`Try Me!`** link on the left panel.

	![Try Me](images/ex82-5.png)

6. Press the **"expand" > Icon** to show the connection details. Please note, that the properties have to be adjusted else the connection will not work. We will update them in the next steps.
	
 	![Pic 13](./images/ex1-13.png) 

7. Keep your current browser tab open and **go back to the other/main tab** and click on the **"Connect"** tab. Select **View by "Protocol"**.

	![Pic 14](./images/ex1-14.png) 

8. Open the **"Solace Web Messaging"** entry and select **"Solace JavaScript API"**. Now the section on the right shows you the connection details.

9.  Copy these details into the relevant fields of the "**Try Me!**" opened in step 2 and overwrite the existing values.
   
	![Pic 15-1](./images/ex1-15.png) 

10. Press **"Connect"** button.

	>**Important**: If your browser asks to select a certificate for authentication, press **"Cancel"** otherwise the connection will fail! If you accidentially clicked on a certificate, please restart your browser to show the dialog again.

	![Pic 17](./images/ex1-17.png)  

11. Choose **"Topic"** and provide the name of the topic subscribed in step 4 of previous exercise: user**XXX**/businesspartner/change  (replace **XXX** with your user number).

12. Change "Deliver Mode" to **"Direct** and add the following "**Message Content**":
	```json
	
	
	```

13. Click **"Publish"** to send your message to the topic.

	![Pic 18](./images/ex1-18.png)  

14. You should see 1 message got published.

	![Pic 20](./images/ex1-19.png)  

15. Switch back to the tab **"Queues"** and click on your queue.

	![Pic 21](./images/ex1-20.png)  

16. You should see 1 Message got Queued.

	![Pic 22](./images/ex1-21.png)  

	You have sent a message to a topic, and via the subscription this message has been persisted in your queue.

17. You can leave it like this or you can play the same game a little longer: go back to the Try Me! tool and just click publish, and see in the other tab how the count of messages gets increased (don't forget to refresh the page).
</details>

## Summary
You have triggered the new hire onboarding workflow. Your agent has received the **NEWHIRE** event using the **SAP Advanced Event Mesh** and has begun orchestrating the onboarding process.

Continue to - [Exercise 9 — Observe, Approve, and Verify the AI-Orchestrated Onboarding Workflow](../ex9/README.md) to:

- Observe the agent call MCP tools in real time in the terminal
- Review and approve the onboarding plan as the hiring manager
- Verify the Purchase Requisition created in SAP S/4HANA
- Verify the Support Case created in SAP Service Cloud V2
- Trace the end-to-end flow via SAP SuccessFactors OData API audit logs
- Monitor MCP tool call analytics and health in SAP Integration Suite
