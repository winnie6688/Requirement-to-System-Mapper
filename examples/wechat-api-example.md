# Example: WeChat Official Account API Validation Workflow

## 1. User input

```text
我想把一张封面图上传到微信公众号，用于后续创建图文草稿，这个应该怎么设计？
```

---

## 2. Expected route

```text
Orchestrator Agent
-> Terminology Agent
-> Integration Mapper Agent
-> Documentation Research Agent
-> Verification Reviewer Agent
-> Final response
```

---

## 3. Orchestrator Agent expected output

```json
{
  "task_type": "mixed",
  "reason": "The user asks for professional design guidance and also involves WeChat Official Account API capability validation around cover image, material upload, and draft creation.",
  "detected_triggers": [
    "封面图",
    "上传",
    "微信公众号",
    "图文草稿",
    "怎么设计"
  ],
  "next_agents": [
    "terminology_agent",
    "integration_mapper_agent",
    "documentation_research_agent",
    "verification_reviewer_agent"
  ],
  "missing_information": [],
  "risk_flags": [
    "publishing"
  ]
}
```

---

## 4. Terminology Agent expected output

```json
{
  "original_requirement": "用户想将本地或外部封面图片上传到微信公众号平台，并在后续创建图文草稿时使用该图片。",
  "professional_terms": [
    {
      "term_cn": "素材上传",
      "term_en": "Material Upload",
      "category": "api",
      "explanation": "将图片、音频、视频等媒体文件上传到平台素材库，获得后续接口可引用的素材标识。",
      "when_to_use": "当外部文件需要被微信公众号接口引用时使用。"
    },
    {
      "term_cn": "图文草稿",
      "term_en": "Article Draft",
      "category": "product",
      "explanation": "微信公众号平台中用于编辑和准备发布的图文内容对象。",
      "when_to_use": "当内容还未正式发布，只处于编辑或待确认状态时使用。"
    },
    {
      "term_cn": "封面图",
      "term_en": "Cover Image / Thumb Media",
      "category": "product",
      "explanation": "图文内容展示时使用的封面图片，可能需要先上传为平台可识别的素材。",
      "when_to_use": "当图文内容需要指定展示封面时使用。"
    }
  ],
  "confusing_concepts": [
    {
      "concept_a": "Material Upload",
      "concept_b": "Draft Creation",
      "difference": "Material Upload 是把图片等文件上传到平台；Draft Creation 是创建图文草稿，两者是不同动作。"
    },
    {
      "concept_a": "Draft Creation",
      "concept_b": "Publishing",
      "difference": "Draft Creation 只是生成草稿；Publishing 是正式对外发布，属于更高风险动作。"
    }
  ],
  "recommended_expression": "I want to upload a cover image to the WeChat Official Account material library, obtain a platform media identifier, and then use it when creating an article draft. Draft creation and final publishing should be treated as separate actions.",
  "involves_external_system": true,
  "external_system_signals": [
    "微信公众号",
    "上传",
    "图文草稿"
  ],
  "handoff_recommendation": "handoff_to_integration_mapper"
}
```

---

## 5. Integration Mapper Agent expected output

