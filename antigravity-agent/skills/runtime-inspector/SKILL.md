---
name: runtime-inspector
description: Analyze runtime behavior, process boundaries, and IPC mechanisms to detect protocol drift risks and process lifecycle issues.
---

# The Wiretapper’s Casebook

> “Code can lie, but processes don’t. One `.spawn()` reveals more than a thousand lines of comments.”
> — Veteran Wiretapper’s Saying

The purpose of this skill is to **trace inter-process communication lines**.

**Master rule**:
If two processes talk to each other, but no one defines **what language**, **which version**, or **what format** they speak, you are sitting on a delayed **protocol mismatch** disaster.

---

## ⚠️ Mandatory Deep Thinking

> [!IMPORTANT]
> Before performing any analysis, you **must** invoke the `mcp_sequential-thinking_sequentialthinking` tool and reason for **3–10 steps**, or more if needed.
>
> Example thinking prompts:
>
> 1. “How many entry points (`main` functions) does this project have? One process or many?”
> 2. “How do processes communicate—pipes, HTTP, shared databases?”
> 3. “If I update only process A’s communication module, will process B crash? Is there a version handshake?”

---

## ⚡ Mission Objective

Identify **runtime boundaries** and **communication contracts**.

---

## 🧭 Investigation Process

### Step 1: Identify Entry Points

Each `main` function potentially represents a separate process.

**Search targets**:

* Rust: `fn main()`, `#[tokio::main]`
* Python: `if __name__ == "__main__":`
* Node.js: `require.main === module`, `bin` in `package.json`
* Go: `func main()`

**Veteran instinct**:
If you find multiple entry points, immediately ask:
“Do these run independently, or are they managed by a parent process?”

---

### Step 2: Trace Spawning Chains

If process A launches process B, that is a **line of descent**.

**Search targets**:

* Rust: `Command::new(...)`, `std::process::Stdio`, `tauri-plugin-shell`
* Python: `subprocess.Popen`, `multiprocessing.Process`
* Node.js: `child_process.spawn`, `child_process.fork`

**Lifecycle risk warnings**:

* ⚠️ “If the parent dies, does the child know? Is there a heartbeat?”
  → **Zombie process risk**
* ⚠️ “If the child crashes, does the parent restart it—or fail silently?”
  → **Silent failure risk**

---

### Step 3: Tap the Wire

How do processes “talk”? Where is the protocol defined?

**Search for channels**:

* `Pipe`, `NamedPipe`, `unix_stream`, `zmq`
* `TcpListener`, `UdpSocket`, `websocket`, `http::server`

**Search for protocols**:

* `Handshake`, `Version`, `MagicBytes`, `schema`
* `protobuf`, `serde_json`, `JSON.parse`, `enum Message`

**Contract status assessment**:

| Observation                                     |     Status    | Recommendation                                |
| ----------------------------------------------- | :-----------: | --------------------------------------------- |
| Channel + `enum Message` or Protobuf definition | 🟢 **Strong** | Explicit contract exists; relatively safe     |
| Channel + version or handshake check            | 🟢 **Strong** | Version negotiation in place; good practice   |
| Channel + raw JSON / strings only               |  🟡 **Weak**  | No explicit contract; changes may break peers |
| Channel with no protocol definition             |  🔴 **None**  | **Communication black hole** — high risk      |

---

## 🛡️ IPC Risk Pattern Quick Reference

(from security research)

| Risk Pattern                          | Detection Signal                                | Recommendation                                   |
| ------------------------------------- | ----------------------------------------------- | ------------------------------------------------ |
| **Protocol mismatch**                 | Channel exists, no handshake/version            | Force version handshake tasks in future planning |
| **Zombie processes**                  | `spawn` without kill-on-drop or heartbeat       | Flag lifecycle management risk                   |
| **Single point of failure (SPOF)**    | One process controls all IPC                    | Add reconnection or restart logic                |
| **Named Pipe permission漏洞 (Windows)** | Named Pipe without explicit security descriptor | 🔴 High risk: default ACL may allow Everyone     |
| **Race conditions**                   | Rapid IPC without ordering guarantees           | Add message sequencing or locking                |

---

## 📤 Required Output

Your analysis must include:

1. **Process Roots**
   Discovered entry points (file paths and roles)
2. **Spawning Chains**
   Process relationships (A spawns B)
3. **IPC Surfaces**
   Communication channels (type, keywords, locations)
4. **Contract Status**
   `[Strong / Weak / None]` with justification
5. **Lifecycle Risks**
   Zombie processes, silent crashes, etc.
6. **Security Flags (Windows)**
   For Named Pipes: whether ACLs are explicitly set
