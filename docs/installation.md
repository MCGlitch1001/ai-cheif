# Installing AI-Chief

Installing AI-Chief is designed to be as frictionless as installing a developer tool. Because AI-Chief is 100% portable and instruction-based, new users do not need to configure directories by hand.

---

## The 1-Line Installer Prompt

To install AI-Chief into any codebase or repository, simply paste this single line into your AI coding assistant (Google Antigravity, Claude Code, Cursor, Windsurf, ChatGPT CLI):

```text
Install AI-Chief into this project.
```

---

## What Happens During Installation

When your AI assistant reads this prompt, it follows the specification in [`install.md`](file:///home/ishaan/Work/ai-chief/install.md):

1. **Scans the Project:** Verifies whether an existing `ai-chief/` setup is already in place.
2. **Scaffolds Framework Files:** Sets up the clean 3-tier hierarchy (`agents/`, `protocols/`, `templates/`, `config/`).
3. **Initializes Persistent Memory:** Creates pristine Markdown memory files in `memory/` without touching your code.
4. **Verifies Core Directives:** Ensures [`SKILL.md`](file:///home/ishaan/Work/ai-chief/SKILL.md) and [`AGENTS.md`](file:///home/ishaan/Work/ai-chief/AGENTS.md) are established.
5. **Confirms Readiness:** Outputs the standardized onboarding message so you can immediately begin working.

---

## Standard Confirmation Message

After installation finishes, your AI will respond with the fixed onboarding summary:

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

## Safety Guarantees

AI-Chief follows strict non-destructive installation rules:
- 🛡️ **Zero Application Impact:** Never modifies your existing application code, configs, or package files.
- 🛡️ **No File Deletions:** Never deletes user files.
- 🛡️ **No Dependencies:** Zero npm, pip, or cargo packages installed. No background daemons or Docker containers.
- 🛡️ **Non-Destructive Overwrites:** If an existing `ai-chief/` directory exists, the installer will ask for confirmation before modifying saved memory.

---

## Alternative Installation Methods

If you prefer manual installation:

### Method 1: Git Clone
```bash
git clone https://github.com/MCGlitch1001/ai-cheif.git .ai-chief
```

### Method 2: Copy Directory
Copy the `ai-chief/` directory directly into the root of your project:
```bash
cp -r /path/to/ai-chief /path/to/my-project/ai-chief
```

### Method 3: Point Your AI to `SKILL.md`
In environments like Google Antigravity, simply load [`SKILL.md`](file:///home/ishaan/Work/ai-chief/SKILL.md) directly into your active workspace skills.
