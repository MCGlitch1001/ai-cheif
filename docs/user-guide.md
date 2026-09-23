# AI-Chief User Guide

Welcome to **AI-Chief**! This guide covers everything you need to orchestrate software engineering tasks using the 3-tier AI-Chief framework.

---

## 1. What is AI-Chief?

AI-Chief is a portable, tool-agnostic AI agent orchestration framework. It acts as an intelligent communication and execution barrier between you (the human developer) and AI coding agents.

Instead of talking directly to an LLM that reads your entire repo, outputs 500 lines of messy terminal dumps, gets confused, and hallucinates, AI-Chief divides work into three clear tiers:
1. **Chief Agent:** Your personal assistant. Speaks clearly, stays concise (<10 sentences), never writes code, and never dumps logs.
2. **Manager Agent:** Your engineering lead. Plans tasks, bounds context, orchestrates workers, compresses outputs, and updates persistent memory.
3. **Worker Agent:** Your disposable execution engine. Spawns per task, writes code, runs tests, inspects files, and reports back before self-terminating.

---

## 2. First Activation

To activate AI-Chief in any AI coding environment (Antigravity, Claude Code, Cursor, Codex, etc.), simply type:

```text
/chief
```

*What happens:*
- The AI loads [`SKILL.md`](file:///home/ishaan/Work/ai-chief/SKILL.md) and [`agents/chief.md`](file:///home/ishaan/Work/ai-chief/agents/chief.md).
- The AI adopts the Chief persona.
- The Chief greets you and asks how it can direct the engineering team today.

---

## 3. Core Lifecycle Example

Let's walk through an end-to-end user request:

### User Prompt:
```text
/chief add authentication to my Express app using JWT
```

### The 5-Step Under-The-Hood Pipeline:
1. **Chief Receives Request:** Chief parses your intent, replies with a brief acknowledgment, and delegates an internal directive to the Manager Agent. Chief **never** attempts to code or inspect files directly.
2. **Manager Plans & Bounds Context:** Manager inspects [`memory/manager/project_state.md`](file:///home/ishaan/Work/ai-chief/memory/manager/project_state.md) and [`memory/manager/decisions.md`](file:///home/ishaan/Work/ai-chief/memory/manager/decisions.md), curates the exact files needed (`src/auth.ts`, `src/server.ts`, `test/auth.test.ts`), and writes `runtime/tasks/task-001.md`.
3. **Worker Executes:** A disposable Worker is spawned. It writes the JWT middleware, runs `npm test`, handles token expiration, and creates a structured 5-point report (`COMPLETED`, `CHANGED`, `TESTS`, `ISSUES`, `NEXT STEPS`).
4. **Manager Compresses Output:** Manager ingests the Worker's test logs, filters out noise, records the ADR in memory, and produces a 4-line status digest (`STATUS`, `DONE`, `IMPORTANT`, `NEXT`).
5. **Chief Delivers Result:** Chief translates the status digest into a polite, natural explanation in under 10 sentences.

---

## 4. Complete Command Reference

AI-Chief provides 8 commands that work consistently across all AI tools:

### `/chief [request]`
- **Description:** Normal AI-Chief activation. Loads framework files from disk and initiates work.
- **When to use:** Starting a project, session, or assigning a new development goal.
- **Example:** `/chief set up Tailwind CSS and configure dark mode`

### `/chief+ [request]`
- **Description:** Full framework reload. Forces a complete cold-start reload of all instructions, protocols, and memory files from disk.
- **When to use:** In long conversations, when context compression occurs, or if the AI forgets its persona/rules.
- **Example:** `/chief+ refactor the authentication layer`

### `/chief-status`
- **Description:** Returns a snapshot of the current milestone and active tasks.
- **When to use:** Checking what was done recently and what tasks are in flight.
- **Example:** `/chief-status`

### `/chief-plan [goal]`
- **Description:** Plans tasks and identifies architectural impacts without executing any code.
- **When to use:** When you want to review proposed changes before authorizing code modifications.
- **Example:** `/chief-plan migrate database from SQLite to PostgreSQL`

### `/chief-build [task or goal]`
- **Description:** Authorizes the Manager to dispatch Worker agents to execute code changes.
- **When to use:** After reviewing a plan, or when you are ready for code to be written.
- **Example:** `/chief-build TASK-001` or `/chief-build implement PostgreSQL client connection`

### `/chief-clean`
- **Description:** Activates the Cleaner Agent to deduplicate and compact memory.
- **When to use:** When project tasks have accumulated or when memory files feel too large.
- **Example:** `/chief-clean`

### `/chief-memory`
- **Description:** Displays stored project overview, user preferences, and ADR decisions.
- **When to use:** Reviewing what the AI remembers about your preferences and system architecture.
- **Example:** `/chief-memory`

### `/chief-reset`
- **Description:** Clears temporary task files in `runtime/tasks/` while preserving permanent memory.
- **When to use:** Clearing scratchpad files or resetting after aborted tasks.
- **Example:** `/chief-reset`
