# Link Validation｜链接验证规则

## 1. 文档目的

本文件定义 Requirement-to-System Mapper 在处理 URL、官网链接、API 文档链接、CLI 文档链接和其他资料来源链接时的验证规则。

核心原则：

> 系统生成或推荐的任何 URL，在验证前都只能视为候选链接。只有确认可访问、非 404、域名匹配、内容相关后，才能升级为信源入口。

这条规则用于降低 AI URL 幻觉风险，避免 AI 根据品牌名、产品名或常见路径模式生成一个“看起来合理但实际不存在”的链接。

---

## 2. 为什么需要链接验证

大模型在生成 URL 时，可能不是基于真实访问结果，而是基于语言模式推断。

例如：

- 产品名是 Coze；
- 用户问“扣子编程官网”；
- 模型可能根据常见路径生成 `https://www.coze.cn/program`；
- 但真实入口可能是另一个子域名或路径。

因此，URL 不能因为“看起来像官网”就被视为真实信源。

---

## 3. 链接证据分层

Requirement-to-System Mapper 使用以下证据层级：

```text
Candidate URL → Link Validation → Source Seed → Interface Evidence → Runtime Evidence
```

| 层级 | 含义 | 是否可作为事实依据 |
|---|---|---|
| Candidate URL | 用户或 AI 提供的候选链接 | 不可作为事实依据 |
| Link Validation | 验证链接是否可访问、非 404、域名匹配、内容相关 | 可判断链接是否能升级 |
| Source Seed | 通过验证的官方入口或可信资料入口 | 只能作为信源入口 |
| Interface Evidence | 具体接口页、参数、字段、权限、返回值说明 | 可作为接口级证据 |
| Runtime Evidence | 在测试环境或最小请求中验证通过 | 可作为运行级证据 |

重要规则：

> Source Seed 不等于 Interface Evidence。Interface Evidence 不等于 Runtime Evidence。

---

## 4. 链接验证检查项

官方资料检索器在输出任何链接前，应尽量完成以下检查：

| 验证项 | 作用 |
|---|---|
| 不凭记忆生成 URL | 避免编造路径 |
| 必须访问验证 | 确认链接真实存在 |
| 排除 404 / 403 / 跳转异常 | 避免无效链接或权限受限链接 |
| 输出最终 URL | 防止中间跳转误导 |
| 输出页面标题 | 判断是否对应目标页面 |
| 说明匹配理由 | 避免“链接能打开但不相关” |
| 无法验证则标注未验证 | 防止假确定性 |

---

## 5. 链接状态分类

| 状态 | 含义 | 使用规则 |
|---|---|---|
| validated | 已验证可访问，且内容与需求相关 | 可以升级为 Source Seed |
| redirected | 发生跳转，需记录最终 URL | 只有最终页面匹配时才可使用 |
| inaccessible | 当前工具无法访问 | 不能作为已验证信源 |
| not_found | 返回 404 或页面不存在 | 不应继续使用 |
| forbidden_or_login_required | 403、登录页或权限受限 | 可作为候选入口，但不能作为已验证内容 |
| content_mismatch | 链接可访问，但内容与需求不匹配 | 不应作为目标信源 |
| unverified | 未完成验证 | 只能作为候选链接 |

---

## 6. URL 输出标准

当系统输出链接时，建议采用以下结构：

```json
{
  "candidate_url": "",
  "validation_status": "validated | redirected | inaccessible | not_found | forbidden_or_login_required | content_mismatch | unverified",
  "final_url": "",
  "page_title": "",
  "domain_match": true,
  "content_relevance": "",
  "evidence_level": "candidate_url | source_seed | interface_evidence | runtime_evidence",
  "notes": ""
}
```

---

## 7. 与官方资料检索器的关系

Link Validation 是官方资料检索器的基础规则。

官方资料检索器不得直接把一个未经验证的 URL 当作官方证据。它必须先判断该 URL 是否只是 Candidate URL。

只有当链接满足以下条件时，才可升级为 Source Seed：

1. 链接可以成功打开或解析；
2. 链接不是 404；
3. 域名与目标官方系统匹配；
4. 页面标题或内容与用户需求相关；
5. 若发生跳转，最终 URL 仍然可信且相关。

---

## 8. 与 source-map 的关系

`docs/source-map.md` 中记录的是官方资料入口。

这些入口通常属于 Source Seed，而不是具体接口证据。

例如：

- 飞书开放平台 API 总表是飞书 API 的官方入口；
- 微信公众号 API 文档首页是微信接口资料入口；
- Feishu CLI 官网是 CLI 资料入口。

但是，只有继续读取到具体接口页、命令页、参数说明、权限说明或返回值说明后，才能形成 Interface Evidence。

---

## 9. 高风险场景

以下场景必须更加严格验证 URL：

- 官网链接；
- API 文档链接；
- 登录入口；
- OAuth 授权入口；
- 下载链接；
- CLI 安装链接；
- SDK 文档链接；
- 涉及支付、发布、群发、删除、生产环境配置的链接。

这些链接不能凭品牌名和路径模式生成。

---

## 10. 标准提示词片段

当需要 AI 查找或输出链接时，可使用以下约束：

```text
请不要凭记忆或路径模式生成 URL。
在给出任何官网、文档、API 页面、产品页面链接前，必须先搜索或访问验证。
请排除 404、403、跳转异常和内容不匹配的链接。
输出最终可访问 URL、页面标题、验证状态，以及为什么该页面对应我的需求。
如果无法验证，请明确标注“未验证”，不要给确定结论。
```

---

## 11. 一句话总结

任何 URL 在验证前都只是 Candidate URL；只有经过可访问性、域名匹配和内容相关性验证后，才能升级为 Source Seed；Source Seed 仍不等于具体接口证据或运行结果。
