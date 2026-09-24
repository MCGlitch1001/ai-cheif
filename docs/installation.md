# AI-Chief Installation & Setup Guide

Installing AI-Chief is designed to be as frictionless as installing a developer tool. Because AI-Chief is 100% portable and instruction-based, new users do not need to configure directories by hand.

---

## 1. The One-Line Installer Instruction

To install AI-Chief into any codebase or repository, paste this single line into your AI coding assistant (Google Antigravity, Claude Code, OpenAI Codex, Cursor, Windsurf, Gemini CLI):

```text
Install AI-Chief into this project.
```

Alternatively, you can instruct your agent:
```text
Load AI-Chief system prompt from core/system-prompt.md
```

---

## 2. What the AI Does During Installation

When your AI assistant reads this instruction, it executes the automated protocol specified in [`install.md`](file:///home/ishaan/Work/ai-chief/install.md):

1. **Pre-Installation Safety Inspection:** Checks if an existing `ai-chief/` setup is present. If found, it preserves existing user memory and preferences.
2. **Non-Destructive Directory Scaffolding:** Creates the required folder hierarchy (`core/`, `agents/`, `memory/`, `runtime/tasks/`, `protocols/`, `templates/`, `config/`, `docs/advanced/`).
3. **Core Directive Deployment:** Ensures [`core/system-prompt.md`](file:///home/ishaan/Work/ai-chief/core/system-prompt.md), [`core/config.md`](file:///home/ishaan/Work/ai-chief/core/config.md), [`AGENTS.md`](file:///home/ishaan/Work/ai-chief/AGENTS.md), and [`SKILL.md`](file:///home/ishaan/Work/ai-chief/SKILL.md) are established.
4. **Persistent Memory Initialization:** Creates pristine Markdown memory files in `memory/` without touching any of your application source code.
5. **10-Point Lightweight Self-Audit:** Verifies that all files exist, references resolve cleanly, and operational bounds are primed.
6. **Standard Onboarding Response:** Concludes with the fixed summary block.

---

## 3. Standard Confirmation Message

Upon successful installation and verification, your AI assistant will respond with **exactly this text**:

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

---

## 4. Execution Architecture

AI-Chief provides a unified, structured workflow:

### Single-Agent Operating System (Default)
- **Supported on:** Claude Code, OpenAI Codex, Cursor (Composer), Windsurf, Google Antigravity, Gemini CLI, standard chat interfaces.
- **How it works:** The AI assistant runs the 7-stage internal pipeline (`Intent → Context Check → Bounding → Internal Plan → Execute → Verify → Summarize`).
- **User Interface:** Zero simulated agent chatter (no *"Chief says..."*, *"Worker says..."*). Output is delivered in the standard 4-field format (`Summary`, `Changes`, `Status`, `Next`).

### Optional: Native Multi-Agent Orchestration
- **Supported on:** Harnesses with native subagent IPC (e.g., Google Antigravity `invoke_subagent`).
- **How it works:** The orchestrator disposes background subagents for isolated task execution while keeping user chat clean. See [`docs/advanced/native-agents.md`](file:///home/ishaan/Work/ai-chief/docs/advanced/native-agents.md).

---

## 5. Non-Destructive Safety Guarantees

AI-Chief adheres to strict safety boundaries:
- 🛡️ **Zero Application Code Modification:** Never edits or refactors your existing application files (`src/`, `package.json`, `go.mod`, etc.) during setup.
- 🛡️ **Zero Deletions:** Never deletes user files.
- 🛡️ **Zero External Dependencies:** No npm/pip packages installed, no background daemons, no external databases.
- 🛡️ **Zero Secret Exposure:** Never prints, logs, or requests API keys, tokens, credentials, or private URLs.
- 🛡️ **Safe Overwrite Protection:** Existing memory files (`preferences.md`, `decisions.md`) are never overwritten without explicit user confirmation.

---

## 6. Installation Verification Procedure

To manually verify that AI-Chief was installed correctly in your repository:

1. **Verify Files Exist:**
   ```bash
   ls -la core/ AGENTS.md SKILL.md protocols/ memory/ runtime/
   ```
2. **Run a Dry-Run Activation:**
   Type `/chief-status` in your chat. The model should read `memory/manager/project_state.md` and report a clean status snapshot.
3. **Test Non-Execution Planning:**
   Type `/chief-plan add health check route`. The model should draft a plan in `memory/manager/active_tasks.md` without modifying any source code.
4. **Test Full Framework Recovery:**
   Type `/chief+`. The model should reload all directives from the filesystem.

---

## 7. Troubleshooting Failed Installations

If your AI assistant returns `❌ AI-Chief installation incomplete`:
- Verify write permissions in the project directory.
- Confirm all files in `core/` and `protocols/` were created.
- Type `/chief+` to force a complete filesystem re-sync.
