---
name: concept-modeler
description: Extract domain concepts from vague user requirements—entities, flows, and “dark matter” (what users don’t say). Based on Domain-Driven Design (DDD).
---

# The Modeler’s Field Guide

> “If you can’t describe it clearly, you can’t build it.”
> — Eric Evans

This skill transforms users’ fuzzy language into **Entities**, **Data Flows**, and **Dark Matter (Missing Components)**.

---

## ⚠️ Mandatory Deep Thinking

> [!IMPORTANT]
> Before performing any analysis, you **must** invoke the `mcp_sequential-thinking_sequentialthinking` tool and reason for **5–15 steps**, or more if needed.
>
> Example thinking prompts:
>
> 1. “When the user says ‘sync’, is it one-way or bidirectional? Real-time or batch?”
> 2. “What does this ‘list’ mean in code? `Wishlist` or `ShoppingCart`?”
> 3. “The user only described the happy path—what happens if X fails?”
> 4. “Does this feature require a new database table or a new API endpoint?”
> 5. “Are there hidden security, performance, or reliability concerns the user didn’t mention?”

---

## ⚡ Mission Objective

Extract from natural-language requirements:

1. **Entities** — the nouns of the system
2. **Flows** — the verbs between those nouns
3. **Dark Matter (Missing)** — components the user didn’t mention but must exist

---

## 🧭 Extraction Process

### Step 1: Noun Hunting

* **Input**: “I want users to *sync* their *lists*.”
* **Modeler’s challenge**: What is a “list”? `Wishlist`? `ShoppingCart`? `TodoList`?
* **Master rule**: Never assume you understand the user’s words.
  **Ubiquitous Language is the core of DDD.**
* **Output**: A clearly defined list of **Entities**.

---

### Step 2: Verb Analysis

* **Input**: “Sync.”
* **Modeler’s questions**:

  * One-way or bidirectional?
  * Real-time or batch?
  * What is the failure strategy—retry, rollback, alert?
* **Master rule**: Verbs define system complexity.
  One word like “sync” can hide ten edge cases.
* **Output**: Defined **Data Flows** and **Consistency Model**.

---

### Step 3: Dark Matter Detection

* **Master law**: Users describe only the happy path.
  **Your job is to find everything they didn’t say.**
* **Checklist**:

| Category                         | Key Question                                   |
| -------------------------------- | ---------------------------------------------- |
| Error Handling                   | What happens if X fails?                       |
| Persistence                      | Where is data stored? Backups needed?          |
| Auth & Authz                     | Who can access this? How is identity verified? |
| Logging & Monitoring             | How do we observe system health?               |
| Configuration                    | Hardcoded or externalized?                     |
| Rate Limiting & Circuit Breaking | How is the system protected under load?        |

* **Output**: Identified **Missing Components** with priority.

---

## 🛡️ Master Rules

1. **Do not invent**: If information is insufficient, list questions for the user.
2. **Be conservative**: Over-identify missing components rather than miss them.
3. **Explain reasoning**: Every judgment must include a rationale.
4. **Link to build topology**: Map identified entities to build roots found by `build-inspector`.

---

## 📤 Output Format

You **must** use `write_to_file` to save to `genesis/v{N}/concept_model.json` in the following format:

```json
{
  "entities": [
    {
      "name": "Wishlist",
      "type": "data",
      "necessity": "required",
      "description": "User’s wishlist"
    }
  ],
  "flows": [
    {
      "from": "User",
      "action": "add",
      "to": "Wishlist",
      "data": "Product ID"
    }
  ],
  "missing_components": [
    {
      "component": "Retry mechanism",
      "category": "Error handling",
      "priority": "High",
      "reason": "API calls may timeout"
    }
  ],
  "questions_for_user": [
    "Is synchronization real-time or batch-based?"
  ]
}
```

---

## 🧰 Toolbox

* `scripts/glossary_gen.py --path src/`
  Extract candidate domain terms from code
* `prompts/GLOSSARY_PROMPT.md`
  Ubiquitous language alignment prompts
* `references/ENTITY_EXTRACTION_PROMPT.md`
  Full entity-extraction prompt template (with few-shot examples)

---