```json
{
  "involved_systems": [
    {
      "system_name": "WeChat Official Account API",
      "system_type": "wechat_official_account_api",
      "role": "External platform for material upload and article draft creation.",
      "needs_external_verification": true
    },
    {
      "system_name": "Internal backend service",
      "system_type": "internal_backend",
      "role": "Receives image file or URL, calls platform API, stores returned identifiers, and coordinates draft creation.",
      "needs_external_verification": false
    }
  ],
  "business_actions": [
    {
      "action_name": "upload_cover_image_material",
      "description": "Upload a cover image to WeChat Official Account platform and obtain a media identifier.",
      "possible_systems": [
        "WeChat Official Account API",
        "Internal backend service"
      ]
    },
    {
      "action_name": "create_article_draft",
      "description": "Create a WeChat article draft using title, body, and uploaded cover image reference.",
      "possible_systems": [
        "WeChat Official Account API",
        "Internal backend service"
      ]
    }
  ],
  "data_objects": [
    {
      "object_name": "cover_image",
      "possible_meaning": "Image file or accessible image URL to be used as article cover.",
      "needs_verification": true
    },
    {
      "object_name": "media_id",
      "possible_meaning": "Platform-side media identifier returned after material upload.",
      "needs_verification": true
    },
    {
      "object_name": "draft_id",
      "possible_meaning": "Possible identifier for a created article draft.",
      "needs_verification": true
    }
  ],
  "data_flow": [
    {
      "from": "User or content system",
      "to": "Internal backend service",
      "data": "cover image, title, body",
      "trigger": "User wants to prepare a WeChat article draft"
    },
    {
      "from": "Internal backend service",
      "to": "WeChat Official Account API",
      "data": "cover image material upload request",
      "trigger": "Backend prepares cover asset"
    },
    {
      "from": "WeChat Official Account API",
      "to": "Internal backend service",
      "data": "media identifier or upload result",
      "trigger": "Material upload response"
    }
  ],
  "possible_api_or_cli_capabilities": [
    {
      "system": "WeChat Official Account API",
      "capability": "Upload image material",
      "reason": "The cover image must become a platform-recognized media object before being referenced by draft creation.",
      "status": "needs_verification"
    },
    {
      "system": "WeChat Official Account API",
      "capability": "Create article draft",
      "reason": "The user wants to use the uploaded cover image in a later draft creation step.",
      "status": "needs_verification"
    }
  ],
  "possible_fields_or_ids": [
    {
      "name": "media_id",
      "type": "media_id",
      "description": "Possible identifier for uploaded media material.",
      "needs_verification": true
    },
    {
      "name": "draft_id",
      "type": "draft_id",
      "description": "Possible identifier for created draft object.",
      "needs_verification": true
    }
  ],
  "possible_permissions": [
    {
      "permission_name": "WeChat Official Account material and draft API permissions",
      "reason": "Material upload and draft creation may require platform permissions and account capability configuration.",
      "needs_verification": true
    }
  ],
  "verification_questions": [
    {
      "question": "Does WeChat Official Account API support image material upload for later article draft use?",
      "why_verify": "The system must confirm whether the image can be uploaded and referenced later.",
      "domain": "wechat_official_account_api"
    },
    {
      "question": "Is material upload separate from article draft creation and publishing?",
      "why_verify": "The system must avoid mixing upload, draft, and publishing actions.",
      "domain": "wechat_official_account_api"
    },
    {
      "question": "What platform credential, permission, and account requirements are needed for material upload and draft creation?",
      "why_verify": "Missing permission or account capability can cause implementation failure.",
      "domain": "wechat_official_account_api"
    }
  ],
  "high_risk_candidates": [
    "Do not treat draft creation as publishing.",
    "Do not automatically publish content after draft creation."
  ]
}
```

---

## 6. Documentation Research Agent expected output

> Note: This example is a structure sample. In a real run, current WeChat official documentation links must be inserted.

```json
{
  "research_scope": [
    "wechat_official_account_api"
  ],
  "evidence_results": [
    {
      "question": "Does WeChat Official Account API support image material upload for later article draft use?",
      "domain": "wechat_official_account_api",
      "conclusion": "partially_supported",
      "official_source": "WeChat Official Account Platform documentation",
      "source_link": "TO_BE_FILLED_WITH_CURRENT_OFFICIAL_LINK",
      "evidence_summary": "Official documentation should be checked for material upload and draft APIs. This example does not provide a live verified endpoint.",
      "is_sufficient": false,
      "remaining_uncertainties": [
        "Exact endpoint, required media type, credential requirement, and response field need current official verification."
      ]
    },
    {
      "question": "Is material upload separate from article draft creation and publishing?",
      "domain": "wechat_official_account_api",
      "conclusion": "partially_supported",
      "official_source": "WeChat Official Account Platform documentation",
      "source_link": "TO_BE_FILLED_WITH_CURRENT_OFFICIAL_LINK",
      "evidence_summary": "The workflow should treat upload, draft creation, and publishing as separate capabilities unless official documentation proves otherwise.",
      "is_sufficient": false,
      "remaining_uncertainties": [
        "Current official API boundaries must be checked before implementation."
      ]
    }
  ],
  "conflicts_or_gaps": [
    "This example intentionally does not include current official links."
  ],
  "research_limitations": [
    "Use this file as a workflow example, not as final official evidence."
  ]
}
```

