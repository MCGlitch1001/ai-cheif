# Fallback Mode: Operating in Single-Agent Environments

## 1. Overview
While AI-Chief is designed to shine in multi-agent or subagent-capable environments (such as Google Antigravity or custom orchestration pipelines), **many AI coding environments only provide a single persistent conversation window** (e.g. basic chat interfaces, standard web LLMs, or single-process CLI agents).

**AI-Chief natively supports Single-Agent Fallback Mode.** In this mode, Chief, Manager, and Worker operate not as separate processes, but as **explicit, structured role switches** within the same conversation thread.

---

## 2. Core Principle: Identical Protocols, Switched Mindsets

Even in Fallback Mode:
- **The protocols remain 100% identical.**
- The model must still write bounded tasks using [`templates/task.md`](file:///home/ishaan/Work/ai-chief/templates/task.md).
- The model must still generate execution reports using [`templates/worker_report.md`](file:///home/ishaan/Work/ai-chief/templates/worker_report.md).
- The model must still compress worker outputs into 4-line status digests.
- The human user still only sees the concise Chief response (<10 sentences).

---

## 3. How Fallback Mode Works Step-by-Step

In a single-agent conversation turn, the AI model executes a three-phase cognitive loop internally:

```
[Phase A: Role = Chief]
1. Parse user input.
2. Formulate internal Chief → Manager directive.
3. Update runtime/current_agent.md -> ACTIVE_AGENT: Manager.

[Phase B: Role = Manager]
1. Read memory/manager/*.md.
2. Select files and write runtime/tasks/task-001.md.
3. Update runtime/current_agent.md -> ACTIVE_AGENT: Worker.

[Phase C: Role = Worker]
1. Read runtime/tasks/task-001.md.
2. Perform file edits and terminal test commands.
3. Formulate Worker execution report (COMPLETED, CHANGED, TESTS, ISSUES, NEXT STEPS).
4. Update runtime/current_agent.md -> ACTIVE_AGENT: Manager.

[Phase D: Role = Manager Compression]
1. Ingest Worker execution report.
2. Update memory/manager/project_state.md and active_tasks.md.
3. Produce 4-line status digest (STATUS, DONE, IMPORTANT, NEXT).
4. Update runtime/current_agent.md -> ACTIVE_AGENT: Chief.

[Phase E: Role = Chief Delivery]
1. Convert Manager digest into courteous, concise user response (<10 sentences).
2. Emit ONLY the Chief response to the user.
```

---

## 4. Runtime Simulation Tracking

To ensure transparency and state recovery, AI-Chief uses three files in `runtime/`:

1. [`runtime/current_agent.md`](file:///home/ishaan/Work/ai-chief/runtime/current_agent.md):
   ```markdown
   ACTIVE_AGENT: Chief
   CURRENT_TASK: Implementing JWT middleware
   WAITING_FOR: Human feedback on OAuth options
   LAST_ACTION: Reported task completion
   ```

2. [`runtime/active_session.md`](file:///home/ishaan/Work/ai-chief/runtime/active_session.md):
   Tracks session metadata, task counts, and execution mode (`FALLBACK`).

3. [`runtime/message_queue.md`](file:///home/ishaan/Work/ai-chief/runtime/message_queue.md):
   Buffers inter-agent payloads if an action requires multiple sequential tool calls.

---

## 5. Safeguards for Fallback Mode

To maintain the integrity of AI-Chief in single-agent environments:
1. **Never print internal churn:** Do not expose Manager task files or Worker reports directly to the user. Keep them in internal reasoning or file artifacts.
2. **Always enforce Chief constraints in final output:** The final markdown emitted to the user must be under 10 sentences, contain no code blocks, and contain no raw log dumps.
3. **Persist memory after every turn:** Always update `memory/manager/` files so that if the conversation is interrupted, the next turn resumes cleanly.
