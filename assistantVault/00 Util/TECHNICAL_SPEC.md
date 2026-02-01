# Personal Communication & Action Hub — Technical Specification (v1)

## Terminology (canonical)
- **Application**: Spring Boot service that ingests, analyzes, and produces notes.
- **Vault**: Obsidian vault used as the primary human UI and long‑term knowledge base.
- **Operational DB**: Internal SQLite database used for processing, deduplication, and scheduling. Not a UI.
- **Entity**: A real‑world object represented by exactly one Markdown note (company, person, project, dialog, call, document).
- **Note**: A Markdown file inside the vault representing one entity.
- **Dialog**: A conversation thread (messages/emails/chats) stored as one append‑only note.
- **Call**: A single call or recording stored as one note.
- **Task**: An actionable item stored as a Markdown checkbox in task notes.

These terms MUST be used consistently across TECHNICAL‑SPEC.md, AGENTS.md, and db‑structure.md.

---

## Purpose
This document defines the technical architecture, data flows, and constraints of the system.
The system is designed as a **personal assistant**, not a CRM or messenger replacement.

Primary UI: **Obsidian vault**  
Core runtime: **Spring Boot (Java)**

---

## High‑level architecture

Sources → Ingestion → **Operational DB** → AI Analysis → Outputs

Outputs:
- Obsidian notes (dialogs, calls, knowledge)
- Obsidian task notes
- Telegram reminders

The application is strictly **read‑only** with respect to external communication channels.

---

## Data sources

### Messages / Emails
- Telegram Web
- WhatsApp Web
- Outlook Web
- Teams Web

Collected via a browser adapter (Playwright/Puppeteer).  
Only data visible in the UI is ingested.

### Calls
- Audio files (Zoom, Meet, Teams recordings, voice notes)
- File‑based ingestion only (no live recording)

### Knowledge base
- Markdown notes inside the Obsidian vault
- Used as AI context (RAG)

---

## Operational database

Purpose:
- Timeline storage
- Deduplication
- Scheduling state
- AI processing state

Recommended:
- SQLite (v1)

The operational DB is **not** a UI and **not** a long‑term knowledge store.

---

## AI analysis

AI produces **structured output only**:
- summary
- intent
- suggested reply
- action items
- confidence

AI never sends messages and never performs actions.

Optional:
- RAG over Obsidian knowledge, specifications, and matrices

---

## Obsidian outputs

### Dialog notes
Contain:
- original content (excerpt)
- AI summary
- suggested reply
- extracted tasks
- links to related entities

### Call notes
Contain:
- call metadata
- transcript or summary
- decisions
- action items

### Task notes
- Stored separately from dialogs and calls
- Linked back to the source note

---

## Reminder system

- Scheduler reads task notes
- Telegram bot sends contextual reminders
- Actions: Done / Snooze / Open source note

Telegram is a **notification channel**, not a UI.

---

## Configuration

All integrations must be configurable and disable‑able:
- ingestion
- ai
- transcription
- reminders

Secrets must never be committed.

---

## Non‑goals
- No auto‑replies
- No multi‑user support
- No UI inside the application
- No historical chat crawling

---

## Acceptance criteria (v1)
- Messages and calls produce Obsidian notes
- AI output is structured and reproducible
- Tasks are created as Obsidian notes
- Telegram reminders work for due/overdue tasks
- All components can be disabled via configuration

