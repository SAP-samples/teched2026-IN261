# Exercise 4 — Create and Deploy a MCP Server for Purchase Requisition Creation in SAP S/4HANA for New Hire Equipments Post Manager Approval

## Overview

Once the policies applicable to New Hire are determined in (Exercise 3)

In this exercise, you will wrap the pre-delivered Integration Flow (IFlow) for Creating Purchase Requisition as an **MCP Server** — giving the onboarding agent governed, real-time access to creating Purchase Requisition in SAP S/4HANA via SAP Integration Suite. You will create an MCP artifact, configure it with the IFlow's OpenAPI specification, deploy it to the Integration Cell, and verify it is running.

By the end, the onboarding agent can create Purchase Requisition post manager approval — through a single secured MCP tool.

> [!NOTE]
> This exercise builds on the same Integration Package created in [Exercise 3](../ex3/README.md). You will add a new MCP Server artifact to the existing package — no need to create a new one.

---

## Pre-Delivered: Integration Flow for Creating Purchase Requisition

When the manager has approved the relevant equipment for the new hire, the Purchase Requisition needs to be created in S/4 HANA system to make it available to the new hire to carry out day-to-day tasks

A **pre-delivered Integration Flow (IFlow)** has been configured for this session. It acts as the mediation layer to post purchase requisition in S/4HANA

- MCP tool call invokes the integration flow via GET with the approved equipment and trainings payload
- Authenticates and forwards the request to S/4 HANA System based on oAuth2SAMLBearerAssertion
- Returns the Purchase Requisition created in S/4HANA system

Instead of building this IFlow from scratch, you use this pre-configured IFlow as the data source for your MCP Server. It comes with an **OpenAPI Specification** and a **Destination** — both of which you will use when creating the MCP artifact in the next steps.

<br>![Pre-delivered IFlow: Create Purchase Requisition in S/4HANA system](/exercises/ex3/images/IFlow_Image.png)

> [!NOTE]
> Download the **OpenAPI Specification** for this exercise: [OAS_CREATE_PR_API_POST.json](../ex4/OAS_CREATE_PR_API_POST.json)
## Concept: Principal Propagation — Why It Matters Here

When your AI agent calls the create Purchase Requisition MCP server, the backend system needs to know *who* is making that request and has the right scopes. Ensuring  audit trails are logged, access controls, and compliance requirements adherence.

**Principal Propagation** solves this by forwarding the authenticated user's identity from the MCP Server all the way through to the backend SAP system.

Here is how it works in this exercise:

| Step | What Happens |
|---|---|
| 1 | The AI agent authenticates to the MCP Server using **OIDC** |
| 2 | SAP Integration Suite verifies the caller via the configured **IAS (Identity Authentication Service)** trust |
| 3 | The **JWT Bearer Assertion** carrying the user's identity is used to implicitly fetch the oAuth token using the oAuth2JWTBearer authentication type|
| 4 | This assertion is forwarded to the backend S/4HANA system using oAuth2SAMLBearer authentication via integration flow — the backend sees the *real* user, not a generic service account |

