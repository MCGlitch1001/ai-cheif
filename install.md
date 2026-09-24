# AI-Chief: Universal Automated Installer Specification

This document defines the automated setup and verification procedure for any AI coding assistant (Google Antigravity, Claude Code, OpenAI Codex, Cursor, Gemini CLI, or generic AI coding agents) when a user pastes:

> **`Install AI-Chief into this project.`**

---

## 1. Core Installer Directives & Safety Guardrails

### ❌ What the Installer Must NEVER Do:
1. **Never delete user files:** Zero deletion of any existing project files.
2. **Never modify application code:** Never edit `src/`, `lib/`, `app/`, `package.json`, `go.mod`, `Cargo.toml`, or any application source code.
3. **Never overwrite existing memory without confirmation:** If `ai-chief/` already exists, preserve `memory/chief/preferences.md` and `memory/manager/decisions.md`.
4. **Never install external packages or daemons:** Zero npm, pip, cargo, docker, or background services.
5. **Never expose secrets:** Never print, log, or request API keys, tokens, SSH credentials, private URLs, or environment variables.
6. **Never fake success:** If any file creation or path validation fails, explain the failure clearly instead of emitting the success message.

### ✅ What the Installer Must ALWAYS Do:
1. **Zero-dependency & portable:** Operate strictly using local filesystem operations (Markdown files).
2. **Handle existing installations safely:** Detect existing files and perform non-destructive repair/sync if needed.
3. **Configure `/chief` and `/chief+` activation:** Ensure the model is primed to read the filesystem on command.
4. **Perform post-installation verification:** Run the 10-point verification checklist before emitting confirmation.
5. **Conclude with the exact standard onboarding response:** Emit the exact fixed summary block.

---

## 2. Deterministic Installation Sequence

When the AI assistant receives the installation instruction, it must execute the following 6 stages:

### Stage 1: Inspection & Detection
1. Check if `ai-chief/` directory or root AI-Chief governance files (`AGENTS.md`, `SKILL.md`) already exist.
2. If already installed:
   - Inspect existing files against the manifest in Section 3.
   - If all files exist and are healthy, proceed directly to Verification (Stage 5) without overwriting user memory.
   - If files are missing, non-destructively scaffold only the missing files.
3. If not installed:
   - Determine installation target path (typically `./ai-chief/` if installed inside a larger codebase, or project root if dedicated).

### Stage 2: Directory Scaffolding
Create the standard directory tree:
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

