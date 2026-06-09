# Example: Feishu CLI Validation Workflow

## 1. User input

```text
飞书 CLI 是否支持帮我创建多维表格字段？
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
  "reason": "The user asks whether Feishu CLI supports a specific Bitable field creation capability. CLI capability verification is required.",
  "detected_triggers": [
    "飞书 CLI",
    "是否支持",
    "创建",
    "多维表格字段"
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
      "system_name": "Feishu CLI / lark-cli / @larksuite/cli",
      "system_type": "feishu_cli",
      "role": "Command-line tool that may or may not expose Bitable field creation capability.",
      "needs_external_verification": true
    },
    {
      "system_name": "Feishu Open Platform API",
      "system_type": "feishu_api",
      "role": "Possible fallback capability provider if CLI does not support the requested operation.",
      "needs_external_verification": true
    }
  ],
  "business_actions": [
    {
      "action_name": "create_bitable_field",
      "description": "Create a new field in a Feishu Bitable table.",
      "possible_systems": [
        "Feishu CLI",
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
      "possible_meaning": "Identifier of the target table.",
      "needs_verification": true
    },
    {
      "object_name": "field_name",
      "possible_meaning": "Name of the new field to create.",
      "needs_verification": true
    },
    {
      "object_name": "field_type",
      "possible_meaning": "Type of the new field, such as text, number, date, single select, etc.",
      "needs_verification": true
    }
  ],
  "data_flow": [
    {
      "from": "User or automation script",
      "to": "Feishu CLI",
      "data": "field creation command and parameters",
      "trigger": "User wants to create a Bitable field through CLI"
    },
    {
      "from": "Feishu CLI",
      "to": "Feishu platform or API layer",
      "data": "field creation request",
      "trigger": "CLI command execution if supported"
    }
  ],
  "possible_api_or_cli_capabilities": [
    {
      "system": "Feishu CLI",
      "capability": "Create Bitable field via CLI command",
      "reason": "The user specifically asks whether CLI supports field creation.",
      "status": "needs_verification"
    },
    {
      "system": "Feishu Open Platform API",
      "capability": "Create Bitable field via API",
      "reason": "If CLI does not expose the operation, API may be a fallback path.",
      "status": "needs_verification"
    }
  ],
  "possible_fields_or_ids": [
    {
      "name": "app_token",
      "type": "app_token",
      "description": "Possible base/app identifier required for field operation.",
      "needs_verification": true
    },
    {
      "name": "table_id",
      "type": "table_id",
      "description": "Possible target table identifier required for field operation.",
      "needs_verification": true
    }
  ],
  "possible_permissions": [
    {
      "permission_name": "Bitable field management permission",
      "reason": "Creating a field is a write-level schema operation and may require special permission.",
      "needs_verification": true
    }
  ],
  "verification_questions": [
    {
      "question": "Does Feishu CLI / lark-cli / @larksuite/cli officially support creating Bitable fields?",
      "why_verify": "The user asks about CLI capability specifically, which must not be inferred from API capability.",
      "domain": "feishu_cli"
    },
    {
      "question": "If CLI does not support creating Bitable fields, does Feishu Open Platform API support it?",
      "why_verify": "The system may need to propose API as an alternative path, but that must also be verified.",
      "domain": "feishu_api"
    },
    {
      "question": "What permissions and identifiers are required to create Bitable fields?",
      "why_verify": "Field creation may require table identifiers, field type parameters, and write permissions.",
      "domain": "feishu_api"
    }
  ],
  "high_risk_candidates": [
    "Do not assume CLI supports all Feishu API capabilities.",
    "Creating fields changes table schema and should be tested carefully."
  ]
}
```

---

## 5. Documentation Research Agent expected output

> Note: This is a structure sample. In a real run, current official CLI repository or documentation must be checked.

