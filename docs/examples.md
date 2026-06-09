# Examples

This file collects typical use cases for Requirement-to-System Mapper.

---

## Example 1: Terminology conversion

### User input

```text
我看到有的网页顶部可以切换不同页面，有的顶部只是起到标题作用，这个怎么专业表达？
```

### Expected route

```text
Orchestrator -> Terminology Agent
```

### Expected task type

```json
{
  "task_type": "terminology_only",
  "reason": "The user asks for professional UI / frontend terminology and does not involve external API or permissions.",
  "next_agents": ["terminology_agent"]
}
```

### Expected output summary

- Tab Navigation / Tab Switching: used for switching between panels or sections.
- Navigation Bar: used for navigating between pages or modules.
- Page Header / Header Title: used to display the current page title, not to switch content.
- Recommended expression for AI Coding tools:

```text
I want to design a top Tab Navigation component for switching between different content panels. If the top area only displays the current page title, use a Page Header instead of a Navigation Bar.
```

---

## Example 2: Feishu API capability check

### User input

```text
我想知道飞书 API 是否支持获取多维表格字段结构。
```

### Expected route

```text
Orchestrator -> Integration Mapper Agent -> Documentation Research Agent -> Verification Reviewer Agent
```

### Expected task type

```json
{
  "task_type": "integration_validation",
  "reason": "The user asks whether Feishu API supports a specific capability related to Bitable field schema.",
  "next_agents": [
    "integration_mapper_agent",
    "documentation_research_agent",
    "verification_reviewer_agent"
  ]
}
```

### Expected verification questions

- Does Feishu Open Platform API support retrieving Bitable field metadata?
- Which endpoint and HTTP method are used?
- What token and permission are required?
- What does the response structure look like?

---

## Example 3: Feishu to WeChat Official Account workflow

### User input

```text
我想让飞书多维表格新增一条文章记录后，把标题、正文、封面传给后端，后端根据阶段判断是否上传到微信公众号素材库。
```

### Expected route

```text
Orchestrator -> Terminology Agent -> Integration Mapper Agent -> Documentation Research Agent -> Verification Reviewer Agent -> Final synthesis
```

### Expected involved systems

- Feishu Bitable;
- internal backend service;
- WeChat Official Account API;
- possible file storage service;
- optional automation trigger or webhook.

### Expected data objects

- app_token;
- table_id;
- record_id;
- field_id;
- title;
- body;
- cover image;
- media_id;
- status field;
- access_token.

### Expected risks

- Do not assume Feishu record creation automatically triggers backend without a configured trigger.
- Do not assume field display names can be used as API field identifiers.
- Do not assume uploading an image is the same as setting a WeChat article cover.
- Do not assume material upload is the same as publishing.
- Do not use production credentials for testing.

---

## Example 4: Feishu CLI capability check

### User input

```text
飞书 CLI 是否支持帮我创建多维表格字段？
```

### Expected route

```text
Orchestrator -> Integration Mapper Agent -> Documentation Research Agent -> Verification Reviewer Agent
```

### Expected review logic

- Distinguish Feishu Open Platform API support from Feishu CLI support.
- If official CLI docs do not explicitly support the command, do not claim it is supported.
- If API supports a capability but CLI does not clearly expose it, mark it as partially supported or pending.
- Recommend checking official CLI README, command list, or using API directly.

---

## Example 5: Out-of-scope platform

### User input

```text
Notion API 能不能实现同样的字段同步？
```

### Expected behavior

The system may explain the general integration concept, but must state:

> The current official research scope of Requirement-to-System Mapper is limited to Feishu API, WeChat Official Account API, and Feishu CLI. Notion API can be considered a future extension, but this version should not provide an official validation conclusion.
