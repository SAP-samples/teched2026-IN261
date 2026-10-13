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

When a new employee is hired in SAP SuccessFactors, their data is spread across multiple independent APIs — personal profile, job assignment, location, business unit, department, division, manager, and more. Each API answers one question, but no single API tells the full story of a new hire.

If an AI agent were to call each of these APIs as separate MCP tools, it would quickly run into **context rot** — a problem where the agent's context window fills up with redundant, overlapping, or inconsistently structured data from too many tool calls. The agent loses coherence, misses relationships between fields, and produces unreliable results.

**API Composition** solves this by combining multiple SuccessFactors APIs into a single, semantically connected endpoint. For this hands-on session, this Composed API has been pre-configured for you — exposing two clean custom entities, **Employee Details** and **Job Details**, that give the onboarding agent exactly what it needs in one call, with no noise.

<br>![BDG Overview](/exercises/ex2/images/BDG_Overview.png)

The Composed API is built from multiple data entities — each representing a different SuccessFactors API domain:

<br>![BDG Data Sources](/exercises/ex2/images/BDG_DataSource.png)

From these data sources, two custom entities have been created to structure the information for the onboarding use case:

- **Employee Details entity** — consolidates personal profile information of the new hire such as name, contact details, and employment status
- **Job Details entity** — consolidates job-related information including role, location, business unit, and department

<br>![BDG Custom Entities](/exercises/ex2/images/BDG_CustomEntities.png)

Instead of building this from scratch, you use this pre-configured Composed API as the data source for your MCP Server. It comes with an **OpenAPI Specification** and an **OData URL** — both of which you will use when creating the MCP artifact in the next steps.

> [!NOTE]
> The **OpenAPI Specification** and **OData URL** for this session will be shared by the instructors.
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

In this sub-exercise, you create an Integration Package and add an MCP Server artifact configured against the pre-delivered Employee Lookup Composed API.

### Step 1: Create an Integration Package

1. In SAP Integration Suite, navigate to **Design → Integrations and APIs**.

   <br>![Navigate to Integrations and APIs](/exercises/ex2/images/Create_IntegrationPackage1.png)

2. Click **Create**.
3. Fill in the package details:

   | Field | Value |
   |---|---|
   | Name | `employee-onboarding-package-XX` |
   | Short Description | MCP Server for Employee Lookup using pre-delivered Composed API |
   | Version | `1.0.0` |
   | Vendor | `SAP` |

   > Replace **XX** with your assigned participant number throughout this exercise.

   <br>![Fill in Package Details](/exercises/ex2/images/Create_IntegrationPackage2.png)

4. Click **Save**.

### Step 2: Add an MCP Server Artifact

1. Open the package you just created and go to the **Artifacts** tab.
2. Click **Add** and select **MCP Server** from the dropdown.

   <br>![Add MCP Server](/exercises/ex2/images/Add_MCP_Server.png)

3. In the **Add MCP Server** dialog, select **HTTP Endpoint with OpenAPI Specification** as the source type and click **Next**.

   <br>![Select Source Type](/exercises/ex2/images/Add_MCP_Server2.png)

4. Fill in the MCP Server details:

   | Field | Value |
   |---|---|
   | Method | `Upload` |
   | File Name | Upload `SAP_HCM_Graph.json` *(provided by the instructors)* |
   | Source | `Destination` |
   | Destination | `in261_hcmcloud_mcp` |
   | Relative URL | `/api/in261-hcmcloud` |
   | Name / ID | `IN261_0XX_Employee_Lookup_MCP` |
   | MCP Path | `/in261_0XX_employee_lookup_mcp` |
   | Version | `1.0.0` |
   | Runtime Profile | `Integration Cell` |

   > Replace **XX** with your assigned participant number.

   <br>![Provide MCP Details](/exercises/ex2/images/Add_MCP_Server3.png)

5. Click **Next**.

### Step 3: Select Tools

On the **Create Tools** screen, two tools are available from the uploaded API specification. Select both:

| Method | Path | Description |
|---|---|---|
| `GET` | `/bestrun.hcmcloud/employee` | Returns employee personal info and email. Supports `$filter=employeeId eq '{id}'` and `$expand=personalInfo,emailInfo` |
| `GET` | `/bestrun.hcmcloud/employeeJob` | Returns job details including manager, company, department, business unit, division, and location |

   <br>![Create Tools](/exercises/ex2/images/Add_MCP_Server4.png)

