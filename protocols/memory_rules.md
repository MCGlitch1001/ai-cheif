# Protocol: Memory Rules & Hygiene

## 1. Core Architectural Tenet
All memory in AI-Chief is persisted solely in human-readable, Git-versioned Markdown files within the `memory/` directory tree and staging areas in `runtime/tasks/`. There are zero databases, key-value stores, or external microservices.

---

## 2. Memory Tier Classification

AI-Chief establishes three distinct tiers of data durability:

```
┌─────────────────────────────────────────────────────────────┐
│                      PERMANENT MEMORY                       │
│ - Agent instructions (agents/)                              │
│ - User preferences (memory/chief/preferences.md)            │
│ - Architecture decisions (memory/manager/decisions.md)      │
│ ──► NEVER COMPRESSED OR DELETED                             │
├─────────────────────────────────────────────────────────────┤
│                    COMPRESSIBLE MEMORY                      │
│ - Old conversations (memory/chief/conversation_state.md)    │
│ - Worker execution reports (ingested by Manager)            │
│ - Temporary logs & task lists (memory/manager/active_tasks) │
│ ──► PERIODICALLY COMPACTED BY MANAGER & CLEANER             │
├─────────────────────────────────────────────────────────────┤
│                     TEMPORARY MEMORY                        │
│ - Staging files in runtime/tasks/                           │
│ ──► PURGED AFTER TASK INTEGRATION OR VIA /chief-reset       │
└─────────────────────────────────────────────────────────────┘
```

---

## 3. Memory Zone Breakdown

### 3.1 Permanent Memory (Zero-Loss Immutables)
These files represent system law, user constraints, and core architectural records.
- **Agent Instructions (`agents/`):** [`chief.md`](file:///home/ishaan/Work/ai-chief/agents/chief.md), [`manager.md`](file:///home/ishaan/Work/ai-chief/agents/manager.md), [`worker.md`](file:///home/ishaan/Work/ai-chief/agents/worker.md), [`cleaner.md`](file:///home/ishaan/Work/ai-chief/agents/cleaner.md).
- **User Preferences:** [`memory/chief/preferences.md`](file:///home/ishaan/Work/ai-chief/memory/chief/preferences.md) (Tone, response length, guidelines).
- **Architecture Decisions (ADRs):** [`memory/manager/decisions.md`](file:///home/ishaan/Work/ai-chief/memory/manager/decisions.md) (All technical decisions must remain intact for future reference).
- **Project Overview:** [`memory/project/overview.md`](file:///home/ishaan/Work/ai-chief/memory/project/overview.md) (Core domain context and stack definition).

### 3.2 Compressible Memory (Targeted Compaction)
These files capture operational history and may be condensed when context thresholds are reached:
- **Old Conversations:** [`memory/chief/conversation_state.md`](file:///home/ishaan/Work/ai-chief/memory/chief/conversation_state.md) is consolidated into high-level summary paragraphs when older than 5 conversation turns.
- **Worker Reports:** Verbose reports from workers (compiler output, test dumps) are absorbed by Manager, stripped of noise, and condensed into 4-line status records.
- **Temporary Logs & Task Lists:** Completed tasks in [`memory/manager/active_tasks.md`](file:///home/ishaan/Work/ai-chief/memory/manager/active_tasks.md) are consolidated into milestone bullet points.

### 3.3 Temporary Memory (Ephemeral Staging)
- **Runtime Task Files:** [`runtime/tasks/`](file:///home/ishaan/Work/ai-chief/runtime/tasks/) contains single-task instructions (`task-<id>.md`) used exclusively during active worker execution.
- Once completed and incorporated into `project_state.md`, these files can be safely deleted manually, via `/chief-reset`, or by the Cleaner Agent.

---

## 4. Cleaner Agent Trigger Conditions

Memory maintenance is triggered when any of the following occur:
1. The user issues `/chief-clean`.
2. `memory/manager/active_tasks.md` contains more than **15 completed tasks**.
3. Total line count across `memory/` exceeds **1,000 lines**.
4. A major release or milestone is finalized.

---

## 5. Sanitization Safeguards
When Cleaner executes:
1. It **must** verify that `preferences.md` and `decisions.md` have not been altered or deleted.
2. It **must not** delete tasks currently marked `IN_PROGRESS` or `QUEUED`.
3. It **must** write an audit entry summarizing lines freed, duplicate entries removed, and files archived.
