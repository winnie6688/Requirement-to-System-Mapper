# Example: Terminology Workflow

## 1. User input

```text
我看到有的网页顶部可以切换不同页面，有的顶部只是起到标题作用，这个怎么专业表达？
```

---

## 2. Expected route

```text
Orchestrator Agent
-> Terminology Agent
-> Final response
```

---

## 3. Orchestrator Agent expected output

```json
{
  "task_type": "terminology_only",
  "reason": "The user is asking for professional UI / frontend terminology. The request does not involve external systems, APIs, CLI, schema, permissions, or credentials.",
  "detected_triggers": [
    "网页顶部",
    "切换不同页面",
    "标题作用",
    "专业表达"
  ],
  "next_agents": [
    "terminology_agent"
  ],
  "missing_information": [],
  "risk_flags": []
}
```

---

## 4. Terminology Agent expected output

```json
{
  "original_requirement": "用户想区分网页顶部区域的不同用途：一种顶部区域用于切换页面或内容，另一种顶部区域只是显示当前页面标题。",
  "professional_terms": [
    {
      "term_cn": "Tab 页导航 / Tab 切换",
      "term_en": "Tab Navigation / Tab Switching",
      "category": "frontend",
      "explanation": "用于在同一页面内切换不同内容面板，通常表现为一组并列的标签按钮。",
      "when_to_use": "当顶部区域用于切换同一页面内的不同内容区时使用。"
    },
    {
      "term_cn": "导航栏",
      "term_en": "Navigation Bar",
      "category": "frontend",
      "explanation": "用于在不同页面、模块或路由之间跳转的导航区域。",
      "when_to_use": "当顶部区域用于跳转到不同页面或模块时使用。"
    },
    {
      "term_cn": "页面标题区",
      "term_en": "Page Header / Header Title",
      "category": "ux",
      "explanation": "用于展示当前页面的标题、说明或页面操作，不一定承担切换功能。",
      "when_to_use": "当顶部区域只用于显示当前页面名称或说明时使用。"
    }
  ],
  "confusing_concepts": [
    {
      "concept_a": "Tab Navigation",
      "concept_b": "Navigation Bar",
      "difference": "Tab Navigation 通常在同一页面内切换内容；Navigation Bar 通常用于页面或模块跳转。"
    },
    {
      "concept_a": "Navigation Bar",
      "concept_b": "Page Header",
      "difference": "Navigation Bar 强调导航和跳转；Page Header 强调页面标题和当前页面信息展示。"
    }
  ],
  "recommended_expression": "I want to design a top Tab Navigation component for switching between different content panels. If the top area is only used to display the current page title, it should be implemented as a Page Header instead of a Navigation Bar.",
  "involves_external_system": false,
  "external_system_signals": [],
  "handoff_recommendation": "final_answer"
}
```

---

## 5. Final response example

# Professional Terminology Result

## 1. Original requirement restatement

你想区分两种网页顶部区域：一种用于切换页面或内容，另一种只是显示页面标题。

## 2. More professional expression

- 如果顶部区域用于在不同内容之间切换，可以叫 **Tab Navigation** 或 **Tab Switching**。
- 如果顶部区域用于跳转到不同页面，可以叫 **Navigation Bar**。
- 如果顶部区域只是显示页面名称或说明，可以叫 **Page Header** 或 **Header Title**。

## 3. Relevant terms

| Term | English | Meaning | When to use |
|---|---|---|---|
| Tab 页导航 | Tab Navigation | 在同一页面内切换不同内容面板 | 内容切换 |
| 导航栏 | Navigation Bar | 在不同页面或模块之间跳转 | 页面跳转 |
| 页面标题区 | Page Header | 展示当前页面标题或说明 | 页面信息展示 |

## 4. Easily confused concepts

- **Tab Navigation** 更偏“同一页面内切换内容”。
- **Navigation Bar** 更偏“不同页面之间跳转”。
- **Page Header** 更偏“显示当前页面标题”。

## 5. Recommended expression for AI Coding tools

```text
I want to design a top Tab Navigation component for switching between different content panels. If the top area is only used to display the current page title, implement it as a Page Header instead of a Navigation Bar.
```
