# Exercise 5 — MCP Server for Support Case Creation in SAP Service Cloud V2 (Pre-Configured and Pre-Deployed)

> [!IMPORTANT]
> **Pre-Configured and Pre-Deployed by Instructor — No Action Required**
> <br>This MCP Server has already been configured and deployed for all participants. You do not need to create or deploy anything in this exercise. This exercise is for reference only — read it to understand what was built and how it fits into the onboarding flow, then proceed to [Exercise 6](../ex6/README.md).

In this session, the employee onboarding AI agent creates a Support Case in SAP Service Cloud V2 after manager approval — covering role-specific IT system access and training enrollment for the new hire. The MCP Server that powers this step has been deployed by your instructor and is already available in your package.

⏱ **Estimated time: 5 minutes** — Reference only, no hands-on steps

## Overview

Unlike the Employee Lookup MCP (Exercise 2) which sits in front of a Composed API, and the HR Policy and Purchase Requisition MCPs (Exercises 3 and 4) which go through pre-configured Integration Flows, this MCP Server connects **directly to the SAP Service Cloud V2 API** — no iFlow, no BDG, no intermediate artifact.

SAP Integration Suite's MCP Gateway sits in front of the Service Cloud API, enforcing authentication and authorization before any request reaches the backend. The agent calls two tools in sequence:

1. **`get_sap_c4c_api_v1_employee_service_employees`** — Looks up the logged-in user's identity in Service Cloud by their display ID to retrieve their internal UUID as this is required to create a support case.
2. **`post_sap_c4c_api_v1_case_service_cases`** — Creates a Support Case in Service Cloud for the new hire with the role-specific system access and training details from the onboarding plan.

![Service Cloud MCP](images/ex5-1.png)

<details>
<summary><strong>📖 For the Curious: Inside the Service Cloud MCP Server</strong></summary>

<br>

The MCP Server was created from the SAP Service Cloud V2 OpenAPI Specification — exposing two tools directly from the Service Cloud REST API with no middleware in between:

**MCP Server details:**

| Field | Value |
|---|---|
| Name | `IN261_Create_SC_ServiceCloud_MCP` |
| MCP Path | `/in261_create_sc_servicecloud_mcp` |
| Source | `URL` |

| Tool | Method | Path | Description |
|---|---|---|---|
| `get_sap_c4c_api_v1_employee_service_employees` | `GET` | `/sap/c4c/api/v1/employee-service/employees` | Looks up a Service Cloud employee using `$filter=displayId eq '{id}'` to retrieve their internal UUID |
| `post_sap_c4c_api_v1_case_service_cases` | `POST` | `/sap/c4c/api/v1/case-service/cases` | Creates a Support Case with subject, employee, reporter, system access details, and training details |

**Authentication:** Technical user via Client Credentials Grant Type — unlike Exercise 2, Principal Propagation is not used here. The MCP Gateway authenticates the agent call via IAS OAuth, then the backend call uses a pre-configured technical user credential.

![Service Cloud MCP Authentication](images/ex5-2.png)

</details>

<details>
<summary><strong>📖 How This MCP Fits in the New Hire Employee Onboarding Flow</strong></summary>
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
    └── MCP Tool: get_sap_c4c_api_v1_employee_service_employees    (Exercise 5 — this exercise)
        MCP Tool: post_sap_c4c_api_v1_case_service_cases           (Exercise 5 — this exercise)
```

The agent calls `get_sap_c4c_api_v1_employee_service_employees` first to resolve the logged-in user's identity, then calls `post_sap_c4c_api_v1_case_service_cases` to raise a Support Case covering all role-specific system access and training items that require manual provisioning.

The response includes a case ID — the agent constructs a direct link to the case and presents it in the onboarding summary.

</details>

## Summary

The Service Cloud Case Creation MCP Server has been pre-deployed and is ready to use. It will appear alongside your other MCP Servers when you publish the product in Develop Hub in Exercise 6.

Continue to - [Exercise 6 - Publish MCP Servers as a Product and Subscribe via Developer Hub](../ex6/README.md)
