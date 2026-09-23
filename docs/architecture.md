# Architecture Whitepaper: AI-Chief

## 1. Abstract
As autonomous coding agents grow in capability, monolithic agents that handle human interaction, multi-file code editing, package management, and system architecture within a single context window inevitably fail. The failure modes include context pollution, lost instructions, hallucinated edits, and poor user communication.

**AI-Chief** introduces an ergonomic, tool-agnostic, 3-tier layered agent framework. By decoupling the human conversation layer (**Chief**) from the context orchestration and memory layer (**Manager**) and the disposable execution engines (**Workers**), AI-Chief guarantees crisp human communication, strict context protection, and clean, auditable technical execution.

---

## 2. Core Architectural Stratification

```
┌────────────────────────────────────────────────────────┐
│                      HUMAN USER                        │
└──────────────────────────┬─────────────────────────────┘
                           │ Conversational updates (<10 sentences)
                           ▼
┌────────────────────────────────────────────────────────┐
│                     CHIEF AGENT                        │
│ - Natural language persona                             │
│ - Empathy & intent extraction                          │
│ - Zero code generation / zero log dumps                │
│ - Reads: memory/chief/preferences.md                   │
└──────────────────────────┬─────────────────────────────┘
                           │ Goal Delegation
                           ▼
┌────────────────────────────────────────────────────────┐
│                    MANAGER AGENT                       │
│ - Permanent intelligence layer                         │
│ - Task decomposition & context bounding                │
│ - Compression of worker execution outputs              │
│ - Manages memory/manager/ & memory/project/            │
└────────────┬─────────────────────────────▲─────────────┘
             │ Task Payload                │ Compressed
             │ (Bounded Context)           │ Report
             ▼                             │
┌──────────────────────────┐               │
│ DISPOSABLE WORKER AGENT  │───────────────┘
│ - Code & tool execution  │
│ - Ephemeral lifecycle    │
│ - Zero persistent state  │
└──────────────────────────┘
```

---

## 3. Component Deep Dive

### 3.1 Chief Agent (Human Interface)
The Chief Agent is the outward-facing ambassador.
- **Intent Filtration:** Strips conversational ambiguities and synthesizes actionable goals.
- **Tone Guard:** Protects the human from technical friction (stack traces, git conflicts, test runner logs).
- **Execution Firewall:** Never directly triggers file modifications or shell tools.

### 3.2 Manager Agent (Context Controller)
The Manager Agent is the system's brain and context router.
- **Boundary Formulation:** Rather than passing an entire codebase to a worker, the Manager selects only the targeted files, symbols, and constraints.
- **Semantic Compression:** Workers generate vast volumes of raw text (e.g., `npm test` output with 200 passing lines). Manager filters this down to high-signal summaries.
- **Memory Persistence:** Writes architectural choices into `memory/manager/decisions.md` (ADRs) and updates task milestones in `memory/manager/project_state.md`.

### 3.3 Worker Agent (Disposable Execution Engine)
Workers are stateless, disposable worker bees.
- **Lifecycle:** Born with a single task payload (`runtime/tasks/task-<id>.md`), execute their objective using code editing, terminal, and debugging tools, write a structured report (`templates/worker_report.md`), and terminate.
- **Isolation:** A worker failure or crash does not taint the conversation context of Chief or the planning state of Manager.

### 3.4 Cleaner Agent (Hygiene & Compaction)
The Cleaner Agent runs asynchronously or periodically to keep the repository's Markdown memory lean.
- **Pruning:** Consolidates closed tasks, removes old runtime task files, and deduplicates conversation state.
- **Safety Rails:** Strictly prevented from touching user preferences, ADRs, or agent prompt definitions.

---

## 4. Token Economics & Context Protection

In a standard monolithic agent workflow:
- Turn 1: 2,000 tokens (System prompt + user request)
- Turn 2: 12,000 tokens (Read 4 files + build command)
- Turn 3: 25,000 tokens (Test failure traceback + fix attempt)
- Turn 10: 80,000+ tokens (Degraded reasoning, high cost, loss of early instructions)

In **AI-Chief**:
- **Chief Context:** Stays consistently under 2,000 tokens (pure human conversation).
- **Manager Context:** Retains only curated project state, ADRs, and high-level milestones (~3,000 - 5,000 tokens).
- **Worker Context:** Fresh, pristine context for every task (~4,000 - 8,000 tokens). Terminated immediately upon report delivery.

---

## 5. Portability & Zero-Dependency Philosophy

AI-Chief requires:
- No database engine (PostgreSQL, SQLite, ChromaDB, etc.)
- No backend server process or daemon
- No Node.js / Python framework installation
- No proprietary API bindings

Any system capable of reading and editing files (such as Claude Code, OpenAI Codex, Google Antigravity, Cursor, or Aider) can run AI-Chief by navigating the directory and following the role instructions.