### Stage 3: Core Governance & Agent Deployment
Ensure the following core directives and agent contracts are populated:
- [`AGENTS.md`](file:///home/ishaan/Work/ai-chief/AGENTS.md) — Root system governance, 3-tier rules, and command specifications.
- [`SKILL.md`](file:///home/ishaan/Work/ai-chief/SKILL.md) — Primary entry point with Activation Rules and 5-step operational loop.
- [`agents/chief.md`](file:///home/ishaan/Work/ai-chief/agents/chief.md) — Chief Agent persona (<10 sentences, zero code/log dumps).
- [`agents/manager.md`](file:///home/ishaan/Work/ai-chief/agents/manager.md) — Manager Agent persona (context controller, task planner, memory sync).
- [`agents/worker.md`](file:///home/ishaan/Work/ai-chief/agents/worker.md) — Worker Agent persona (ephemeral execution engine, tools, testing).
- [`agents/cleaner.md`](file:///home/ishaan/Work/ai-chief/agents/cleaner.md) — Cleaner Agent persona (memory compaction, deduplication).

### Stage 4: Protocol, Memory & Runtime Staging
Ensure all protocols, persistent memory files, runtime state, and templates are established:
- **Protocols:**
  - `protocols/activation.md` (6-step persistent activation bootstrap)
  - `protocols/context_recovery.md` (Self-healing `/chief+` cold-start reload)
  - `protocols/commands.md` (Universal text command protocol)
  - `protocols/delegation.md` (Inter-agent communication schemas)
  - `protocols/compression.md` (Semantic output compression rules)
  - `protocols/memory_rules.md` (3-tier memory durability hierarchy)
  - `protocols/response_format.md` (Exact output schemas across all tiers)
- **Memory (Pristine Initial State):**
  - `memory/chief/preferences.md` (User communication preferences)
  - `memory/chief/conversation_state.md` (Active conversational state)
  - `memory/manager/project_state.md` (Architecture, stack, and active milestone)
  - `memory/manager/decisions.md` (Architectural Decision Records log)
  - `memory/manager/active_tasks.md` (Task registry)
  - `memory/project/overview.md` (Project overview and domain context)
- **Runtime:**
  - `runtime/active_session.md` (Active session state & telemetry)
  - `runtime/current_agent.md` (Active agent tracking for Fallback Mode)
  - `runtime/message_queue.md` (Inter-agent protocol message buffer)
  - `runtime/tasks/README.md` (Ephemeral task staging documentation)
- **Templates & Config:**
  - `templates/task.md` (Manager → Worker delegation template)
  - `templates/worker_report.md` (Worker → Manager execution report template)
  - `templates/memory_update.md` (ADR and task status update templates)
  - `config/settings.md` (Model tiers, token limits, thresholds)

---

## 3. Post-Installation Lightweight Verification

Before returning the final response to the user, the AI assistant **MUST** perform this 10-point self-audit:

| # | Checkpoint | Verification Criteria | Status |
| :-: | :--- | :--- | :---: |
| 1 | **Directory Hierarchy** | `agents/`, `memory/`, `runtime/tasks/`, `protocols/`, `templates/`, `config/`, `docs/` exist. | PASS / FAIL |
| 2 | **Role Files** | `chief.md`, `manager.md`, `worker.md`, `cleaner.md` exist in `agents/`. | PASS / FAIL |
| 3 | **Protocol Files** | All 7 protocols exist in `protocols/`. | PASS / FAIL |
| 4 | **Memory Structure** | All 6 memory files exist in `memory/` across `chief/`, `manager/`, `project/`. | PASS / FAIL |
| 5 | **Runtime Structure** | `active_session.md`, `current_agent.md`, `message_queue.md`, and `runtime/tasks/` exist. | PASS / FAIL |
| 6 | **Commands Documented** | Commands `/chief`, `/chief+`, `/chief-status`, `/chief-plan`, `/chief-build`, `/chief-clean`, `/chief-memory`, `/chief-reset` documented in `protocols/commands.md`. | PASS / FAIL |
| 7 | **Activation Discoverable** | `SKILL.md` and `AGENTS.md` define persistent activation rules. | PASS / FAIL |
| 8 | **Reference Validity** | Relative links between files (`protocols/`, `agents/`, `memory/`) resolve without broken paths. | PASS / FAIL |
| 9 | **Application Safety** | No user application files or code were modified or deleted. | PASS / FAIL |
| 10 | **Fallback Documented** | Single-agent fallback role-switching mode documented in `docs/fallback-mode.md`. | PASS / FAIL |

### Error Reporting (If Verification Fails):
If any checkpoint fails, **DO NOT** output the standard success message. Output:
```text
❌ AI-Chief installation incomplete.
Failed Checkpoint: [Checkpoint Name]
Reason: [Detailed diagnostic explanation]
Action: [Remediation taken or needed]
```

---

## 4. Platform-Specific Setup & Fallback Adaptation

AI-Chief runs across diverse AI coding tools without relying on proprietary platform features:

| Platform | Discovery Method | Execution Mode | Native Subagents? | Fallback Mode Required? |
| :--- | :--- | :--- | :---: | :---: |
| **Google Antigravity** | Auto-discovers `SKILL.md` & `AGENTS.md` | Subagent Mode via `invoke_subagent` | Yes | No (Optional) |
| **Claude Code (Anthropic CLI)** | Reads `AGENTS.md` / `CLAUDE.md` / `@SKILL.md` | Role-switching inside session thread | No | **Yes** |
| **OpenAI Codex / ChatGPT CLI** | Reads `AGENTS.md` via system prompt | Role-switching inside session thread | No | **Yes** |
| **Cursor / Windsurf** | Reads `.cursorrules` / `.windsurfrules` | Role-switching inside Composer / Cascade | No | **Yes** |
| **Gemini Coding Agents** | Reads `AGENTS.md` / `SKILL.md` in workspace | Role-switching or native tool dispatch | Depends on harness | **Yes** (Default) |
| **Generic File-Reading AI Agents** | Reads `AGENTS.md` in root directory | Structured role-switching | No | **Yes** |

> **Single-Agent Fallback Invariant:** On platforms where native subagents are unavailable, the AI model executes Chief → Manager → Worker → Summary via internal role switches tracked in [`runtime/current_agent.md`](file:///home/ishaan/Work/ai-chief/runtime/current_agent.md), adhering strictly to the identical protocol schemas without dumping raw logs to the user.

---

## 5. Standard Onboarding Response

When all 10 verification checkpoints pass, the AI assistant **MUST** respond with **exactly this text**:

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