---

## 7. Verification Reviewer Agent expected output

```json
{
  "overall_judgment": "requires_minimum_validation",
  "verified_facts": [],
  "reasonable_assumptions": [
    {
      "assumption": "Cover image handling likely requires a material upload step before draft creation.",
      "why_reasonable": "External content platforms commonly require uploaded media identifiers before media can be referenced in draft objects.",
      "what_to_verify": "Current WeChat Official Account API documentation for material upload and draft creation."
    }
  ],
  "pending_items": [
    {
      "item": "Exact material upload API endpoint and response field are not verified in this example.",
      "why_important": "The backend cannot reliably implement upload logic without exact API contract.",
      "how_to_confirm": "Check current WeChat Official Account official API documentation."
    },
    {
      "item": "Account permissions and capability status are unknown.",
      "why_important": "Different WeChat account types or configurations may affect available API capabilities.",
      "how_to_confirm": "Check the target account's developer settings and official permission requirements."
    }
  ],
  "high_risk_assumptions": [
    {
      "assumption": "Draft creation means content is already published.",
      "risk": "Publishing is a separate high-impact action and may expose content publicly.",
      "mitigation": "Treat draft creation and publishing as separate steps. Publishing must require human confirmation."
    }
  ],
  "invalid_assumptions": [
    {
      "assumption": "Uploading an image file is automatically equivalent to setting it as article cover.",
      "why_invalid": "The uploaded media may need to be referenced by a specific field in draft creation. Exact relationship must be verified.",
      "replacement": "Verify material upload response and draft creation field requirements separately."
    }
  ],
  "minimum_validation_steps": [
    {
      "step": "Check current WeChat Official Account official material upload documentation.",
      "purpose": "Confirm upload endpoint, media type, request format, and response identifier.",
      "priority": "P0"
    },
    {
      "step": "Check current official draft creation documentation.",
      "purpose": "Confirm how uploaded media is referenced when creating a draft.",
      "priority": "P0"
    },
    {
      "step": "Use a test asset and test account to validate upload and draft creation separately.",
      "purpose": "Avoid confusing upload success with draft creation or publishing success.",
      "priority": "P1"
    }
  ],
  "recommendation_for_orchestrator": "Explain that cover upload, draft creation, and publishing must be treated as separate actions. Do not recommend automatic publishing."
}
```

---

## 8. Final response example

# Verified Engineering Requirement Spec

## 1. Original requirement restatement

用户希望将封面图上传到微信公众号平台，并在后续创建图文草稿时引用该封面图。

## 2. Professional terminology conversion

- 封面图上传：Cover Image Upload / Material Upload
- 图文草稿：Article Draft
- 素材标识：Media Identifier

## 3. Task classification

This is a `mixed` task because it requires both professional terminology conversion and WeChat API capability verification.

## 4. System capability mapping

| Dimension | Content |
|---|---|
| External system | WeChat Official Account API |
| Internal system | Backend service |
| Business actions | upload material, create draft, store returned media identifier |
| High-risk boundary | draft creation must not be confused with publishing |

## 5. Minimum validation steps

1. Check current official material upload documentation.
2. Check current official draft creation documentation.
3. Verify upload response identifier with a test image.
4. Confirm how the identifier is referenced in draft creation.
5. Keep publishing as a separate human-confirmed action.
