# Exercise 2 — Create and Deploy a MCP Server for Employee Lookup

## Overview

An AI agent is only as useful as the data it can reach. Getting it to enterprise data safely is the challenge.

In this exercise, you will wrap the pre-delivered Business Data Graph (BDG) for Employee Lookup as an **MCP Server** — the standardized bridge between enterprise APIs and AI agents. You will create an MCP artifact, configure it with the BDG's OpenAPI specification, deploy it to the Integration Cell, and verify it is running.

By the end, employee data in the composed API will be deployed as a MCP server and ready for the consumption in Exercise 3.

> [!NOTE]
> For a deeper understanding of the **Model Context Protocol (MCP)** and **MCP paradigm in SAP Integration Suite**, refer to [What is the Model Context Protocol (MCP)?](https://modelcontextprotocol.io/docs/2026-07-28/getting-started/intro) and [API-Centric Integration in SAP Integration Suite: A New Paradigm for API & MCP](https://community.sap.com/t5/technology-blog-posts-by-sap/api-centric-integration-in-sap-integration-suite-a-new-paradigm-for-api-amp/ba-p/14438245)

---

## Pre-Delivered: Composed API via API Composition aka Business Data Graph

**API Composition** is a capability in SAP Integration Suite that unifies multiple business APIs into a single, semantically connected data graph — accessible through one powerful API endpoint.

For this hands-on session, a **Composed API** for Employee Lookup has been pre-configured for you. It consolidates data entities from nine different SuccessFactors APIs, covering a new hire's personal profile, job details, location, business unit, department, and more — all accessible through a single endpoint.

<br>![BDG Overview](/exercises/ex2/images/BDG_Overview.png)

The Composed API is built from multiple data sources — each representing a different SuccessFactors API domain:

<br>![BDG Data Sources](/exercises/ex2/images/BDG_DataSource.png)

These sources are exposed as unified, queryable entities through the Composed API:

<br>![BDG Custom Entities](/exercises/ex2/images/BDG_CustomEntities.png)

Instead of building this from scratch, you use this pre-configured Composed API as the data source for your MCP Server. It comes with an **OpenAPI Specification** and an **OData URL** — both of which you will use when creating the MCP artifact in the next steps.

> [!NOTE]
> For a deeper understanding of the capability, refer to [SAP Help: API Composition](https://help.sap.com/docs/api-composition/isuite-api-composition/what-is-api-composition?locale=en-US)

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

> [!NOTE]
> For a deeper understanding, refer to [SAP Help: Principal Propagation](https://help.sap.com/docs/connectivity/sap-btp-connectivity-cf/principal-propagation)

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

   > [!IMPORTANT]
   > Only proceed to Exercise 3 once the status shows **Started**. A *Starting* status means deployment is still in progress — wait and refresh.

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
