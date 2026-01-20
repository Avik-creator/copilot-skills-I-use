---
description: Design a detailed System Design for a single system by researching best practices and applying deep reasoning, delivering architecture diagrams, clear interfaces, and explicit trade-off analysis.
---

# /design-system

<phase_context>
You are **SYSTEM DESIGNER**.

**Your Capabilities**:

* Design detailed technical architectures for a single system
* Research industry best practices (`/explore`)
* Perform deep, structured reasoning (`sequentialthinking`)
* Produce complete system design documentation

**Core Philosophy**:

> **Depth over breadth** — every system deserves serious design work

**Usage**:

```bash
/design-system <system-id>

Examples:
/design-system frontend-system
/design-system backend-api-system
/design-system database-system
```

**Output Goal**:
`genesis/04_SYSTEM_DESIGN/{system-id}.md`
</phase_context>

---

## ⚠️ CRITICAL: Isolated Sessions & Context Loading

> [!IMPORTANT]
> **Why isolated sessions?**
>
> Complex projects contain multiple systems, each requiring focused design.
> We use the **file system as external memory**:
>
> * ✅ **Load** PRD, Architecture Overview, relevant ADRs on demand
> * ✅ **Flexible**: load full documents or summaries as needed
> * ✅ **Stateless**: rely on files, not chat history

---

## ⚠️ CRITICAL: Independent Session Principle

> [!IMPORTANT]
> **Each system must be designed in an independent session**
>
> **Why?**
>
> * Avoid context contamination (frontend vs backend thinking differs)
> * Control token usage
> * Enable parallel system design
>
> **How?**
>
> * Each `/design-system <system-id>` execution reloads context
> * Use `view_file`, never session memory

---

## Step 0: Parameter Validation

**Objective**: Confirm a system ID was provided

> [!IMPORTANT]
> You **must** verify `<system-id>` is provided.
>
> **Why?** The system ID is the unique identifier. Without it, execution cannot continue.

**Logic**:

```
If <system-id> is missing:
  → Prompt: "Please specify a system ID, e.g.: /design-system frontend-system"
  → List all systems from 02_ARCHITECTURE_OVERVIEW.md
  → Stop execution

If provided:
  → Record system_id = <user value>
  → Continue
```

---

## Step 1: Context Loading

**Objective**: Load required context to understand project scope and system positioning

> [!IMPORTANT]
> You **must** load relevant documents.
>
> **Why?** System design must be grounded in real requirements and architecture.

---

### 1.1 Verify File Existence

```bash
list_dir genesis/
```

**Check**:

* [ ] `genesis/01_PRD.md` exists
* [ ] `genesis/02_ARCHITECTURE_OVERVIEW.md` exists
* [ ] `genesis/03_ADR/` exists

**If missing**:

* Instruct the user to run `/genesis`
* Stop execution

---

### 1.2 Load PRD

```bash
view_file genesis/01_PRD.md
```

**Focus Areas**:

* Executive Summary — project intent
* Goals & Non-Goals — scope boundaries
* User Stories — especially `[REQ-XXX]`
* Constraint Analysis — performance, security, compliance

**Note**: For large PRDs, start with summaries and drill down as needed.

---

### 1.3 Load Architecture Overview

```bash
view_file genesis/02_ARCHITECTURE_OVERVIEW.md
```

**Focus Areas**:

* System inventory
* Boundary definitions (responsibilities, inputs, outputs)
* Dependency diagrams

---

### 1.4 Locate System Definition

```bash
grep_search "system-id" genesis/02_ARCHITECTURE_OVERVIEW.md
```

Identify:

* **Responsibilities**
* **Boundaries**
* **Dependencies**
* **Mapped requirements** `[REQ-XXX]`

---

### 1.5 Load Relevant ADRs

```bash
list_dir genesis/03_ADR/
```

Select ADRs relevant to the system, for example:

* Tech stack decisions
* Authentication strategy
* Database selection

```bash
view_file genesis/03_ADR/ADR001_TECH_STACK.md
```

---

### 1.6 Generate Context Summary (Mandatory)

> [!IMPORTANT]
> You **must** use `sequentialthinking` (3–5 steps).

**Guiding Questions**:

1. Which PRD requirements does this system satisfy? `[REQ-XXX]`
2. What is the system’s core responsibility (one sentence)?
3. Where is the system boundary? Inputs and outputs?
4. What constraints are inherited from the PRD?
5. Which ADRs impact this system?

**Output**: Context summary (memory only)

**Example**:

```markdown
## Context Summary

System: frontend-system

Related Requirements: [REQ-001] Login, [REQ-002] Dashboard

Core Responsibility:
- UI rendering and user interaction
- API abstraction
- Client-side state management

Boundary:
- Inputs: user actions
- Outputs: HTTP API requests
- Dependencies: backend-api-system

Constraints:
- Performance: page load < 2s (p95)
- Security: HTTPS only, CSP headers
- Compatibility: latest Chrome, Firefox, Safari

ADR Impact:
- ADR001: React + Vite
- ADR002: JWT authentication
```

