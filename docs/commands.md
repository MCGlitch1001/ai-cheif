# User Commands & Tool Integration Guide

## 1. Overview
AI-Chief is designed as a **portable, zero-dependency framework**. It does not require a hosted SaaS backend, custom plugin store, or proprietary API key. Instead, it utilizes a standardized text command protocol that operates inside any AI coding assistant or autonomous coding harness.

---

## 2. Command Reference

| Command | Usage | Description |
| :--- | :--- | :--- |
| `/chief` | `/chief [request]` | Activates AI-Chief mode and executes request through the 7-stage pipeline. |
| `/chief+` | `/chief+ [request]` | Full framework reload. Re-syncs all instructions and memory from disk. |
| `/chief-status` | `/chief-status` | Returns a concise overview of current project state and task list from memory. |
| `/chief-plan` | `/chief-plan [goal]` | Decomposes a goal into an architectural plan without executing code. |
| `/chief-build` | `/chief-build [task]` | Executes planned or requested technical modifications. |
| `/chief-clean` | `/chief-clean` | Compacts and deduplicates compressible memory in `memory/`. |
| `/chief-memory` | `/chief-memory` | Displays stored preferences, project overview, and key ADR decisions. |
| `/chief-reset` | `/chief-reset` | Cleans up ephemeral runtime files in `runtime/tasks/`. |

---

## 3. How Users Activate AI-Chief

### Scenario A: Executing a Feature
When starting a task in any AI assistant, type:
```text
/chief Let's begin working on the user registration system.
```
*Effect:* The model loads [`core/system-prompt.md`](file:///home/ishaan/Work/ai-chief/core/system-prompt.md) and [`core/config.md`](file:///home/ishaan/Work/ai-chief/core/config.md), runs the 7-stage internal execution pipeline, and responds with the standard 4-field format (`Summary`, `Changes`, `Status`, `Next`).

### Scenario B: Full Reload After Long Sessions
```text
/chief+ refactor the authentication layer
```
*Effect:* Discards stale conversation context, re-reads all core instructions and memory from disk, and executes with pristine context.

### Scenario C: Checking Health & Status
```text
/chief-status
```
*Effect:* Reads `memory/manager/project_state.md` and `memory/manager/active_tasks.md`, returning a concise summary under 10 sentences.

### Scenario D: Safe Staged Planning
```text
/chief-plan Add Stripe subscription billing with monthly and annual tiers
```
*Effect:* Analyzes existing repository models, writes proposed task roadmap to `memory/manager/active_tasks.md`, and outputs the plan. **No source code is modified.**

### Scenario E: Authorizing Execution
```text
/chief-build Implement Stripe checkout session creation
```
*Effect:* Executes code changes, runs unit tests, updates memory, and delivers the 4-field response.

### Scenario F: Cleaning Up Memory
```text
/chief-clean
```
*Effect:* Archives completed tasks, removes duplicate notes, and compacts conversation state while preserving permanent ADRs.

---

## 4. How Different AI Tools Implement AI-Chief

Because AI-Chief relies on standard file hierarchies and markdown prompts, different AI coding environments can implement it easily without code modifications:

### 4.1 Google Antigravity (AGY)
- **Discovery:** Antigravity automatically detects [`SKILL.md`](file:///home/ishaan/Work/ai-chief/SKILL.md) and [`AGENTS.md`](file:///home/ishaan/Work/ai-chief/AGENTS.md) in the project workspace.
- **Execution:** Runs natively using [`core/system-prompt.md`](file:///home/ishaan/Work/ai-chief/core/system-prompt.md) (with optional subagent dispatch via `invoke_subagent` per [`docs/advanced/native-agents.md`](file:///home/ishaan/Work/ai-chief/docs/advanced/native-agents.md)).

### 4.2 Claude Code (Anthropic CLI)
- **Prompt Reference:** Reference `@core/system-prompt.md` or `@AGENTS.md` in prompt instructions.
- **Slash Commands:** Commands like `/chief` or `/chief-status` can be used directly in prompts.
- **Memory Tracking:** Claude Code reads and updates `memory/` using filesystem tools.

### 4.3 OpenAI Codex / ChatGPT CLI
- **System Prompt Reference:** Anchor the project by passing `core/system-prompt.md` in the system prompt.
- **Text Triggers:** Match prefix `/chief <message>` to trigger the 7-stage internal pipeline.

### 4.4 Cursor / Windsurf / GitHub Copilot
- **Rule Files:** Include `@core/system-prompt.md` and `@AGENTS.md` in `.cursorrules` or `.windsurfrules`.
- **Chat Interface:** In Composer or Cascade, preface requests with `/chief` to enforce the 4-field output schema and non-destructive planning.

---

## 5. Architectural Philosophy: Portable Over Proprietary
AI-Chief deliberately avoids:
- Custom binary CLI tools that require compiling.
- Hosted cloud databases that add recurring costs or vendor lock-in.
- Proprietary plugin APIs that break between tool upgrades.

By grounding the entire orchestration engine in Git-tracked Markdown and structured prompts, AI-Chief remains universally compatible with every current and future AI coding environment.
