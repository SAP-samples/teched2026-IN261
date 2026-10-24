# Troubleshooting Guide — IN261

This guide covers the most common issues participants encounter across the exercises, based on the hints, important notes, and known pitfalls in the session. Check here before calling the instructor.

---

## General

**Make sure you always use the same SAP Integration Suite tenant URL and the same participant number throughout all exercises**
> Your SAP Integration Suite tenant (01a or 01b) is assigned based on your participant number printed on your seat card. Use the same tenant URL and the same participant number across every exercise — mixing them will cause artifact conflicts, deployment failures, and agent configuration mismatches.

**Wrong tenant selected during setup — agent is calling wrong MCP Server URLs**
> Re-run `node setup.js`, select the correct tenant, and update `mcp-servers.json` with the correct URLs before restarting the agent.

**Participant number confusion — artifacts named incorrectly**
> Your participant number is printed on your seat card. All artifacts must include your participant number in their name and path. Artifacts with wrong numbers may conflict with other participants and cause deployment failures.
> All artifacts in this session use underscore (`_`) i.e. **`IN261_0**`**. Hyphen is only used in the login username (**`IN261-0**`**). So make sure you dont use hyphen (`-`) while naming your artifacts.

**Any system is unreachable**
> Contact the instructors — system URLs and credentials are listed on the [main page](../../README.md#system-url-and-login-details).

---

## Exercise 1 — AEM Queue Setup

**Cannot see the `TechEd2026-IN261` tile in Cluster Manager**
> You may have the **Only show my services** checkbox enabled. Uncheck it to see all services including the shared workshop cluster.

**Queue name entered incorrectly — using hyphen instead of underscore**
> The AEM queue name uses an **underscore**: **`IN261_0**`** — not a hyphen. All artifacts and queue names in this session use underscore (`_`). Hyphen is only used in the login username (`IN261-0**`). Using a hyphen in the queue name will mean your agent cannot find the queue.

**Wrong topic subscription entered**
> The topic must match exactly: `sap/sfsf/newhire/IN261_0**` *(replace ** with your participant number)*. Any typo means your queue will never receive the NEWHIRE event. Delete the wrong subscription and create a new one.

**Queue or topic subscription not appearing after creation**
> It may take a moment to reflect. Reload the browser if the changes are not visible.

---

## Exercise 2 — Employee Lookup MCP Server

**Cannot find or download the OpenAPI spec `SAP_HCM_Graph.json`**
> The link to download **`SAP_HCM_Graph.json`** is in the exercise. Download it from the GitHub release assets and upload it in the Add MCP Server dialog. Do not use a different spec — using the wrong spec will result in incorrect or missing tools.

**MCP Server deployed but tools are missing**
> On the Create Tools screen, you must select the tools from the uploaded spec. If you skipped this step, delete the artifact and recreate it — ensure you select all required tools before clicking Add.

**Deployment status stays "Not Deployed" after clicking Deploy**
> Wait a moment and refresh the page. Deployment to the Integration Cell takes a few seconds. Do not click Deploy again — check the status bar first.

**MCP Server deployed but status is not STARTED**
> Check if the MCP Path was saved correctly before deployment. The path must be unique across the tenant — if another participant has the same path, deployment will fail. Verify your path includes your participant number: **`/in261_0**_employee_lookup_mcp`**.

**Authentication policy — where to find the Well-Known URL and ClientID Key**
> These values are the same for all participants — copy them exactly as shown in the exercise. The Audience field must be left **empty** — IAS tokens in this setup do not carry a fixed audience claim. Filling it in will cause authentication failures.
---

## Exercise 3 — HR Policy Lookup MCP Server

**Copied artifact name exceeds 32 characters and cannot be saved after editing**
> If the artifact name exceeds 32 characters it will copy successfully but you will not be able to edit and save it later. Make sure to follow the naming convention exactly: **`IN261_0**_HR_Policy_Lookup_MCP`** — and do not include the word "Copy" in the name.

**Copied artifact name has "Copy" in it**
> In the Copy dialog, remove the word "Copy" from the name suffix and add your participant number instead: **`IN261_0**_HR_Policy_Lookup_MCP`**.

**MCP Path not updated before deployment**
> Each MCP Server must have a unique MCP path. If you deploy without updating the path to include your participant number, it will conflict with another participant's deployment. Update to **`/in261_0**_hr_policy_lookup_mcp`** before deploying.

**Deployment fails with path conflict error**
> Two participants cannot share the same MCP path on the same tenant. Verify your path includes your participant number and is not already in use.

---

## Exercise 4 — Purchase Requisition MCP Server

**Cannot find or download the OpenAPI spec for the Purchase Requisition iFlow**
> The link to download the spec is in the exercise. Download it from the GitHub release assets and upload it in the Add MCP Server dialog. Do not use a different spec — using the wrong spec will result in incorrect or missing tools.

**MCP Server deployed but tools are missing**
> On the Create Tools screen, you must select the tools from the uploaded spec. If you skipped this step, delete the artifact and recreate it — ensure you select all required tools before clicking Add.

**Deployment status stays "Not Deployed" after clicking Deploy**
> Wait a moment and refresh the page. Deployment to the Integration Cell takes a few seconds. Do not click Deploy again — check the status bar first.

**MCP Server deployed but status is not STARTED**
> Check if the MCP Path was saved correctly before deployment. The path must be unique across the tenant — if another participant has the same path, deployment will fail. Verify your path includes your participant number: **`/in261_0**_employee_lookup_mcp`**.

**Authentication policy — where to find the Well-Known URL and ClientID Key**
> These values are the same for all participants — copy them exactly as shown in the exercise. The Audience field must be left **empty** — IAS tokens in this setup do not carry a fixed audience claim. Filling it in will cause authentication failures.

---

## Exercise 5 — Support Case MCP Server

**Copied artifact name exceeds 32 characters and cannot be saved after editing**
> If the artifact name exceeds 32 characters it will copy successfully but you will not be able to edit and save it later. Make sure to follow the naming convention exactly: **`IN261_0**_Create_SupportCase_MCP`** — and do not include the word "Copy" in the name.

**Copied artifact name has "Copy" in it**
> In the Copy dialog, remove the word "Copy" from the name suffix and add your participant number instead: **`IN261_0**_Create_SupportCase_MCP`**.

**MCP Path not updated before deployment**
> Each MCP Server must have a unique MCP path. If you deploy without updating the path to include your participant number, it will conflict with another participant's deployment. Update to **`/in261_0**_create_supportcase_mcp`** before deploying.

**Deployment fails with path conflict error**
> Two participants cannot share the same MCP path on the same tenant. Verify your path includes your participant number and is not already in use.
---

## Exercise 6 — Developer Hub: Publish and Subscribe

**Cannot create a new Agent subscription — the option is disabled**
> Due to a platform constraint, only one Agent subscription can exist per OIDC Client ID. Since all participants share the same Client ID, you cannot create a new subscription. Instead, click **Add To Existing Subscription for Agent** to add your MCP Servers to the existing instructor-created subscription. If this option is also disabled, call the instructor for support.

**Published product not visible in Developer Hub**
> It may take a moment to reflect. Reload the browser if the product does not appear after publishing.

**MCP Servers (AI Artifacts) not appearing in the product**
> Make sure all four MCP Servers (AI Artifacts) are in **STARTED** state before publishing. An MCP Server that is not deployed or has errors will not be available to add to the product.

---

## Exercise 7 — Download, Configure and Run the Agent

**Copying the correct MCP Server URLs into the correct section of `mcp-servers.json` is critical**
> You must copy all four MCP Server URLs from **Developer Hub** and paste them into the **`active`** section of `mcp-servers.json` — not any other section. Each URL must go under the correct server name (`employee-lookup-mcp`, `applicable-hr-policy-lookup-mcp`, `create-pr-s4hana-mcp`, `create-supportcase-mcp`). A wrong URL, a URL in the wrong entry, or a URL left empty will cause the agent to fail at startup or when calling that tool.

**`mcp-servers.json` validation fails — URLs do not match tenant**
> The URLs in the `active` section must contain your assigned tenant key (`workshop01a` or `workshop01b`). If you copied URLs from the wrong tenant or from the solution sections (`workshop_01a_solution` / `workshop_01b_solution`) for the wrong tenant, the agent will reject them. Re-run `node setup.js` after correcting the URLs.

**Browser does not open automatically for IAS login**
> Copy the Authorization URL printed in the terminal and paste it manually into your browser.

**"Port 3000 is already in use" error on login**
> Another process is using port 3000. Follow the instructions printed in the terminal to find and kill the process, then re-run `node agent.js`.

**Login times out after 5 minutes**
> Re-run `node agent.js` — a fresh login attempt will open the browser again.

**"OpenID provider cannot process the request because the configuration is incorrect"**
> This means the OIDC Client ID being used is not registered correctly in IAS. Make sure you selected the correct tenant (01a or 01b) during `node setup.js`. If the issue persists, contact the instructor — the IAS application for your tenant may need a configuration fix (redirect URI registration or missing scope).

**Agent starts but shows "MCP server configuration mismatch"**
> The URLs in the `active` section of `mcp-servers.json` do not match your configured tenant. Re-run `node setup.js` and ensure the URLs contain your tenant key (e.g. `workshop01a` or `workshop01b`).

---

## Exercise 8 — Trigger a New Hire Event

**Route 1 — New employee added in SFSF but no event received by the agent**
> Check that your AEM queue topic subscription from Exercise 1 exactly matches `sap/sfsf/newhire/IN261_0**` with your participant number. Also verify the agent is still running — if the terminal was closed, re-run `node agent.js` and trigger a new employee again.

**Route 2 — Published message but agent did not receive it**
> Direct messages are delivered only to active consumers. If the agent was not running at the moment of publishing, the message is lost. Make sure the agent is running, then publish again. Alternatively change the Delivery Mode to **Persistent** before publishing.

**Cannot connect in AEM Try Me! — Client Password not working**
> Make sure you copied the Client Password **before** clicking Open Broker Manager. The password is only shown once per session — if you lost it, go back to the Try Me! tab and copy it again.

---

## Exercise 9 — Observe, Approve, and Verify

**The equipment, system access, and training list in the onboarding plan looks different from other participants**
> This is expected — the onboarding plan is dynamic and determined by the position, department and division of the new hire you created or selected in Exercise 8. Different positions (Research & Development, Sales, Marketing, Manufacturing, or Default) will produce different equipment, system access, and training requirements. There is no single correct output — what matters is that the plan is generated, presented, and acted upon correctly.

**Agent received the event but stopped without presenting the onboarding plan**
> Check the terminal for error messages. If you see `Error 3000 — Failed to build headers`, it is a Principal Propagation issue — the IAS token could not be forwarded to the BDG backend. Contact the instructor.

**Agent is looping and not stopping**
> The agent has a maximum of 10 tool iterations. If it exceeds this, it will exit automatically. This is a safety cap. If it loops consistently, check that all four MCP Server URLs in `mcp-servers.json` are correct and the servers are deployed and started.

**Typed "Approved" but agent did not proceed**
> The approval input is case-sensitive to common approval words. Make sure you type `Approved` and press **Enter**. Do not include extra spaces or punctuation.

**PR Link or Case Link in the summary is not clickable**
> Hold **Ctrl** and click the link — plain click does not open URLs in most terminals.

**OData API Audit Log in SuccessFactors shows no entries**
> The audit log may take a minute to reflect. Refresh the page. Also make sure you are filtering for the correct time window — set it to the last 30 minutes to see entries from your agent run.