```json
{
  "research_scope": [
    "feishu_cli",
    "feishu_api"
  ],
  "evidence_results": [
    {
      "question": "Does Feishu CLI / lark-cli / @larksuite/cli officially support creating Bitable fields?",
      "domain": "feishu_cli",
      "conclusion": "not_found",
      "official_source": "TO_BE_FILLED_WITH_CURRENT_OFFICIAL_CLI_SOURCE",
      "source_link": "TO_BE_FILLED_WITH_CURRENT_OFFICIAL_LINK",
      "evidence_summary": "This example does not include a live check of the official CLI command list. CLI support must not be assumed.",
      "is_sufficient": false,
      "remaining_uncertainties": [
        "Current official CLI command list needs to be checked."
      ]
    },
    {
      "question": "If CLI does not support creating Bitable fields, does Feishu Open Platform API support it?",
      "domain": "feishu_api",
      "conclusion": "not_found",
      "official_source": "TO_BE_FILLED_WITH_CURRENT_OFFICIAL_API_SOURCE",
      "source_link": "TO_BE_FILLED_WITH_CURRENT_OFFICIAL_LINK",
      "evidence_summary": "This example does not include live official API verification. API fallback must be verified separately.",
      "is_sufficient": false,
      "remaining_uncertainties": [
        "Current official API field management documentation needs to be checked."
      ]
    }
  ],
  "conflicts_or_gaps": [
    "CLI support and API support must be treated as separate questions."
  ],
  "research_limitations": [
    "This file is a test example, not final official evidence."
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
      "assumption": "Feishu Open Platform API may provide more complete Bitable schema management capability than CLI.",
      "why_reasonable": "Open Platform APIs usually expose broader platform capabilities than CLI tools, but this must be verified.",
      "what_to_verify": "Official Feishu API documentation for Bitable field creation or field management."
    }
  ],
  "pending_items": [
    {
      "item": "Official CLI command support is unknown.",
      "why_important": "The user specifically asks about CLI, so API capability cannot be used as proof of CLI capability.",
      "how_to_confirm": "Check official CLI README, command reference, package documentation, or command help output."
    },
    {
      "item": "Official API fallback support is unknown.",
      "why_important": "If CLI does not support field creation, API may be the alternative, but it must be verified. ",
      "how_to_confirm": "Check current Feishu / Lark Open Platform Bitable field management documentation."
    }
  ],
  "high_risk_assumptions": [
    {
      "assumption": "CLI supports all capabilities exposed by Feishu Open Platform API.",
      "risk": "This may cause implementation failure or wrong tool selection.",
      "mitigation": "Verify CLI command list separately from API documentation."
    },
    {
      "assumption": "Creating a field is a low-risk action.",
      "risk": "Creating fields changes table schema and may affect downstream automations or data mapping.",
      "mitigation": "Use a test table before changing real table schema."
    }
  ],
  "invalid_assumptions": [
    {
      "assumption": "Feishu CLI definitely supports Bitable field creation.",
      "why_invalid": "No official CLI evidence is provided in this example.",
      "replacement": "Treat CLI support as pending until official command documentation confirms it."
    }
  ],
  "minimum_validation_steps": [
    {
      "step": "Check official Feishu CLI / lark-cli command list or README.",
      "purpose": "Confirm whether field creation is exposed as a CLI command.",
      "priority": "P0"
    },
    {
      "step": "Check official Feishu Open Platform Bitable field management documentation.",
      "purpose": "Confirm whether API fallback exists and what parameters are required.",
      "priority": "P0"
    },
    {
      "step": "Use a test Bitable table before creating fields in any important table.",
      "purpose": "Avoid schema pollution or downstream mapping issues.",
      "priority": "P1"
    }
  ],
  "recommendation_for_orchestrator": "State clearly that CLI support and API support are different. Do not claim CLI support without official CLI evidence."
}
```

---

## 7. Final response example

# Verified Engineering Requirement Spec

## 1. Original requirement restatement

用户想确认飞书 CLI 是否支持创建多维表格字段。

## 2. Task classification

This is an `integration_validation` task because it asks about a specific CLI capability.

## 3. Key judgment

当前不能直接假设飞书 CLI 支持创建多维表格字段。CLI 能力和开放平台 API 能力必须分开验证。

## 4. Pending items

| Pending item | Why important | How to confirm |
|---|---|---|
| CLI command support unknown | CLI support cannot be inferred from API support | Check official CLI README or command list |
| API fallback support unknown | API may be an alternative path | Check official Feishu Bitable field management documentation |
| Permission requirement unknown | Field creation modifies schema | Check official permission requirements |

## 5. Minimum validation steps

1. Check official Feishu CLI command list.
2. Check official Feishu API field management documentation.
3. Use a test Bitable table if field creation is supported.

## 6. Recommended wording

```text
Do not assume Feishu CLI supports all Feishu Open Platform API capabilities. First verify whether the CLI exposes a Bitable field creation command. If not, verify whether the Open Platform API supports field creation and use API-based implementation instead.
```
