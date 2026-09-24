# AI-Chief Installation & Setup Guide

Installing AI-Chief is designed to be as frictionless as installing a developer tool. Because AI-Chief is 100% portable and instruction-based, new users do not need to configure directories by hand.

---

## 1. The One-Line Installer Instruction

To install AI-Chief into any codebase or repository, paste this single line into your AI coding assistant (Google Antigravity, Claude Code, OpenAI Codex, Cursor, Windsurf, Gemini CLI):

```text
Install AI-Chief into this project.
```

---

## 2. What the AI Does During Installation

When your AI assistant reads this instruction, it executes the automated protocol specified in [`install.md`](file:///home/ishaan/Work/ai-chief/install.md):

1. **Pre-Installation Safety Inspection:** Checks if an existing `ai-chief/` setup is present. If found, it preserves existing user memory and preferences.
2. **Non-Destructive Directory Scaffolding:** Creates the 3-tier folder hierarchy (`agents/`, `memory/`, `runtime/tasks/`, `protocols/`, `templates/`, `config/`, `docs/`).
3. **Core Directive Deployment:** Ensures [`AGENTS.md`](file:///home/ishaan/Work/ai-chief/AGENTS.md), [`SKILL.md`](file:///home/ishaan/Work/ai-chief/SKILL.md), and all agent contracts (`chief.md`, `manager.md`, `worker.md`, `cleaner.md`) are established.
4. **Persistent Memory Initialization:** Creates pristine Markdown memory files in `memory/` without touching any of your application source code.
5. **10-Point Lightweight Self-Audit:** Verifies that all files exist, references resolve cleanly, and fallback mode is configured.
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

Your AI workflow is now:
Chief → Manager → Worker → Summary
```

---

## 4. Execution Modes: Native Subagents vs. Single-Agent Fallback

AI-Chief supports two distinct operational modes depending on your AI platform:

### Mode A: Native Subagents (Multi-Agent Platforms)
- **Supported on:** Google Antigravity, custom subagent harnesses.
- **How it works:** Chief, Manager, and Worker run as distinct background subagent processes. The Worker executes tasks in an isolated sub-thread, keeping the parent chat completely pristine.

### Mode B: Single-Agent Fallback (Single-Thread Platforms)
- **Supported on:** Claude Code, OpenAI Codex, Cursor (Composer), Gemini CLI, standard chat interfaces.
- **How it works:** The AI assistant switches cognitive roles internally (`Chief → Manager → Worker → Summary`) within the same conversation thread, updating [`runtime/current_agent.md`](file:///home/ishaan/Work/ai-chief/runtime/current_agent.md).
- **Protocol Guarantee:** The communication schemas, context bounding, and `<10 sentences` Chief non-coding boundary remain **100% identical**. See [`docs/fallback-mode.md`](file:///home/ishaan/Work/ai-chief/docs/fallback-mode.md) for complete details.

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
   ls -la AGENTS.md SKILL.md agents/ protocols/ memory/ runtime/
   ```
2. **Run a Dry-Run Activation:**
   Type `/chief-status` in your chat. The model should read `memory/manager/project_state.md` and report a clean status snapshot.
3. **Test Non-Execution Planning:**
   Type `/chief-plan add health check route`. The model should draft a plan in `memory/manager/active_tasks.md` without modifying any source code.
4. **Test Full Framework Recovery:**
   Type `/chief+`. The model should reload all directives from the filesystem.

---

## 7. Troubleshooting Failed Installations

| Symptom | Cause | Solution |
| :--- | :--- | :--- |
| **Model writes code to chat during install** | Model did not follow `install.md` | Re-issue: *"Read install.md and install AI-Chief non-destructively."* |
| **Missing directory error** | Filesystem permissions or restricted environment | Ensure the AI has write permissions to create subdirectories. |
| **Existing memory overwritten** | Partial re-installation | Restore memory from Git: `git checkout memory/` or check `runtime/current_agent.md`. |
| **Model dumps internal logs** | Model slipping out of Chief persona | Issue `/chief+` to trigger a cold-start filesystem reload. |
| **Platform lacks subagent support** | Normal for CLI/single-thread tools | Explicitly tell model: *"Operate in Fallback Mode using docs/fallback-mode.md."* |

---

## 8. Alternative Manual Installation Methods

If you prefer manual installation without prompting an agent:

### Method 1: Git Clone
```bash
git clone https://github.com/MCGlitch1001/ai-cheif.git .ai-chief
```

### Method 2: Copy Directory
Copy the `ai-chief/` directory directly into your repository:
```bash
cp -r /path/to/ai-chief /path/to/your-project/ai-chief
```

### Method 3: Point Your AI to `SKILL.md`
In environments like Google Antigravity, simply load [`SKILL.md`](file:///home/ishaan/Work/ai-chief/SKILL.md) directly into your active workspace skills.
