# Exercise 1 - Set Up Your SAP Integration Suite, advanced event mesh (AEM) Queue and Topic Subscription
In this exercise, you will create a queue in SAP Integration Suite, advanced event mesh (AEM) and subscribe it to your dedicated new hire topic — so your agent can receive **NEWHIRE** events from SAP SuccessFactors in real-time.

⏱ **Estimated time: 10 minutes**

## Overview

SAP Integration Suite, advanced event mesh (AEM) is the event backbone that connects SAP SuccessFactors to your AI agent. When a new hire completes onboarding in SAP SuccessFactors, a **NEWHIRE** event is published to a topic dedicated to each participant. Your agent needs a dedicated queue subscribed to that topic to receive and process the event.

Each participant needs to create a queue and subscribe to their dedicated topic. This queue will be used in Exercise 7 when configuring and running the AI agent.

## Exercise 1.1 - Create a queue and subscribe to the dedicated new hire topic in AEM

1. Log on to [AEM](https://url.sap/hu5r93) tenant using the user ID and password that is already provided by the instructors.
   <br>For login, use the participant number that is assigned to you.
	
	![Log on](images/ex11-1.png)
        
2. Click **Cluster Manager** in the left navigation menu on the landing page.

   ![Cluster Manager](images/ex11-2.png)
   
3. In the **Cluster Manager: Services** screen, click on the **TechEd2026-IN261** tile.
   >**HINT:** If you cannot see the tile, uncheck the **"Only show my services"** checkbox.

	![Cluster](images/ex11-3.png)

4. Switch to the **Manage** tab.
	
	![Manage](images/ex11-4.png)

5. Click on the **Queues** link on the left panel.

	![Queues](images/ex11-5.png)

6. A new browser tab will open, listing all the available queues. Click the **+ Queue** button on the top right to create a queue.
   >**HINT:** You will be able to see the queues created by other participants as well.
	
	![Queues](images/ex11-6.png)    

7. In the pop up, enter the queue name: <b>IN261_0**</b> *(replace ** with the participant number that is assigned to you)* and click on the **Create** button.

	![Create Queue](images/ex11-7.png)     

8. On the next screen, leave the configurations at their default settings and click on the **Apply** button.
	
	![Queue Default Settings](images/ex11-8.png)     

9. Select and open the queue that you have just created i.e., <b>IN261_0**</b> *(replace ** with the participant number that is assigned to you)*

	![Open Queue](images/ex11-9.png)   
   
10. Switch to the **Subscriptions** tab.

    ![Navigate Queue Subscriptions](images/ex11-10.png)   

11. Click on the **+ Subscription** on the top right area to create a new topic subscription.

	![Create Queue Subscriptions](images/ex11-11.png)  

12. In the pop up, enter the topic: <b>"sap/sfsf/newhire/TechEd_IN261_0**"</b>  *(replace ** with the participant number that is assigned to you)* and click on the **Create** button.
    >**HINT:** Each participant will have their own topic, dynamically created by SAP SuccessFactors based on the new hire's employee ID.

    ![Add Topic Subscriptions](images/ex11-12.png)

13. Verify that your newly created topic subscription <b>"sap/sfsf/newhire/TechEd_IN261_0**"</b> appears in the list.

	![Check Topic Subscriptions](images/ex11-13.png)

## Summary

At the end of this exercise, you have created your personal AEM queue and subscribed it to your dedicated new hire event topic. With this in place, your agent will receive **NEWHIRE** events from SAP SuccessFactors in real-time as they happen.

Continue to - [Exercise 2 - Create, Configure and Deploy an MCP Server for New Hire Employee Profile Lookup using SAP API Composition](../ex2/README.md)
