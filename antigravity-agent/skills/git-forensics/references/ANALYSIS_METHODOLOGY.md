# Git Forensics Analysis Methodology

## Overview

The core value of Git Forensics is not merely data extraction, but **analysis** and **actionable recommendations**.
This document defines the analytical methodology and decision rules.

---

## Core Metrics

### 1. Co-change Frequency

**Definition**:

```
frequency = number of co-changes / total commits modifying the target file
```

**Risk Classification**:

| Frequency Range | Risk Level | Tag               | Interpretation                                                  |
| --------------- | ---------- | ----------------- | --------------------------------------------------------------- |
| ≥ 0.70          | 🔴 HIGH    | `HIGH_COUPLING`   | Strong logical coupling; changes to one must consider the other |
| 0.40 – 0.69     | 🟡 MEDIUM  | `MEDIUM_COUPLING` | Moderate coupling; requires attention                           |
| < 0.40          | 🟢 LOW     | (no tag)          | Occasional co-change; may be coincidental                       |

---

### 2. File Type Classification

Different file types imply different coupling semantics:

| Type                | Match Pattern                           | Coupling Semantics      | Handling                                                     |
| ------------------- | --------------------------------------- | ----------------------- | ------------------------------------------------------------ |
| Test files          | `*test*`, `*spec*`, `__tests__/`        | ✅ Expected coupling     | Normal—tests should evolve with code                         |
| Configuration files | `*config*`, `.env*`, `*.yaml`, `*.json` | ⚠️ Caution              | May indicate hardcoding or config management issues          |
| Documentation       | `*.md`, `README*`, `CHANGELOG*`         | ✅ Expected coupling     | Normal—docs should track code                                |
| Production code     | Others                                  | ❓ Needs deeper analysis | High frequency without physical dependency = hidden coupling |

---

### 3. Temporal Decay Factor

Recent co-changes are more indicative than older ones:

| Time Range   | Suggested Weight | Rationale                                      |
| ------------ | ---------------- | ---------------------------------------------- |
| Last 30 days | 1.5×             | Recent change patterns reflect current reality |
| 30–90 days   | 1.0×             | Baseline weight                                |
| 90–180 days  | 0.7×             | Historical reference                           |
| > 180 days   | 0.5×             | Low relevance                                  |

**Note**: The current script does not yet apply temporal decay; this is a future enhancement.

---

## Recommendation Generation Rules

### Rule 1: High-Frequency Coupling in Production Code

**Conditions**:

* Both files are production code
* Co-change frequency ≥ 0.70
* No physical dependency (A does not import B)

**Recommendations**:

* “Consider merging A and B into the same module”
* “Or extract a shared interface and make dependencies explicit”
* “Check for hidden data or state dependencies”

---

### Rule 2: Cross-Module High Coupling

**Conditions**:

* Files reside in different directories/modules
* Co-change frequency ≥ 0.50

**Recommendations**:

* “Module boundaries may need redefinition”
* “Consider event-based or message-based decoupling”
* “Evaluate whether these files belong in the same module”

---

### Rule 3: High Coupling with Configuration Files

**Conditions**:

* Production code ↔ configuration file co-change frequency ≥ 0.30

**Recommendations**:

* “Check for hardcoded configuration values”
* “Consider environment variables or a config service”
* “Review the configuration management strategy”

---

### Rule 4: Test Coverage Signal

**Conditions**:

* Production code has no corresponding test files co-changing
* OR test co-change frequency < 0.30

**Recommendations**:

* “This file may lack sufficient test coverage”
* “Changes without test updates increase regression risk”

---

### Rule 5: Single-Owner Risk

**Conditions**:

* More than 80% of changes to a file are made by a single author

**Recommendations**:

* “Knowledge silo risk detected”
* “Recommend knowledge transfer or enforced code reviews”

---

## Output Structure

### Standard Output Schema

```json
{
  "target_file": "file being analyzed",
  "analysis_period_days": 180,
  "total_commits_modifying_target": 25,
  "co_changed_files": [
    {
      "file": "path to co-changed file",
      "co_change_count": 20,
      "frequency": 0.80,
      "warning": "HIGH_COUPLING|MEDIUM_COUPLING",
      "category": "PRODUCTION|TEST_FILE|CONFIG_FILE|DOC_FILE"
    }
  ],
  "last_modified_date": "2024-12-20",
  "primary_authors": ["Alice", "Bob"],
  "analysis": {
    "high_risk_files": ["list of high-risk files"],
    "recommendations": ["actionable recommendations"]
  }
}
```

---

### Recommendation Quality Guidelines

Recommendations must be **actionable**, not vague.

✅ **Good examples**:

* “Consider merging `login.ts` and `session.ts` since they are modified together in 80% of commits”
* “`config.ts` frequently changes with multiple production files—evaluate environment-based configuration”

❌ **Bad examples**:

* “There is coupling” (too vague)
* “Needs attention” (no guidance)

---

## Edge Case Handling

### Large Refactor Commits

**Problem**: Large refactors touching many files can skew results.

**Mitigation**:

* Identify commits modifying more than 20 files
* Either ignore them or down-weight their impact

**Current status**: Not implemented; future enhancement.

---

### New Files

**Problem**: Insufficient history for analysis.

**Mitigation**:

* If file age < 30 days, mark analysis as “insufficient data”
* Recommend revisiting after more history accrues

---

### Deleted Files

**Problem**: Historical data references files that no longer exist.

**Mitigation**:

* Mark as `[DELETED]` in output
* Still report historical coupling relationships

---

## Integration with Other Skills

1. **With `build-inspector`**:

   * Build-inspector reveals build boundaries and version-skew risks
   * Git-forensics reveals logical coupling
   * Intersection = **cross-build-root coupling (highest risk)**

2. **With `runtime-inspector`**:

   * Inspect IPC-related code within highly coupled files
   * These carry elevated protocol drift risk

3. **As input to Scout conflict analysis**:

   * High-risk file list
   * Generated recommendations
   * Coupling graph data for reporting

---

