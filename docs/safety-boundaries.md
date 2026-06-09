# Safety Boundaries

## 1. Purpose

Requirement-to-System Mapper is designed to help users translate requirements and verify engineering assumptions before implementation.

It must not behave as an uncontrolled execution agent.

The system should be especially careful when requirements involve external systems, credentials, production data, publishing, deletion, payment, or batch writes.

---

## 2. Default forbidden actions

The MVP must not execute the following actions by default:

- delete data;
- process payment;
- mass message users;
- publish WeChat Official Account articles;
- modify production configuration;
- batch write production data;
- expose or store sensitive tokens;
- call unverified production APIs;
- perform destructive CLI operations.

---

## 3. High-risk triggers

When the user requirement includes the following words or actions, the system must mark them as high risk:

| Trigger | Risk |
|---|---|
| publish | Content may become public. |
| mass send / group send | Large number of users may be affected. |
| delete | Data may be unrecoverable. |
| payment | Money or financial transaction risk. |
| batch write | Data pollution risk. |
| production environment | Real business impact. |
| token / key / secret | Sensitive credential exposure risk. |
| permission escalation | Account or platform security risk. |

---

## 4. Token and credential rules

The system must not ask users to paste sensitive tokens directly into normal conversation.

If credentials are needed, the system should recommend:

- using environment variables;
- using a secret manager;
- using platform-provided credential storage;
- using test credentials instead of production credentials;
- rotating tokens after testing when needed.

---

## 5. Testing rules

Future testing capability must follow these rules:

1. Use a test environment.
2. Use test records, test files, and test accounts.
3. Avoid production tables and production official accounts.
4. Never execute delete, publish, payment, or mass send without explicit human confirmation.
5. Log request intent, not sensitive credential values.
6. Prefer minimal validation requests before full workflows.

---

## 6. Evidence rules

When the system discusses APIs, CLI commands, fields, permissions, schemas, tokens, or external platform capabilities, it must distinguish:

- verified facts;
- reasonable assumptions;
- pending items;
- high-risk assumptions;
- invalid assumptions.

No external capability should be written as a verified fact unless supported by official documentation, official schema, real test results, or user-provided reliable evidence.

---

## 7. Human-in-the-loop rules

Human confirmation is required before:

- publishing public content;
- sending messages to real users;
- deleting or overwriting data;
- writing to production systems;
- changing permissions;
- using production tokens;
- running batch operations;
- triggering irreversible workflows.

---

## 8. Recommended safe wording

When uncertain, the system should say:

> Current evidence is insufficient. This should remain a pending item until official documentation or a minimum test confirms it.

When something is risky, the system should say:

> This action may affect production data or public content. It should not be executed automatically. Use a test environment and require human confirmation.

---

## 9. Core safety principle

> Requirement-to-System Mapper should help users think and verify before implementation, not blindly execute high-risk operations.
