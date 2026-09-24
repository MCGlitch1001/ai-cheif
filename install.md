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
3. **Configure `/chief` and `/chief+` activation:** Ensure the model is primed to read `core/system-prompt.md` and the filesystem on command.
4. **Perform post-installation verification:** Run the 10-point verification checklist before emitting confirmation.
5. **Conclude with the exact standard onboarding response:** Emit the exact fixed summary block.

---

## 2. Deterministic Installation Sequence

When the AI assistant receives the installation instruction, it must execute the following 5 stages:

### Stage 1: Inspection & Detection
1. Check if `ai-chief/` directory or root AI-Chief governance files (`AGENTS.md`, `SKILL.md`, `core/system-prompt.md`) already exist.
2. If already installed:
   - Inspect existing files against the manifest in Section 3.
   - If all files exist and are healthy, proceed directly to Verification without overwriting user memory.
   - If files are missing, non-destructively scaffold only the missing files.
3. If not installed:
   - Determine installation target path (typically `./ai-chief/` if installed inside a larger codebase, or project root if dedicated).

### Stage 2: Directory Scaffolding
Create the standard directory tree:
```text
ai-chief/
├── core/
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
    └── advanced/
```

### Stage 3: Core System Prompt & Governance Deployment
Ensure the following core directives and governance contracts are populated:
- [`core/system-prompt.md`](file:///home/ishaan/Work/ai-chief/core/system-prompt.md) — Unified single-agent AI operating system prompt.
- [`core/config.md`](file:///home/ishaan/Work/ai-chief/core/config.md) — Editable user configuration (style, limits, risk).
- [`AGENTS.md`](file:///home/ishaan/Work/ai-chief/AGENTS.md) — System governance and operational directives.
- [`SKILL.md`](file:///home/ishaan/Work/ai-chief/SKILL.md) — Universal entry point and activation rules.
- Optional multi-agent roles in `agents/` (`chief.md`, `manager.md`, `worker.md`, `cleaner.md`) for platforms with native subagent engines.

### Stage 4: Protocol, Memory & Runtime Staging
Ensure all protocols, persistent memory files, and runtime state are established:
- **Protocols:**
  - `protocols/activation.md` (6-step persistent activation bootstrap)
  - `protocols/context_recovery.md` (Self-healing `/chief+` cold-start reload)
  - `protocols/commands.md` (Universal text command protocol)
  - `protocols/response_format.md` (Exact 4-field output schema)
  - `protocols/compression.md` (Semantic output compression rules)
  - `protocols/memory_rules.md` (3-tier memory durability hierarchy)
  - `protocols/delegation.md` (Inter-agent schemas for native subagent environments)
- **Memory (Pristine Initial State):**
  - `memory/chief/preferences.md` (User communication preferences)
  - `memory/chief/conversation_state.md` (Active conversational state)
  - `memory/manager/project_state.md` (Architecture, stack, and active milestone)
  - `memory/manager/decisions.md` (Architectural Decision Records log)
  - `memory/manager/active_tasks.md` (Task registry)
  - `memory/project/overview.md` (Project overview and domain context)
- **Runtime:**
  - `runtime/active_session.md` (Active session state & telemetry)
  - `runtime/tasks/README.md` (Ephemeral task staging documentation)
- **Templates & Config:**
  - `templates/task.md` (Task specification template)
  - `templates/memory_update.md` (ADR and task status update templates)
  - `config/settings.md` (Model tiers, token limits, thresholds)

---

## 3. Post-Installation Lightweight Verification

Before returning the final response to the user, the AI assistant **MUST** perform this 10-point self-audit:

| # | Checkpoint | Verification Criteria | Status |
| :-: | :--- | :--- | :--- |
| 1 | **Directory Hierarchy** | `core/`, `agents/`, `memory/`, `runtime/tasks/`, `protocols/`, `templates/`, `config/`, `docs/` exist. | PASS / FAIL |
| 2 | **Core System Files** | `core/system-prompt.md` and `core/config.md` exist and are valid. | PASS / FAIL |
| 3 | **Governance Files** | `AGENTS.md` and `SKILL.md` exist and anchor root rules. | PASS / FAIL |
| 4 | **Protocol Files** | Protocols in `protocols/` exist (activation, commands, response_format, etc.). | PASS / FAIL |
| 5 | **Memory Structure** | All memory files exist in `memory/` across `chief/`, `manager/`, `project/`. | PASS / FAIL |
| 6 | **Runtime Structure** | `active_session.md` and `runtime/tasks/` exist. | PASS / FAIL |
| 7 | **Commands Documented** | Commands `/chief`, `/chief+`, `/chief-status`, `/chief-plan`, `/chief-build`, `/chief-clean`, `/chief-memory`, `/chief-reset` documented in `protocols/commands.md`. | PASS / FAIL |
| 8 | **Response Format Configured** | Standard 4-field response format defined in `protocols/response_format.md`. | PASS / FAIL |
| 9 | **Application Safety** | No user application files or code were modified or deleted. | PASS / FAIL |
| 10 | **Native Subagent Docs** | Optional native subagent capabilities documented in `docs/advanced/native-agents.md`. | PASS / FAIL |

### Error Reporting (If Verification Fails):
If any checkpoint fails, **DO NOT** output the standard success message. Output:
```text
❌ AI-Chief installation incomplete.
Failed Checkpoint: [Checkpoint Name]
Reason: [Detailed diagnostic explanation]
Action: [Remediation taken or needed]
```

---

## 4. Platform-Specific Setup

AI-Chief runs across diverse AI coding tools without relying on proprietary platform features:

| Platform | Primary Discovery Method | Execution Architecture |
| :--- | :--- | :--- |
| **Google Antigravity** | Auto-discovers `SKILL.md` & `AGENTS.md` | Single-agent OS prompt (optional `invoke_subagent` extensions) |
| **Claude Code (Anthropic CLI)** | Reads `core/system-prompt.md` / `AGENTS.md` / `CLAUDE.md` | Single-agent 7-stage internal pipeline |
| **OpenAI Codex / ChatGPT CLI** | Reads `core/system-prompt.md` via system prompt | Single-agent 7-stage internal pipeline |
| **Cursor / Windsurf** | Loads `core/system-prompt.md` via `.cursorrules` / `.windsurfrules` | Single-agent 7-stage internal pipeline |
| **Gemini Coding Agents** | Reads `core/system-prompt.md` / `SKILL.md` | Single-agent 7-stage internal pipeline |

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
```
