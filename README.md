[![REUSE status](https://api.reuse.software/badge/github.com/SAP-samples/teched2026-IN261)](https://api.reuse.software/info/github.com/SAP-samples/teched2026-IN261)

# IN261 - Model context protocol (MCP) unlocked: Enable governed access to AI agents 

## Description

This repository contains the material for the SAP TechEd 2026 Hands-on Workshop session [IN261 | Model context protocol (MCP) unlocked: Enable governed access to AI agents](https://www.sap.com/events/teched/berlin/flow/sap/te26/catalog/page/catalog/session/1786027688241001k1kf)

In this hands-on workshop you will learn how to **operationalize AI agents** from pilots to production with governed connectivity. Learn how the **MCP Gateway in SAP Integration Suite** applies policy, **security**, and **analytics** while curating MCP tools for AI agents. Securely connect enterprise tools to an AI agent - including **unified APIs** via **API Composiition**, **event-driven context** from **SAP Integration Suite, advanced event mesh (AEM)**, and **Retrieval-Augmented Generation (RAG)** grounded retrieval via **SAP AI Launchpad Orchestration** - with **governed access** using **Principal Propagation** and **human-in-the-loop** approvals to achieve **end-to-end traceability**.

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

---

### 📖 The Scenario

A new employee has just accepted an offer and completed their profile in SAP SuccessFactors. From this moment, multiple teams — HR, IT, Finance, and the hiring manager — need to act: equipment must be ordered, system access provisioned, training enrolled, and the manager kept in the loop.

Traditionally this involves manual handoffs, emails, and tickets across disconnected systems. In this session, an AI agent handles the entire flow automatically — triggered by a real-time event, grounded in company policy, and governed at every step.

---

### 🔁 AI Agentic WorkFlow

```
New Hire joins Best Run company, details entered using SAP SuccessFactors
        │
        ▼
SAP Advanced Event Mesh (AEM)
        │
        ▼  new hire event triggers the agent
🤖 Best Run Employee Onboarding AI Agent  ──  Claude Sonnet via SAP AI Core
        │
        ├──────────────────────────────────────────────────────┐
        ▼                                                      ▼
👤 Employee Profile + Job Data                    📄 Best Run HR Onboarding Policy
   SAP API Composition                           RAG via SAP AI Launchpad
   (unified API over SuccessFactors)                 Orchestration
        │                                                      │
        └──────────────────┬───────────────────────────────────┘
                           │
                           ▼
              📋 Onboarding Plan Prepared
                           │
                           ▼
              ⏸️  Manager Approval  ← human-in-the-loop
                           │
           ┌───────────────┴───────────────┐
           ▼                               ▼
  🛒 Purchase Requisition         🎫 Support Case
     SAP S/4HANA                     SAP Service Cloud V2
     (equipment)                     (system access + training)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  All tool calls flow through SAP Integration Suite MCP Gateway
  Principal Propagation ensures end-to-end identity traceability
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
---

### 🎯 What You Will Learn

| Concept | What You Experience |
|---|---|
| **Model Context Protocol (MCP)** | AI agent dynamically discovers and calls enterprise tools at runtime |
| **SAP Integration Suite MCP Gateway** | Governed bridge between the AI agent and SAP backends — policy, security, analytics in one place |
| **Event-Driven Architecture** | SAP Advanced Event Mesh triggers the agentic workflow in real time |
| **Principal Propagation** | Logged-in user identity flows through every tool call for end-to-end traceability |
| **API Composition — SAP Business Data Graph** | Complex SuccessFactors data unified into a single API call |
| **Retrieval-Augmented Generation (RAG)** | AI decisions grounded in company HR policy documents, not model assumptions |
| **Human-in-the-Loop** | Manager approval gate pauses the agent mid-workflow — no automation without consent |

## System URL and login information 
To complete the exercises, the access to the following systems have been provided. \
**In case you are not able to access any of these systems please contact the workshop instructors**.

- [SAP Integration Suite, advanced event mesh](https://url.sap/hu5r93)
- [SAP SuccessFactors](https://url.sap/1ubd08)
- [SAP S/4HANA Cloud](https://url.sap/3iahmk)
- [SAP Service Cloud Version 2](https://url.sap/5bivj9)
- **SAP Integration Suite Tenant | Two tenants — see assignment below**
    - Participants are split across two pre-configured SAP Integration Suite tenants. Use the tenant assigned to your participant number:
    Your participant number is printed on your seat card. You will be asked to select your tenant during setup

        | Participant Numbers | Workshop Tenant |
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

Provide the exercise content here directly in README.md using [markdown](https://guides.github.com/features/mastering-markdown/) and linking to the specific exercise pages, below is an example.

- [Getting Started](exercises/ex0/)
- [Exercise 1 - First Exercise Description](exercises/ex1/)
    - [Exercise 1.1 - Exercise 1 Sub Exercise 1 Description](exercises/ex1#exercise-11-sub-exercise-1-description)
    - [Exercise 1.2 - Exercise 1 Sub Exercise 2 Description](exercises/ex1#exercise-12-sub-exercise-2-description)
- [Exercise 2 - Second Exercise Description](exercises/ex2/)
    - [Exercise 2.1 - Exercise 2 Sub Exercise 1 Description](exercises/ex2#exercise-21-sub-exercise-1-description)
    - [Exercise 2.2 - Exercise 2 Sub Exercise 2 Description](exercises/ex2#exercise-22-sub-exercise-2-description)

  
**OR** Link to the Tutorial Navigator for example...

Start the exercises [here](https://developers.sap.com/tutorials/abap-environment-trial-onboarding.html).

**IMPORTANT**

Your repo must contain the .reuse and LICENSES folder and the License section below. DO NOT REMOVE the section or folders/files. Also, remove all unused template assets(images, folders, etc) from the exercises folder. 

## Contributing
Please read the [CONTRIBUTING.md](./CONTRIBUTING.md) to understand the contribution guidelines.

## Code of Conduct
Please read the [SAP Open Source Code of Conduct](https://github.com/SAP-samples/.github/blob/main/CODE_OF_CONDUCT.md).

## How to obtain support

Support for the content in this repository is available during the actual time of the online session for which this content has been designed. Otherwise, you may request support via the [Issues](../../issues) tab.

## License
Copyright (20xx-)20xx SAP SE or an SAP affiliate company and <your-project> contributors. Please see our [LICENSE](LICENSE) for copyright and license information. Detailed information including third-party components and their licensing/copyright information is available [via the REUSE tool](https://api.reuse.software/info/github.com/SAP/<your-project>).
