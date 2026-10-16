# Exercise 7 — Set Up and Run the Employee Onboarding AI Agent

With all four MCP Servers deployed and published as a governed product in the Developer Hub, the last step is to connect an AI agent to them. In this exercise, you will download the pre-built employee onboarding agent from the Solution Package, configure it with your deployed MCP Server URLs, and run it — connecting it to your SAP Advanced Event Mesh (AEM) queue so it is ready to respond when a new hire event arrives.

> [!NOTE]
> This exercise builds on the MCP Servers deployed in [Exercise 2](../ex2/README.md), [Exercise 3](../ex3/README.md), [Exercise 4](../ex4/README.md) and [Exercise 5](../ex5/README.md), and the product published in [Exercise 6](../ex6/README.md). Ensure all four MCP Servers show **Runtime Status: STARTED** before proceeding.

## What is the Employee Onboarding AI Agent?

The **Employee Onboarding AI Agent** is a Claude-powered agent that listens for new hire events from SAP SuccessFactors via Advanced Event Mesh and orchestrates the full onboarding process through the four MCP Servers you have deployed.

When a new hire event arrives, the agent:

1. Fetches the new hire's full profile and job details via the **Employee Lookup MCP**
2. Retrieves the applicable HR onboarding policies via the **HR Policy Lookup MCP**
3. Presents an onboarding plan to the manager for approval (**human-in-the-loop**)
4. After approval — creates a Purchase Requisition in SAP S/4HANA via the **Create PR MCP**
5. After approval — creates a Support Case in SAP Service Cloud via the **Service Cloud MCP**

All tool calls are routed through SAP Integration Suite MCP Gateway, which enforces authentication, authorization, and audit logging for every agent action.

## Exercise 7.1 — Download the Agent Bundle

In this sub-exercise, you download the pre-built agent bundle from the **TechEd 2026 IN261 - Solution Package** in SAP Integration Suite.

### Step 1: Navigate to the Solution Package

1. In SAP Integration Suite, navigate to **Design → Integrations and APIs**.

   <br>![Navigate to Integrations and APIs](/exercises/ex7/images/Navigate_to_SolutionPackage.png)

2. In the search field, type **`TechEd 2026 IN261 - Sol`** and press Enter. Click on **TechEd 2026 IN261 - Solution Package** to open it.

   <br>![Search and open the Solution Package](/exercises/ex7/images/Navigate_to_SolutionPackage2.png)

### Step 2: Download the Agent Bundle

1. Inside the Solution Package, click the **Documents (1)** tab.
2. Click on **IN261 Best Run Employee Onboarding AI Agent** to download it.

The file `IN261_employee_onboarding_ai_agent.zip` will begin downloading — you will see it appear in your browser's download bar.

   <br>![Documents tab — download the agent bundle](/exercises/ex7/images/Navigate_to_SolutionPackage3.png)

### Step 3: Extract the Bundle

1. Open your **Downloads** folder. You will find `IN261_employee_onboarding_ai_agent.zip` there.

   <br>![Downloaded zip in Downloads folder](/exercises/ex7/images/unzip.png)

2. Right-click the zip file and select **Extract All...**.

   <br>![Right-click — Extract All](/exercises/ex7/images/unzip2.png)

3. In the **Extract Compressed (Zipped) Folders** dialog, keep the default destination path and click **Extract**.

   <br>![Extract dialog — click Extract](/exercises/ex7/images/unzip3.png)

4. The extracted folder **`IN261_employee_onboarding_ai_agent`** now appears in your Downloads folder alongside the zip.

   <br>![Extracted folder in Downloads](/exercises/ex7/images/updateMCP.png)

## Exercise 7.2 — Configure Your MCP Server URLs

The agent bundle includes a file called `mcp-servers.json`. The **`active`** section of this file contains entries for all four MCP Servers — but their `url` fields are empty. You need to fill in the MCP Server URLs from your published product in the Developer Hub.

> [!TIP]
> **Fast Track:** If you have not yet deployed your own MCP Servers, the same `mcp-servers.json` file already contains the instructor-deployed solution URLs under `workshop_01a_solution` or `workshop_01b_solution`. Copy the URL values from the section that matches your assigned tenant and paste them into the corresponding `url` fields in the `active` section, then proceed to Exercise 7.3.

### Step 1: Open mcp-servers.json

1. Open the extracted folder **`IN261_employee_onboarding_ai_agent`**.
2. Open **`mcp-servers.json`** in VS Code or any text editor.

   <br>![Open the extracted folder — mcp-servers.json highlighted](/exercises/ex7/images/updateMCP2.png)

3. In the `active` section you will see four MCP Server entries. All four `"url"` fields are currently empty — you will fill these in from your Developer Hub product.

   <br>![mcp-servers.json open — active section with empty URL fields](/exercises/ex7/images/updateMCP3.png)

### Step 2: Navigate to Developer Hub

1. In SAP Integration Suite, click the **grid icon (⠿)** in the top-right navigation bar and select **Developer Hub**.

   <br>![Navigate to Developer Hub via grid icon](/exercises/ex7/images/CopyMCP.png)

