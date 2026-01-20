---
name: build-inspector
description: Analyze build-system topology to identify independent build units, multi-artifact risks, and version-drift hazards.
---

# The Surveyor’s Field Guide

> “The map is not the territory. The `Cargo.toml` you see may only be the tip of the iceberg.”
> — An old surveyor’s proverb

Static analysis looks at **code structure**.
This skill examines **engineering scaffolding**—the rules that determine *how software turns from source code into runnable artifacts*.

---

## ⚠️ Mandatory Deep Thinking

> [!IMPORTANT]
> Before performing any analysis, you **must** invoke the `mcp_sequential-thinking_sequentialthinking` tool and reason for **at least 3 steps**, and up to **10 or more** if necessary.
>
> Example prompts:
>
> 1. “Which build system does this project use? (Cargo? npm? Make? …)”
> 2. “Do I see multiple build configuration files? Are they unified or independent?”
> 3. “Are the final artifacts (exe, dll, bundle) released together, or can they be upgraded independently?”

---

## ⚡ Mission Objective

Identify **build boundaries**.

**Master Surveyor’s Law**:
If two things are built separately, *any interface between them is a ticking time bomb*—because **version skew** can occur.

---

## 🧭 Exploration Process (The Expedition)

### Step 1: Locate Base Camps

* **Command**:

  ```text
  find_by_name(pattern="Cargo.toml|package.json|go.mod|pom.xml|build.gradle|requirements.txt|CMakeLists.txt")
  ```
* **Surveyor’s intuition**:
  Each build configuration = one “camp”.
  Multiple camps = potential factionalization.

---

### Step 2: Check for Unified Command

If multiple camps are found, you **must** answer the following:

| Ecosystem | Check                                                                            | Master Surveyor’s Judgment         |
| --------- | -------------------------------------------------------------------------------- | ---------------------------------- |
| Rust      | Does the root `Cargo.toml` contain `[workspace]`? Are all members listed?        | No → 🔴 Independent kingdoms       |
| Node      | Does the root `package.json` define `workspaces`, or use Lerna / Nx / Turborepo? | No → 🔴 Independent kingdoms       |
| Go        | Is `go.work` used to coordinate modules?                                         | No → 🟡 Needs further verification |

**Veteran Warnings (from industry cases)**:

* ⚠️ **Independent Kingdoms (Polyrepo Hell)**
  Each can upgrade independently with no compatibility guarantees → dependency chaos.
* ⚠️ **Fake Monorepo (Distributed Monolith)**
  One repo, but independent builds → same risk as polyrepo.
* ⚠️ **Sidecar / Auxiliary Binaries**
  Are they packaged and deployed together with the main app?
  Same CI pipeline?
  **If a sidecar can be updated independently, IPC version handshakes are mandatory.**

---

### Step 3: Identify Artifacts

Inspect build configs to infer final outputs:

* `[[bin]]` in `Cargo.toml` → executable
* `"bin"` in `package.json` → CLI tool
* `cmd/` directory in Go → multiple commands
* `tauri.conf.json` → Tauri desktop app
  (Check `externalBin` for sidecars)

**Mandatory Questions**:

* “Do these artifacts run on the same machine?”
* “Are they produced by the **same CI pipeline** and **same version number**?”
* “Can users update only one artifact?”

---

## 🛡️ Risk Classification System (Industry-Derived)

| Pattern                                      | Risk Level | Master Surveyor’s Advice                                |
| -------------------------------------------- | :--------: | ------------------------------------------------------- |
| **Single Workspace**                         |     🟢     | Version consistency is guaranteed                       |
| **Multiple Roots + Shared Workspace**        |     🟢     | Ensure all members are actually included                |
| **Multiple Roots + No Workspace (Polyrepo)** |     🔴     | **Split-brain risk**. Severe version drift potential    |
| **Sidecar + Atomic Release**                 |     🟢     | Sidecar updated with main app; manageable               |
| **Sidecar + Non-Atomic Release**             |     🔴     | **Critical risk**. IPC-level version handshake required |

---

## 📤 Required Output

Your report must include:

1. **Build Roots**
   Paths to all discovered build configurations
2. **Topology**
   One of: `[Monolith / Workspace / Polyrepo / Distributed-Local]`
3. **Artifacts**
   List of final outputs (name, type, source)
4. **Sidecar Warning**
   If sidecars exist, clearly state release mode (atomic / non-atomic)
5. **Risks Flagged**
   Explicitly identify risks and **which artifacts may drift out of sync**

---

