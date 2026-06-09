# Example: Out-of-Scope Platform Workflow

## 1. User input

```text
Notion API 能不能实现同样的字段同步？
```

---

## 2. Expected route

```text
Orchestrator Agent
-> Final response with boundary clarification
```

Depending on product settings, the Orchestrator may optionally call the Terminology Agent for general explanation, but it must not claim official validation for Notion API in the current MVP.

---

## 3. Orchestrator Agent expected output

```json
{
  "task_type": "unclear",
  "reason": "The user asks about Notion API, but the current official research scope of Requirement-to-System Mapper is limited to Feishu API, WeChat Official Account API, and Feishu CLI. The system can provide general conceptual guidance but should not provide an official validation conclusion.",
  "detected_triggers": [
    "Notion API",
    "字段同步"
  ],
  "next_agents": [],
  "missing_information": [
    "Which Notion database or field schema should be synchronized?",
    "What is the source system and target system?",
    "Is official Notion API validation required?"
  ],
  "risk_flags": []
}
```

---

## 4. Expected system behavior

The system should clearly state:

> The current official research scope of Requirement-to-System Mapper is limited to Feishu API, WeChat Official Account API, and Feishu CLI. Notion API can be treated as a future extension, but this version should not provide an official validation conclusion.

It may provide general integration thinking:

- Field synchronization usually involves source fields, target fields, field mapping, identifiers, permissions, and update strategy.
- Any Notion-specific conclusion must be checked against current official Notion API documentation before implementation.
- If Notion is added to the product scope later, it should become a new documentation domain.

---

## 5. Final response example

# Scope Boundary Response

当前 Requirement-to-System Mapper 的官方资料检索范围暂时限定为：

1. 飞书开放平台 API；
2. 微信公众号 API；
3. 飞书 CLI / lark-cli / @larksuite/cli。

所以，对于 **Notion API 是否支持字段同步**，当前版本不能给出官方核验结论。

但从通用系统集成角度看，字段同步通常需要确认：

| Dimension | What to verify |
|---|---|
| Source system | 数据从哪里来 |
| Target system | 数据写到哪里去 |
| Field mapping | 源字段和目标字段如何对应 |
| Identifier | 用什么 ID 识别同一条记录 |
| Permission | 是否具备读取和写入权限 |
| Update strategy | 是新增、覆盖、增量更新还是双向同步 |
| Conflict handling | 两边同时修改时如何处理 |

如果未来要把 Notion API 纳入 Requirement-to-System Mapper，建议新增一个资料域：

```text
notion_api
```

并新增对应的官方资料检索规则、权限核验规则和示例案例。

---

## 6. Expected review principle

Do not say:

```text
Notion API definitely supports this workflow.
```

Say instead:

```text
This is outside the current official research scope. It can be analyzed conceptually, but official Notion API validation is required before implementation.
```
