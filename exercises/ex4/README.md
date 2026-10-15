# Exercise 4 — Create and Deploy a MCP Server for Purchase Requisition Creation in SAP S/4HANA for New Hire Equipments Post Manager Approval

Once the policies applicable to New Hire are determined in (Exercise 3)

In this exercise, you will deploy an MCP Server for Creating Purchase Requisition from the pre-configured Integration Flow (iFlow) — giving the onboarding agent governed, real-time access to creating Purchase Requisition in SAP S/4HANA via SAP Integration Suite. You will create an MCP artifact, configure it with the iFlow's OpenAPI specification, deploy it to the Integration Cell, and verify it is running.

> [!NOTE]
> This exercise builds on the same Integration Package created in [Exercise 2](../ex2/README.md). You will add a new MCP Server artifact to the existing package — no need to create a new one.


## Pre-Configured: Integration Flow for Creating Purchase Requisition

When the manager has approved the relevant equipment for the new hire, the Purchase Requisition needs to be created in S/4 HANA system to make it available to the new hire to carry out day-to-day tasks

A **pre-configured Integration Flow (iFlow)** has been configured for this session. It acts as the mediation layer to post purchase requisition in S/4HANA

- MCP tool call invokes the integration flow via POST with the approved equipment and trainings payload
- Authenticates and forwards the request to S/4 HANA System based on oAuth2SAMLBearerAssertion
- Returns the Purchase Requisition created in S/4HANA system

Instead of building this iFlow from scratch, you use this pre-configured iFlow as the data source for your MCP Server. It comes with an **OpenAPI Specification** and a **Destination** — both of which you will use when creating the MCP artifact in the next steps.

<br>![Pre-configured iFlow: Create Purchase Requisition in S/4HANA system](/exercises/ex4/images/Predeployed_createPR_Iflow.jpg)

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

## Exercise 4.1 — Create the MCP Server

In this sub-exercise, you add a new MCP Server artifact to your existing Integration Package, configured against the pre-configured Create Purchase Requisition iFlow.

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

3. Fill in the MCP Server details and Click **Next**.:

   | Field | Value |
   |---|---|
   | Method | `Upload` |
   | File Name | Download The **OpenAPI Specification** from [`OAS_CREATE_PR_API_POST.json`](https://github.com/SAP-samples/teched2026-IN261/releases/download/exercise-assets/OAS_CREATE_PR_API_POST.json) and upload it.|
   | Source | `Destination` |
   | Destination | `in261_iflow_as_mcp` |
   | Relative URL | `/http` |
   | Name | `IN261_0XX_Create_PR_S4Hana_MCP` (replace XX with your assigned participant number) |
   | ID | `IN261_0XX_Create_PR_S4Hana_MCP` (replace XX with your assigned participant number) |
   | MCP Path | `/in261_0XX_create_pr_s4hana_mcp` (replace XX with your assigned participant number) |
   | Version | `1.0.0` |
   | Runtime Profile | `Integration Cell` |


   <br>![Provide MCP Details](/exercises/ex4/images/CreateMCP.jpg)

4. On the **Create Tools** screen, select the HR Policy Lookup operation and Click **Add**:

| Method | Path | Description |
|---|---|---|
| `POST` | `/newhire/equipments` | Retrieve HR onboarding policy for a new hire |

   <br>![Select Tools](/exercises/ex4/images/Tool_Selection.jpg)


### Step 3: Edit the MCP Policy

The policy model for the Create PR follows the same authentication configuration as Exercise 2 — External OAuth (OIDC) via IAS, ensuring every agent request is authenticated ,authorised and propagates the user identity to S/4HANA backend

1. From the tools tab in which the tool selected gets addded can be seen, click **Policies** tab.

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

   **Audience:** 
   
   Leave it empty.
   

   **ClientID Key:**
   ```
   ${authn.oidc.jwt.azp}
   ```

   **UserInfo Key:**
   ```
   ${authn.oidc.jwt.email}
   ```

   <br>![Authentication Policy Settings](/exercises/ex4/images/Auth_Policy.jpg)

3.Click **HTTP Adapter** and got to connections.The destination details will be shown
<br>![Authentication Policy Settings](/exercises/ex4/images/Connection_details.jpg)


4. Click **Save** (top right).
     <br>![Authentication Policy Settings](/exercises/ex4/images/Save.jpg)

## Exercise 3.2 — Deploy the MCP Server

In this sub-exercise, you deploy the Create Purchase Requisition MCP Server artifact to the Integration Cell and verify it is running.

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
> Only proceed to Exercise 5 once the status shows **STARTED**. If it still shows *Not Deployed*, wait a moment and refresh the page.

## Summary

You have now deployed a MCP Server on the Integration Cell. The deployed MCP Servers from this exercise will be published as a **Product** in the following exercises — making them discoverable and consumable by authorized AI agents.

- The iFlow acts as mediation layer between the MCP Server and the S/4 HANA backend system
- The MCP Server handles authentication and authorization using External OIDC oAuth2JWTBearer Authentication
- Principal Propagation ensures the backend sees the real caller identity
- The MCP endpoint is live on the Integration Cell.

Continue to - [Exercise 4](../ex4/README.md)
