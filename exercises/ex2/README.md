# Exercise 2 — Create, Configure and Deploy an MCP Server for New Hire Employee Profile Lookup using SAP API Composition
In this exercise, you will create an Integration Package and an MCP Server artifact backed by the pre-configured Employee Lookup Composed API, configure it with a prompt and authentication policy, and deploy it to the Integration Cell.

⏱ **Estimated time: 15 minutes**

## Overview
SAP Integration Suite's **MCP Gateway** lets you expose any enterprise API as an MCP tool — with authentication, authorization, and audit logging enforced at the gateway level, keeping your backend systems protected.

> **Note:** For a deeper understanding of the **Model Context Protocol (MCP)** and **MCP Gateway and API Composition in SAP Integration Suite**, refer to [What is the Model Context Protocol (MCP)](https://modelcontextprotocol.io/docs/2026-07-28/getting-started/intro) and [MCP Gateway in SAP Integration Suite](https://community.sap.com/t5/technology-blog-posts-by-sap/mcp-gateway-in-sap-integration-suite-your-apis-ready-for-the-age-of-agents/ba-p/14438250)

A new hire's data in SAP SuccessFactors is spread across multiple independent APIs — personal profile, job assignment, manager, department, business unit, division, and location. If an AI agent called each of these as a separate MCP tool, it would make too many round trips, receive overlapping and inconsistently structured data, and lose coherence trying to piece it all together.

**API Composition** solves this by unifying those APIs into a single, semantically connected endpoint — exposing exactly two clean entities: **Employee Details** and **Job Details**. The agent gets the full new hire picture in two focused calls instead of many fragmented ones.

In this exercise, you will create an **MCP Server** backed by this pre-configured Composed API. You will upload the OpenAPI specification, configure the authentication policy with your IAS tenant, add a prompt that tells the agent exactly how to call the tools, and deploy the MCP Server to the Integration Cell. 

By the end, you will have a deployed MCP Server that exposes the complete new hire profile with governed access and Principal Propagation — ensuring every agent call to SAP SuccessFactors is traceable to the real user identity, not a shared service account — ready to be consumed by the AI agent in Exercise 7.

<details>
<summary><strong>📖 Pre-Configured: Composed API via SAP API Composition (Business Data Graph)</strong></summary>

For this hands-on session, the Composed API has been pre-configured for you, giving the onboarding agent exactly what it needs in two focused calls, with no noise.
You will use this pre-configured Composed API as the data source for your MCP Server — it comes with an **OpenAPI Specification** that you will upload in the next steps.

The Composed API is built from multiple SuccessFactors data entities — each representing a different domain:

![BDG Data Sources](/exercises/ex2/images/BDG_DataSource.png)

From these, two custom entities have been created to give the onboarding agent exactly what it needs:

- **Employee Details entity** — consolidates personal profile information such as name, contact details, and employment status
- **Employee Job Details entity** — consolidates job-related information including role, location, business unit, department, and manager

![BDG Custom Entities](/exercises/ex2/images/BDG_CustomEntities.png)

The entities and their underlying source mappings are:

| Entity | Source Entities |
|---|---|
| `employee` | `sap.hcm/PerPerson`, `sap.hcm/PerPersonal`, `sap.hcm/PerEmail` |
| `employeeJob` | `sap.hcm/EmpJob`, `sap.hcm/User`, `sap.hcm/Position`, `sap.hcm/FOCompany`, `sap.hcm/FOLocation`, `sap.hcm/FODivision`, `sap.hcm/FOBusinessUnit`, `sap.hcm/FODepartment`|

If an AI agent were to call each of these APIs as separate MCP tools, it would quickly run into **context rot** — a problem where the agent's context window fills up with redundant, overlapping, or inconsistently structured data from too many tool calls. The agent loses coherence, misses relationships between fields, and produces unreliable results. The Composed API solves this by surfacing exactly these two focused entities — nothing more, nothing less.

![BDG Overview](/exercises/ex2/images/BDG_Overview.png)

> **Note:** For a deeper understanding of the capability, refer to [SAP Help: API Composition in SAP Integration Suite](https://help.sap.com/docs/api-composition/isuite-api-composition/what-is-api-composition?locale=en-US)
</details>

<details>
<summary><strong>📖 Background: Principal Propagation — Why It Matters Here</strong></summary>

When your AI agent calls the MCP Server to look up an employee, the backend needs to know *who* is making that request — not just that a valid service account is calling. Using a shared technical user hides the real caller, breaking audit trails, access controls, and compliance requirements — especially critical when accessing sensitive new hire personal and employment details.

**Principal Propagation** solves this by forwarding the authenticated user's identity from the AI Agent to the MCP Server all the way through to SAP SuccessFactors.

> AI Agent → **(OAuth 2.0 / OIDC)** → MCP Server → **(JWT Bearer)** → SAP BDG → **(SAML Bearer Assertion)** → SAP SuccessFactors

Here is how it flows in this exercise:

| Step | What Happens |
|---|---|
| 1 | The AI agent obtains an **OAuth 2.0 token** from **IAS** on behalf of the logged-in user|
| 2 | The agent calls the MCP Server presenting the token |
| 3 | The MCP Server validates the token against the configured **IAS Well-Known URL** |
| 4 | The MCP Server forwards the request to **SAP Business Data Graph** using a **JWT Bearer** BTP destination, carrying the real user's identity |
| 5 | SAP BDG exchanges the JWT Bearer for a **SAML Bearer Assertion** and calls SAP SuccessFactors — the backend sees the *real* user, not a shared service account |

> **Note:** Principal Propagation is specific to this MCP Server — the employee profile lookup talks directly to SAP SuccessFactors on behalf of the calling user.
> <br>For a deeper understanding, refer to [SAP Help: Principal Propagation](https://help.sap.com/docs/connectivity/sap-btp-connectivity-cf/principal-propagation)
</details>

## Exercise 2.1 — Create an Integration Package

1. Log in to your assigned SAP Integration Suite tenant — refer to the [System URL and Login Details](../../README.md#system-url-and-login-details) for your SAP Integration Suite tenant URL.
   For login, use the participant number that is assigned to you.

   ![Login to Integration Suite](/exercises/ex2/images/Login_IntegrationSuite.png)
2. After login, navigate to **Design → Integrations and APIs** and click **Create**.

   ![Navigate to Integrations and APIs](/exercises/ex2/images/Create_IntegrationPackage1.png)

4. Fill in the package details — copy and paste the values below:

   | Field | Value |
   |---|---|
   | Name | <b>IN261_0**_TechEd_2026_Package</b> *(replace ** with the participant number that is assigned to you)*|
   | Technical Name | Auto populated |
   | Short Description | `Learn how the MCP Gateway in SAP Integration Suite applies policy, security, and analytics while curating MCP tools. Securely connect enterprise tools to an AI agent, including unified APIs with event-driven context and governed access.` |
   | Version | `1.0.0` |
   | Vendor | `SAP` |

   Click **Save**.
   
   ![Fill in Package Details](/exercises/ex2/images/Create_IntegrationPackage2.png)

## Exercise 2.2 — Create the MCP Server Artifact

1. Go to the **Artifacts** tab, click **Add** and select **MCP Server** from the dropdown.

   ![Add MCP Server](/exercises/ex2/images/Add_MCP_Server.png)

2. In the **Add MCP Server** dialog, select **HTTP Endpoint with OpenAPI Specification** as the source type and click **Next**.

   ![Select Source Type](/exercises/ex2/images/Add_MCP_Server2.png)

3. Download the OpenAPI Specification: [`SAP_HCM_Graph.json`](https://github.com/SAP-samples/teched2026-IN261/releases/download/exercise-assets/SAP_HCM_Graph.json) — you will upload it in the next step.

4. Fill in the MCP Server details:

   | Field | Value |
   |---|---|
   | Method | `Upload` |
   | File Name | Upload the `SAP_HCM_Graph.json` downloaded in the previous step |
   | Source | `Destination` |
   | Destination | Select `in261_hcmcloud_mcp` from the destination list |
   | Relative URL | `/api/in261-hcmcloud` <br>**Note:** The Destination and Relative URL are pre-configured for the workshop — copy exactly as shown, do not modify.|
   | Name | <b>IN261_0**_Employee_Lookup_MCP</b> *(replace ** with the participant number that is assigned to you)* |
   | ID | Auto populated |
   | MCP Path | <b>/in261_0**_employee_lookup_mcp</b> *(replace ** with the participant number that is assigned to you)* |
   | Version | `1.0.0` |

   Click **Next**.
   
   ![Provide MCP Details](/exercises/ex2/images/Add_MCP_Server3.png)


5. On the **Create Tools** screen, two tools are available from the uploaded API specification.
   <br>Select both and click **Add** to finish creating the MCP Server artifact.

   ![Create Tools](/exercises/ex2/images/Add_MCP_Server4.png)

## Exercise 2.3 — Configure the MCP Server

1. Navigate to **MCP Configuration** tab, click **Edit** (top right).

   ![Navigate to MCP Configurations and Edit](/exercises/ex2/images/Edit_MCP.png)

2. Then go to the **Prompts** sub-tab and click **Add**.
   >**HINT:** A **Prompt** in an MCP Server is a reusable, parameterized instruction that tells the AI agent exactly what to do and how to call the tools. Instead of the agent deciding how to query SuccessFactors, the prompt defines the exact sequence — which tools to call, with which parameters, and how to combine the results.
   
   ![Add Prompt](/exercises/ex2/images/Add_Prompt.png)

3. In the **Add Prompt** dialog, fill in the following **Define Prompt** details and click **Next**.

   | Field | Value |
   |---|---|
   | Name | `get_new_hire_context` |
   | Title | `Fetch complete onboarding context for a new hire from SuccessFactors` |
   | Description | `Fetches complete onboarding context for a new hire from SAP SuccessFactors by calling employee personal info and job info tools and combining them into a single enriched payload` |

   ![Define Prompt](/exercises/ex2/images/Add_Prompt2.png)

4. On the **Define Arguments** screen, fill in the following details and click **Next**.

   | Field | Value |
   |---|---|
   | Name | `employeeId` |
   | Title | `New Hire's Employee ID` |
   | Description | `New Hire's Employee ID` |
   | Required | ✓ |

   ![Define Arguments](/exercises/ex2/images/Add_Prompt3.png)

5.  On the **Define Messages** screen, keep the **Role** as `User`, paste the following into the **Content** field, and click **Add** to save the prompt.

      ```
      Fetch the complete onboarding context for new hire {{ .employeeId }}.
   
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
      ![Define Messages](/exercises/ex2/images/Add_Prompt4.png)

6. Then click **Save** (top right) to save the artifact.

   ![Save MCP](/exercises/ex2/images/Add_Prompt5.png)
   
7. Now click on the **Policies** tab.
   >**HINT:** The **Policy Model** defines how every agent request is authenticated and authorized before it reaches the backend. 
   
   ![Navigate to Policies](/exercises/ex2/images/Edit_Policies.png)

8. In the policy model diagram, double click on the **Authentication** step to open its property sheet.

   ![Navigate to Authentication Step](/exercises/ex2/images/Navigate_Authentication.png)
   
9. In the bottom panel, go to the **Policy Settings** tab and configure the following:

   - Check **External OAuth (OIDC)**
   - Set **Configuration Type** to `Well-Known URL`

      Then copy and paste each value below into the corresponding field:
   
      **Well-Known URL:**
      ```
      https://attkvky2u.accounts.ondemand.com/.well-known/openid-configuration
      ```
      **Audience:**
      <br>Leave it empty.
      
      **ClientID Key:**
      ```
      ${authn.oidc.jwt.azp}
      ```
   
      **UserInfo Key:**
      ```
      ${authn.oidc.jwt.email}
      ```

   > **HINT:** Audience is intentionally left empty — IAS tokens in this setup do not carry a fixed audience claim.

10. Click **Save** (top right).
   
   ![Authentication Policy Settings](/exercises/ex2/images/Edit_Policies2.png)

## Exercise 2.4 — Deploy the MCP Server

1.  Click **Deploy** (top right). In the confirmation dialog, verify the defaults and click **Deploy** to confirm:
      - **Runtime Profile:** `Integration Cell`
      - **Virtual Host:** *(pre-selected, leave as default)*

      ![Deploy confirmation dialog](/exercises/ex2/images/Deploy.png)

2. Once deployed, the status bar at the top of the artifact page updates to confirm:
   <br>**`Deployed on [date], Runtime Status: STARTED`**

   ![Deployment status STARTED](/exercises/ex2/images/Deploy2.png)

   > [!IMPORTANT]
   > Make sure the status shows **STARTED** before proceeding to next exercise. If it still shows *Not Deployed*, wait a moment and refresh the page.

## Summary

At the end of this exercise, you have created and deployed an MCP Server for New Hire Employee Profile Lookup on the Integration Cell. The MCP Server exposes two tools backed by the pre-configured Composed API — giving the onboarding agent governed, identity-aware access to the complete new hire profile from SAP SuccessFactors.

Continue to - [Exercise 3 - Create and Deploy an MCP Server for HR Policy Lookup](../ex3/README.md)
