# AI-Chief: 5-Minute Quick Start ⚡

Get started with **AI-Chief** in under five minutes.

---

## What is AI-Chief in 30 Seconds?

Most AI coding agents mix human conversation, project planning, terminal commands, and multi-thousand-line logs into one single messy context window. Eventually, the agent gets confused, loses your original instructions, and hallucinates.

**AI-Chief fixes this by separating work into three tiers:**
1. **Chief:** You talk only to Chief. Chief speaks clearly, never writes code, and never dumps logs.
2. **Manager:** Plans the work, selects files, and supervises disposable workers.
3. **Worker:** Writes code, runs tests, and reports back. Discarded immediately after.

---

## 1-Minute Setup

Choose the installation method that fits your workflow:

### Option A: Clone into your repository
```bash
git clone https://github.com/your-username/ai-chief.git .ai-chief
```

### Option B: Copy into existing project
Copy the `ai-chief/` folder directly into your project root.

### Option C: Point your AI tool to SKILL.md
If using Google Antigravity, Claude Code, or Cursor, tell your assistant:
> *"Load AI-Chief using `SKILL.md`."*

---

## Your First 3 Commands

### 1. Activate Chief
```text
/chief
```
> **Chief:** *"Hello! I'm Chief. I'm here to coordinate engineering tasks for you. What would you like to build or improve today?"*

### 2. Plan a Feature Safely
```text
/chief-plan add health check endpoint to our server
```
> **Chief:** *"I've had the manager analyze the server structure. We've queued a single task to add a GET `/health` endpoint returning server uptime and status code 200, covered by a unit test. Would you like us to proceed with `/chief-build`?"*

### 3. Build & Verify
```text
/chief-build
```
> **Chief:** *"The team has implemented the `/health` endpoint in `src/routes/health.ts` and wired it into `src/app.ts`. All test suites passed with 100% coverage. Everything is ready for deployment."*

---

## Quick Command Cheat Sheet

| Command | Action |
| :--- | :--- |
| `/chief [goal]` | Start working on a goal |
| `/chief-status` | See what the team is doing |
| `/chief-plan [goal]` | Plan without touching code |
| `/chief-build [task]` | Execute code modifications |
| `/chief-clean` | Compact and tidy up memory |
| `/chief-memory` | View stored preferences and ADRs |
| `/chief-reset` | Clear temporary scratchpad files |

*That's it! You're ready to use AI-Chief.*
