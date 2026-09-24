# Protocol: Command System

## 1. Overview
The AI-Chief Command System provides a uniform, portable interface for human users and AI coding environments. Because AI-Chief is tool-agnostic, these commands operate purely via standard text triggers that any LLM or AI coding agent (Claude Code, Google Antigravity, OpenAI Codex, Cursor, Windsurf) can recognize and handle.

---

## 2. Command Specifications

### `/chief [task]`
- **Purpose:** Normal AI-Chief activation. Loads required framework files and executes work via the 7-stage pipeline.
- **Behavior:**
  1. Follows the activation bootstrap in [`protocols/activation.md`](file:///home/ishaan/Work/ai-chief/protocols/activation.md).
  2. Ingests [`core/system-prompt.md`](file:///home/ishaan/Work/ai-chief/core/system-prompt.md), [`core/config.md`](file:///home/ishaan/Work/ai-chief/core/config.md), and [`AGENTS.md`](file:///home/ishaan/Work/ai-chief/AGENTS.md).
  3. Restores essential memory files (`preferences.md`, `project_state.md`, `decisions.md`).
  4. Executes the 7-stage internal pipeline without emitting simulated agent chatter.
  5. Returns the final result adhering to [`protocols/response_format.md`](file:///home/ishaan/Work/ai-chief/protocols/response_format.md).
- **Example:**
  ```text
  /chief add authentication to the application using JWT
  ```

---

### `/chief+ [task]`
- **Purpose:** Full framework reload. Used when conversations become very long, behavior appears degraded, or context compression has occurred.
- **Behavior:**
  1. Executes the cold-start self-healing protocol in [`protocols/context_recovery.md`](file:///home/ishaan/Work/ai-chief/protocols/context_recovery.md).
  2. Discards stale conversational context and treats the filesystem as the sole source of truth.
  3. Reloads all core files:
     - [`core/system-prompt.md`](file:///home/ishaan/Work/ai-chief/core/system-prompt.md) & [`core/config.md`](file:///home/ishaan/Work/ai-chief/core/config.md)
     - [`SKILL.md`](file:///home/ishaan/Work/ai-chief/SKILL.md) & [`AGENTS.md`](file:///home/ishaan/Work/ai-chief/AGENTS.md)
     - All files in [`protocols/`](file:///home/ishaan/Work/ai-chief/protocols/)
     - All files in [`memory/`](file:///home/ishaan/Work/ai-chief/memory/)
  4. Resumes execution with pristine instructions.
- **Example:**
  ```text
  /chief+ refactor the database layer
  ```

---

### `/chief-status`
- **Purpose:** Shows current project state and progress snapshot from memory without modifying anything.
- **Behavior:**
  1. Reads [`memory/manager/project_state.md`](file:///home/ishaan/Work/ai-chief/memory/manager/project_state.md) and [`memory/manager/active_tasks.md`](file:///home/ishaan/Work/ai-chief/memory/manager/active_tasks.md).
  2. Ingests [`memory/project/overview.md`](file:///home/ishaan/Work/ai-chief/memory/project/overview.md).
  3. Delivers a high-signal conversational summary: active milestone, queued tasks, and recent completions.
- **Example:**
  ```text
  /chief-status
  ```

---

### `/chief-clean`
- **Purpose:** Runs memory maintenance to compact entries, deduplicate notes, and archive outdated task logs.
- **Behavior:**
  1. Scans `memory/manager/active_tasks.md` and `memory/chief/conversation_state.md`.
  2. Compacts completed task records into milestone summaries.
  3. Strictly preserves permanent memory (`preferences.md`, `decisions.md`, `core/system-prompt.md`).
  4. Emits a clean maintenance audit report conforming to [`protocols/response_format.md`](file:///home/ishaan/Work/ai-chief/protocols/response_format.md).
- **Example:**
  ```text
  /chief-clean
  ```

---

### `/chief-memory`
- **Purpose:** Shows important stored project memory for human inspection.
- **Behavior:**
  1. Reads key contents from:
     - [`memory/project/overview.md`](file:///home/ishaan/Work/ai-chief/memory/project/overview.md) (Project goals & stack)
     - [`memory/chief/preferences.md`](file:///home/ishaan/Work/ai-chief/memory/chief/preferences.md) (Human communication preferences)
     - [`memory/manager/decisions.md`](file:///home/ishaan/Work/ai-chief/memory/manager/decisions.md) (Architectural Decision Records)
  2. Delivers an organized, human-readable summary under 10 sentences.
- **Example:**
  ```text
  /chief-memory
  ```

---

### `/chief-reset`
- **Purpose:** Clears temporary runtime staging files only (`runtime/tasks/`).
- **Behavior:**
  1. Purges all temporary staging files inside [`runtime/tasks/`](file:///home/ishaan/Work/ai-chief/runtime/tasks/).
  2. Leaves `memory/` (both permanent and compressible zones) completely intact.
  3. Resets runtime scratchpad state to a clean slate.
- **Example:**
  ```text
  /chief-reset
  ```

---

### `/chief-plan [goal]`
- **Purpose:** Creates architectural and task plan without executing any code modifications.
- **Behavior:**
  1. Ingests project memory and inspects existing repository architecture.
  2. Formulates a structured, atomic task sequence.
  3. Records proposed tasks to `memory/manager/active_tasks.md` with status `QUEUED / PROPOSED`.
  4. Delivers the proposed plan to the user for review.
  5. **Halts execution without modifying source code.**
- **Example:**
  ```text
  /chief-plan Refactor authentication to support OAuth2 with Google and GitHub
  ```

---

### `/chief-build [task or goal]`
- **Purpose:** Executes planned or requested technical modifications.
- **Behavior:**
  1. Resolves target task from `memory/manager/active_tasks.md` or user prompt.
  2. Bounds context to the necessary file set.
  3. Executes code changes, runs tests, and validates results.
  4. Updates project memory and returns the standard 4-field response (`Summary`, `Changes`, `Status`, `Next`).
- **Example:**
  ```text
  /chief-build Implement user signup endpoint with bcrypt hashing
  ```

---

## 3. Command Implementation Matrix

| Command | Read Targets | Write / Action Targets | Execution Permitted? | Response Format |
| :--- | :--- | :--- | :---: | :--- |
| `/chief [task]` | `core/`, `AGENTS.md`, `preferences.md` | Memory + Source files | **YES** | 4-Field Standard |
| `/chief+ [task]` | Full filesystem reload | Resets runtime cache | Full Reload | 4-Field Standard |
| `/chief-status` | `project_state.md`, `active_tasks.md` | None (Read-only) | No | Concise Markdown |
| `/chief-clean` | All `memory/` files | Compacts `active_tasks.md` | No (Hygiene only) | Maintenance Audit |
| `/chief-memory` | `overview.md`, `preferences.md`, `decisions.md` | None (Read-only) | No | Concise Markdown |
| `/chief-reset` | `runtime/tasks/` | Clears `runtime/tasks/*.md` | No | Direct Confirmation |
| `/chief-plan [goal]` | Memory + repository structure | Queues tasks in `active_tasks.md` | No (Planning only) | Structured Plan |
| `/chief-build [task]` | Bounded task files | Edits source code, runs tests | **YES** | 4-Field Standard |
