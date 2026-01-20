---
name: git-forensics
description: Analyze Git history to uncover “logical coupling” (files that always change together) and “hotspots” (frequently modified, complex modules). Based on Adam Tornhill’s *Your Code as a Crime Scene*.
---

# The Archaeologist’s Field Notes

> “History doesn’t repeat itself, but it rhymes. Static analysis tells you structure; Git forensics tells you the painful truth.”
> — Adam Tornhill

This skill is based on **Adam Tornhill’s *Your Code as a Crime Scene*** methodology.
Core idea: a system’s **evolution history** reveals design problems better than the code alone.

---

## ⚠️ Mandatory Deep Thinking

> [!IMPORTANT]
> Before performing any analysis, you **must** invoke the `mcp_sequential-thinking_sequentialthinking` tool and reason for **3–10 steps**, or more if needed.
>
> Example thinking prompts:
>
> 1. “How deep is this project’s Git history? Do I need `git fetch --unshallow`?”
> 2. “Which time window should I focus on? (Last 3 months? 1 year?)”
> 3. “Are there obvious noise files (e.g. `package-lock.json`) that must be excluded?”

---

## ⚡ Quick Start

1. **Coupling analysis**

   ```bash
   python scripts/git_forensics.py --repo . --threshold 0.3
   ```
2. **Hotspot detection**

   ```bash
   python scripts/git_hotspots.py --repo . --days 180
   ```

---

## 🧭 Exploration Process (The Dig)

### Step 1: Sense the Flow of Time (The Tornhill Method)

* **Master’s saying**:
  “The value of code is not what it is, but how it became that way.”
* Run:

  ```bash
  git log --oneline -n 100
  ```

  to quickly gauge recent project activity.
* **Key inference**:
  If the last ~50 commits touch only one or two directories, that’s the **epicenter**—the area most likely hiding risk.

---

### Step 2: Discover Hidden Coupling

(Temporal Coupling / Change Coupling)

* **Core question**:
  “Are there two files that have **no import/use relationship**, yet appear together in **70% of commits**?”
* **Master warnings (from Adam Tornhill)**:

  * ⚠️ **Logical coupling with physical separation** → strong signal of **architectural decay**
  * ⚠️ **Cross–build-root coupling** → if `service/ipc.rs` and `gui/api.ts` change together but belong to different build roots, this is a breeding ground for **version skew**
  * **Prescription**:

    * Either merge them into the same module, or
    * Extract a shared schema / contract layer

---

### Step 3: Identify Hotspots

(CodeScene Methodology)

* **Formula**:
  `Hotspot = High Change Frequency (Churn) × High Complexity`
* **Master strategy matrix (from CodeScene)**:

|                | Low Complexity                            | High Complexity                                                 |
| -------------- | ----------------------------------------- | --------------------------------------------------------------- |
| **High Churn** | Config / generated code — often ignorable | 🔴 **Top refactor priority** (bug breeding ground, highest ROI) |
| **Low Churn**  | Stable modules — leave them alone         | 🟡 Legacy minefield — tread carefully                           |

* **Master advice**:
  With limited refactoring capacity, **only attack the High-Churn + High-Complexity quadrant**. That’s where ROI is highest.

---

## 🛡️ Master Rules

1. **Unshallow first**:
   Run `git fetch --unshallow`. No history = no data = blind analysis.
2. **Filter noise**:
   Exclude `package-lock.json`, `Cargo.lock`, `*.min.js`, `dist/`, and other generated artifacts—they pollute results.
3. **Beware mass renames**:
   `git mv` can distort coupling signals. If results look strange, manually verify rename history.
4. **Link to build topology**:
   When reporting coupled file pairs, **annotate their respective build roots**.
   This is the critical bridge between Git forensics and build analysis.

---

## 📤 Required Output

Your report must include:

1. **Coupling Pairs**
   File pairs with coupling score > 0.7, **annotated with their build roots**
2. **Cross-Root Couplings**
   Highlight explicitly — if two highly coupled files belong to different build roots, this is the **#1 risk**
3. **Hotspots**
   List of high-risk modules (high churn + high complexity)
4. **Orphans**
   Files untouched for over 1 year (knowledge-decay warning)
5. **Refactoring Priority**
   Suggested refactor order based on the churn/complexity matrix

---

