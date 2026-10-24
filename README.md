[![REUSE status](https://api.reuse.software/badge/github.com/SAP-samples/teched2026-IN261)](https://api.reuse.software/info/github.com/SAP-samples/teched2026-IN261)

# IN261 — Model context protocol (MCP) unlocked: Enable governed access to AI agents 

## Description

This repository contains the material for the SAP TechEd 2026 Hands-on Workshop session [IN261 | Model context protocol (MCP) unlocked: Enable governed access to AI agents](https://www.sap.com/events/teched/berlin/flow/sap/te26/catalog/page/catalog/session/1786027688241001k1kf)

In this hands-on workshop you will learn how to **operationalize AI agents** from pilots to production with governed connectivity. Learn how the **MCP Gateway in SAP Integration Suite** applies policy, **security**, and **analytics** while curating MCP tools for AI agents. Securely connect enterprise tools to an AI agent - including **unified APIs** via **API Composition**, **event-driven context** from **SAP Integration Suite, advanced event mesh (AEM)**, and **Retrieval-Augmented Generation (RAG)** grounded retrieval via **SAP AI Launchpad Orchestration** - with **governed access** using **Principal Propagation** and **human-in-the-loop** approvals to achieve **end-to-end traceability**.

⏱ **Total Duration: 120 minutes**

## Requirements
There are no dedicated requirements for this session. You will be able to complete all exercises by following the step-by-step instructions, even without prior experience with SAP Integration Suite, SAP AI Core, or AI agent development. 

You will derive the most value from this session if you have some familiarity with the following:

- SAP Integration Suite
- AI agents and large language models (LLMs)
- The Model Context Protocol (MCP)
- Event-driven architectures (events, queues, topics)
- API composition and unified APIs
- Principal Propagation and identity federation
- Retrieval-Augmented Generation (RAG)

> 💡 **This is an advanced session.** It combines multiple SAP technologies and concepts to implement a realistic end-to-end AI agentic workflow. All tenant access, credentials, and pre-configured environments are provided — just bring curiosity.

## Overview

Enterprise AI agents are only as powerful as the systems they can reliably and securely access. This session demonstrates how to build a **production-grade AI agentic workflow** that orchestrates a complete employee onboarding workflow across multiple SAP systems — governed, traceable, and identity-aware.

A new employee has just joined **Best Run** and completed their profile in SAP SuccessFactors. A **NEWHIRE** event triggers an AI agent that fetches the employee profile via **API Composition**, retrieves applicable HR policies via **RAG-grounded AI Launchpad Orchestration**, and prepares a complete onboarding plan — covering equipment, system access, and training. The hiring manager reviews and approves the plan. Only then does the agent create a **Purchase Requisition in SAP S/4HANA** and a **Support Case in SAP Service Cloud V2**.

Every tool call flows through the **SAP Integration Suite MCP Gateway** — authenticated, authorized, and traceable via **Principal Propagation** end to end. No backend is touched without policy enforcement. No action is taken without human approval.

For the full scenario walkthrough and architecture:

- [📖 Scenario Introduction](intro/README.md)

## System URL and Login Details 
To complete the exercises, the access to the following systems have been provided.
<br>**In case you are not able to access any of these systems please contact the workshop instructors**.

- [SAP Integration Suite, advanced event mesh](https://url.sap/hu5r93)
- [SAP SuccessFactors](https://url.sap/1bfohh)
- [SAP S/4HANA Cloud](https://url.sap/3iahmk)
- [SAP Service Cloud Version 2](https://url.sap/5bivj9)
- **SAP Integration Suite Tenant** - Participants are split across two pre-configured SAP Integration Suite tenants.
  <br>Use the tenant assigned to your participant number: Your participant number is printed on your seat card.
  <br>Note your tenant as you will be asked to select your tenant during the AI agent setup in Exercise 7.

  | Participant Number | Workshop Tenant |
  |---|---|
  | 001 – 020 | [**workshop 01a**](https://url.sap/500ufj) |
  | 021 – 040 | [**workshop 01b**](https://url.sap/f9f56f)|

> [!IMPORTANT]
> - _For a smooth experience, tenants have been preconfigured, and you already have all the roles and permissions needed to complete this exercise._
> - _User ID and password information will be provided to you by the instructors._
> - _When you run through the exercise steps, you need to ensure that the technical IDs of the integration artifacts that you will create are unique. Hence, add a participant number to your integration artifacts. The instructors will assign the participant number to you._
> - _Please adhere strictly to the instructions regarding the naming conventions for the artifacts you create. This will ensure successful completion of the tasks without conflicting with other participants._
> - _Do not delete, change or undeploy any artifact in the tenant other than yours._

## Exercises
The complete list of exercise steps are listed below, run through them in the given order.
This section also serves as a Table of Content; use the breadcrumb navigation at the top of the pages to return here at any time.

- [Getting Started](exercises/ex0/)
- [Exercise 1 — Set Up your SAP Integration Suite, advanced event mesh (AEM) Queue and Topic Subscription](exercises/ex1/README.md)
- [Exercise 2 — Create, Configure and Deploy an MCP Server for New Hire Employee Profile Lookup using SAP API Composition](exercises/ex2/README.md)
- [Exercise 3 — Copy, Configure and Deploy an MCP Server for HR Policy Lookup using SAP AI Launchpad Orchestration with Document Grounding](exercises/ex3/README.md)
- [Exercise 4 — Create, Configure and Deploy an MCP Server for Purchase Requisition Creation in SAP S/4HANA](exercises/ex4/README.md)
- [Exercise 5 — Copy, Configure and Deploy an MCP Server for Support Case Creation in SAP Service Cloud V2](exercises/ex5/README.md)
- [Exercise 6 — Publish Deployed MCP Servers as a Product and Subscribe to the AI Agent in Developer Hub](exercises/ex6/README.md)
- [Exercise 7 — Download, Configure, and Run the Employee Onboarding AI Agent](exercises/ex7/README.md)
- [Exercise 8 — Trigger a New Hire Event](exercises/ex8/README.md)
- [Exercise 9 — Observe, Approve, and Verify the AI-Orchestrated Onboarding Workflow](exercises/ex9/README.md)

## Feedback
Kindly provide your feedback on this hands-on session i.e. **IN261**.

## Additional Relevant Sessions
You can also gain some further knowledge around SAP Integration Suite and its capabilities by attending the following SAP TechEd Hands-on Workshop sessions:

- [IN262 | Build an integration scenario from scratch with AI assistance at every step | Hands-on Workshop](http://sap.com/events/teched/berlin/flow/sap/te26/catalog/page/catalog/session/1786027888165001UXeS)
- [IN263 | AI-supported integrations: From design quality to agent readiness | Hands-on Workshop](https://www.sap.com/events/teched/berlin/flow/sap/te26/catalog/page/catalog/session/1786028735882001Q0Hd)
- [IN163 | Build your own event-driven scenario with advanced event mesh | Hands-on Workshop](https://www.sap.com/events/teched/berlin/flow/sap/te26/catalog/page/catalog/session/1786029069284001qBM4)


## Contributing
Please read the [CONTRIBUTING.md](./CONTRIBUTING.md) to understand the contribution guidelines.

## Code of Conduct
Please read the [SAP Open Source Code of Conduct](https://github.com/SAP-samples/.github/blob/main/CODE_OF_CONDUCT.md).

## How to obtain support

Support for the content in this repository is available during the actual time of the online session for which this content has been designed. Otherwise, you may request support via the [Issues](../../issues) tab.

## License
Copyright (c) 2026 SAP SE or an SAP affiliate company and teched2026-IN261 contributors. Please see our [LICENSE](LICENSE) for copyright and license information. Detailed information including third-party components and their licensing/copyright information is available [via the REUSE tool](https://api.reuse.software/info/github.com/SAP/<your-project>).
