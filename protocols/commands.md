# Protocol: Command System

## 1. Overview
The AI-Chief Command System provides a uniform, portable interface for human users and AI coding environments to interact with the orchestration hierarchy. Because AI-Chief is tool-agnostic, these commands operate purely via text triggers that any LLM or AI coding agent (Claude Code, Google Antigravity, OpenAI Codex, Cursor, Windsurf) can recognize and handle.

---

## 2. Command Specifications

### `/chief [message]`
- **Purpose:** Activates AI-Chief mode and assigns the Chief persona to the AI.
- **Behavior:**
  1. Loads directives from [`SKILL.md`](file:///home/ishaan/Work/ai-chief/SKILL.md) and [`agents/chief.md`](file:///home/ishaan/Work/ai-chief/agents/chief.md).
  2. Adopts the Chief Agent persona (conversational, empathetic, concise <10 sentences, no code blocks or log dumps).
  3. Parses the user's message, formulates an internal Manager directive, and initiates the execution lifecycle.
- **Example:**
  ```text
  /chief Add payments to my app using Stripe
  ```

---

### `/chief-status`
- **Purpose:** Returns the current project state and progress snapshot without modifying anything.
- **Behavior:**
  1. Chief asks Manager for project status.
  2. Manager reads [`memory/manager/project_state.md`](file:///home/ishaan/Work/ai-chief/memory/manager/project_state.md) and [`memory/manager/active_tasks.md`](file:///home/ishaan/Work/ai-chief/memory/manager/active_tasks.md).
  3. Chief presents a high-signal conversational summary to the human: current milestone, active tasks, and recent completions.
- **Example:**
  ```text
  /chief-status
  ```

---

### `/chief-clean`
- **Purpose:** Triggers memory maintenance, deduplication, and compaction.
- **Behavior:**
  1. Activates the **Cleaner Agent** ([`agents/cleaner.md`](file:///home/ishaan/Work/ai-chief/agents/cleaner.md)).
  2. Scans `memory/manager/active_tasks.md` and `memory/chief/conversation_state.md`.
  3. Removes duplicate notes, compacts completed task records into milestone summaries, and archives outdated data.
  4. Strictly preserves permanent memory (`preferences.md`, `decisions.md`, `agents/`).
  5. Outputs a clean maintenance audit report.
- **Example:**
  ```text
  /chief-clean
  ```

---

### `/chief-memory`
- **Purpose:** Displays high-priority stored memory for human inspection.
- **Behavior:**
  1. Reads and presents key contents from:
     - [`memory/project/overview.md`](file:///home/ishaan/Work/ai-chief/memory/project/overview.md) (Project goals & stack)
     - [`memory/chief/preferences.md`](file:///home/ishaan/Work/ai-chief/memory/chief/preferences.md) (Human communication preferences)
     - [`memory/manager/decisions.md`](file:///home/ishaan/Work/ai-chief/memory/manager/decisions.md) (Architectural Decision Records)
  2. Chief delivers an organized, human-readable summary.
- **Example:**
  ```text
  /chief-memory
  ```

---

### `/chief-reset`
- **Purpose:** Clears temporary runtime staging files without touching persistent project memory.
- **Behavior:**
  1. Purges all temporary task files inside [`runtime/tasks/`](file:///home/ishaan/Work/ai-chief/runtime/tasks/).
  2. Leaves `memory/` (both permanent and compressible zones) completely intact.
  3. Resets runtime scratchpad state to a clean slate.
- **Example:**
  ```text
  /chief-reset
  ```

---

### `/chief-plan [goal]`
- **Purpose:** Performs architectural analysis and plans tasks without executing any code.
- **Behavior:**
  1. Chief translates goal into a planning directive for Manager.
  2. Manager reads project memory, inspects existing architecture, and drafts the necessary task sequence.
  3. Manager writes proposed tasks to `memory/manager/active_tasks.md` with status `QUEUED / PROPOSED`.
  4. Chief presents the proposed technical roadmap to the human for review and feedback.
  5. **Zero Worker execution or code changes occur.**
- **Example:**
  ```text
  /chief-plan Refactor authentication to support OAuth2 with Google and GitHub
  ```

---

### `/chief-build [task-id or goal]`
- **Purpose:** Authorizes the Manager to dispatch Worker tasks and execute changes.
- **Behavior:**
  1. Manager pulls the planned task (or creates a new task from the goal).
  2. Manager generates `runtime/tasks/task-<id>.md` with bounded files and criteria.
  3. Worker is spawned, modifies code, runs tests, and debugs failures.
  4. Worker submits structured report; Manager compresses output and updates memory.
  5. Chief delivers the final concise conversational update to the human.
- **Example:**
  ```text
  /chief-build TASK-002
  ```
  *or*
  ```text
  /chief-build Fix broken user session expiration logic
  ```

---

## 3. Command Implementation Matrix

| Command | Primary Agent | Read Targets | Write / Action Targets | Execution Permitted? |
| :--- | :--- | :--- | :--- | :---: |
| `/chief` | Chief | `SKILL.md`, `chief.md`, `preferences.md` | `conversation_state.md` | No (Delegates) |
| `/chief-status` | Chief + Manager | `project_state.md`, `active_tasks.md` | None (Read-only) | No |
| `/chief-clean` | Cleaner | All `memory/` files | Compacts `active_tasks.md`, `conversation_state.md` | No (Hygiene only) |
| `/chief-memory` | Chief | `overview.md`, `preferences.md`, `decisions.md` | None (Read-only) | No |
| `/chief-reset` | Manager / Cleaner | `runtime/tasks/` | Clears `runtime/tasks/*.md` | No |
| `/chief-plan` | Chief + Manager | Memory + repository structure | Queues tasks in `active_tasks.md` | No (Planning only) |
| `/chief-build` | Chief + Manager + Worker | Bounded task files | Edits source code, runs tests, updates memory | **YES** |
