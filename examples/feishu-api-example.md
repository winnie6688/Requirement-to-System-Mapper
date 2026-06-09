# Example: Feishu API Validation Workflow

## 1. User input

```text
我想知道飞书 API 是否支持获取多维表格字段结构。
```

---

## 2. Expected route

```text
Orchestrator Agent
-> Integration Mapper Agent
-> Documentation Research Agent
-> Verification Reviewer Agent
-> Final response
```

---

## 3. Orchestrator Agent expected output

```json
{
  "task_type": "integration_validation",
  "reason": "The user asks whether Feishu API supports a specific Bitable schema capability, so official capability verification is required.",
  "detected_triggers": [
    "飞书 API",
    "是否支持",
    "多维表格",
    "字段结构"
  ],
  "next_agents": [
    "integration_mapper_agent",
    "documentation_research_agent",
    "verification_reviewer_agent"
  ],
  "missing_information": [],
  "risk_flags": []
}
```

---

## 4. Integration Mapper Agent expected output

```json
{
  "involved_systems": [
    {
      "system_name": "Feishu Bitable",
      "system_type": "feishu_api",
      "role": "The external data table system whose field schema needs to be retrieved.",
      "needs_external_verification": true
    }
  ],
  "business_actions": [
    {
      "action_name": "retrieve_field_schema",
      "description": "Retrieve field metadata or field structure from a Feishu Bitable table.",
      "possible_systems": [
        "Feishu Open Platform API"
      ]
    }
  ],
  "data_objects": [
    {
      "object_name": "app_token",
      "possible_meaning": "Identifier of the Feishu Bitable app/base.",
      "needs_verification": true
    },
    {
      "object_name": "table_id",
      "possible_meaning": "Identifier of the specific table whose fields need to be retrieved.",
      "needs_verification": true
    },
    {
      "object_name": "field_id",
      "possible_meaning": "Stable field identifier used by API calls.",
      "needs_verification": true
    }
  ],
  "data_flow": [
    {
      "from": "Internal backend or tool",
      "to": "Feishu Open Platform API",
      "data": "app_token and table_id",
      "trigger": "User requests field schema retrieval"
    },
    {
      "from": "Feishu Open Platform API",
      "to": "Internal backend or tool",
      "data": "field metadata or field list response",
      "trigger": "API response"
    }
  ],
  "possible_api_or_cli_capabilities": [
    {
      "system": "Feishu Open Platform API",
      "capability": "Retrieve Bitable field metadata or field list",
      "reason": "The user needs to know whether field structure can be obtained programmatically.",
      "status": "needs_verification"
    }
  ],
  "possible_fields_or_ids": [
    {
      "name": "app_token",
      "type": "app_token",
      "description": "Possible base/app identifier required in Feishu Bitable API path.",
      "needs_verification": true
    },
    {
      "name": "table_id",
      "type": "table_id",
      "description": "Possible table identifier required in Feishu Bitable API path.",
      "needs_verification": true
    }
  ],
  "possible_permissions": [
    {
      "permission_name": "Bitable read or metadata permission",
      "reason": "Retrieving field schema may require read or metadata-related permission.",
      "needs_verification": true
    }
  ],
  "verification_questions": [
    {
      "question": "Does Feishu Open Platform API support retrieving Bitable field schema or field list?",
      "why_verify": "The system must confirm whether the capability exists before recommending it.",
      "domain": "feishu_api"
    },
    {
      "question": "Which identifiers are required to retrieve Feishu Bitable field metadata?",
      "why_verify": "The system must confirm whether app_token, table_id, or other identifiers are required.",
      "domain": "feishu_api"
    },
    {
      "question": "Which permission or credential type is required for retrieving Feishu Bitable field metadata?",
      "why_verify": "Permission mismatch can cause implementation failure.",
      "domain": "feishu_api"
    }
  ],
  "high_risk_candidates": []
}
```

---

## 5. Documentation Research Agent expected output

> Note: This example is a structure sample. When used in a real run, the Documentation Research Agent must verify current official Feishu documentation and include actual official links.