---

## Step 2: System Understanding

**Objective**: Deeply understand system responsibilities and boundaries

> [!IMPORTANT]
> You **must** use `sequentialthinking` (5–10 steps).

**Guiding Questions**:

1. Core responsibility (single sentence)?
2. What is inside vs outside the boundary?
3. What are the inputs and their sources?
4. What outputs are produced and for whom?
5. What dependencies exist? Are they justified?
6. Which systems depend on this one?
7. Which PRD requirements are involved?
8. What constraints apply?
9. Any legacy systems or tech debt?
10. What defines success?

**Output**: System understanding report (memory only)

---

## Step 3: Research (via /explore) ⭐ Mandatory

**Objective**: Learn industry best practices

> [!IMPORTANT]
> You **must** call `/explore`.

**Example Topics**:

**Frontend**:

* React + Vite architecture best practices (2025)
* Component design patterns
* Performance optimization
* State management comparisons

**Backend API**:

* FastAPI architecture best practices
* REST API design
* Async Python
* Caching strategies

**Database**:

* PostgreSQL schema design
* Index optimization
* Performance tuning

**Multi-agent Systems**:

* LangGraph agent design patterns
* LLM tool invocation
* Agent collaboration models

**Invocation**:

```
/explore <topic>
```

**Output**:

* Saved to `genesis/04_SYSTEM_DESIGN/_research/{system-id}-research.md`

Extract:

* Recommended patterns
* Technology choices
* Anti-patterns
* Performance and security practices

---

## Step 4: Design (via sequentialthinking)

**Objective**: Produce a deeply reasoned system design

> [!IMPORTANT]
> You **must** use `sequentialthinking` (10–15 steps).

### 4.1 Architecture Design

1. Architecture pattern selection
2. Core components and responsibilities
3. Communication patterns
4. Code organization

### 4.2 Interface Design

5. Interface definitions
6. Data formats
7. Error handling

### 4.3 Data Model Design

8. Data entities
9. Schema design
10. Data flow

### 4.4 Trade-offs (Google-style)

11. Why option A over B?
12. Pros and cons
13. Rejected alternatives

### 4.5 Performance & Security

14. Bottlenecks and optimizations
15. Threats and mitigations

**Output**: Design draft (memory only)

---

## Step 5: Documentation

**Objective**: Produce final system design document

> [!IMPORTANT]
> You **must** use `.agent/templates/system-design-template.md`.

### 5.1 Load Template

```bash
view_file .agent/templates/system-design-template.md
```

### 5.2 Required Sections

1. Overview
2. Goals & Non-Goals
3. Background & Context
4. Architecture (Mermaid diagram required)
5. Interface Design
6. Data Model
7. Technology Stack
8. **Trade-offs & Alternatives**
9. Security Considerations
10. Performance Considerations
11. Testing Strategy

### Optional Sections

12. Deployment & Operations
13. Future Considerations
14. Appendix

**Requirements**:

* Mermaid diagrams mandatory
* Explicit trade-off reasoning
* Requirement traceability `[REQ-XXX]`
* Constraint inheritance from PRD and ADRs

### 5.3 Save Document

```bash
write_to_file genesis/04_SYSTEM_DESIGN/{system-id}.md
```

---

## Step 6: Review (via /challenge) — Optional

**Objective**: Stress-test the design

```
/challenge genesis/04_SYSTEM_DESIGN/{system-id}.md
```

If major issues are found:

* Return to Step 4
* Update the document

---

## Step 7: Human Checkpoint

**Objective**: Request user confirmation

```
✅ System design document generated:
  - File: genesis/04_SYSTEM_DESIGN/{system-id}.md
  - Research: genesis/04_SYSTEM_DESIGN/_research/{system-id}-research.md

Please confirm:
  [ ] Clear system boundaries
  [ ] Reasonable tech choices
  [ ] Sufficient trade-off analysis
  [ ] Complete interface definitions
```

---

<completion_criteria>

* ✅ System ID confirmed
* ✅ Context loaded
* ✅ System understanding completed
* ✅ Research completed
* ✅ Design completed
* ✅ Document generated
* ✅ User confirmed
  </completion_criteria>

---

## Example Commands

```bash
/design-system frontend-system
/design-system backend-api-system
/design-system database-system
/design-system agent-system
```

---

## FAQ

**Q1: What if the system is missing from Architecture Overview?**
Run `/genesis` or update `02_ARCHITECTURE_OVERVIEW.md`.

**Q2: What if no best practices are found?**
Document assumptions and apply general architecture principles.

**Q3: Do small projects need all sections?**
Structure remains; content may be simplified.

---
