---

name: tech-evaluator
description: Evaluate technology stack options and produce Architecture Decision Records (ADR) using weighted decision matrices and the ATAM methodology.
---------------------------------------------------------------------------------------------------------------------------------------------------------

# The Tech Evaluator’s Manual

> “There is no best technology stack, only the most suitable one.”
> — ThoughtWorks Technology Radar

This skill is based on **SEI’s ATAM (Architecture Tradeoff Analysis Method)** and **weighted decision matrix** methodologies.

---

## ⚠️ Mandatory Deep Thinking

> [!IMPORTANT]
> Before performing the evaluation, you **must** invoke the `mcp_sequential-thinking_sequentialthinking` tool and conduct **5–15 steps** of reasoning.
> Example lines of thinking include:
>
> 1. “What are the user’s core needs? Which scenarios must be supported?”
> 2. “Which technologies is the team already familiar with? What is the time budget for learning new ones?”
> 3. “What are the budget constraints? How sensitive is the project to cloud service costs?”
> 4. “What is the expected project scale? How many concurrent users must be supported?”
> 5. “Are there compliance requirements (GDPR, local security standards, etc.) that affect technology choices?”

---

## ⚡ Objective

Produce an **ADR (Architecture Decision Record)** document that records the technology stack decision and its rationale.

---

## 🧭 Evaluation Process

### Step 1: Gather Constraints

You **must obtain from the user**:

* **Functional requirements**: List of core features
* **Non-functional requirements**: Performance targets, availability requirements, security level
* **Team context**: Team size, skill set, willingness to learn
* **Budget**: Development budget, operations budget, time budget
* **Special constraints**: Compliance requirements, legacy system integration, customer-mandated technologies

---

### Step 2: Identify Candidate Technology Stacks

**Mainstream 2025 technology stack references**:

| Scenario           | Recommended Stack           | Alternatives                |
| ------------------ | --------------------------- | --------------------------- |
| **Web Full Stack** | Next.js + TypeScript        | Nuxt, SvelteKit             |
| **Backend API**    | Go / Rust / Node.js         | Python FastAPI, Java Spring |
| **Desktop Apps**   | Tauri (Rust + Web)          | Electron, Flutter Desktop   |
| **Mobile Apps**    | React Native / Flutter      | Native Swift/Kotlin         |
| **AI / ML**        | Python + PyTorch/TensorFlow | Rust (Candle), Julia        |
| **Data-Intensive** | PostgreSQL + TimescaleDB    | ClickHouse, DuckDB          |

---

### Step 3: 12-Dimension Evaluation

Score each candidate stack using the following matrix (1–5 points per dimension):

| Dimension                         | Suggested Weight | Evaluation Question                                 |
| --------------------------------- | :--------------: | --------------------------------------------------- |
| **Requirement Fit**               |       ★★★★★      | Can it satisfy all core requirements?               |
| **Scalability**                   |       ★★★★       | Can it support 10x growth?                          |
| **Performance**                   |       ★★★★       | Can it meet latency/throughput targets?             |
| **Security**                      |       ★★★★       | Built-in security features? Compliance support?     |
| **Team Skills**                   |       ★★★★★      | Team familiarity? Learning curve?                   |
| **Talent Market**                 |        ★★★       | Ease of hiring?                                     |
| **Development Speed**             |       ★★★★       | Can it support rapid iteration?                     |
| **TCO (Total Cost of Ownership)** |       ★★★★       | Development + operations + licensing cost?          |
| **Community Ecosystem**           |        ★★★       | Library/tool maturity? Problem-solving velocity?    |
| **Long-term Maintainability**     |        ★★★       | Technology lifespan? LTS support?                   |
| **Integration Capability**        |        ★★★       | Ease of integrating with existing systems/services? |
| **AI Readiness**                  |        ★★        | Ease of integrating AI/LLMs?                        |

---

### Step 4: Trade-off Analysis

Use the **ATAM method**:

1. Identify **quality attribute scenarios** (e.g., “Response time < 200ms with 1000 concurrent users”)
2. Evaluate how well each candidate stack supports these scenarios
3. Identify **trade-off points** (e.g., “Go offers high performance but requires team upskilling”)
4. Identify **risk points** (e.g., “Choosing a new framework may introduce unknown pitfalls”)

---

### Step 5: Generate ADR

You **must** use `write_to_file` to save the document to:
`genesis/v{N}/03_ADR/ADR_001_TECH_STACK.md`

---

## 📤 ADR Output Template

```markdown
# ADR-001: Technology Stack Selection

## Status
Accepted / Proposed / Deprecated

## Context
[Project background and constraints]

## Decision
[Chosen technology stack and core rationale]

## Candidate Comparison

| Candidate | Total Score | Strengths | Weaknesses |
|----------|------------|-----------|------------|
| Option A | 42/60 | ... | ... |
| Option B | 38/60 | ... | ... |

## Trade-offs
- [Trade-off 1]
- [Trade-off 2]

## Consequences
- Positive: [...]
- Negative: [...]
- Follow-up actions required: [...]
```

---

## 🛡️ Veteran Engineer’s Rules

1. **“Boring” technology first**: Unless there is a strong justification, prefer mature and stable technologies.
2. **Limited innovation budget**: Each project gets only 1–2 “innovation slots”; everything else should use “boring” tech.
3. **Team capability is king**: A great technology is useless if the team cannot use it.
4. **TCO is more than money**: Time cost and cognitive cost are also real costs.

---

## 🧰 Toolbox

* `references/ADR_TEMPLATE.md`: ADR template
* `references/TECH_RADAR_2025.md`: 2025 Technology Radar reference
