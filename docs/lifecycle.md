# The AI-Chief Execution Lifecycle

## 1. Lifecycle Overview
The AI-Chief execution lifecycle describes the exact step-by-step path a user request takes from initial prompt to final high-signal delivery. Rather than dumping raw reasoning, intermediate tool chatter, or multi-agent role tags into the chat, AI-Chief executes a disciplined **7-stage internal pipeline**:

```
[1. User Request: /chief Add payments to my app]
                 │
                 ▼
[2. Context & Config Check] ──► Reads core/config.md, preferences, decisions
                 │
                 ▼
[3. Context Bounding] ────────► Selects minimal necessary files to inspect/modify
                 │
                 ▼
[4. Internal Planning] ───────► Formulates atomic steps (stops here if /chief-plan)
                 │
                 ▼
[5. Modular Execution] ───────► Edits files, implements features, runs commands
                 │
                 ▼
[6. Verification & Testing] ──► Validates with test suites, linters, or dry-runs
                 │
                 ▼
[7. Semantic Compression] ────► Delivers clean 4-field response (Summary/Changes/Status/Next)
```

---

## 2. Detailed Lifecycle Stages

### Stage 1: User Sends Request
The interaction begins when the user issues a request, optionally prefixed with `/chief` or a specialized command:

```text
/chief Add payments to my app using Stripe checkout
```

---

### Stage 2: Context & Config Ingestion
Before writing any code or searching files, AI-Chief inspects its persistent instructions and memory on disk:
- **User Settings:** [`core/config.md`](file:///home/ishaan/Work/ai-chief/core/config.md) (Communication tone, sentence limit, risk level).
- **User Preferences:** [`memory/chief/preferences.md`](file:///home/ishaan/Work/ai-chief/memory/chief/preferences.md).
- **Architectural Decisions (ADRs):** [`memory/manager/decisions.md`](file:///home/ishaan/Work/ai-chief/memory/manager/decisions.md).
- **Project State:** [`memory/manager/project_state.md`](file:///home/ishaan/Work/ai-chief/memory/manager/project_state.md).

---

### Stage 3: Context Bounding
To prevent context window saturation and token exhaustion, AI-Chief explicitly isolates the minimal file set needed:
- Identifies relevant source files, types, and configs.
- Avoids indiscriminate repository sweeps.
- Keeps token usage efficient and prevents hallucination.

---

### Stage 4: Internal Planning
AI-Chief drafts an atomic, minimal-dependency execution roadmap:
- Breaks large requirements into verifiable steps.
- Identifies potential edge cases, breaking changes, or missing environment secrets.
- **Planning Mode Trigger:** If the command is `/chief-plan`, AI-Chief saves the proposed roadmap to `memory/manager/active_tasks.md`, formats the plan for the user, and **halts execution without touching source code**.

---

### Stage 5: Modular Execution
When execution is authorized (`/chief` or `/chief-build`):
- Writes modular, defensive, well-commented code.
- Adheres strictly to project conventions and existing style guides.
- Avoids monolithic edits; modifies targeted code blocks cleanly.

---

### Stage 6: Verification & Testing
Before declaring a task done, AI-Chief runs verification:
- Executes unit tests (`npm test`, `pytest`, `cargo test`, `go test`).
- Runs type-checking or linter passes if available.
- Debugs and fixes any runtime errors encountered during testing.

---

### Stage 7: Semantic Compression & Response Delivery
AI-Chief absorbs intermediate logs and formats the final deliverable into the standard 4-field schema:

```markdown
Summary:
[High-level, plain-English summary of what was accomplished]

Changes:
- [File path or action taken]
- [File path or action taken]

Status:
[Completed | Needs attention | Blocked]

Next:
[Optional recommended next technical action or question for confirmation]
```

---

## 3. Advanced Multi-Agent Adaptation

On platforms featuring native background subagents (such as Google Antigravity `invoke_subagent`), this 7-stage lifecycle can optionally be distributed across subagent processes:
- Stages 1–3: Handled by Orchestrator / Manager.
- Stages 4–6: Handled by disposable Worker subagent.
- Stage 7: Handled by Manager compression and Chief user interface.

See [`docs/advanced/native-agents.md`](file:///home/ishaan/Work/ai-chief/docs/advanced/native-agents.md) for complete details.