2. In the Developer Hub search bar, type **`IN261_0XX Employee Onboarding`** (replace XX with your participant number) and click the search icon.

   <br>![Search for your product in Developer Hub](/exercises/ex7/images/SearchProduct.png)

3. Under the **Products** tab in the search results, click on **`IN261_0XX Employee Onboarding`** to open it.

   <br>![Search results — click your product](/exercises/ex7/images/SearchProduct2.png)

### Step 3: Copy Each MCP Server URL

1. Click the **AI Artifacts** tab. You will see all four MCP Servers in your product.

   <br>![Product — AI Artifacts tab showing all four MCP Servers](/exercises/ex7/images/EnterProduct_toCopyMCP.png)

2. Click on **`IN261_0XX_Employee_Lookup_MCP`** to open it. On the **Overview** page, locate the **MCP Server URL** field and click the **copy icon** next to it.

   <br>![MCP artifact — copy the MCP Server URL](/exercises/ex7/images/Copy_MCPhere.png)

3. Switch back to `mcp-servers.json` in VS Code. Paste the copied URL into the `"url"` field of **`"employee-lookup-mcp"`** in the `active` section.

   <br>![Paste the URL into the active section](/exercises/ex7/images/paste_MCPURL.png)

4. Go back to the **AI Artifacts** tab and repeat the same process for the remaining three MCP Servers — click each one, copy its **MCP Server URL**, and paste it into the corresponding entry in the `active` section:

   | MCP Artifact in Developer Hub | Entry in `active` section |
   |---|---|
   | `IN261_0XX_HR_Policy_Lookup_MCP` | `"applicable-hr-policy-lookup-mcp"` |
   | `IN261_0XX_Create_PR_S4Hana_MCP` | `"create-pr-s4hana-mcp"` |
   | `IN261_Create_SC_ServiceCloud_MCP` | `"create-sc-servicecloud-mcp"` |

   <br>![AI Artifacts — repeat for the remaining three MCP Servers](/exercises/ex7/images/Repeat_for_remaining_MCPs.png)

5. Once all four `"url"` fields are filled in, **save the file** (`Ctrl + S`).

   <br>![mcp-servers.json — all four URLs filled in and saved](/exercises/ex7/images/paste_all_four_MCP.png)

### Step 4: Return to the Folder

After saving, go back to the **`IN261_employee_onboarding_ai_agent`** folder in File Explorer — you will open a terminal from here in the next step.

   <br>![Return to the agent folder](/exercises/ex7/images/Save_and_return_to_folder.png)

## Exercise 7.3 — Install Dependencies and Run Setup

### Step 1: Open a Terminal

In File Explorer, right-click on the **`IN261_employee_onboarding_ai_agent`** folder and select **Open in Terminal**.

   <br>![Right-click the folder — Open in Terminal](/exercises/ex7/images/openTerminal.png)

### Step 2: Verify Node.js and Install Dependencies

In the terminal, run the following commands:

```bash
node -v
```

This confirms Node.js is installed. The version shown is fine — **do not update Node.js or npm**.

```bash
npm install
```

This installs all required dependencies. You may see a deprecation warning (`npm warn deprecated node-domexception`) — this is expected and can be safely ignored.

> [!IMPORTANT]
> Do **not** run `npm install -g` or attempt to update npm. Doing so will break the installed dependencies.

   <br>![Terminal — node -v and npm install complete](/exercises/ex7/images/Terminal1.png)

### Step 3: Run Setup

```bash
node setup.js
```

The setup wizard will ask you two things:

1. **Workshop tenant** — enter `1` for **workshop 01a** or `2` for **workshop 01b**, as assigned by your instructor
2. **AEM Queue name** — enter your assigned queue in the format `IN261_0**` (replace `**` with your participant number, e.g. `IN261_050`)

Once complete, you will see:

```
Setup complete! Run 'node agent.js' to start the agent.
```

   <br>![Terminal — node setup.js complete, tenant and AEM queue set](/exercises/ex7/images/Terminal2.png)

## Exercise 7.4 — Run the Agent

```bash
node agent.js
```

Your browser will open automatically for SAP IAS login. Log in with your workshop credentials. Once authenticated, close the browser tab and return to the terminal.

The agent is now connected to your AEM queue and listening for new hire events.

   <br>![Terminal — node agent.js running, browser login initiated](/exercises/ex7/images/terminal3.png)

> [!NOTE]
> Keep the terminal open and the agent running — you will need it in Exercise 8, where your instructor will trigger a new hire event and the agent will orchestrate the full onboarding workflow.

## Summary

You have now set up and started the Employee Onboarding AI Agent.

- The agent bundle was downloaded from the **TechEd 2026 IN261 - Solution Package** in SAP Integration Suite
- The four MCP Server URLs from your published Developer Hub product are configured in `mcp-servers.json`
- `node setup.js` set the correct workshop tenant and connected the agent to your AEM queue
- The agent is running and authenticated — ready to respond to new hire events

Continue to - [Exercise 8](../ex8/README.md)
