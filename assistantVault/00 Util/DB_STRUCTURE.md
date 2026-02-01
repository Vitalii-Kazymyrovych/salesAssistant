# Obsidian Vault Structure (Canonical Schema)

## 1) Purpose

This document defines the **canonical vault schema and terminology**.
All agents, specs, and code MUST align with this file.

- The **Application** produces notes.
- The **Vault** is the primary human UI and long‑term knowledge base.
- The **Operational DB** exists only for processing and scheduling.

---

## 2) Canonical terminology

- **Entity**: One real‑world object represented by exactly one Markdown note.
- **Note**: A Markdown file inside the vault.
- **Dialog**: A conversation thread (messages/emails/chats) stored as one append‑only note.
- **Call**: One call or recording stored as one note.
- **Task**: An actionable checkbox item stored in task notes.

These terms are authoritative and must be used verbatim.

---

## 3) Non‑negotiable principles

1. One entity = one note
2. Links over duplication (`[[WikiLinks]]`)
3. YAML frontmatter is mandatory
4. Append‑only by default
5. Versioned schema (`schema_version`)

---

## 4) Root folder layout

```
Sales Assistant/
├── Distributors/
├── Integrators/
├── Customers/
├── Tech Partners/
├── People/
│   ├── Customers/
│   ├── Distributors/
│   ├── Integrators/
│   ├── Tech Partners/
│   └── Vendor/
├── Colleagues/
├── Projects/
├── Dialogs/
├── Calls/
├── Inbox/
├── Knowledge/
├── Technical Documentation/
├── Specifications/
├── Pricing/
├── Requirement Matrices/
├── Other Documents/
├── Tasks/
└── Dashboards/
```

Folders are organizational; **type** in YAML is authoritative.

---

## 5) Entity schemas

### Companies (distributors, integrators, tech partners)
```yaml
schema_version: 1
type: [distributor|integrator|tech_partner]
name:
region:
specialization:
technology:
contacts:
```

### Customers (company)
```yaml
schema_version: 1
type: customer_company
industry:
location:
primary_contacts:
tags: [customer]
```

### People
```yaml
schema_version: 1
type: person
role:
company:
contacts:
tags: [person]
```

---

## 6) Projects
```yaml
schema_version: 1
type: project
customer:
distributor:
integrator:
tech_partner:
status: [lead|presale|delivery|support|closed|initiative]
owners:
start:
end:
tags: [project]
```

---

## 7) Communication notes

### Dialogs
```yaml
schema_version: 1
type: dialog
channel: [telegram|whatsapp|outlook|teams|other]
chat_id:
customer:
project:
participants:
status: [waiting_for_me|waiting_for_customer|closed]
created:
updated:
tags: [dialog]
```

### Calls
```yaml
schema_version: 1
type: call
datetime:
format: [phone|zoom|meet|teams|whatsapp|telegram|other]
customer:
project:
participants:
duration:
recording_ref:
tags: [call]
```

---

## 8) Tasks

Tasks are Markdown checkbox items stored in task notes.
They MUST link back to their source dialog or call.

---

## 9) AI output blocks

AI output must be appended as a new block and never overwrite previous content.

---

## 10) Interaction rules

### Allowed
- Create notes
- Append blocks
- Add missing YAML keys to app‑generated notes

### Forbidden
- Rewriting user content
- Renaming or moving notes without deterministic rules
- Deleting content

---

## 11) Mental model

> Vault = graph  
> Application = producer  
> User = editor‑in‑chief
