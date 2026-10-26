# Getting Started — What Has Been Pre-Configured for You

> [!IMPORTANT]
> **No Action Required — Skip to Exercise 1**
> <br>You do not need to read this page to complete the session. All backend systems are already set up and ready. This page is for reference only — read it offline, after the session, or if you're curious about what was pre-configured and why.

System URLs are in the [main README](../../README.md#system-url-and-login-details). Your participant number is printed on your seat card.

---

Continue to - [Exercise 1 - Set Up Your SAP Integration Suite, advanced event mesh (AEM) Queue and Topic Subscription](../ex1/README.md)

---

## SAP Advanced Event Mesh (AEM)

AEM is the event backbone that connects SAP SuccessFactors to your AI agent — delivering new hire events in real time.

**Pre-configured for this session:**

| What | Details |
|---|---|
| Cluster | `TechEd2026-IN261` — shared across all participants |
| Topics | One dedicated topic per participant: `sap/sfsf/newhire/IN261_0**` |
| Credentials | Pre-provided in the AI agent `.env` file (Exercise 7) |

**What you do:** Create your personal queue and subscribe to your topic in **Exercise 1**.

---

## SAP SuccessFactors

SAP SuccessFactors is the system of record for new hire data. When a new hire completes their profile, SuccessFactors publishes a **NEWHIRE** event to SAP Advanced Event Mesh — triggering the AI agent automatically.

**Pre-configured for this session:**

| What | Details |
|---|---|
| Tenant | BestRun company — `apisalesdemo8.successfactors.eu` |
| New Hire Event | Configured to publish `NEWHIRE` events to AEM — dynamic topic per participant |
| OAuth Client | Configured for BTP Destination to call SuccessFactors APIs via SAML Bearer Assertion |
| BTP Destination | `in261_hcmcloud_mcp` — points to the Business Data Graph endpoint |

---

## SAP Integration Suite — API Composition (Business Data Graph)

The Business Data Graph (BDG) unifies multiple SuccessFactors APIs into two clean custom entities — eliminating the need for the agent to call multiple APIs separately.

**Pre-configured for this session:**

| What | Details |
|---|---|
| Composed API | Two custom entities: `bestrun.hcmcloud.employee` and `bestrun.hcmcloud.employeeJob` |
| Employee Details entity | Consolidates personal profile — name, DOB, hire date, email, employment status |
| Employee Job Details entity | Consolidates job data — title, manager, company, dept, BU, division, location |

**What you do:** Use this pre-configured Composed API as the backend for your MCP Server in **Exercise 2**.

---

## SAP AI Launchpad — Orchestration with Document Grounding

SAP AI Launchpad Orchestration with Document Grounding indexes HR policy documents into a vector store and exposes a semantic search API — enabling the agent to retrieve the most relevant HR policies for a new hire's role and location.

**Pre-configured for this session:**

| What | Details |
|---|---|
| Document Collection | BestRun HR policy documents indexed — covering equipment, system access, and training policies |
| Orchestration Pipeline | Configured to accept a natural language policy query and return relevant policy content |
| Access | Via pre-deployed iFlow — no direct access needed |

**What you do:** Nothing in SAP AI Launchpad — the iFlow handles the connection in **Exercise 3**.

---

## SAP Integration Suite — Integration Flows (iFlows)

Two Integration Flows have been pre-configured and deployed — acting as governed middleware between MCP Servers and backend systems.

**Pre-configured for this session:**

| iFlow | Purpose | Destination |
|---|---|---|
| `Prompt to HR Policy RAG Pipeline using SAP AI Core` | Receives policy query, maps to SAP AI Launchpad Orchestration Document Grounding API, returns relevant HR policies | `in261_iflow_as_mcp` / `/http` |
| `Create PR in S/4HANA` | Receives approved equipment payload, maps data, creates Purchase Requisition in SAP S/4HANA | `in261_iflow_as_mcp` / `/http` |

**What you do:** Use these pre-configured iFlows as the backends for your MCP Servers in **Exercise 3** and **Exercise 4**.

---

## SAP S/4HANA

SAP S/4HANA is the ERP backend where Purchase Requisitions are created for new hire equipment after manager approval.

**Pre-configured for this session:**

| What | Details |
|---|---|
| Tenant | SAP S/4HANA Cloud — pre-configured for PR creation |
| API | Purchase Requisition creation API configured and accessible |
| BTP Destination | Pre-configured destination for iFlow → S/4HANA connectivity |

**What you do:** Nothing in S/4HANA — the iFlow creates the PR on behalf of the agent in **Exercise 4** and **Exercise 7**.

---

## SAP Service Cloud V2

SAP Service Cloud V2 is where Support Cases are created for new hire IT system access and training enrollment after manager approval.

**Pre-configured for this session:**

| What | Details |
|---|---|
| Tenant | Pre-configured BestRun Service Cloud tenant |
| Case Type | `ZEST` — configured for new hire onboarding support cases |
| Technical User | Pre-configured for MCP Server → Service Cloud connectivity via Client Credentials |

**What you do:** Nothing in Service Cloud — the pre-deployed MCP Server handles case creation in **Exercise 7**.

---

## Summary

| System | Pre-Configured | 
|---|---|
| SAP Advanced Event Mesh | ✅ Cluster, topics | 
| SAP SuccessFactors | ✅ Tenant, event publishing | 
| SAP Business Data Graph | ✅ Two custom entities, OpenAPI spec |
| SAP AI Launchpad | ✅ Document Grounding, HR policies indexed | 
| SAP Integration Suite iFlows | ✅ HR Policy + PR Creation iFlows deployed | 
| SAP S/4HANA | ✅ PR API, destination | 
| SAP Service Cloud V2 | ✅ Case type, technical user | 

Continue to - [Exercise 1 - Set Up Your SAP Integration Suite, advanced event mesh (AEM) Queue and Topic Subscription](../ex1/README.md)
