# Exercise 8 — Trigger a New Hire Event
In this exercise, you will trigger a new hire event that your agent is waiting for. Choose the route that works best for you — both result in a **NEWHIRE** event arriving in your AEM queue and your agent starting the onboarding workflow.

⏱ **Estimated time: 10 minutes**

## Overview
Your agent is running and subscribed to your AEM queue — waiting for a **NEWHIRE** event from SAP SuccessFactors. In this exercise, you trigger that event.

There are two routes to choose from:

- **Route 1: Add a New Employee in SAP SuccessFactors** — triggers a real **NEWHIRE** event through the standard SuccessFactors new hire flow. This is the production-realistic path.
- **Route 2: Publish the Event directly from AEM** — uses the AEM Try Me feature to publish a pre-defined **NEWHIRE** event payload directly to your queue.  This is the fallback path if you want a faster trigger, or if SAP SuccessFactors is unavailable or has some issues.

By the end, a **NEWHIRE** event has been published to your AEM queue and your agent has received it — the onboarding workflow has begun.

> [!NOTE]
> Ensure your agent from [Exercise 7](../ex7/README.md) is still running in the terminal before proceeding. If the terminal was closed, re-run **`node agent.js`** from the **`IN261_employee_onboarding_ai_agent`** folder.

## Exercise 8.1 — Choose your Route and Trigger the Event

<details>
<summary><strong>Route 1 — Add a New Employee in SAP SuccessFactors</strong></summary>
...steps...
</details>

<details>
<summary><strong>Route 2 — Publish the Event directly from AEM</strong></summary>
...steps...
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
