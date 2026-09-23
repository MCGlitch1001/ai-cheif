# AI-Chief: Installation & Setup Specification

This document defines the automated setup instructions for any AI assistant when a user requests:
> **"Install AI-Chief into this project."**

---

## 1. Automated Installation Protocol

When an AI coding assistant (Google Antigravity, Claude Code, Cursor, Windsurf, Codex, etc.) receives the installation request, it must perform the following deterministic steps:

### Step 1: Detect Existing Installation
- Check if an `ai-chief/` directory or root AI-Chief framework files exist in the project workspace.
- If an existing installation is found, **do not overwrite or delete existing memory files without explicit user confirmation**.
- If no installation is detected, proceed with scaffolding.

### Step 2: Create Core Directory Structure
Ensure the following directories are initialized:
```text
ai-chief/
├── agents/
├── memory/
│   ├── chief/
│   ├── manager/
│   └── project/
├── runtime/
│   └── tasks/
├── protocols/
├── templates/
├── config/
└── docs/
```

### Step 3: Populate Governance & Core Agents
Verify or generate the required core framework files:
- [`AGENTS.md`](file:///home/ishaan/Work/ai-chief/AGENTS.md) — Root system governance & directives.
- [`SKILL.md`](file:///home/ishaan/Work/ai-chief/SKILL.md) — Universal framework skill entry point.
- [`agents/chief.md`](file:///home/ishaan/Work/ai-chief/agents/chief.md) — Human interface persona (<10 sentences, non-coding).
- [`agents/manager.md`](file:///home/ishaan/Work/ai-chief/agents/manager.md) — Context controller & task planner.
- [`agents/worker.md`](file:///home/ishaan/Work/ai-chief/agents/worker.md) — Disposable execution engine.
- [`agents/cleaner.md`](file:///home/ishaan/Work/ai-chief/agents/cleaner.md) — Memory hygiene agent.

### Step 4: Initialize Persistent Memory
Create pristine memory templates if not already present:
- `memory/chief/preferences.md` — Default user communication preferences.
- `memory/chief/conversation_state.md` — Active conversational state.
- `memory/manager/project_state.md` — Technology stack and active milestones.
- `memory/manager/decisions.md` — Architectural Decision Records log.
- `memory/manager/active_tasks.md` — Task registry.
- `memory/project/overview.md` — Project context and domain description.

### Step 5: Verify Critical Protocols
Verify presence of:
- `protocols/activation.md`
- `protocols/context_recovery.md`
- `protocols/commands.md`
- `protocols/delegation.md`
- `protocols/compression.md`
- `protocols/memory_rules.md`

### Step 6: Non-Destructive Invariant
- **DO NOT** edit, refactor, or delete any of the user's existing application files (`src/`, `package.json`, `index.html`, etc.).
- **DO NOT** install npm/pip/cargo packages or background daemons.

---

## 2. Installer Safety Guardrails

### ❌ What the Installer Must NEVER Do:
- **Never delete user files.**
- **Never overwrite existing `ai-chief/` memory or preferences without confirmation.**
- **Never modify user application code during installation.**
- **Never install third-party dependencies or background services.**

### ✅ What the Installer Must ALWAYS Do:
- **Keep setup 100% portable and filesystem-based.**
- **Preserve existing codebase state.**
- **Conclude with the exact standard onboarding message below.**

---

## 3. Standard Onboarding Response

Upon verifying completion of Steps 1 through 6, the AI **MUST NOT** provide long verbose explanations or dump file listings. It must respond with **exactly** the following text:

```
✅ AI-Chief installed successfully.

AI-Chief is ready.

Start:
 /chief <your task>

Commands:
 /chief-reset — reset temporary session data
 /chief-clean — clean and organize memory
 /chief-status — view project status
 /chief-memory — view saved memory
 /chief-plan — plan without executing
 /chief-build — execute approved tasks

Your AI workflow is now:
Chief → Manager → Worker → Summary
```