6. Click **Add** to finish creating the MCP Server artifact.

### Step 4: Add Prompt

A **Prompt** in an MCP Server is a reusable, parameterized instruction that tells the AI agent exactly what to do and how to call the tools. Instead of the agent deciding how to query SuccessFactors, the prompt defines the exact sequence — which tools to call, with which parameters, and how to combine the results.

1. From the artifact overview, click **Edit** (top right).

   <br>![Edit and navigate to Prompts](/exercises/ex2/images/Add_Prompt.png)

2. Go to the **MCP Configuration** tab → **Prompts** sub-tab → click **Add**.

3. In the **Add Prompt** dialog, fill in the **Define Prompt** details:

   | Field | Value |
   |---|---|
   | Name | `get_new_hire_context` |
   | Title | *(see below)* |
   | Description | *(see below)* |

   Copy and paste into the **Title** field:

   ```
   Fetch complete onboarding context for a new hire from SuccessFactors
   ```

   Copy and paste into the **Description** field:

   ```
   Fetches complete onboarding context for a new hire from SAP SuccessFactors by calling employee personal info and job info tools and combining them into a single enriched payload.
   ```

   <br>![Define Prompt](/exercises/ex2/images/Add_Prompt2.png)

4. Click **Next**. On the **Define Arguments** screen, fill in the following:

   | Field | Value |
   |---|---|
   | Name | `employeeId` |
   | Title | `New Hire's Employee ID` |
   | Description | `New Hire's Employee ID` |
   | Required | ✓ |

   <br>![Define Arguments](/exercises/ex2/images/Add_Prompt3.png)

5. Click **Next**. On the **Define Messages** screen, set the **Role** to `User` and paste the following into the **Content** field:

   ```
   Fetch the complete onboarding context for new hire {{ .employeeId }}'.

   1. Call getEmployee with $filter=employeeId eq '{{ .employeeId }}'&$expand=personalInfo,emailInfo

   2. Call getEmployeeJob with $filter=employeeId eq '{{ .employeeId }}'&$expand=positionInfo,companyInfo,businessUnitInfo,divisionInfo,departmentInfo,locationInfo

   Combine both responses into a single new hire context object with fields:
   {
     "employeeId": "...",
     "employeeFirstName": "...",
     "employeeLastName": "...",
     "employeeName": "...",
     "hireDate": "...",
     "gender": "...",
     "dateOfBirth": "...",
     "email": "...",
     "managerId": "...",
     "managerName": "...",
     "jobTitle": "...",
     "company": "...",
     "companyText": "...",
     "departmentCode": "...",
     "departmentText": "...",
     "businessUnitCode": "...",
     "businessUnitText": "...",
     "divisionCode": "...",
     "divisionText": "...",
     "locationCode": "...",
     "locationText": "...",
     "country": "..."
   }
   ```

   <br>![Define Messages](/exercises/ex2/images/Add_Prompt4.png)

6. Click **Add** to save the prompt. Then click **Save** (top right) to save the artifact.

---

### Step 5: Review the Policy Model

After the artifact is created, open it and review the default policy model. No changes are needed — it is pre-configured correctly for this exercise.

The policy model contains four components that process every agent request:

| Component | Purpose |
|---|---|
| **MCP Sender Adapter** | Entry point — receives requests from the AI agent |
| **Authentication** | Verifies the caller's identity (OAuth 2.0) |
| **Authorization** | Checks permissions using OAuth Scope `API.invoke` |
| **HTTP Receiver Adapter** | Forwards the request to the Employee Lookup Composed API backend, carrying the propagated user identity |

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

You have now created and deployed an MCP Server that wraps the pre-delivered Employee Lookup Composed API and exposes it securely to AI agents.

- The Composed API surfaces two custom entities — **Employee Details** and **Job Details** — giving the agent structured access to both personal and job-related onboarding data
- The MCP Server handles authentication and authorization using OAuth 2.0
- Principal Propagation ensures the backend sees the real caller identity, not a shared service account
- The MCP endpoint is live on the Integration Cell and ready to receive agent calls

Continue to - [Exercise 3](../ex3/README.md)
