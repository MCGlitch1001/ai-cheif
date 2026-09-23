# Protocol: Inter-Agent Delegation & Reporting

## 1. Purpose
This protocol governs all communications across the agent hierarchy in AI-Chief. By enforcing rigid message headers, agents eliminate ambiguity, isolate context, and ensure predictable execution flows.

---

## 2. Delegation Flow Overview

```
Human User
    │
    │ (Natural, conversational request)
    ▼
Chief Agent
    │
    │ (Goal delegation)
    ▼
Manager Agent
    │
    │ (Manager → Worker Protocol)
    ▼
Disposable Worker Agent
    │
    │ (Worker → Manager Protocol)
    ▼
Manager Agent (Compression & Memory Sync)
    │
    │ (Manager → Chief Protocol)
    ▼
Chief Agent
    │
    │ (Concise conversational response)
    ▼
Human User
```

---

## 3. Protocol Specifications

### 3.1 Manager → Worker Protocol
Used when Manager assigns an execution task to a disposable Worker.

```markdown
TASK: [Clear, single-objective task title]
CONTEXT: [Essential background, why this is being done, and architectural constraints]
FILES: [Comma-separated absolute or workspace-relative file paths to inspect or edit]
EXPECTED RESULT: [Deterministic verification criteria for success]
LIMITATIONS: [Forbidden packages, out-of-scope files, breaking changes to avoid]
```

#### Field Rules:
- `TASK:` Must be actionable and atomic. If multiple unrelated tasks exist, spawn separate Worker runs.
- `CONTEXT:` Only include context necessary for this specific task.
- `FILES:` List primary files. The worker may explore adjacent files if needed, but primary focus is constrained here.
- `EXPECTED RESULT:` Must be testable (e.g. "Unit test passes", "CLI command returns code 0").
- `LIMITATIONS:` Explicit guardrails to prevent regressions or scope creep.

---

### 3.2 Worker → Manager Protocol
Used when a Worker completes its execution and reports back to Manager.

```markdown
COMPLETED: [Summary of actions taken and core objective status]
CHANGED: [Explicit list of files created, modified, or deleted]
TESTS: [Test commands executed and test suite results (PASSED/FAILED)]
ISSUES: [Blockers, unexpected errors, or edge cases discovered during execution]
NEXT STEPS: [Immediate technical follow-ups or next recommended tasks]
```

#### Field Rules:
- `COMPLETED:` Plain technical summary of changes made.
- `CHANGED:` Format as `path/to/file.ext (NEW | MODIFIED | DELETED)`.
- `ISSUES:` Note if any behavior was unexpected or if additional credentials/dependencies are required. If none, state "None".
- `TESTS:` Include the exact command run (e.g. `npm test`, `pytest tests/test_auth.py`) and result count.
- `NEXT STEPS:` Suggest concrete technical follow-ups for Manager to review.

---

### 3.3 Manager → Chief Protocol
Used when Manager has processed the Worker output, compressed findings, updated memory, and reports back to Chief.

```markdown
STATUS: [SUCCESS | IN_PROGRESS | BLOCKED | FAILED]
DONE: [High-level summary of what was accomplished]
IMPORTANT: [Decisions made, environmental requirements, or risks the user must know]
NEXT: [Proposed next step or question for user confirmation]
```

#### Field Rules:
- `STATUS:` Must be one of `SUCCESS`, `IN_PROGRESS`, `BLOCKED`, or `FAILED`.
- `DONE:` 1-2 high-level sentences capturing the milestone achievement.
- `IMPORTANT:` Any configuration needed from the human (e.g., API keys, environment variables) or major architectural choices.
- `NEXT:` Concrete next action proposed.

---

## 4. Chief → Human Delivery Rule
Chief **MUST NEVER** display raw protocol tags (`STATUS:`, `DONE:`, `TASK:`, etc.) to the user. Chief reads the Manager's update and writes a polite, conversational response in fewer than 10 sentences.
