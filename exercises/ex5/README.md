# Exercise 5 — MCP Server for Service Cloud Case Creation (Pre-Configured)

> [!IMPORTANT]
> **Pre-Configured and Pre-Deployed by Instructor — No Action Required**
> This MCP Server has already been configured and deployed for all participants. You do not need to create or deploy anything in this exercise. This exercise is for reference only — read it to understand what was built and how it fits into the onboarding flow, then proceed to [Exercise 6](../ex6/README.md).

In this session, the onboarding agent creates a Support Case in SAP Service Cloud V2 after manager approval — covering IT system access and role-specific training enrollment for the new hire. The MCP Server that powers this step has been deployed by your instructor and is already available in your package.

⏱ **Reference only — no hands-on steps**

## Overview

Unlike the Employee Lookup MCP (Exercise 2) which sits in front of a Composed API, and the HR Policy and Purchase Requisition MCPs (Exercises 3 and 4) which go through pre-configured Integration Flows, this MCP Server connects **directly to the SAP Service Cloud V2 API** — no iFlow, no BDG, no intermediate artifact.

SAP Integration Suite's MCP Gateway sits in front of the Service Cloud API, enforcing authentication, authorization, and audit logging before any request reaches the backend. The agent calls two tools in sequence:

1. **`queryemployeeservice_employee`** — looks up the logged-in user's identity in Service Cloud by their display ID to retrieve their internal UUID
2. **`createcaseservice_case`** — creates a Support Case in Service Cloud with the system access and training details from the onboarding plan

<details>
<summary><strong>📖 What Was Pre-Configured: MCP Server for Service Cloud Case Creation</strong></summary>

<br>

The MCP Server was created from the SAP Service Cloud V2 OpenAPI Specification — exposing two tools directly from the Service Cloud REST API with no middleware in between:

| Tool | Method | Path | Description |
|---|---|---|---|
| `queryemployeeservice_employee` | `GET` | `/sap/c4c/api/v1/employee-service/employees` | Looks up a Service Cloud employee using `$filter=displayId eq '{id}'` to retrieve their internal UUID |
| `createcaseservice_case` | `POST` | `/sap/c4c/api/v1/case-service/cases` | Creates a Support Case with subject, employee, reporter, system access details, and training details |

**Authentication:** Technical user via Basic Auth destination — unlike Exercise 2, Principal Propagation is not used here. The MCP Gateway authenticates the agent call via IAS OAuth, then the backend call uses a pre-configured technical user credential stored in the BTP Destination.

**Why technical user here?**
Service Cloud V2 in this workshop environment does not have IAS trust configured for JWT Bearer exchange. A technical user is the practical choice for the session — in a production setup, Principal Propagation would be the recommended approach.

**How the agent uses these tools:**

```
Step 1: queryemployeeservice_employee
  $filter=displayId eq '{loggedInUsername}'
  → returns value[0].id (UUID)

Step 2: createcaseservice_case
  caseType: "ZEST"
  subject: "New Hire Onboarding — System Access & Training Assignment for {employeeName} ({employeeId})"
  employee: { id: "<UUID from Step 1>" }
  reporter: { id: "<UUID from Step 1>" }
  extensions:
    SystemAccessDetails: "<comma-separated systems>"
    TrainingDetails: "<comma-separated training courses>"
  → returns id → agent constructs case URL as {SERVICE_CLOUD_TENANT}/go/detail/case?id={id}
```

**MCP Server details:**

| Field | Value |
|---|---|
| Name | `IN261_Service_Cloud_Case_MCP` |
| MCP Path | `/in261_service_cloud_case_mcp` |
| Source | `Destination` |
| Destination | `in261_service_cloud_mcp` |
| Runtime Profile | `Integration Cell` |

</details>

<details>
<summary><strong>📖 How This MCP Fits in the Onboarding Flow</strong></summary>

<br>

This MCP Server is the last step in the onboarding flow — called by the agent only after manager approval:

```
AEM Event (new hire)
    │
    ├── MCP Tool: get_bestrun_hcm_employee        (Exercise 2)
    ├── MCP Tool: get_bestrun_hcm_employeeJob      (Exercise 2)
    ├── MCP Tool: post_newhire_applicable_hr_policy (Exercise 3)
    │
    │   ── Manager Approval ──
    │
    ├── MCP Tool: create_purchase_requisition      (Exercise 4)
    │
    └── MCP Tool: queryemployeeservice_employee    (Exercise 5 — this exercise)
        MCP Tool: createcaseservice_case           (Exercise 5 — this exercise)
```

The agent calls `query_employees` first to resolve the logged-in user's identity, then calls `create_case` to raise a Support Case covering all system access and training items that require manual provisioning.

The response includes a case ID — the agent constructs a direct link to the case in the SAP Service Cloud tenant and presents it in the onboarding summary.

</details>

## Summary

The Service Cloud Case Creation MCP Server has been pre-deployed and is ready to use. It will appear alongside your other MCP Servers when you publish the product in Exercise 6.

Continue to - [Exercise 6 - Publish MCP Servers as a Product and Subscribe via Developer Hub](../ex6/README.md)