```json
{
  "research_scope": [
    "feishu_api"
  ],
  "evidence_results": [
    {
      "question": "Does Feishu Open Platform API support retrieving Bitable field schema or field list?",
      "domain": "feishu_api",
      "conclusion": "supported",
      "official_source": "Feishu / Lark Open Platform documentation",
      "source_link": "TO_BE_FILLED_WITH_CURRENT_OFFICIAL_LINK",
      "evidence_summary": "Official documentation should be checked for a Bitable field list or field metadata API. If present, it can support field schema retrieval.",
      "is_sufficient": false,
      "remaining_uncertainties": [
        "The exact endpoint, method, permission scope, and response format must be filled from current official documentation."
      ]
    }
  ],
  "conflicts_or_gaps": [
    "This example does not include a live official documentation lookup."
  ],
  "research_limitations": [
    "Official link is intentionally left as a placeholder in this example file."
  ]
}
```

---

## 6. Verification Reviewer Agent expected output

```json
{
  "overall_judgment": "requires_minimum_validation",
  "verified_facts": [],
  "reasonable_assumptions": [
    {
      "assumption": "Feishu Open Platform likely provides Bitable metadata or field-related APIs.",
      "why_reasonable": "Bitable APIs commonly expose records, tables, and field metadata capabilities, but this must be checked against current official documentation.",
      "what_to_verify": "Exact official endpoint, method, permission, credential type, and response fields."
    }
  ],
  "pending_items": [
    {
      "item": "Current official API endpoint and method are not filled in this example.",
      "why_important": "Without exact endpoint and method, the result cannot be used as implementation-ready guidance.",
      "how_to_confirm": "Check current Feishu / Lark Open Platform official Bitable field documentation."
    },
    {
      "item": "Current application permission is unknown.",
      "why_important": "Missing permission can cause request failure even if the API exists.",
      "how_to_confirm": "Check the app permission configuration in Feishu / Lark developer console."
    }
  ],
  "high_risk_assumptions": [],
  "invalid_assumptions": [
    {
      "assumption": "The API can be used immediately without checking current official documentation.",
      "why_invalid": "This example does not include live official evidence or exact permission details.",
      "replacement": "Treat this as a validation workflow example and verify official documentation before implementation."
    }
  ],
  "minimum_validation_steps": [
    {
      "step": "Check current official Feishu / Lark Open Platform documentation for Bitable field list or field metadata APIs.",
      "purpose": "Confirm the API exists and obtain endpoint, method, and response format.",
      "priority": "P0"
    },
    {
      "step": "Confirm the required app permission and credential type.",
      "purpose": "Avoid authorization failure during implementation.",
      "priority": "P0"
    },
    {
      "step": "Use a test Bitable base and table to retrieve field metadata.",
      "purpose": "Verify real response structure before building field mapping logic.",
      "priority": "P1"
    }
  ],
  "recommendation_for_orchestrator": "Explain that this is an external capability validation task. Do not present implementation details as verified until current official documentation and a minimum test confirm them."
}
```

---

## 7. Final response example

# Verified Engineering Requirement Spec

## 1. Original requirement restatement

用户想确认飞书开放平台 API 是否支持获取多维表格字段结构，也就是是否可以通过接口拿到某张多维表格的字段列表、字段 ID、字段类型等元数据。

## 2. Task classification

This is an `integration_validation` task because it involves Feishu API capability verification.

## 3. System capability mapping

| Dimension | Content |
|---|---|
| External system | Feishu Bitable / Feishu Open Platform API |
| Business action | Retrieve field schema or field metadata |
| Possible identifiers | app_token, table_id, field_id |
| Key risk | Do not assume field display name equals field ID |

## 4. Verification questions

| Question | Why verify | Domain |
|---|---|---|
| Does Feishu API support retrieving Bitable field metadata? | To confirm capability exists | feishu_api |
| Which identifiers are required? | To construct valid request path or parameters | feishu_api |
| What permission is required? | To avoid authorization failure | feishu_api |

## 5. Verification review conclusion

This example should not be treated as final official verification because the official documentation link is not filled in this sample. Before implementation, check current Feishu / Lark Open Platform documentation and run a minimum test with a test table.

## 6. Minimum validation steps

1. Check official Feishu / Lark Open Platform Bitable field documentation.
2. Confirm endpoint, method, request parameters, response fields, and required permissions.
3. Use a test Bitable table to retrieve field metadata.
4. Save the real response sample for later field mapping.
