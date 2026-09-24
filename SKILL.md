---
name: ai-chief
description: Portable, tool-agnostic AI agent orchestration framework. Establishes a 3-tier communication and execution layer between humans and AI workers (Chief -> Manager -> Worker -> Compression -> Chief) with a persistent filesystem-first activation protocol.
---

# AI-Chief: Portable Agent Framework (Main Entry Point)

Welcome to **AI-Chief v0.3**. This file serves as the universal entry point for AI coding environments, autonomous harnesses, and human developers.

---

## Activation Rules

> [!IMPORTANT]
> **Core Principle:** *The conversation is temporary. The filesystem is the source of truth.*

### Critical Directives:
1. **Never rely on previous conversation history:** Host AI platforms routinely summarize, compress, or purge conversational context. AI-Chief must never depend on chat history to maintain its persona, rules, or architectural decisions.
2. **Behavior must always come from the filesystem:**
   - [`SKILL.md`](file:///home/ishaan/Work/ai-chief/SKILL.md) (Entry point)
   - [`AGENTS.md`](file:///home/ishaan/Work/ai-chief/AGENTS.md) (Root governance)
   - Agent definitions in [`agents/`](file:///home/ishaan/Work/ai-chief/agents/)
   - Protocols in [`protocols/`](file:///home/ishaan/Work/ai-chief/protocols/)
   - Persistent memory files in [`memory/`](file:///home/ishaan/Work/ai-chief/memory/)
3. **When `/chief` is activated:** The filesystem has **absolute priority** over conversation history. If any past conversational message contradicts what is on disk, follow the filesystem.
4. **Context Recovery via `/chief+`:** If context is degraded or behavior appears incorrect, invoke `/chief+` to trigger a cold-start reload from disk.

---

## What Happens When Activated

When `/chief [task]` is detected:

```
1. LOAD RULES ──► Read protocols/activation.md, SKILL.md, and AGENTS.md
       │
       ▼
2. CHIEF MODE ──► Assume Chief Agent persona (<10 sentences, no code/logs, polite)
       │
       ▼
3. RESTORE MEMORY ──► Ingest preferences.md, project_state.md, decisions.md
       │
       ▼
4. MANAGER PLANNING ──► Decompose intent, bound context, write runtime/tasks/task-<id>.md
       │
       ▼
5. WORKER EXECUTION ──► Execute code edits, run tests, debug within bounds
       │
       ▼
6. COMPRESS & RETURN ──► Compress raw output to 4 lines, update memory, Chief replies
```

1. **Load AI-Chief Rules:** Ingest root governance from [`AGENTS.md`](file:///home/ishaan/Work/ai-chief/AGENTS.md) and [`protocols/activation.md`](file:///home/ishaan/Work/ai-chief/protocols/activation.md).
2. **Enter Chief Mode:** Adopt [`agents/chief.md`](file:///home/ishaan/Work/ai-chief/agents/chief.md). Speak naturally (<10 sentences), never code, never dump logs.
3. **Restore Memory:** Read [`memory/chief/preferences.md`](file:///home/ishaan/Work/ai-chief/memory/chief/preferences.md), [`memory/manager/project_state.md`](file:///home/ishaan/Work/ai-chief/memory/manager/project_state.md), and [`memory/manager/decisions.md`](file:///home/ishaan/Work/ai-chief/memory/manager/decisions.md).
4. **Use Manager for Planning:** Delegate to [`agents/manager.md`](file:///home/ishaan/Work/ai-chief/agents/manager.md). Select required files and draft atomic tasks in [`runtime/tasks/`](file:///home/ishaan/Work/ai-chief/runtime/tasks/).
5. **Use Worker for Execution:** Dispatch an ephemeral Worker Agent ([`agents/worker.md`](file:///home/ishaan/Work/ai-chief/agents/worker.md)). In single-agent environments, follow [Fallback Mode](file:///home/ishaan/Work/ai-chief/docs/fallback-mode.md).
6. **Compress Outputs Before Returning:** Manager compresses Worker output to 4-line status (`STATUS`, `DONE`, `IMPORTANT`, `NEXT`), updates memory, and Chief delivers the final concise response.

---

## Supported Commands

- `/chief [task]` - Normal AI-Chief activation. Loads required framework files from disk.
- `/chief+ [task]` - Full framework reload. Used after context compression or when recovering behavior.
- `/chief-status` - Shows current project state from memory.
- `/chief-plan [goal]` - Creates an architectural plan without executing code.
- `/chief-build [task]` - Authorizes Manager to dispatch Worker to execute code.
- `/chief-clean` - Runs Cleaner Agent to deduplicate and compact memory.
- `/chief-memory` - Shows important stored memory (preferences, overview, ADRs).
- `/chief-reset` - Clears temporary runtime files only.

---

## Documentation Quick Links
- [Installation Guide](file:///home/ishaan/Work/ai-chief/docs/installation.md)
- [Platform Compatibility](file:///home/ishaan/Work/ai-chief/docs/platforms.md)
- [Persistent Context Guide](file:///home/ishaan/Work/ai-chief/docs/persistent-context.md)
- [Activation Protocol](file:///home/ishaan/Work/ai-chief/protocols/activation.md)
- [Context Recovery Protocol](file:///home/ishaan/Work/ai-chief/protocols/context_recovery.md)
- [5-Minute Quick Start](file:///home/ishaan/Work/ai-chief/docs/quick-start.md)
- [Complete User Guide](file:///home/ishaan/Work/ai-chief/docs/user-guide.md)
- [Execution Lifecycle](file:///home/ishaan/Work/ai-chief/docs/lifecycle.md)
- [Fallback Mode](file:///home/ishaan/Work/ai-chief/docs/fallback-mode.md)
- [Architecture Whitepaper](file:///home/ishaan/Work/ai-chief/docs/architecture.md)
