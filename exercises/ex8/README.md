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
<summary><strong>Route 1 — Add a New Employee in SAP SuccessFactors to Trigger a New Hire Event</strong></summary>

1. Logon to the [SAP SuccessFactors](https://url.sap/1bfohh) tenant using the user ID and password that is already provided by the instructors.
   For login, use the participant number that is assigned to you.
   	
	- **User Name:** **`IN261-0**`** *(replace ** with your assigned participant number)*
   	- **Password:** *will be shared by the instructors*
   	  
   	![Log on to SAP Success Factors](images/ex81-1.png)
   
2. Search for `Add new Employee` and select **`Add New Employee`** list entry and this would bring us to the **Add New Employee Wizard**.

   ![Add New Employee](images/ex81-2.png)

3. In the **Identity** section, enter the following details:
   
   	| Field | Value |
	|---|---|
	| **Hire Date** | Keep the default current date |
	| **Company** | From the drop-down list — BestRun (10000) |
	| **Event Reason** | From the drop-down list — New Hire (HIRNEW) |
	| **First Name** | Any name of your choice |
	| **Last Name** | Any name of your choice |
      
	 ![Add New Employee Identity](images/ex81-3.png)

4. In the **Biographical** and **Employee Information** section, enter the following details:
   
   	| Field | Value |
	|---|---|
	| **Date of Birth** | Select any date, e.g. Feb 29, 1992 |
	| **Country of Birth** | Select any country, e.g. India |
	| **Person ID** | **`IN261_0**`** *(replace ** with your assigned participant number)* |
	| **User Name** | **`IN261-0**`** *(replace ** with your assigned participant number)* |

    Then we click **`Continue`** to continue the wizard.
	
	![Add New Employee Biographical and Employee Information](images/ex81-4.png)

5. In the **Personal Information** section, enter the following details:
	| Field | Value |
	|---|---|
	| **Gender** | Your choice |
	| **Preferred Language** | Select any from the drop-down list, e.g. English (UK) |
	| **Nationality** | Pre-filled |
	| **Email Type** | **`Business`** |
	| **Email Address** | Enter any email address of your choice |
	| **Is Primary** | From the drop-down list — **`Yes`** |

	Then we click **`Continue`** to continue the wizard.

	![Add New Employee Personal Information](images/ex81-5.png)
	![Add New Employee Email Information](images/ex81-6.png)

6. In the **Job Information** section, enter the following details:

	<blockquote>
	<b>⚠️ Important</b><br>
	You can choose any available position. Note that the BestRun equipment, system access, and training enrollment are determined by position, department, division, etc. — specifically <b>Research and Development</b>, <b>Sales</b>, <b>Marketing</b>, <b>Manufacturing</b>, and <b>Default</b>. The selected position will determine the equipment, system access, and training requirements in the onboarding plan.
	</blockquote>

	**Select Position:** From drop down list - Select any available position (It automatically fills all the other fields)
    
	![Add New Employee Job Information](images/ex81-7.png)

7. Scroll down, in **Employment Details**, set the **First Date Worked** as same as **Hire Date** i.e. today's date.
   
   Then we click **`Continue`** to continue the wizard.
	
	![Add New Employee Personal Information](images/ex81-8.png)

8. As a final step to create a new employee, click on the **`Submit`** button.

   ![Submit to Add New Employee](images/ex81-9.png)
   
9. When the employee has been added successfully, the following screen appears.
   <br>This will also trigger a new hire event to AEM, which in turn triggers the Employee Onboarding AI Agent that you started in Exercise 7.
    
   ![Confirmation of New Employee](images/ex81-10.png)

</details>

**or**

<details>
<summary><strong>Route 2 —  Publish the New Hire Event directly from the AEM Try Me! Tool</strong></summary>

1. Log on to [AEM](https://url.sap/hu5r93) tenant using the user ID and password that is already provided by the instructors.
   For login, use the participant number that is assigned to you.
	
	- **User Name:** **`IN261-0**`** *(replace ** with your assigned participant number)*
   	- **Password:** *will be shared by the instructors*
   	  
   ![Log on](images/ex82-1.png)

2. Click **`Cluster Manager`** in the left navigation menu on the landing page.

   ![Cluster Manager](images/ex82-2.png)
   
3. In the **`Cluster Manager: Services`** screen, click on the **`TechEd2026-IN261`** tile.
   >**HINT:** If you cannot see the tile, uncheck the **`Only show my services`** checkbox.

	![Cluster](images/ex82-3.png)

4. Switch to the **`Try Me!`** tab.
	
	![Try Me!](images/ex82-4.png)

5. To connect to the event broker, **copy** the **`Client Password`**.

   ![Copy Client Password](./images/ex82-5.png)

6. Once you copy the Client Password, click on **`Open Broker Manager`** button.

   ![Open Broker Manager](./images/ex82-6.png)
   
7. A new browser tab will open. Click on the **expand** **`>`** **Icon** under **`Publisher`** section to show the connection details.

   ![Expand](./images/ex82-7.png) 

8. Paste the copied client password from step 5 into the **`Client Password`** field and click on the **"Connect"** button.

   ![Paste Client Password](./images/ex82-8.png) 

9. You will see the **`Connected`** text appear under the **`Publisher`** section.
    ![Connected](./images/ex82-9.png) 

10. Choose **`Queue`** radio button.
    ![Choose Queue](./images/ex82-10.png) 

11. Enter **`IN261_0**`** as the Queue name *(replace ** with your assigned participant number, e.g. `IN261_050`)* — the same queue you created in Exercise 1. Change **`Delivery Mode`** to **`Direct`**.

12.  Choose an employee from the table below and copy their Employee ID — the HR Policy Lookup MCP will return different equipment, system access, and training requirements based on the employee's role and department:

		| Employee ID | Employee Name | Role |
		|---|---|---|
		| `E12` | Elizabeth Miller | Engineer III | 
		| `E10` | Fonda Yen | Engineer I | 
		| `awegner` | Anna Wegner | Sales Manager | 
		| `ATPUSER` | Andrew Phillips | Sales Representative |
		| `MCARTER` | Mary Carter | Marketing Manager |
		| `M1` | Molly Huddleston | Marketing Manager |
		| `E19` | Dawn Marie Hensley | Maintenance Engineer I | 
		| `E22` | Andy Rise | Maintenance Engineer III | 

		Add the following **`Message Content`**, replacing the **employeeId** with the ID you chose:
		```json
		{
		  "context": {
		    "employeeId": "<replace with employee id of your choice from the table above>"
		  }
		}
		```

    	Click **"Publish"** to send your message to the queue.
	
    	![Publish message to the Queue](./images/ex82-11.png) 

13. You see under **`Messages Published`** that 1 Direct message has been published successfully.

	> **💡 Direct message is delivered immediately to any active consumer (your running agent). If the consumer is not connected at the moment of publishing, the message is lost. No guarantee of delivery, so make sure your agent is running else run the agent and publish it again or change the Delivery Mode to Persistent.**

	![Stats](./images/ex82-12.png) 

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
