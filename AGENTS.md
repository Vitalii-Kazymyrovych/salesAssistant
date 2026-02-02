## Purpose
This file defines **rules and constraints for agents and developers** working on this repository.

This project uses an **Obsidian vault as the primary human UI and long‑term knowledge base**.
Its structure, schema, and terminology are defined in:

➡ **DB_STRUCTURE** (MANDATORY REFERENCE)

Agents MUST use the canonical terminology defined jointly by:
- TECHNICAL‑SPEC.md
- AGENTS.md
- DB_STRUCTURE.md

---

## Canonical terminology (DO NOT DEVIATE)
- **Application**: Spring Boot service that produces notes.
- **Vault**: Obsidian vault used by the user.
- **Operational DB**: Internal SQLite database for processing and scheduling.
- **Entity**: One real‑world object represented by one Markdown note.
- **Note**: A Markdown file inside the vault.
- **Dialog**: One conversation thread stored as one append‑only note.
- **Call**: One call or recording stored as one note.
- **Task**: An actionable checkbox item stored in task notes.

If a term is not listed here, it must not be invented.

---

## Agent discipline

### Logs and documentation
- Before starting work, read assistantVault/00 Util/AGENT_LOG.md
- After completing work:
  - append a factual entry to assistantVault/00 Util/AGENT_LOG.md
  - update README.md if behavior changed
- After code changes, run tests

### Log format (assistantVault/00 Util/AGENT_LOG.md)
- Append‑only
- Factual, no commentary
- Prefer:
  - YYYY‑MM‑DD: <change> (tests: <cmd>)

---

## Core principles (NON‑NEGOTIABLE)

1. Vault = human UI + long‑term memory
2. Application = producer, not owner, of notes
3. Markdown notes are append‑only by default
4. YAML frontmatter is a contract
5. One entity = one note
6. Links instead of duplication

Violation of these principles makes the system unmaintainable.

---

## Interaction with the vault

### Allowed
- Create new notes
- Append AI blocks or timeline entries
- Add missing YAML keys to app‑generated notes
- Create links between entities

### Forbidden
- Rewriting user‑authored text
- Renaming files without explicit rule
- Reformatting notes
- Deleting user data

---

## Testing rules

- Unit tests must not call:
  - AI providers
  - Telegram API
  - Transcription services
- External integrations must be mocked
- Scheduling must be disabled in tests:
  - spring.task.scheduling.enabled=false

---

## Startup behavior flags (mandatory)

Every integration must be disable‑able:
- ingestion.enabled=false
- ai.enabled=false
- reminders.enabled=false
- transcription.enabled=false

Agents must respect these flags.

---

## Obsidian schema governance

- Every app‑generated note must include:
  - schema_version
  - type
- Schema changes require:
  - version bump
  - backward compatibility or migration notes

---

## Mental model

> Vault = graph  
> Application = assistant  
> User = editor‑in‑chief

Any feature contradicting this model must not be implemented.
