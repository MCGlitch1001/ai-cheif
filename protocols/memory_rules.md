# Protocol: Memory Rules & Hygiene

## 1. Core Architectural Tenet
All memory in AI-Chief is persisted solely in human-readable, Git-versioned Markdown files within the `memory/` directory tree and staging areas in `runtime/tasks/`. There are zero databases, key-value stores, or external microservices.

**The conversation is temporary. The filesystem is the source of truth.**

---

## 2. The 3-Tier Memory Hierarchy

AI-Chief establishes a rigid 3-tier memory durability hierarchy:

```
┌─────────────────────────────────────────────────────────────┐
│              TIER 1: IMMUTABLE INSTRUCTIONS                 │
│ - SKILL.md                                                  │
│ - AGENTS.md                                                 │
│ - Agent definitions (agents/chief.md, manager.md, etc.)     │
│ ──► NEVER COMPRESSED OR MODIFIED DURING SESSIONS            │
├─────────────────────────────────────────────────────────────┤
│              TIER 2: PERSISTENT KNOWLEDGE                   │
│ - Project state (memory/manager/project_state.md)           │
│ - Architectural decisions (memory/manager/decisions.md)     │
│ - User preferences (memory/chief/preferences.md)            │
│ - Project overview (memory/project/overview.md)             │
│ ──► PERSISTS ACROSS SESSIONS & TOOL SWITCHES                │
├─────────────────────────────────────────────────────────────┤
│              TIER 3: TEMPORARY CONTEXT                      │
│ - Active conversations (memory/chief/conversation_state.md) │
│ - Worker execution logs & raw diffs                         │
│ - Ephemeral runtime tasks (runtime/tasks/task-*.md)         │
│ ──► ONLY TIER 3 MAY DISAPPEAR DURING CONTEXT COMPRESSION    │
└─────────────────────────────────────────────────────────────┘
```

> [!IMPORTANT]
> **Compression Invariant:** ONLY Tier 3 may disappear or undergo lossy compression during context truncation or session resets. Tier 1 and Tier 2 must always be restored from the filesystem.

---

## 3. Tier Detailed Breakdown

### Tier 1: Immutable Instructions (System Law)
These files govern agent behavior, role constraints, and communication protocols. They are defined statically in the repository:
- **Root Governance:** [`AGENTS.md`](file:///home/ishaan/Work/ai-chief/AGENTS.md)
- **Framework Entry Point:** [`SKILL.md`](file:///home/ishaan/Work/ai-chief/SKILL.md)
- **Agent Prompts:** [`agents/chief.md`](file:///home/ishaan/Work/ai-chief/agents/chief.md), [`agents/manager.md`](file:///home/ishaan/Work/ai-chief/agents/manager.md), [`agents/worker.md`](file:///home/ishaan/Work/ai-chief/agents/worker.md), [`agents/cleaner.md`](file:///home/ishaan/Work/ai-chief/agents/cleaner.md)
- **Protocols:** All files in [`protocols/`](file:///home/ishaan/Work/ai-chief/protocols/)

### Tier 2: Persistent Knowledge (Long-Term Memory)
These files store permanent facts, user specifications, and project architectural decisions. They survive across different AI tools, context window purges, and project months:
- **User Preferences:** [`memory/chief/preferences.md`](file:///home/ishaan/Work/ai-chief/memory/chief/preferences.md) (Tone, constraints, language).
- **Decisions (ADRs):** [`memory/manager/decisions.md`](file:///home/ishaan/Work/ai-chief/memory/manager/decisions.md) (Append-only record of choices and rationales).
- **Project State:** [`memory/manager/project_state.md`](file:///home/ishaan/Work/ai-chief/memory/manager/project_state.md) (Current architecture, stack, milestones).
- **Project Overview:** [`memory/project/overview.md`](file:///home/ishaan/Work/ai-chief/memory/project/overview.md) (Core purpose, constraints).

### Tier 3: Temporary Context (Ephemeral Working Memory)
These files represent in-flight progress and disposable artifacts. They are expected to be pruned, compacted, or discarded:
- **Conversation State:** [`memory/chief/conversation_state.md`](file:///home/ishaan/Work/ai-chief/memory/chief/conversation_state.md) (Compacted into high-level summaries).
- **Worker Execution Reports:** Absorbed and compressed by Manager into high-signal summaries.
- **Active Task Registry:** Closed tasks in [`memory/manager/active_tasks.md`](file:///home/ishaan/Work/ai-chief/memory/manager/active_tasks.md) compacted into milestone bullets.
- **Runtime Tasks:** Ephemeral files in [`runtime/tasks/`](file:///home/ishaan/Work/ai-chief/runtime/tasks/) purged upon completion or via `/chief-reset`.

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
1. It **must** verify that `preferences.md` and `decisions.md` (Tier 2) remain intact.
2. It **must not** modify or delete any Tier 1 file under any circumstance.
3. It **must not** delete tasks currently marked `IN_PROGRESS` or `QUEUED`.
4. It **must** write an audit entry summarizing lines freed, duplicate entries removed, and files archived.
