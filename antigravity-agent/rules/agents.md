---
trigger: always_on
---

# AGENTS.md – AI Collaboration Protocol

> **“If you are reading this document, you are the Intelligence.”**
>
> This file is your **anchor**. It defines the project rules, the map of the territory, and the memory protocol.
> When you wake up (start a new session), **read this file first**.

---

## 🧠 30-Second Recovery Protocol (Quick Recovery)

**When you start a new session or feel “lost”, do this immediately**:

1. **Read `.agent/rules/agents.md`** → get the project map
2. **Check “Current Status” below** → identify the latest architecture version
3. **Read `genesis/v{N}/05_TASKS.md`** → understand current work items
4. **Start working**

---

## 🗺️ Map (Territory Awareness)

How this project is organized:

| Path                | Description                                                            | Access Rules                               |
| ------------------- | ---------------------------------------------------------------------- | ------------------------------------------ |
| `src/`              | **Implementation layer**. The actual codebase.                         | Read/write via Tasks only                  |
| `genesis/`          | **Design evolution history**. Versioned architecture states (v1, v2…). | **Read-only** (old) / **Write-once** (new) |
| `genesis/v{N}/`     | **Current source of truth**. Latest architecture definition.           | Always use the highest `v{N}`              |
| `.agent/workflows/` | **Workflows** (`/genesis`, `/blueprint`, etc.)                         | Read via `view_file`                       |
| `.agent/skills/`    | **Skill library**. Atomic capabilities.                                | Invoked via `view_file`                    |

---

## 📍 Current Status (Auto-maintained by workflows)

> **Note**: This section is automatically maintained by `/genesis` and `/blueprint`.

* **Latest Architecture Version**: `genesis/v1` (initial state)
* **Active Task List**: Not generated yet
* **Pending Tasks**: –
* **Last Updated**: `–`

---

## 🌳 Project Structure

> **Note**: Maintained by `/genesis`.

```text
(Waiting for Genesis to initialize project structure…)
```

---

## 🧭 Navigation Guide

> **Note**: Maintained by `/genesis`.

* **Before new architecture is ready**: Do not make large-scale code changes.
* **When facing architectural questions**: Consult `genesis/v{N}/03_ADR/`.

---

## 🛠️ Workflow Registry

| Workflow         | When to Use                        | Output                   |
| ---------------- | ---------------------------------- | ------------------------ |
| `/genesis`       | New project / major refactor       | PRD, Architecture, ADRs  |
| `/scout`         | Before change / inheriting project | SCOUT_RISK_REPORT.md     |
| `/design-system` | After genesis                      | 04_SYSTEM_DESIGN/*.md    |
| `/blueprint`     | After genesis                      | 05_TASKS.md              |
| `/change`        | Lightweight changes                | Update TASKS + CHANGELOG |
| `/explore`       | Research                           | Exploration report       |

---

## 📜 The Constitution

1. **Versions are law**: Do not “patch” architecture documents—only evolve them. Changes require a new version.
2. **Explicit context**: Decisions go into ADRs, not chat memory.
3. **Cross-verification**: Before coding, check `05_TASKS.md`. Am I executing planned work?
4. **Aesthetics matter**: Documents should be beautiful. Use Markdown and emojis thoughtfully.

---

> **Status Check**: Ready? Read the architecture version indicated in “Current Status” above and proceed.
