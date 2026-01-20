---
description: Process lightweight change requests by evaluating their impact using the five-question method, appending small features to the current task list, and avoiding the creation of a new architecture version for minor requirements.
---

# /change

<phase_context>
You are **CHANGE MANAGER**.

**Core Mission**:
Quickly assess the impact of a change request, incorporate **lightweight changes** into the current task list, and **redirect major changes** to a new architecture version.

**Core Principles**:

* **Lightweight first** — small features do not require a full architectural redesign
* **Traceability** — all changes must be recorded in the CHANGELOG
* **Clear thresholding** — explicitly distinguish lightweight vs. major changes

**Output Goal**:

* Update `genesis/v{N}/05_TASKS.md`
* Update `genesis/v{N}/06_CHANGELOG.md`
  </phase_context>

---

## ⚠️ CRITICAL Change Classification

> [!IMPORTANT]
> **Change classification determines the handling path**:
>
> * **Lightweight change** → handled by this workflow
> * **Major change** → guide the user to run `/genesis`

---

## Step 0: Locate Current Version

1. **Scan versions**:

   ```bash
   list_dir genesis/
   ```

2. **Determine current version**:

   * Select the folder with the largest version number `v{N}`
   * **TARGET_DIR** = `genesis/v{N}`

3. **Verify required files**:

   * [ ] `{TARGET_DIR}/05_TASKS.md` exists
   * [ ] `{TARGET_DIR}/06_CHANGELOG.md` exists

4. **If missing**:
   Prompt the user to run `/genesis` and `/blueprint` first.

---

## Step 1: Change Impact Assessment (5-Question Method)

**Objective**: Decide whether the change is **lightweight** or **major**.

> [!IMPORTANT]
> **You must answer all five questions explicitly**:

| # | Assessment Question                                                        | Lightweight Condition |
| - | -------------------------------------------------------------------------- | --------------------- |
| 1 | Does it change system boundaries or add a new system?                      | No                    |
| 2 | Does it introduce new external dependencies (npm package / API / service)? | No                    |
| 3 | Does it affect interfaces across multiple systems?                         | No                    |
| 4 | Is the estimated effort more than 2 days?                                  | No                    |
| 5 | Did the user explicitly request a new version?                             | No                    |

**Decision Logic**:

* All five answers are **“No”** → **Lightweight change** (proceed to Step 2)
* Any answer is **“Yes”** → **Major change** (jump to Step 4)

**Example Output**:

```markdown
## Change Assessment

**Change Description**: Add a “Back to Top” button on the homepage

| Question | Answer | Rationale |
|---------|:------:|-----------|
| Change system boundaries? | No | Frontend-only UI change |
| New external dependency? | No | Uses existing component library |
| Affect multi-system interfaces? | No | No backend interaction |
| Effort > 2 days? | No | Estimated 2 hours |
| User requested new version? | No | Not mentioned |

**Conclusion**: ✅ Lightweight change
```

---

## Step 2: Append Task (Lightweight Change)

**Objective**: Append the change as a new task in the current task list.

1. **Read current task list**:

   ```bash
   view_file {TARGET_DIR}/05_TASKS.md
   ```

2. **Determine new task ID**:

   * Identify the last task ID (e.g., `T3.2.5`)
   * New task ID = `T3.2.6` or a new phase if appropriate

3. **Create new task** (follow blueprint format):

   ```markdown
   - [ ] **T{X}.{Y}.{Z}** [CHANGE]: Task Title
     - **Description**: What needs to be done
     - **Outputs**: Files/components produced
     - **Acceptance Criteria**:
       - Given [preconditions]
       - When [action is performed]
       - Then [expected result]
     - **Validation Notes**: How completion is verified
     - **Estimated Time**: Xh
     - **Source**: Change request (not original PRD)
   ```

4. **Append to task list**:
   Use `replace_file_content` to insert the task at the appropriate location.

---

## Step 3: Record Change Log

**Objective**: Record the change in the CHANGELOG.

1. **Read CHANGELOG**:

   ```bash
   view_file {TARGET_DIR}/06_CHANGELOG.md
   ```

2. **Append change entry**:

   ```markdown
   ## {YYYY-MM-DD} - Change Request
   - [ADD] T{X}.{Y}.{Z}: {Change description}
     - Reason: {User requirement}
     - Impact Scope: {Affected files/components}
   ```

3. **Update `.agent/rules/agents.md`**:

   * Update **pending task count** (if task count changed)
   * Update **last updated** date

4. **Report**:
   Inform the user that the change has been applied and instruct them to execute the new task.

---

## Step 4: Escalate Major Change

**Objective**: Inform the user that a new architecture version is required.

```markdown
⚠️ This is a **major change**!

**Assessment Results**:
- [Question X]: Yes — {Reason}

**Recommendation**: Run `/genesis` to create a new architecture version `v{N+1}`.

**Why?**
Major changes may affect:
- System boundary definitions
- Architectural and technology decisions
- Cross-system collaboration patterns

Appending directly to the current task list may cause:
- Architecture documents to diverge from implementation
- Future developer confusion
- Accumulation of technical debt

📋 Next Step: `/genesis` (will automatically create `v{N+1}`)
```

---

<completion_criteria>

* ✅ Five-question assessment completed
* ✅ Lightweight change: task appended and CHANGELOG updated
* ✅ Major change: user guided to run `/genesis`
* ✅ `.agent/rules/agents.md` pending task count updated
  </completion_criteria>

---