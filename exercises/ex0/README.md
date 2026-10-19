# Getting Started — What Has Been Pre-Configured for You

This session is designed so that you focus on what matters — creating MCP Servers, configuring governed access, and running an AI agent. All backend systems have been pre-configured and are ready to use.

This page gives you a clear picture of what is already set up per system, so you know what you are working with before you begin.

> [!NOTE]
> System URLs and login credentials are in the [main README](../../README.md#system-url-and-login-details). Your participant number is printed on your seat card.

---

## SAP SuccessFactors

SAP SuccessFactors is the system of record for new hire data. It publishes a **NEWHIRE** event to SAP Advanced Event Mesh when a new hire completes their profile.

**Pre-configured for this session:**

| What | Details |
|---|---|
| Tenant | BestRun company — `apisalesdemo8.successfactors.eu` |
| Test Employees | One test employee per participant — hired and ready in the system |
| New Hire Event | Configured to publish `NEWHIRE` events to AEM topic `sap/sfsf/newhire/TechEd_IN261_0**` per participant |
| OAuth Client | Configured for BTP Destination to call SuccessFactors APIs via SAML Bearer Assertion |
| BTP Destination | `in261_hcmcloud_mcp` — points to the Business Data Graph endpoint |

**What you do:** Nothing in SuccessFactors — your instructor triggers the new hire event in Exercise 8.

---

## SAP Integration Suite — API Composition (Business Data Graph)

The Business Data Graph (BDG) unifies multiple SuccessFactors APIs into two clean custom entities — eliminating the need for the agent to call multiple APIs separately.

**Pre-configured for this session:**

| What | Details |
|---|---|
| Composed API | Two custom entities: `bestrun.hcmcloud.employee` and `bestrun.hcmcloud.employeeJob` |
| Employee Details entity | Consolidates personal profile — name, DOB, hire date, email, employment status |
| Employee Job Details entity | Consolidates job data — title, manager, company, dept, BU, division, location |
| OpenAPI Specification | `SAP_HCM_Graph.json` — available in [release assets](https://github.com/SAP-samples/teched2026-IN261/releases/download/exercise-assets/SAP_HCM_Graph.json) |

**What you do:** Use this pre-configured Composed API as the backend for your MCP Server in **Exercise 2**.

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

## SAP S/4HANA

SAP S/4HANA is the ERP backend where Purchase Requisitions are created for new hire equipment after manager approval.

**Pre-configured for this session:**

| What | Details |
|---|---|
| Tenant | SAP S/4HANA Cloud — pre-configured for PR creation |
| API | Purchase Requisition creation API configured and accessible |
| BTP Destination | Pre-configured destination for iFlow → S/4HANA connectivity |
| Cost Center | Pre-configured per department for PR creation |

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
| Employee Records | Test employees available for case creation |
| MCP Server | Pre-deployed — `IN261_Create_SC_ServiceCloud_MCP` (see Exercise 5) |

**What you do:** Nothing in Service Cloud — the pre-deployed MCP Server handles case creation in **Exercise 7**.

---

## SAP Integration Suite — Integration Cell & Destinations

All MCP Servers you create in this session are deployed to the **Integration Cell** — SAP Integration Suite's runtime for edge and hybrid deployments.

**Pre-configured for this session:**

| What | Details |
|---|---|
| Integration Cell | Deployed and running — ready to host your MCP Server artifacts |
| Virtual Host | Pre-configured — selected by default during deployment |
| IAS Tenant | `attkvky2u.accounts.ondemand.com` — used for OAuth authentication across all MCP Servers |
| Destinations | `in261_hcmcloud_mcp`, `in261_iflow_as_mcp`, `in261_service_cloud_mcp` — all pre-configured |

---

## SAP Advanced Event Mesh (AEM)

AEM is the event backbone that connects SAP SuccessFactors to your AI agent — delivering new hire events in real time.

**Pre-configured for this session:**

| What | Details |
|---|---|
| Cluster | `TechEd2026-IN261` — shared across all participants |
| Topics | One dedicated topic per participant: `sap/sfsf/newhire/TechEd_IN261_0**` |
| Credentials | Pre-provided in the AI agent `.env` file (Exercise 7) |

**What you do:** Create your personal queue and subscribe to your topic in **Exercise 1**.

---

## Summary

| System | Pre-Configured | Your Action |
|---|---|---|
| SAP SuccessFactors | ✅ Tenant, employees, event publishing | None — instructor triggers event |
| SAP Business Data Graph | ✅ Two custom entities, OpenAPI spec | Use as backend in Exercise 2 |
| SAP Integration Suite iFlows | ✅ HR Policy + PR Creation iFlows deployed | Use as backend in Exercises 3 & 4 |
| SAP AI Launchpad | ✅ Document Grounding, HR policies indexed | None — iFlow handles connection |
| SAP S/4HANA | ✅ PR API, destination, cost centers | None — iFlow creates PRs |
| SAP Service Cloud V2 | ✅ Case type, technical user, MCP Server | None — pre-deployed MCP handles it |
| SAP Integration Suite Cell | ✅ Runtime, IAS, destinations | Deploy your MCP Servers |
| SAP Advanced Event Mesh | ✅ Cluster, topics | Create your queue in Exercise 1 |

Continue to - [Exercise 1 - Set Up Your SAP Integration Suite, advanced event mesh (AEM) Queue and Topic Subscription](../ex1/README.md)
