# AI-Chief User Guide

Welcome to **AI-Chief**! This guide covers everything you need to build, test, and maintain software projects using the AI-Chief operating system framework.

---

## 1. What is AI-Chief?

AI-Chief is a portable, tool-agnostic AI operating system prompt and execution framework.

Instead of an assistant that floods your chat with internal reasoning, intermediate test logs, or fake multi-agent dialogues (*"Chief says..."*, *"Manager says..."*, *"Worker says..."*), AI-Chief runs as **one single AI assistant** with a disciplined 7-stage internal execution pipeline:

1. **Understand Intent:** Clarifies primary requirements and edge cases.
2. **Context & Config Check:** Ingests user settings in [`core/config.md`](file:///home/ishaan/Work/ai-chief/core/config.md) and persistent memory in [`memory/`](file:///home/ishaan/Work/ai-chief/memory/).
3. **Context Bounding:** Selects strictly the minimal file set needed.
4. **Internal Planning:** Formulates atomic, minimal-dependency steps.
5. **Execution:** Performs modular, defensive code changes.
6. **Verification:** Validates modifications with tests and linters.
7. **Semantic Compression:** Outputs a clean, predictable 4-field response (`Summary`, `Changes`, `Status`, `Next`).

---

## 2. Configuration & Preferences

You can customize AI-Chief's behavior without modifying the core system prompt by editing [`core/config.md`](file:///home/ishaan/Work/ai-chief/core/config.md):

- **Communication style:** `Concise` (default) or `Detailed`.
- **Maximum response length:** `10 sentences` (default).
- **Coding behavior:** `Allowed` or `Planning-only`.
- **Explain before changes:** `Yes` or `No`.
- **Risk level:** `Conservative` (default) or `Aggressive`.

---

## 3. Core Lifecycle Walkthrough

### User Request:
```text
/chief add JWT authentication to our Express routes
```

### What Happens Internally:
1. AI-Chief loads [`core/system-prompt.md`](file:///home/ishaan/Work/ai-chief/core/system-prompt.md) and checks [`core/config.md`](file:///home/ishaan/Work/ai-chief/core/config.md).
2. It bounds context to `src/routes/`, `src/middleware/`, and `tests/`.
3. It formulates an atomic plan internally.
4. It creates `src/middleware/auth.ts`, mounts it on protected routes, and writes tests.
5. It runs the test suite internally.
6. It delivers the final response in the standard 4-field schema:

```markdown
Summary:
Implemented JWT authentication middleware and applied token verification across protected API endpoints.

Changes:
- src/middleware/auth.ts (New JWT verification middleware)
- src/routes/api.ts (Mounted auth middleware)
- tests/auth.test.ts (Unit tests validating token validation and expiration)

Status:
Completed

Next:
Run `npm test` to verify your test suite, and ensure `JWT_SECRET` is defined in `.env`.
```

---

## 4. Complete Command Reference

AI-Chief provides 8 text commands that work consistently across all AI tools:

### `/chief [request]`
- **Description:** Normal AI-Chief activation. Loads framework files from disk and executes work via the 7-stage pipeline.
- **Example:** `/chief set up Tailwind CSS and configure dark mode`

### `/chief+ [request]`
- **Description:** Full framework reload. Forces a complete cold-start reload of all instructions, configurations, and memory files from disk.
- **When to use:** In long conversations, when context compression occurs, or if the model drifts from its constraints.
- **Example:** `/chief+ refactor the authentication layer`

### `/chief-status`
- **Description:** Returns a snapshot of current project state and active tasks from memory without modifying any files.
- **Example:** `/chief-status`

### `/chief-plan [goal]`
- **Description:** Plans tasks and identifies architectural impacts without executing any code.
- **When to use:** When you want to review proposed changes before authorizing code modifications.
- **Example:** `/chief-plan migrate database from SQLite to PostgreSQL`

### `/chief-build [task or goal]`
- **Description:** Executes technical changes for an approved task or goal.
- **Example:** `/chief-build implement PostgreSQL client connection`

### `/chief-clean`
- **Description:** Deduplicates and compacts compressible memory in `memory/`.
- **Example:** `/chief-clean`

### `/chief-memory`
- **Description:** Displays stored project overview, user preferences, and ADR decisions.
- **Example:** `/chief-memory`

### `/chief-reset`
- **Description:** Clears temporary task staging files in `runtime/tasks/` while preserving permanent memory.
- **Example:** `/chief-reset`

---

## 5. Standard Output Schema

Deliverables strictly adhere to:

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
