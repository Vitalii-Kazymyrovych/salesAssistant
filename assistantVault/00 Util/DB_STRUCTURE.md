# Obsidian Vault Structure (Canonical Schema)

## 1) Purpose

This document defines the **canonical vault schema and terminology**.
All agents, specs, and code MUST align with this file.

- The **Application** produces notes.
- The **Vault** is the primary human UI and long-term knowledge base.
- The **Operational DB** exists only for processing and scheduling.

---

## 2) Canonical terminology

- **Entity**: One real-world object represented by exactly one Markdown note.
- **Note**: A Markdown file inside the vault.
- **Dialog**: A conversation thread (messages/emails/chats) stored as one append-only note.
- **Call**: One call or recording stored as one note.
- **Task**: An actionable checkbox item stored in task notes.

These terms are authoritative and must be used verbatim.

---

## 3) Non-negotiable principles

1. One entity = one note
2. Links over duplication (`[[WikiLinks]]`)
3. YAML frontmatter is mandatory and acts as a contract
4. Append-only by default

---

## 4) Vault folder layout (current)

```
assistantVault/
├── 00 Util/
│   ├── AGENT_LOG.md
│   ├── DB_STRUCTURE.md
│   ├── TECHNICAL_SPEC.md
│   └── Countries and regions/
├── 01 Vendor/
├── 02 Partners/
├── 03 Projects/
├── 04 Dashboards/
├── Calls/
├── Dialogs/
└── People/
```

Folders are organizational; YAML frontmatter and links are authoritative.

---

## 5) Note types and structures

### 5.1) Dashboards

#### Task List (tasks note)
**Path example:** `04 Dashboards/Task List.md`

**Frontmatter**
```yaml
type: tasks
created: YYYY-MM-DD
```

**Body**
- Markdown task items with inline metadata:
  - `due:: YYYY-MM-DD`
  - `priority:: low|medium|high`
  - `source:: [[<Call note>]]`
- Each task MUST link back to its source dialog or call via `source::`.

#### Inbox (dashboard)
**Path example:** `04 Dashboards/Inbox.md`

**Frontmatter**
```yaml
type: dashboard
```

**Body**
- Dataview query that surfaces waiting dialogs.

---

### 5.2) People
**Path example:** `People/Andrey Shumarev.md`

**Frontmatter**
```yaml
role: <role>
company: "[[<Company note>]]"
contacts:
  - <email>
  - <phone>
geo: "[[<Country/Region note>]]"
```

**Body**
- Info callout describing the person.
- Optional sections:
  - `## Projects` (links to project notes)
  - `## Dialogs` (links to dialog notes)
  - `## Calls` (links to call notes)

---

### 5.3) Partners (companies)
**Paths:** `02 Partners/*.md`

This folder contains all company entities (customers, integrators, distributors, tech partners).
The company role is indicated by tags.

**Frontmatter**
```yaml
geo: "[[<Country/Region note>]]"
technology: <optional text>
industry: <optional text>
tags:
  - customer|integrator|distributor|techpartner
```

**Body**
- Info callout describing the company.
- Sections commonly used:
  - `## People` (links to people notes)
  - `## Projects` (links to project notes)
  - `# Documents` or `# Other` (optional placeholders)

---

### 5.4) Projects
**Path example:** `03 Projects/KZ - Zhedel Information - Active warehouse.md`

**Frontmatter**
```yaml
customer: "[[<Customer company note>]]"
integrator: "[[<Integrator company note>]]"
distributor: "[[<Distributor company note>]]"
tech_partner: "[[<Tech partner note>]]"
status: <status>
start_date: YYYY-MM-DD
end_date: <optional>
```

**Body**
- `## Project overview` summary.
- `## Participants and roles` with links to people notes.
- `## Project timeline` containing dated subsections with links to dialog/call anchors.
- `## Documents` optional placeholder section.

---

### 5.5) Dialogs
**Path examples:** `Dialogs/Andrey Shumarev - WhatsApp.md`, `Dialogs/Face recognition demo - Outlook.md`

**Frontmatter**
```yaml
customer: "[[<Customer company note>]]"
project: "[[<Project note>]]"
tags:
  - waiting_for_me|inactive|... (status)
  - whatsapp|outlook|... (channel)
```

**Body**
- `# Participants` list of people links.
- `# Timeline` containing dated headings (`### YYYY-MM-DD`) and message entries.
  - Each entry starts with a timestamp line and a `**type:**` line.
- `## Context` with links to related vendor knowledge and partner notes.
- Dialog notes may be empty placeholders until content is ingested.

---

### 5.6) Calls
**Path example:** `Calls/2026.01.30 - 15-04 - Andrey Shumarev.md`

**Frontmatter**
```yaml
datetime: YYYY-MM-DD
project: "[[<Project note>]]"
duration: <minutes>
tags:
  - teams|zoom|meet|whatsapp|telegram|other
```

**Body**
- `# Participants` list of people links.
- `## Summary` bullet list.
- `## Action items` Markdown tasks (should be copied or referenced in task notes).

---

### 5.7) Vendor knowledge
**Paths:** `01 Vendor/*.md`

**Frontmatter**
```yaml
type: knowledge
topic: <topic>
```

**Body**
- May be empty (placeholder) or contain knowledge content.
- Linked from dialog `## Context` sections and project overviews.

---

### 5.8) Countries and regions (utility notes)
**Paths:** `00 Util/Countries and regions/*.md`

**Body**
- A list of links to countries/regions (e.g., `[[Kazakhstan]]`).
- Notes may be empty placeholders, but should exist if referenced by `geo`.

---

## 6) Interconnections (required links)

- People link to their company and geo in frontmatter.
- Company notes link back to people and projects.
- Project notes link to customer/integrator/distributor/tech partner, and to dialogs/calls in the timeline.
- Dialogs and calls link to the project and customer in frontmatter, and to participants in the body.
- Tasks in Task List must reference their source call or dialog via `source:: [[...]]`.
- Vendor knowledge notes are linked from dialog context sections and project overviews.

---

## 7) AI agent guide for vault management

### Allowed actions
- Create new notes with correct YAML frontmatter and canonical terminology.
- Append new timeline entries, AI blocks, or task items.
- Add missing YAML keys to app-generated notes.
- Add links between related entities.

### Forbidden actions
- Rewriting or deleting user-authored text.
- Renaming or moving notes without deterministic rules.
- Reformatting existing notes.
- Deleting user data.

### Operational guidance
- Always treat notes as append-only.
- Use `[[WikiLinks]]` instead of duplicating text across notes.
- When creating tasks, ensure `source::` points to the originating dialog or call.
- Keep folder placement consistent with the note type described above.

---

## 8) Mental model

> Vault = graph  
> Application = producer  
> User = editor-in-chief
