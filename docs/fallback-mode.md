# Single-Agent Architecture & Mode Transition

## 1. Overview
In AI-Chief, **single-agent execution is the native, default operating model**.

Rather than requiring complex multi-agent frameworks, separate processes, or simulating artificial agent conversations (*"Chief says...", "Manager says...", "Worker says..."*), AI-Chief operates as **one unified AI assistant** running a disciplined 7-stage internal execution pipeline.

---

## 2. Core Principle: One AI, Disciplined Internal Pipeline

AI-Chief provides the rigor and context protection of multi-tiered workflows without conversational clutter:
- **Unified Persona:** Speaks with one professional, high-signal voice.
- **Internal Execution:** Context bounding, planning, code modifications, testing, and compression happen internally.
- **Predictable Output:** Every technical deliverable adheres to the 4-field standard response schema: `Summary`, `Changes`, `Status`, `Next`.
- **Filesystem Persistence:** Memory, architectural decisions (ADRs), and user preferences are written to disk in [`memory/`](file:///home/ishaan/Work/ai-chief/memory/).

---

## 3. How the Single-Agent Pipeline Operates

In every conversation turn, the AI assistant internally transitions through 7 deterministic phases:

```
[Phase 1: Parse Intent]
Analyze primary user goal, edge cases, and technical constraints.

[Phase 2: Context & Config Ingestion]
Read core/config.md, user preferences, and ADRs from memory/.

[Phase 3: Context Bounding]
Identify and inspect strictly the minimal set of files required for the task.

[Phase 4: Internal Planning]
Formulate an atomic implementation sequence. (Halt here if /chief-plan).

[Phase 5: Modular Execution]
Perform modular, defensive code changes adhering to project conventions.

[Phase 6: Verification & Testing]
Run test suites, linters, or syntax checks internally to ensure zero regressions.

[Phase 7: Semantic Compression & Response Delivery]
Filter raw logs and deliver the standard 4-field response (Summary, Changes, Status, Next).
```

---

## 4. Transitioning to Native Multi-Agent Orchestration

On platforms that feature **native, concurrent subagent execution engines** (such as Google Antigravity with `invoke_subagent`), AI-Chief can optionally distribute its internal pipeline stages into isolated background subagent processes.

In native multi-agent mode:
- The parent session acts as the human interface and context controller.
- Tasks are staged in `runtime/tasks/` and dispatched to disposable worker subagents.
- Inter-agent messages remain completely internal to the tool layer.
- The human user continues to receive the clean 4-field response format without conversational noise.

See [`docs/advanced/native-agents.md`](file:///home/ishaan/Work/ai-chief/docs/advanced/native-agents.md) for complete details on configuring native subagent environments.
