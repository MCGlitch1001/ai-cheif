# Protocol: Command System

## 1. Overview
The AI-Chief Command System provides a uniform, portable interface for human users and AI coding environments to interact with the orchestration hierarchy. Because AI-Chief is tool-agnostic, these commands operate purely via text triggers that any LLM or AI coding agent (Claude Code, Google Antigravity, OpenAI Codex, Cursor, Windsurf) can recognize and handle.

---

## 2. Command Specifications

### `/chief [task]`
- **Purpose:** Normal AI-Chief activation. Loads required framework files and initiates work.
- **Behavior:**
  1. Follows the 6-step bootstrap in [`protocols/activation.md`](file:///home/ishaan/Work/ai-chief/protocols/activation.md).
  2. Reads [`SKILL.md`](file:///home/ishaan/Work/ai-chief/SKILL.md) and [`AGENTS.md`](file:///home/ishaan/Work/ai-chief/AGENTS.md).
  3. Restores essential memory files (`preferences.md`, `project_state.md`, `decisions.md`).
  4. Adopts the Chief Agent persona ([`agents/chief.md`](file:///home/ishaan/Work/ai-chief/agents/chief.md)): concise, polite, <10 sentences, no code blocks or log dumps.
  5. Formulates an internal Manager directive and initiates the execution lifecycle.
- **Example:**
  ```text
  /chief add authentication to the application using JWT
  ```

---

### `/chief+ [task]`
- **Purpose:** Full framework reload. Used when conversations become very long, behavior appears incorrect, or context compression has occurred.
- **Behavior:**
  1. Executes the cold-start self-healing protocol in [`protocols/context_recovery.md`](file:///home/ishaan/Work/ai-chief/protocols/context_recovery.md).
  2. Discards stale conversational context and treats the filesystem as the sole source of truth.
  3. Reloads all core files:
     - [`SKILL.md`](file:///home/ishaan/Work/ai-chief/SKILL.md) & [`AGENTS.md`](file:///home/ishaan/Work/ai-chief/AGENTS.md)
     - [`agents/chief.md`](file:///home/ishaan/Work/ai-chief/agents/chief.md) & [`agents/manager.md`](file:///home/ishaan/Work/ai-chief/agents/manager.md)
     - All files in [`protocols/`](file:///home/ishaan/Work/ai-chief/protocols/)
     - All files in [`memory/`](file:///home/ishaan/Work/ai-chief/memory/)
  4. Resets [`runtime/current_agent.md`](file:///home/ishaan/Work/ai-chief/runtime/current_agent.md) to `Chief`.
  5. Resumes execution with pristine instructions.
- **Example:**
  ```text
  /chief+ refactor the database layer
  ```

---

### `/chief-status`
- **Purpose:** Shows current project state and progress snapshot from memory without modifying anything.
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
- **Purpose:** Runs Cleaner Agent to maintain memory, deduplicate entries, and archive outdated information.
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
- **Purpose:** Shows important stored memory for human inspection.
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
- **Purpose:** Clears temporary runtime staging files only (`runtime/tasks/`).
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
- **Purpose:** Creates architectural and task plan without executing any code.
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
- **Purpose:** Authorizes the Manager to dispatch Worker tasks to execute changes.
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

---

## 3. Command Implementation Matrix

| Command | Primary Agent | Read Targets | Write / Action Targets | Execution Permitted? |
| :--- | :--- | :--- | :--- | :---: |
| `/chief [task]` | Chief | `SKILL.md`, `AGENTS.md`, `preferences.md` | `conversation_state.md` | No (Delegates) |
| `/chief+ [task]` | Chief + Manager | Complete filesystem reload (`SKILL.md`, `AGENTS.md`, `protocols/*`, `memory/*`) | Resets `runtime/current_agent.md` | No (Full reload) |
| `/chief-status` | Chief + Manager | `project_state.md`, `active_tasks.md` | None (Read-only) | No |
| `/chief-clean` | Cleaner | All `memory/` files | Compacts `active_tasks.md`, `conversation_state.md` | No (Hygiene only) |
| `/chief-memory` | Chief | `overview.md`, `preferences.md`, `decisions.md` | None (Read-only) | No |
| `/chief-reset` | Manager / Cleaner | `runtime/tasks/` | Clears `runtime/tasks/*.md` | No |
| `/chief-plan [goal]` | Chief + Manager | Memory + repository structure | Queues tasks in `active_tasks.md` | No (Planning only) |
| `/chief-build [task]` | Chief + Manager + Worker | Bounded task files | Edits source code, runs tests, updates memory | **YES** |
