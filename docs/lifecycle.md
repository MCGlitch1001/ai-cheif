# The AI-Chief Execution Lifecycle

## 1. Lifecycle Overview
The AI-Chief execution lifecycle describes the exact step-by-step path a user request takes from initial prompt to final conversational delivery. The architecture guarantees that no single agent is overwhelmed, context windows remain lean, and the user experiences a calm, productive interface.

```
[1. User Request: /chief Add payments to my app]
                 │
                 ▼
[2. Chief Ingestion & Delegation] ── (Understands intent, acknowledges, delegates)
                 │
                 ▼
[3. Manager Planning & Context Bounding] ── (Reads memory, creates runtime task file)
                 │
                 ▼
[4. Worker Execution] ── (Codes, explores, tests, debugs within bounds)
                 │
                 ▼
[5. Worker Completion & Structured Report] ── (COMPLETED, CHANGED, TESTS, ISSUES, NEXT STEPS)
                 │
                 ▼
[6. Manager Semantic Compression & Memory Sync] ── (Compresses to 4-line status, updates memory)
                 │
                 ▼
[7. Chief Human-Facing Response] ── (Conversational update, max 10 sentences)
                 │
                 ▼
[Human User Receives Polished Response]
```

---

## 2. Detailed Lifecycle Stages

### Stage 1: User Sends Request
The interaction begins when the user issues a request, optionally prefixed with `/chief` or a specialized command:

```text
/chief Add payments to my app using Stripe checkout
```

---

### Stage 2: Chief Receives Request
The **Chief Agent** acts as the front door.

#### Responsibilities:
- **Understand User Intent:** Parse the functional objective, identifying key user goals and implicit preferences.
- **Acknowledge Briefly:** Formulate an immediate, courteous acknowledgement if appropriate.
- **Create Internal Manager Request:** Package the intent into a clean delegation directive.

#### Inviolable Constraints (Chief Must NEVER):
- ❌ **Never write code:** Chief does not emit code snippets or diff blocks.
- ❌ **Never analyze the repository directly:** Chief does not inspect directory trees or parse source files.
- ❌ **Never read logs:** Chief does not read compiler traces, test outputs, or terminal dumps.

---

### Stage 3: Manager Receives Request
The **Manager Agent** is the permanent intelligence layer and context controller.

#### Responsibilities:
- **Read Project Memory:** Ingest [`memory/manager/project_state.md`](file:///home/ishaan/Work/ai-chief/memory/manager/project_state.md), [`memory/manager/decisions.md`](file:///home/ishaan/Work/ai-chief/memory/manager/decisions.md), and [`memory/project/overview.md`](file:///home/ishaan/Work/ai-chief/memory/project/overview.md).
- **Understand Architecture:** Map the request against the existing technology stack and established conventions.
- **Decide Task Requirements:** Decompose the request into an atomic, testable unit of work.
- **Select Initial Context:** Curate the specific files, symbols, and dependencies the Worker will need, avoiding prompt bloat.
- **Create Runtime Task File:** Write a structured specification to `runtime/tasks/task-<id>.md` following [`templates/task.md`](file:///home/ishaan/Work/ai-chief/templates/task.md).

---

### Stage 4: Worker Execution
The **Worker Agent** is an ephemeral, disposable execution engine instantiated specifically for the assigned task.

#### Worker Receives:
- **Task Description:** Specific goal and scope.
- **Relevant Files:** Primary paths to inspect and modify.
- **Expected Result:** Explicit, deterministic criteria for success (e.g. test pass).
- **Limitations:** Prohibited libraries, files off-limits, and architectural constraints.

#### Worker May:
- ✅ **Explore the repository:** Inspect adjacent files, types, and configs if required to solve the task.
- ✅ **Read additional files:** Trace imported symbols or dependencies.
- ✅ **Code:** Write new files, refactor existing code, and fix syntax errors.
- ✅ **Test:** Execute test suites (`npm test`, `pytest`, `cargo test`, etc.).
- ✅ **Debug:** Read compiler errors and iteratively resolve runtime failures.

#### Worker Does NOT:
- ❌ **Maintain memory:** Worker has zero persistent state across tasks.
- ❌ **Talk to the user:** Worker communicates solely with the Manager via the execution report.

---

### Stage 5: Worker Completion & Reporting
Upon completing the task, the Worker produces a structured report adhering strictly to the protocol:

```markdown
COMPLETED: [Summary of actions executed]
CHANGED: [List of files created, modified, or deleted]
TESTS: [Commands run and test results]
ISSUES: [Edge cases, remaining risks, or "None"]
NEXT STEPS: [Logical follow-up tasks]
```

The Worker then immediately self-terminates, releasing its execution context.

---

### Stage 6: Manager Compression & Memory Update
The **Manager Agent** receives the raw Worker report, which may include hundreds of lines of build traces and test output.

#### Responsibilities:
1. **Compress Worker Output:** Extract the essential takeaways and format the 4-line status digest:
   ```markdown
   STATUS: [SUCCESS | IN_PROGRESS | BLOCKED | FAILED]
   DONE: [High-level summary of what was achieved]
   IMPORTANT: [Critical decisions or prerequisites the user must know]
   NEXT: [Proposed next step]
   ```
2. **Update Persistent Memory:**
   - Update [`memory/manager/project_state.md`](file:///home/ishaan/Work/ai-chief/memory/manager/project_state.md) with milestone progress.
   - Record completed work in [`memory/manager/active_tasks.md`](file:///home/ishaan/Work/ai-chief/memory/manager/active_tasks.md).
   - If a new architectural pattern was chosen, log an ADR in [`memory/manager/decisions.md`](file:///home/ishaan/Work/ai-chief/memory/manager/decisions.md).

---

### Stage 7: Chief Response
The **Chief Agent** receives the Manager's compressed 4-line digest and translates it for the human user.

#### Responsibilities:
- **Conversational Delivery:** Explain what was completed clearly, warmly, and concisely.
- **Strict Brevity:** Maximum of **10 sentences** (typically 2 to 4 sentences).
- **Surface Actionable Items:** Alert the user to decisions or credentials needed (e.g. "Please add your Stripe API keys to `.env`").

#### Inviolable Constraints (Chief Must NEVER Expose):
- ❌ **Never expose Worker details:** No worker IDs, subagent names, or temporary logs.
- ❌ **Never expose internal protocols:** Never print `STATUS:`, `DONE:`, `TASK:`, or raw protocol blocks.
- ❌ **Never expose the agent chain:** The user experiences a cohesive conversation with Chief, not the internal mechanics of a multi-tiered pipeline.
