# ADR Template (Architecture Decision Record)

## Purpose

This template is used to record significant architectural decisions. Each ADR should focus on **a single decision**.

---

## Template

```markdown
# ADR-[Number]: [Decision Title]

## Status
[Proposed | Accepted | Deprecated | Superseded by ADR-XXX]

## Date
[YYYY-MM-DD]

## Context
[Describe the problem or situation that drives this decision]
- What are the project requirements?
- What constraints exist?
- What are the stakeholders’ expectations?

## Decision Drivers
[List the key factors influencing the decision]
- Driver 1: ...
- Driver 2: ...

## Considered Options

### Option A: [Name]
- **Description**: [Brief description]
- **Pros**:
  - ...
- **Cons**:
  - ...

### Option B: [Name]
[Same structure as above]

## Decision
[Clearly state the chosen option and the core rationale]

## Consequences

### Positive
- ...

### Negative
- ...

### Follow-up Actions Required
- ...

## References
- [Links or citations]
```

---

## Best Practices

1. **Keep it concise**: An ADR should be readable in a few minutes.
2. **Focus on a single decision**: Split complex decisions into multiple ADRs.
3. **Immutable**: Once accepted, it is a historical record; if changes are needed, create a new ADR.
4. **Version control**: Store ADRs in Git alongside the codebase.
5. **Team collaboration**: Gather team feedback before finalizing.
