# AI-Chief: 5-Minute Quick Start ⚡

Get started with **AI-Chief** in under five minutes.

---

## What is AI-Chief in 30 Seconds?

Most AI coding agents mix human conversation, raw code searches, terminal commands, and verbose logs into one messy context window. Eventually, the agent gets confused, loses instructions, or hallucinates changes.

**AI-Chief fixes this by running as a structured AI operating system:**
- **Single AI assistant:** No fake agent chatter (*no "Chief says...", "Worker says..."*).
- **7-Stage internal pipeline:** Understands intent, checks context, bounds files, plans internally, executes code, verifies tests, and compresses output.
- **Predictable output:** Clean 4-field standard format (`Summary`, `Changes`, `Status`, `Next`).
- **Filesystem-first memory:** Architectural decisions and user preferences persist on disk.

---

## 1-Minute Setup

### Option A: The 1-Line Installer (Recommended)
Paste this into your AI coding assistant:
```text
Install AI-Chief into this project.
```

### Option B: Point to the System Prompt
Tell your assistant:
```text
Load AI-Chief system prompt from core/system-prompt.md
```

### Option C: Clone into your repository
```bash
git clone https://github.com/MCGlitch1001/ai-cheif.git .ai-chief
```

---

## Your First 3 Commands

### 1. Execute a Task
```text
/chief add health check endpoint returning uptime and status 200
```

AI-Chief runs its internal pipeline and responds:

```markdown
Summary:
Added a GET /health endpoint returning current server uptime and 200 OK status.

Changes:
- src/routes/health.ts (New route handler)
- src/app.ts (Mounted health router at /health)
- tests/health.test.ts (Unit test validating response payload and status)

Status:
Completed

Next:
Run `npm test` to verify your test suite passes.
```

### 2. Plan Without Touching Code
```text
/chief-plan refactor database queries to use connection pooling
```

AI-Chief plans the architectural roadmap and queues proposed tasks in `memory/manager/active_tasks.md` without editing any source files.

### 3. Check Current Status
```text
/chief-status
```

AI-Chief inspects persistent memory and provides a concise snapshot of active tasks and project milestones.

---

## Quick Command Cheat Sheet

| Command | Action |
| :--- | :--- |
| `/chief [goal]` | Normal activation (executes via 7-stage internal pipeline) |
| `/chief+ [goal]` | Force full reload (recovers degraded context from filesystem) |
| `/chief-status` | See project status and active tasks from memory |
| `/chief-plan [goal]` | Plan without touching code |
| `/chief-build [task]` | Execute code modifications |
| `/chief-clean` | Compact and tidy up memory |
| `/chief-memory` | View stored preferences and ADRs |
| `/chief-reset` | Clear temporary staging files |

*That's it! You're ready to use AI-Chief.*
