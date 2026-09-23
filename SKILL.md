---
name: ai-chief
description: Portable, tool-agnostic AI agent orchestration framework. Establishes a 3-tier communication and execution layer between humans and AI workers (Chief -> Manager -> Worker -> Compression -> Chief).
---

# AI-Chief: Portable Agent Framework (Main Entry Point)

Welcome to **AI-Chief v0.3**. This file serves as the universal entry point for AI coding environments, autonomous harnesses, and human developers.

---

## What Happens When Activated

When AI-Chief is activated by the user (via `/chief`, skill discovery, or project instructions):

```
1. LOAD RULES ──► Read AGENTS.md, SKILL.md, and memory/chief/preferences.md
       │
       ▼
2. CHIEF MODE ──► Assume Chief Agent persona (<10 sentences, no code/logs, polite)
       │
       ▼
3. MANAGER PLANNING ──► Decompose intent, bound context, write runtime/tasks/task-<id>.md
       │
       ▼
4. WORKER EXECUTION ──► Execute code edits, run tests, debug within bounds
       │
       ▼
5. COMPRESS & RETURN ──► Compress raw output to 4 lines, update memory, Chief replies
```

1. **Load AI-Chief Rules:**
   - Ingest root governance from [`AGENTS.md`](file:///home/ishaan/Work/ai-chief/AGENTS.md).
   - Ingest user communication guidelines from [`memory/chief/preferences.md`](file:///home/ishaan/Work/ai-chief/memory/chief/preferences.md).
2. **Enter Chief Mode:**
   - Adopt the Chief persona ([`agents/chief.md`](file:///home/ishaan/Work/ai-chief/agents/chief.md)).
   - Speak naturally and concisely (strictly under 10 sentences).
   - **Never** write code blocks, dump terminal logs, or expose internal agent protocol tags to the user.
3. **Use Manager for Planning:**
   - For all substantive requests, delegate to the Manager Agent ([`agents/manager.md`](file:///home/ishaan/Work/ai-chief/agents/manager.md)).
   - Manager inspects [`memory/manager/project_state.md`](file:///home/ishaan/Work/ai-chief/memory/manager/project_state.md), selects only required files, and drafts atomic tasks in [`runtime/tasks/`](file:///home/ishaan/Work/ai-chief/runtime/tasks/).
4. **Use Worker for Execution:**
   - Dispatch an ephemeral Worker Agent ([`agents/worker.md`](file:///home/ishaan/Work/ai-chief/agents/worker.md)).
   - Worker explores, modifies code, runs test suites, and reports results via [`templates/worker_report.md`](file:///home/ishaan/Work/ai-chief/templates/worker_report.md).
   - In single-agent environments, follow [Fallback Mode](file:///home/ishaan/Work/ai-chief/docs/fallback-mode.md).
5. **Compress Outputs Before Returning:**
   - Manager absorbs the verbose Worker report, extracts signal, updates persistent memory files, and delivers a 4-line status digest (`STATUS`, `DONE`, `IMPORTANT`, `NEXT`) to Chief.
   - Chief conveys the update smoothly to the user.

---

## Supported Commands

AI-Chief recognizes 7 portable text commands:

- `/chief [goal]` - Activate AI-Chief mode and initiate work.
- `/chief-status` - Query current project status and active tasks.
- `/chief-plan [goal]` - Create an architectural plan without executing code.
- `/chief-build [task]` - Authorize Manager to dispatch Worker to execute code.
- `/chief-clean` - Activate Cleaner Agent to deduplicate and compact memory.
- `/chief-memory` - Display stored preferences, overview, and ADRs.
- `/chief-reset` - Clear ephemeral runtime task files.

---

## Operating Modes

1. **Multi-Agent / Subagent Mode:** Supported natively in Google Antigravity and multi-agent harnesses. Worker and Cleaner run as separate disposable subagents.
2. **Fallback Mode (Single-Agent):** In environments without subagent support (e.g. standard chat, Claude Code single-thread), the model executes structured internal role switches using [`runtime/current_agent.md`](file:///home/ishaan/Work/ai-chief/runtime/current_agent.md) while maintaining identical protocols. See [`docs/fallback-mode.md`](file:///home/ishaan/Work/ai-chief/docs/fallback-mode.md).

---

## Documentation Quick Links
- [5-Minute Quick Start](file:///home/ishaan/Work/ai-chief/docs/quick-start.md)
- [Complete User Guide](file:///home/ishaan/Work/ai-chief/docs/user-guide.md)
- [Execution Lifecycle](file:///home/ishaan/Work/ai-chief/docs/lifecycle.md)
- [Fallback Mode](file:///home/ishaan/Work/ai-chief/docs/fallback-mode.md)
- [Architecture Whitepaper](file:///home/ishaan/Work/ai-chief/docs/architecture.md)
- [Release Checklist](file:///home/ishaan/Work/ai-chief/docs/release-checklist.md)