> [!NOTE]
> For a deeper understanding, refer to [SAP Help: Principal Propagation](https://help.sap.com/docs/connectivity/sap-btp-connectivity-cf/principal-propagation)

---

---

## Exercise 3.1 — Create the MCP Server

In this sub-exercise, you add a new MCP Server artifact to your existing Integration Package, configured against the pre-delivered Create Purchase Requisition IFlow.

### Step 1: Open the Existing Integration Package

1. In SAP Integration Suite, navigate to **Design → Integrations and APIs**.

   <br>![Navigate to Integrations and APIs](/exercises/ex4/images/Select_Iflow_Package.jpg)

2. Search for and open your package `IN261_0XX_TechEd_2026_Package` from Exercise 3.

   > Replace **XX** with your assigned participant number.

3. Inside the package, click **Edit** (top right) to enter edit mode. The `IN261_0XX_HR_Policy_Lookup_MCP` artifact from Exercise 3 will already be visible — you will add the HR Policy MCP Server alongside it.

   <br>![Open package and click Edit](/exercises/ex4/images/Edit_Iflow.jpg)

### Step 2: Add an MCP Server Artifact

1. In the **Artifacts** tab, click **Add** and select **MCP Server** from the dropdown.

   <br>![Add MCP Server](/exercises/ex4/images/Add_MCP_Server.jpg)

2. In the **Add MCP Server** dialog, select **HTTP Endpoint with OpenAPI Specification** as the source type and click **Next**.

   <br>![Select Source Type](/exercises/ex4/images/MCP_HTTP_Endpoint.jpg)

3. Fill in the MCP Server details:

   | Field | Value |
   |---|---|
   | Method | `Upload` |
   | File Name | Download and upload [`Company_Policy_RAG_API.json`](../ex3/Company_Policy_RAG_API.json) |
   | Source | `Destination` |
   | Destination | `in261_iflow_as_mcp` |
   | Relative URL | `/http` |
   | Name | `IN261_0XX_Create_PR_S4Hana_MCP` |
   | ID | `IN261_0XX_Create_PR_S4Hana_MCP` |
   | MCP Path | `/in261_0XX_create_pr_s4hana_mcp` |
   | Version | `1.0.0` |
   | Runtime Profile | `Integration Cell` |

   > Replace **XX** with your assigned participant number.

   <br>![Provide MCP Details](/exercises/ex4/images/CreateMCP.jpg)

4. Click **Next**.

### Step 3: Select Tools

On the **Create Tools** screen, select the HR Policy Lookup operation:

| Method | Path | Description |
|---|---|---|
| `POST` | `/newhire/equipments` | Retrieve HR onboarding policy for a new hire |

   <br>![Select Tools](/exercises/ex4/images/Tool_Selection.jpg)

5. Click **Add** to finish creating the MCP Server artifact.

### Step 4: Edit the Policy Model

The policy model for the HR Policy MCP Server follows the same authentication configuration as Exercise 2 — External OAuth (OIDC) via IAS, ensuring every agent request is verified before reaching the HR policy backend.

1. From the artifact overview, click **Edit** (top right), then click on the **Policies** tab. *(If the Policies tab is not already active, click it to open it.)*

   <br>![Navigate to Policies tab](/exercises/ex4/images/after_tool_added.jpg)

2. In the policy model diagram, click on the **Authentication** step to select it.
 In the bottom panel, go to the **Policy Settings** tab and configure the following:

   - Check **External OAuth (OIDC)**
   - Set **Configuration Type** to `Well-Known URL`

   Then copy and paste each value below into the corresponding field:

   **Well-Known URL:**
   ```
   https://attkvky2u.accounts.ondemand.com/.well-known/openid-configuration
   ```

   **Audience:** Leave it empty.

   **ClientID Key:**
   ```
   ${authn.oidc.jwt.azp}
   ```

   **UserInfo Key:**
   ```
   ${authn.oidc.jwt.email}
   ```

   <br>![Authentication Policy Settings](/exercises/ex4/images/Auth_Policy.jpg)

4.Click **HTTP Adapter** and got to connections.The destination details will be shown
<br>![Authentication Policy Settings](/exercises/ex4/images/Connection_details.jpg)


5. Click **Save** (top right).
     <br>![Authentication Policy Settings](/exercises/ex4/images/Save.jpg)

---

## Exercise 3.2 — Deploy the MCP Server

In this sub-exercise, you deploy the HR Policy MCP Server artifact to the Integration Cell and verify it is running.

### Step 1: Deploy

1. From the artifact overview, click **Deploy** (top right).
    <br>![Deploy](/exercises/ex4/images/Click_Deploy.jpg)

2. A confirmation dialog appears. Verify the defaults:
   - **Runtime Profile:** `Integration Cell`
   - **Virtual Host:** *(pre-selected, leave as default)*
3. Click **Deploy** in the dialog to confirm.

   <br>![Deploy confirmation dialog](/exercises/ex4/images/After_Deploy.jpg)

### Step 2: Verify Deployment Status

Once deployed, the status bar at the top of the artifact page updates to confirm:

**`Deployed on [date], Runtime Status: STARTED`**

   <br>![Deployment status STARTED](/exercises/ex4/images/After_Deployed.jpg
)

> [!IMPORTANT]
> Only proceed to Exercise 4 once the status shows **STARTED**. If it still shows *Not Deployed*, wait a moment and refresh the page.

### Step 3: MCP Server Ready

Once the status shows **STARTED**, your MCP Server is deployed on the Integration Cell. The deployed MCP Servers from Exercise 2 and this exercise will be published as a **Product** in the following exercises — making them discoverable and consumable by authorized AI agents.

---

## Summary

You have now created and deployed an MCP Server that wraps the pre-delivered HR Policy Lookup IFlow and exposes it securely to AI agents.

- The IFlow acts as governed middleware between the MCP Server and the AI Launch Pad Document Grounding API
- The MCP Server handles authentication and authorization using OAuth 2.0
- Principal Propagation ensures the backend sees the real caller identity, not a shared service account
- The MCP endpoint is live on the Integration Cell.

Continue to - [Exercise 4](../ex4/README.md)
