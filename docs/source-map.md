# Official Source Map

This file records the official seed sources used by Requirement-to-System Mapper for external capability verification.

The purpose of this file is to give the Documentation Research Agent a clear and bounded source list when verifying Feishu API, WeChat Official Account API, and Feishu CLI capabilities.

---

## 1. Source map overview

| Source domain | Official source | URL | Current role |
|---|---|---|---|
| Feishu CLI | Feishu CLI official documentation | https://feishu-cli.com/zh/#getting-started | Primary source for Feishu CLI / lark-cli / @larksuite/cli capability verification |
| Feishu Open Platform API | Feishu Open Platform server API list | https://open.feishu.cn/document/server-docs/api-call-guide/server-api-list | Primary entry point for Feishu Open Platform server API capability verification |
| WeChat Official Account API | WeChat Official Account subscription API documentation | https://developers.weixin.qq.com/doc/subscription/api/ | Primary entry point for WeChat Official Account / subscription API capability verification |

---

## 2. Feishu CLI source

### URL

```text
https://feishu-cli.com/zh/#getting-started
```

### Use this source for

- Feishu CLI installation and configuration;
- lark-cli command structure;
- AI Agent Skills;
- CLI-supported business domains;
- CLI authentication and identity switching;
- CLI safety features such as dry-run and structured output;
- differences between shortcut commands, API commands, and raw API access;
- whether a specific task can be done through CLI directly.

### Important notes

The Feishu CLI source describes Feishu CLI / Lark CLI as an official open-source command line tool from Feishu / Lark Open Platform. It states that the CLI is designed for AI Agents and developers, distributed through npm as `@larksuite/cli`, and exposes the binary command `lark-cli`.

It also describes a three-layer command system:

1. Shortcut commands;
2. API commands;
3. Raw API access.

For Requirement-to-System Mapper, this means:

> CLI support and API support must still be verified separately, but raw API access through CLI may provide a fallback path when shortcut commands or generated API commands do not cover a task.

### Current verification stance

- Treat this as the primary Feishu CLI seed source.
- For any CLI capability question, first check this documentation and its linked official GitHub / npm / command reference resources.
- Do not assume that a shortcut command exists just because the raw Feishu API exists.

---

## 3. Feishu Open Platform API source

### URL

```text
https://open.feishu.cn/document/server-docs/api-call-guide/server-api-list
```

### Use this source for

- Feishu Open Platform server API discovery;
- API capability existence checks;
- endpoint and method verification;
- API version verification;
- required request parameters;
- response structure;
- permission and credential requirements;
- Bitable / Base API capability discovery;
- record, field, table, view, and schema-related API verification.

### Important notes

This source should be treated as the official API entry point for Feishu server APIs.

For Requirement-to-System Mapper, the Documentation Research Agent should use this page as the starting point and then follow official links into specific API references.

### Current verification stance

- Treat this as the primary Feishu API seed source.
- Do not make a final claim only from the API list page when a specific endpoint page is needed.
- For implementation-ready output, verify the specific endpoint page, method, path, parameters, response fields, and permissions.

---

## 4. WeChat Official Account API source

### URL

```text
https://developers.weixin.qq.com/doc/subscription/api/
```

### Use this source for

- WeChat Official Account API discovery;
- subscription account API boundaries;
- access credential requirements;
- material upload capability;
- draft-related capability;
- publishing boundary checks;
- article/message-related API verification;
- account capability and permission requirements.

### Important notes

This source should be treated as the official entry point for WeChat Official Account / subscription account API documentation.

For Requirement-to-System Mapper, this is especially important when verifying whether a requirement involves:

- material upload;
- image upload;
- cover image usage;
- draft creation;
- publishing;
- message sending;
- official account permissions.

### Current verification stance

- Treat this as the primary WeChat Official Account API seed source.
- Cover image upload, material upload, draft creation, and publishing must be treated as separate capabilities unless official documentation proves otherwise.
- Publishing and mass messaging must always be treated as high-impact actions requiring human confirmation.

---

## 5. Documentation Research Agent usage rules

When the Documentation Research Agent receives a verification question, it should classify the question into one of these source domains:

```json
{
  "source_domain": "feishu_cli | feishu_api | wechat_official_account_api | out_of_scope"
}
```

Then it should:

1. Start from the corresponding seed source in this file.
2. Follow official documentation links to the most specific page.
3. Extract the exact endpoint, method, command, parameters, permissions, response fields, and limitations when available.
4. Mark the conclusion as one of:
   - `supported`
   - `unsupported`
   - `not_found`
   - `partially_supported`
5. Never upgrade a capability to `supported` without source evidence.
6. Never treat `not_found` as `unsupported`.
7. Distinguish CLI capability from API capability.
8. Distinguish upload, draft creation, and publishing.

---

## 6. Source domain mapping examples

| User question | Source domain | Why |
|---|---|---|
| 飞书 CLI 是否支持创建多维表格字段？ | `feishu_cli` first, then `feishu_api` as fallback | The user asks about CLI support specifically. |
| 飞书 API 是否支持获取多维表格字段结构？ | `feishu_api` | The user asks about Feishu Open Platform API capability. |
| 微信公众号 API 是否支持上传封面图？ | `wechat_official_account_api` | The user asks about WeChat Official Account platform capability. |
| Notion API 是否支持字段同步？ | `out_of_scope` | Notion is not part of the current MVP source domain. |

---

## 7. Current MVP boundary

The current official source domains are limited to:

1. Feishu CLI;
2. Feishu Open Platform API;
3. WeChat Official Account API.

Any other platform should be marked as out of scope unless this source map is explicitly expanded.

---

## 8. Maintenance rule

When a new official source is added, update this file with:

- source domain;
- official source name;
- URL;
- what to use it for;
- verification stance;
- important caveats.

Do not mix unofficial blog posts, community notes, or AI-generated explanations into the official source map unless they are explicitly labeled as secondary references.
