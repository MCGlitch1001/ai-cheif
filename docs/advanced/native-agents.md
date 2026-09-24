# Advanced: Native Multi-Agent Orchestration

AI-Chief is designed by default as a **single-agent AI operating system** that runs deterministically inside any AI coding harness (Claude Code, Google Antigravity, OpenAI Codex, Cursor, Windsurf) without requiring multi-agent infrastructure.

However, on platforms that feature **native, concurrent subagent execution engines** (such as Google Antigravity with `invoke_subagent`, AutoGen, or CrewAI), AI-Chief can optionally elevate its internal execution stages into isolated, specialized subagent processes.

---

## 1. Native vs. Simulated Multi-Agent

| Feature | Standard AI-Chief (Default) | Native Multi-Agent Extension |
| :--- | :--- | :--- |
| **Execution Model** | Single AI with 7-stage internal operating pipeline | Parent orchestrator spawning parallel subagents |
| **Conversational Output** | Single unified voice; zero role tags | Single unified human interface; subagents talk via tool channels |
| **Platform Requirement** | Any LLM or coding harness (Zero dependencies) | Harness with subagent IPC (`invoke_subagent`, process forks) |
| **Worker Context** | Bounded in prompt context window | Completely isolated child context window |

> [!IMPORTANT]
> **No Fake Agent Dialogue:** Even when native subagents are running in the background, inter-agent chatter is contained within internal tool messages. The human user never sees fake dialogues such as *"Chief says..."*, *"Manager says..."*, or *"Worker says..."*. The user always receives a concise, human-friendly update.

---

## 2. Advanced Multi-Agent Stratification

When running in native multi-agent mode, the roles defined in [`agents/`](file:///home/ishaan/Work/ai-chief/agents/) map to runtime subagents:

### Tier 1: Chief (Parent / Orchestrator)
- **Host Process:** Main user-facing session.
- **Responsibility:** Ingests human requests, enforces user preferences from [`memory/chief/preferences.md`](file:///home/ishaan/Work/ai-chief/memory/chief/preferences.md), delegates to Manager, and outputs the final 4-field response (`Summary`, `Changes`, `Status`, `Next`).
- **Governing Prompt:** [`agents/chief.md`](file:///home/ishaan/Work/ai-chief/agents/chief.md)

### Tier 2: Manager (Context Controller Subagent)
- **Process:** Background orchestration agent.
- **Responsibility:** Decomposes complex user goals into atomic tasks, isolates the minimal file set, writes task specifications to [`runtime/tasks/`](file:///home/ishaan/Work/ai-chief/runtime/tasks/), spawns ephemeral Workers, and semantically compresses Worker output into high-signal status digests.
- **Governing Prompt:** [`agents/manager.md`](file:///home/ishaan/Work/ai-chief/agents/manager.md)

### Tier 3: Worker (Disposable Execution Subagent)
- **Process:** Ephemeral subagent spawned per task.
- **Responsibility:** Carries out code generation, debugging, testing, or documentation within the strict file boundaries assigned by Manager.
- **Lifecycle:** Terminated immediately upon delivering its structured execution report to Manager.
- **Governing Prompt:** [`agents/worker.md`](file:///home/ishaan/Work/ai-chief/agents/worker.md)

### Auxiliary: Cleaner (Memory Pruning Subagent)
- **Process:** Periodic or on-demand background maintenance agent (`/chief-clean`).
- **Responsibility:** Compulsorily compacts compressible task history and session logs while preserving permanent ADRs and user preferences.
- **Governing Prompt:** [`agents/cleaner.md`](file:///home/ishaan/Work/ai-chief/agents/cleaner.md)

---

## 3. Platform Integration Example: Google Antigravity

In Google Antigravity, the parent agent can invoke child subagents dynamically:

```python
# Conceptual manager invocation in an advanced AGY environment
invoke_subagent(
    Subagents=[
        {
            "TypeName": "worker",
            "Role": "Backend Engineer",
            "Prompt": "Implement JWT authentication in src/auth.ts according to runtime/tasks/task-001.md. Run npm test to verify.",
            "Workspace": "inherit",
            "Model": "inherit"
        }
    ]
)
```

1. **Manager prepares task specification** in `runtime/tasks/task-001.md`.
2. **Manager calls `invoke_subagent`** with bounded context and explicit acceptance tests.
3. **Worker executes** changes in isolation and reports completion.
4. **Manager compresses output**, updates `memory/manager/active_tasks.md`, and returns the result to Chief.
5. **Chief reports back** to the user using the standard 4-field output format.

---

## 4. The Invariant Principle

Regardless of whether AI-Chief runs as a **single-agent operating system** or as a **native multi-agent cluster**:

1. **The filesystem is the source of truth.** All states, tasks, and memory live on disk.
2. **The user sees only one clean interface.** Intermediate churn is never displayed.
3. **No runtime dependencies are required.** The architecture remains pure Markdown and standard filesystem operations.
