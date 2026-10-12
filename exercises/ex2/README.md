# Exercise 2 — Create and Deploy a MCP Server for Employee Lookup

## Overview

An AI agent is only as useful as the data it can reach. Getting it to enterprise data safely is the challenge.

In this exercise, you will wrap the pre-delivered Business Data Graph (BDG) for Employee Lookup as an **MCP Server** — the standardized bridge between enterprise APIs and AI agents. You will create an MCP artifact, configure it with the BDG's OpenAPI specification, deploy it to the Integration Cell, and verify it is running.

By the end, your employee data is accessible to any MCP-compatible agent — secured, governed, and ready for the onboarding agent in Exercise 3.

> [!NOTE]
> For a deeper understanding of the **Model Context Protocol (MCP)** and **MCP paradigm in SAP Integration Suite**, refer to [What is the Model Context Protocol (MCP)?](https://modelcontextprotocol.io/docs/2026-07-28/getting-started/intro) and [API-Centric Integration in SAP Integration Suite: A New Paradigm for API & MCP](https://community.sap.com/t5/technology-blog-posts-by-sap/api-centric-integration-in-sap-integration-suite-a-new-paradigm-for-api-amp/ba-p/14438245)


---

## Pre-Delivered: Composed API via API Composion aka Business Data Graph

API Composition is a solution to unify your business APIs in the form of a semantically connected data graph, accessed via a single powerful API. Out of the box, it provides developers a single connected and unified view of your SAP-managed business data. API Composition consolidates thousands of data entities from SAP systems like SAP S/4HANA, SAP Sales Cloud, and SAP SuccessFactors, into one curated, semantically connected, data model. We call this connected graph a Business Data Graph.

> [!NOTE]
> For a deeper understanding of the capability, refer to [SAP Help: API Composition](https://help.sap.com/docs/api-composition/isuite-api-composition/what-is-api-composition?locale=en-US)

For the hands on we have created a **Composed API** for Employee Lookup. A BDG is a curated, OData-based API layer that exposes structured enterprise data from backend systems through a single, governed endpoint.

Instead of building the composed API from scratch, you use this pre-configured BDG as the data source for your MCP Server. The BDG comes with an **OpenAPI Specification** and a **ODATA URL** that describes the available operations — this is what you will upload when creating the MCP artifact.

---

## Concept: Principal Propagation — Why It Matters Here

When your AI agent calls the MCP Server to look up an employee, the backend system needs to know *who* is making that request. Using a shared service account hides the real caller — this breaks audit trails, access controls, and compliance requirements.

**Principal Propagation** solves this by forwarding the authenticated user's identity from the MCP Server all the way through to the backend SAP system.

Here is how it works in this exercise:

| Step | What Happens |
|---|---|
| 1 | The AI agent authenticates to the MCP Server using **OAuth 2.0** |
| 2 | SAP Integration Suite verifies the caller via the configured **IAS (Identity Authentication Service)** trust |
| 3 | The policy model uses a **SAML Bearer Assertion** to convert the OAuth token into a SAML assertion carrying the user's identity |
| 4 | This assertion is forwarded to the backend — the backend sees the *real* user, not a generic service account |

This is why the Runtime Profile is set to **Integration Cell** — it is the runtime that supports this full OAuth + SAML propagation chain.

---

## Exercise 2.1 — Create the MCP Server

In this sub-exercise, you create an Integration Package and add an MCP Server artifact configured against the pre-delivered Employee Lookup BDG.

### Step 1: Create an Integration Package

1. In SAP Integration Suite, navigate to **Design → Integrations and APIs**.
2. Click **Create**.
3. Fill in the package details:

   | Field | Value |
   |---|---|
   | Name | `employee-onboarding-package-XX` |
   | Short Description | MCP Server for Employee Lookup using pre-delivered BDG |
   | Version | `1.0.0` |
   | Vendor | `SAP` |

   > Replace **XX** with your assigned participant number throughout this exercise.

4. Click **Save**.

### Step 2: Add an MCP Server Artifact

1. Open the package you just created and go to the **Artifacts** tab.
2. Click **Add → MCP Server**.
3. When prompted for the source type, select **HTTP Endpoint with OpenAPI Specification**.

   <br>![](/exercises/ex2/images/02_01_source_type.png)

4. Fill in the MCP Server details:

   | Field | Value |
   |---|---|
   | Method | Upload |
   | File | Upload the OpenAPI Specification provided for this session |
   | Source | URL |
   | URL | Employee Lookup BDG OData URL *(provided in the session materials)* |
   | Name / ID | `employeelookup-XX` |
   | MCP Path | `/mcp-employeelookup-XX` |
   | Version | `1.0.0` |
   | Runtime Profile | `Integration Cell` |

5. Click **Next**.

### Step 3: Select Tools

On the tool selection screen, select the Employee Lookup operations to expose as MCP tools:

- `GET /EmployeeProfile` — retrieve the list of employee profiles
- `GET /EmployeeProfile/{EmployeeID}` — retrieve a single employee by ID

   <br>![](/exercises/ex2/images/02_01_tool_selection.png)

6. Click **Next** and then **Finish**.

### Step 4: Review the Policy Model

After the artifact is created, open it and review the default policy model. No changes are needed — it is pre-configured correctly for this exercise.

The policy model contains four components that process every agent request:

| Component | Purpose |
|---|---|
| **MCP Sender Adapter** | Entry point — receives requests from the AI agent |
| **Authentication** | Verifies the caller's identity (OAuth 2.0) |
| **Authorization** | Checks permissions using OAuth Scope `API.invoke` |
| **HTTP Receiver Adapter** | Forwards the request to the Employee Lookup BDG backend, carrying the propagated user identity |

This chain is what enables Principal Propagation — the identity verified in Authentication is carried to the backend via the HTTP Receiver Adapter using the SAML Bearer Assertion.

<br>![](/exercises/ex2/images/02_01_policy_model.png)

---

## Exercise 2.2 — Deploy the MCP Server

In this sub-exercise, you deploy the MCP Server artifact to the Integration Cell and verify it is running.

### Step 1: Deploy

1. From the artifact overview, click **Deploy**.
2. Confirm the **Runtime Profile** (`Integration Cell`) and **Virtual Host**.
3. Click **Yes** to confirm.

### Step 2: Verify Status in Monitoring

1. Navigate to **Monitor → Integrations and APIs**.
2. Set **Runtime** to `Integration Cell`.
3. Click the **Manage Integration Content** tile.
4. Locate `employeelookup-XX` in the list.
5. Confirm the Runtime Status shows **Started**.

   <br>![](/exercises/ex2/images/02_02_started_status.png)


### Step 3: Note the MCP URL

Once the status is **Started**, copy the MCP endpoint URL shown in the details panel:


> [!IMPORTANT]
> **Save this URL — you will need it in Exercise 3** when you connect this MCP Server to the Employee Onboarding Agent.

---

## Summary

You have now created and deployed an MCP Server that wraps the pre-delivered Employee Lookup BDG and exposes it securely to AI agents.

- The MCP Server handles authentication and authorization using OAuth 2.0
- Principal Propagation ensures the backend sees the real caller identity, not a shared service account
- The MCP endpoint is live on the Integration Cell and ready to receive agent calls

Continue to - [Exercise 3](../ex3/README.md)
