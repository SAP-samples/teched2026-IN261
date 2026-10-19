# Exercise 8 — Trigger a New Hire in SAP SuccessFactors

With the AI agent running and listening on your AEM queue, the final piece is the trigger — a real new hire event from SAP SuccessFactors.

In this exercise, you will navigate to the SAP SuccessFactors system and add a new employee. The moment the profile is submitted, SAP SuccessFactors publishes a **HiringComplete** event to SAP Advanced Event Mesh. Your agent, subscribed to the queue you configured in Exercise 1, receives the event and immediately begins orchestrating the onboarding workflow.

> [!NOTE]
> Ensure your agent from [Exercise 7](../ex7/README.md) is still running in the terminal before proceeding. If the terminal was closed, re-run `node agent.js` from the `IN261_employee_onboarding_ai_agent` folder.

## Summary

You have triggered the onboarding workflow by adding a new hire in SAP SuccessFactors. The **HiringComplete** event has been published to SAP Advanced Event Mesh and your agent has received it — the orchestration has begun.

Continue to - [Exercise 9](../ex9/README.md)
