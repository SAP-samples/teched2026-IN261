# Exercise 1 - Set Up Your SAP Integration Suite, advanced event mesh (AEM) Queue and Topic Subscription

## Overview

SAP Integration Suite, advanced event mesh (AEM) is the event backbone that connects SAP SuccessFactors to your AI agent. When a new hire completes onboarding in SAP SuccessFactors, a **NEWHIRE** event is published to a topic dedicated to each participant. Your agent needs a dedicated queue subscribed to that topic to receive and process the event.

Each participant needs to create a queue and subscribe to their dedicated topic. This queue will be used later in the session when configuring and running the AI agent.

## Exercise 1.1 - Create a queue and subscribe to their dedicated new hire topic in SAP Integration Suite, advanced event mesh (AEM)

Log on to SAP Integration Suite, advanced event mesh (AEM) and explore it
After completing these steps, you will have familiarized yourself with AEM. These steps will give you a first impression and an overview of AEM.

1. Log on to [SAP Integration Suite, advanced event mesh](https://url.sap/hu5r93) tenant using the user ID and password that is already provided by the instructors.

	<img src="/exercises/ex1/images/ex11-1.png" alt="Pic 11-1" width=40% height=40%>
        
2. Click **Cluster Manager** in the left navigation menu on the landing page.
   
        <img src="/exercises/ex1/images/ex11-2.png" alt="Pic 11-2" width=40% height=40%>
   
2. In the **Cluster Manager: Services** screen, click on the **teched-2025-europe** tile.

	**HINT:** If you cannot see the tile, uncheck the **"Only show my services"** checkbox.

	<img src="/exercises/ex1/images/ex11-3.png" alt="Pic 11-3" width=40% height=40%>

3. Switch to the **Manage** tab.

        <img src="/exercises/ex1/images/ex11-4.png" alt="Pic 11-4" width=40% height=40%>

4. Click on the **Queues** link button on the left panel.

        <img src="/exercises/ex1/images/ex11-5.png" alt="Pic 11-5" width=40% height=40%>

6.  A new browser tab will open listing all available queues. Click the **+ Queue** button on the top right to create a queue.
         **HINT:** You will be able to see Queues created by other participants

         <img src="/exercises/ex1/images/ex11-6.png" alt="Pic 11-6" width=40% height=40%>     

8. In the pop up, enter the queue name: <b>IN261-0**</b> *(replace ** with the participant number that is assigned to you)* and click on the **Create** button.

	<img src="/exercises/ex1/images/ex11-7.png" alt="Pic 11-7" width=40% height=40%>  

9. On the next screen, leave the configurations at their default settings, and click on the **Apply** button.
	
	<img src="/exercises/ex1/images/ex11-8.png" alt="Pic 11-8" width=40% height=40%>     

10. Select and open the queue that you have just created i.e., <b>IN162-0**</b> *(replace ** with the participant number that is assigned to you)*

        <img src="/exercises/ex1/images/ex11-9.png" alt="Pic 11-9" width=40% height=40%> 
   
11. Switch to the **Subscriptions** tab and click on the **+ Subscription** button on the top right area to create a new topic subscription.

	<img src="/exercises/ex1/images/ex11-10.png" alt="Pic 11-10" width=40% height=40%> 

12. Click on the **+ Subscription** button on the top right area to create a new topic subscription.

	<img src="/exercises/ex1/images/ex11-11.png" alt="Pic 11-11" width=40% height=40%> 

13. In the pop up, enter the topic: <b>"sap/sfsf/newhire/TechEd_IN261_0**"</b>  *(replace ** with the participant number that is assigned to you)* and click on the **Create** button.

        <img src="/exercises/ex1/images/ex11-12.png" alt="Pic 11-12" width=40% height=40%> 

14. Check whether your queue subscription has been created.

	<img src="/exercises/ex1/images/ex11-13.png" alt="Pic 11-13" width=40% height=40%>

## Summary

At the end of the first exercise, you should have familiarized yourself with SAP Integration Suite, advanced event mesh (AEM), created 2 queues, and subscribed to the relevant topics.
Please make sure that you have used the right nomenclature and replaced <b>***</b> with the participant number that is assigned to you.

Continue to - [Exercise 2 - (AI Core)](../ex2/README.md)
