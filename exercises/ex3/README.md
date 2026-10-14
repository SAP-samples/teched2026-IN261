# Exercise 3 — Create and Deploy a MCP Server for HR Policy Lookup

## Overview

An onboarding agent is only as helpful as the knowledge it can access. Knowing *who* the new hire is (Exercise 2) is the first step — knowing *what policies apply to them* is the next.

In this exercise, you will wrap the pre-delivered Integration Flow (IFlow) for HR Policy Lookup as an **MCP Server** — giving the onboarding agent governed, real-time access to company HR policies via SAP Integration Suite. You will create an MCP artifact, configure it with the IFlow's OpenAPI specification, deploy it to the Integration Cell, and verify it is running.

By the end, the onboarding agent can query HR policies — leave, IT access, code of conduct, and more — through a single secured MCP tool.

> [!NOTE]
> This exercise builds on the same Integration Package created in [Exercise 2](../ex2/README.md). You will add a new MCP Server artifact to the existing package — no need to create a new one.

---

## Pre-Delivered: Integration Flow for HR Policy Lookup

When a new hire joins, they need answers to policy questions — leave entitlements, IT access rules, code of conduct, and more. These policies exist as documents, but an AI agent cannot search unstructured documents on its own.

The **AI Launch Pad Orchestration Document Grounding API** solves this. It indexes HR policy documents into a vector store and exposes a semantic search API — so the agent can ask a natural language question and retrieve the most relevant policy content.

However, calling the Document Grounding API directly from an agent raises the same governance concerns as any other enterprise API — authentication, authorization, audit, and principal propagation. This is where SAP Integration Suite steps in.

A **pre-delivered Integration Flow (IFlow)** has been configured for this session. It acts as the governed middleware between the MCP Server and the Document Grounding API:

- Receives the policy query from the MCP tool call
- Authenticates and forwards the request to the AI Launch Pad Document Grounding API
- Returns the relevant HR policy content back to the agent

Instead of building this IFlow from scratch, you use this pre-configured IFlow as the data source for your MCP Server. It comes with an **OpenAPI Specification** and a **Destination** — both of which you will use when creating the MCP artifact in the next steps.

<br>![Pre-delivered IFlow: Prompt to HR Policy RAG Pipeline](/exercises/ex3/images/IFlow_Image.png)

> [!NOTE]
> Download the **OpenAPI Specification** for this exercise: [Company_Policy_RAG_API.json](../ex3/Company_Policy_RAG_API.json)
> For a deeper understanding of the AI Launch Pad Document Grounding capability, refer to [SAP Help: Document Grounding](https://help.sap.com/docs/sap-ai-launchpad/sap-ai-launchpad/document-grounding)

---

## Exercise 3.1 — Create the MCP Server

In this sub-exercise, you add a new MCP Server artifact to your existing Integration Package, configured against the pre-delivered HR Policy Lookup IFlow.

### Step 1: Open the Existing Integration Package

1. In SAP Integration Suite, navigate to **Design → Integrations and APIs**.

   <br>![Navigate to Integrations and APIs](/exercises/ex3/images/Int_Package1.png)

2. Search for and open your package `IN261_0XX_TechEd_2026_Package` from Exercise 2.

   > Replace **XX** with your assigned participant number.

3. Inside the package, click **Edit** (top right) to enter edit mode. The `IN261_0XX_Employee_Lookup_MCP` artifact from Exercise 2 will already be visible — you will add the HR Policy MCP Server alongside it.

   <br>![Open package and click Edit](/exercises/ex3/images/Add_MCP1.png)

### Step 2: Add an MCP Server Artifact

1. In the **Artifacts** tab, click **Add** and select **MCP Server** from the dropdown.

   <br>![Add MCP Server](/exercises/ex3/images/Add_MCP2.png)

2. In the **Add MCP Server** dialog, select **HTTP Endpoint with OpenAPI Specification** as the source type and click **Next**.

   <br>![Select Source Type](/exercises/ex3/images/Add_MCP3.png)

3. Fill in the MCP Server details:

   | Field | Value |
   |---|---|
   | Method | `Upload` |
   | File Name | Download and upload [`Company_Policy_RAG_API.json`](../ex3/Company_Policy_RAG_API.json) |
   | Source | `Destination` |
   | Destination | `in261_iflow_as_mcp` |
   | Relative URL | `/http` |
   | Name | `IN261_0XX_HR_Policy_Lookup_MCP` |
   | ID | `IN261_0XX_HR_Policy_Lookup_MCP` |
   | MCP Path | `/in261_0XX_hr_policy_lookup_mcp` |
   | Version | `1.0.0` |
   | Runtime Profile | `Integration Cell` |

   > Replace **XX** with your assigned participant number.

   <br>![Provide MCP Details](/exercises/ex3/images/Add_MCP4.png)

4. Click **Next**.

### Step 3: Select Tools

On the **Create Tools** screen, select the HR Policy Lookup operation:

| Method | Path | Description |
|---|---|---|
| `POST` | `/newhire/applicable-hr-policy` | Retrieve HR onboarding policy for a new hire |

   <br>![Select Tools](/exercises/ex3/images/Add_MCP5.png)

5. Click **Add** to finish creating the MCP Server artifact.

### Step 4: Edit the Policy Model

The policy model for the HR Policy MCP Server follows the same authentication configuration as Exercise 2 — External OAuth (OIDC) via IAS, ensuring every agent request is verified before reaching the HR policy backend.

1. From the artifact overview, click **Edit** (top right), then click on the **Policies** tab. *(If the Policies tab is not already active, click it to open it.)*

   <br>![Navigate to Policies tab](/exercises/ex3/images/Edit_Policies.png)

2. In the policy model diagram, click on the **Authentication** step to select it.

3. In the bottom panel, go to the **Policy Settings** tab and configure the following:

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

   <br>![Authentication Policy Settings](/exercises/ex3/images/Edit_Policies2.png)

4. Click **Save** (top right).

---

## Exercise 3.2 — Deploy the MCP Server

In this sub-exercise, you deploy the HR Policy MCP Server artifact to the Integration Cell and verify it is running.

### Step 1: Deploy

1. From the artifact overview, click **Deploy** (top right).
2. A confirmation dialog appears. Verify the defaults:
   - **Runtime Profile:** `Integration Cell`
   - **Virtual Host:** *(pre-selected, leave as default)*
3. Click **Deploy** in the dialog to confirm.

   <br>![Deploy confirmation dialog](/exercises/ex3/images/Deploy1.png)

### Step 2: Verify Deployment Status

Once deployed, the status bar at the top of the artifact page updates to confirm:

**`Deployed on [date], Runtime Status: STARTED`**

   <br>![Deployment status STARTED](/exercises/ex3/images/Deploy2.png)

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
