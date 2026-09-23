# Protocol: Semantic Output & Memory Compression

## 1. Objective
Context window exhaustion and signal degradation are primary points of failure in autonomous AI systems. This protocol defines the mathematical and heuristic rules for how the **Manager Agent** and **Cleaner Agent** compress large outputs and historical state without losing critical context.

---

## 2. Inviolable Compression Boundaries

### ❌ NEVER COMPRESS (Zero-Loss Immutables)
Under no circumstances may any agent compress, summarize, truncate, or alter the following files:
1. **Agent Directives & Personas:** All files in `agents/` (`chief.md`, `manager.md`, `worker.md`, `cleaner.md`).
2. **Core Operational Protocols:** All files in `protocols/` (`delegation.md`, `compression.md`, `memory_rules.md`, `response_format.md`).
3. **User Preferences:** [`memory/chief/preferences.md`](file:///home/ishaan/Work/ai-chief/memory/chief/preferences.md).
4. **Architectural Decisions (ADRs):** [`memory/manager/decisions.md`](file:///home/ishaan/Work/ai-chief/memory/manager/decisions.md) (records must remain verbatim or appended).

### ✅ CAN COMPRESS (Targeted Compaction)
The following sources of state must be compressed proactively:
1. **Raw Worker Reports:** Shell output dumps, multi-line compiler outputs, test traces.
2. **Completed Tasks:** Closed tasks in [`memory/manager/active_tasks.md`](file:///home/ishaan/Work/ai-chief/memory/manager/active_tasks.md) compacted into milestone bullet points.
3. **Historical Conversations:** Old turns in [`memory/chief/conversation_state.md`](file:///home/ishaan/Work/ai-chief/memory/chief/conversation_state.md) consolidated into a single summary paragraph.
4. **Ephemeral Runtime Files:** Task files in `runtime/tasks/` deleted after successful memory integration.

---

## 3. Worker Report Compression Algorithm (Manager)

When a Worker returns an execution report (which may contain 500+ lines of test logs or diffs):

```
Raw Worker Output (500 - 5000 tokens)
               │
               ▼
   [Step 1: Extract Artifacts]
   - Identify files created/modified
   - Extract test pass/fail status
               │
               ▼
   [Step 2: Filter Noise]
   - Strip successful standard out dumps
   - Strip intermediate compiler progress bars
   - Retain only specific error messages (if failed)
               │
               ▼
   [Step 3: Update Persistent Memory]
   - Append milestone to memory/manager/project_state.md
   - Update task status in memory/manager/active_tasks.md
               │
               ▼
   [Step 4: Generate 4-Line Digest for Chief]
   - STATUS: ...
   - DONE: ...
   - IMPORTANT: ...
   - NEXT: ... (Total: ~50-80 tokens)
```

---

## 4. Task History Compaction Algorithm (Cleaner)

When `memory/manager/active_tasks.md` contains more than 10 completed tasks:
1. Read the list of completed tasks.
2. Group tasks belonging to the same milestone or component.
3. Replace detailed task entries with a single summary bullet:
   ```markdown
   - [COMPLETED] Milestone: Auth Module (Tasks: TASK-001 through TASK-005) - JWT middleware, routes, and unit tests verified.
   ```
4. Clear corresponding files from `runtime/tasks/`.
5. Append cleanup metrics to the cleanup audit log.
