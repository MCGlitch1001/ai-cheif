# User Commands & Tool Integration Guide

## 1. Overview
AI-Chief is designed as a **portable, zero-dependency framework**. It does not require a hosted SaaS backend, custom plugin store, or proprietary API key. Instead, it utilizes a standardized text command protocol that operates inside any AI coding assistant or autonomous coding harness.

---

## 2. Command Reference

| Command | Usage | Description |
| :--- | :--- | :--- |
| `/chief` | `/chief [request]` | Activates AI-Chief mode, applies Chief persona, and initiates work. |
| `/chief-status` | `/chief-status` | Returns a concise overview of the current project state and task list. |
| `/chief-clean` | `/chief-clean` | Runs the Cleaner Agent to prune stale tasks and compact memory. |
| `/chief-memory` | `/chief-memory` | Displays stored preferences, project overview, and key ADR decisions. |
| `/chief-reset` | `/chief-reset` | Cleans up ephemeral runtime files in `runtime/tasks/`. |
| `/chief-plan` | `/chief-plan [goal]` | Decomposes a goal into a proposed task list without executing code. |
| `/chief-build` | `/chief-build [task]` | Authorizes Manager to dispatch Worker agents to execute changes. |

---

## 3. How Users Activate AI-Chief

### Scenario A: Starting a New Session
When opening a session in any AI assistant, type:
```text
/chief Let's begin working on the user registration system.
```
*Effect:* The model reads [`SKILL.md`](file:///home/ishaan/Work/ai-chief/SKILL.md) and [`agents/chief.md`](file:///home/ishaan/Work/ai-chief/agents/chief.md), assumes the Chief persona, greets the user, and initiates Manager orchestration.

### Scenario B: Checking Health & Status
```text
/chief-status
```
*Effect:* Returns current milestone progress and any in-flight or recently completed tasks.

### Scenario C: Safe Staged Planning
```text
/chief-plan Add Stripe subscription billing with monthly and annual tiers
```
*Effect:* Manager analyzes existing models and creates queued task specifications in `memory/manager/active_tasks.md`. Chief replies with the plan. **No files or code are touched until authorized.**

### Scenario D: Authorizing Execution
```text
/chief-build TASK-001
```
*Effect:* Manager dispatches a disposable Worker with `runtime/tasks/task-001.md`. Worker writes code, executes tests, and delivers an execution report. Manager updates memory, and Chief reports success.

### Scenario E: Cleaning Up Memory
```text
/chief-clean
```
*Effect:* Cleaner Agent runs, archives completed tasks, removes duplicate notes, and ensures memory files stay lean.

---

## 4. How Different AI Tools Implement AI-Chief

Because AI-Chief relies on standard file hierarchies and markdown prompts, different AI coding environments can implement it easily without code modifications:

### 4.1 Google Antigravity (AGY)
- **Skill Discovery:** Antigravity automatically detects [`SKILL.md`](file:///home/ishaan/Work/ai-chief/SKILL.md) in the project workspace.
- **Subagent Execution:** Antigravity can use its native `invoke_subagent` capability to spawn disposable subagents mapped directly to [`agents/worker.md`](file:///home/ishaan/Work/ai-chief/agents/worker.md) or [`agents/cleaner.md`](file:///home/ishaan/Work/ai-chief/agents/cleaner.md).
- **Governance:** Antigravity enforces [`AGENTS.md`](file:///home/ishaan/Work/ai-chief/AGENTS.md) at the root level before executing tasks.

### 4.2 Claude Code (Anthropic CLI)
- **Custom Commands:** In Claude Code, commands like `/chief` or `/chief-status` can be configured as custom slash commands in `.clauderc` or invoked directly in prompt text.
- **Memory Tracking:** Claude Code reads files directly using bash or view commands, persisting memory in `memory/`.
- **Worker Isolation:** Claude Code can invoke tasks in subshells or separate branch worktrees to guarantee clean worker disposal.

### 4.3 OpenAI Codex / ChatGPT CLI
- **System Prompt Reference:** Anchor the project by passing `AGENTS.md` and `agents/chief.md` in the system prompt or project instructions.
- **Prefix Matching:** When the user types `/chief <message>`, the model matches the pattern in [`protocols/commands.md`](file:///home/ishaan/Work/ai-chief/protocols/commands.md) and triggers the corresponding delegation flow.

### 4.4 Cursor / Windsurf / GitHub Copilot
- **Rule Files:** Include `@AGENTS.md` and `@SKILL.md` in `.cursorrules` or `.windsurfrules`.
- **Chat Interface:** When chatting in Composer or Cascade, preface requests with `/chief` to instruct the model to adopt the Chief persona and follow the 3-tier rules.

---

## 5. Architectural Philosophy: Portable Over Proprietary
AI-Chief deliberately avoids:
- Custom binary CLI tools that require compiling.
- Hosted cloud databases that add recurring costs or vendor lock-in.
- Proprietary plugin APIs that break between tool upgrades.

By grounding the entire orchestration engine in Git-tracked Markdown and structured prompts, AI-Chief remains universally compatible with every current and future AI coding environment.
