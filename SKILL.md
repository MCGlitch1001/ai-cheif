---
name: ai-chief
description: Portable, tool-agnostic AI agent operating system prompt and execution framework. Establishes a disciplined 7-stage internal execution pipeline, persistent filesystem memory, and clean, high-signal human communication without simulated agent chatter.
---

# AI-Chief: Portable AI Operating System (Main Entry Point)

Welcome to **AI-Chief**. This file serves as the universal entry point for AI coding environments, autonomous harnesses, and human developers.

---

## 1. Activation Rules

> [!IMPORTANT]
> **Core Principle:** *The conversation is temporary. The filesystem is the source of truth.*

### Critical Directives:
1. **Never rely on previous conversation history:** Host AI platforms routinely summarize, compress, or purge conversational context. AI-Chief relies on the filesystem to maintain its persona, rules, and architectural decisions.
2. **Behavior must always come from the filesystem:**
   - [`core/system-prompt.md`](file:///home/ishaan/Work/ai-chief/core/system-prompt.md) (Core Operating System Prompt)
   - [`core/config.md`](file:///home/ishaan/Work/ai-chief/core/config.md) (User Settings)
   - [`AGENTS.md`](file:///home/ishaan/Work/ai-chief/AGENTS.md) (System Governance)
   - Protocols in [`protocols/`](file:///home/ishaan/Work/ai-chief/protocols/)
   - Persistent memory files in [`memory/`](file:///home/ishaan/Work/ai-chief/memory/)
3. **When `/chief` is activated:** The filesystem has **absolute priority** over conversation history. If any past conversational message contradicts what is on disk, follow the filesystem.
4. **Context Recovery via `/chief+`:** If context is degraded or behavior appears incorrect, invoke `/chief+` to trigger a cold-start reload from disk.

---

## 2. What Happens When Activated

When `/chief [task]` is detected, execute the 7-stage internal execution pipeline:

```
1. PARSE INTENT ──────► Understand goal, edge cases, and constraints
       │
       ▼
2. CONTEXT CHECK ────► Read core/config.md and persistent memory
       │
       ▼
3. BOUND FILES ──────► Select minimal required file set
       │
       ▼
4. PLAN INTERNALLY ──► Formulate atomic steps (or halt if /chief-plan)
       │
       ▼
5. EXECUTE ──────────► Perform modular, defensive code modifications
       │
       ▼
6. VERIFY ───────────► Run test commands and check for regressions
       │
       ▼
7. SUMMARIZE ────────► Output clean 4-field response (Summary/Changes/Status/Next)
```

1. **Parse Intent:** Ingest the request and isolate explicit goals.
2. **Context Check:** Ingest [`core/config.md`](file:///home/ishaan/Work/ai-chief/core/config.md), user preferences in [`memory/chief/preferences.md`](file:///home/ishaan/Work/ai-chief/memory/chief/preferences.md), and project decisions in [`memory/manager/decisions.md`](file:///home/ishaan/Work/ai-chief/memory/manager/decisions.md).
3. **Bound Files:** Identify strictly the minimal set of files to inspect or modify.
4. **Internal Planning:** Plan minimal-dependency steps without conversational meta-commentary.
5. **Execute:** Edit files, run build commands, or implement features.
6. **Verify:** Check compilation, run test suites, or inspect diffs.
7. **Semantic Compression:** Deliver a structured, human-friendly response using the 4-field format (`Summary`, `Changes`, `Status`, `Next`).

---

## 3. Supported Commands

- `/chief [task]` — Normal AI-Chief activation. Loads required framework files from disk.
- `/chief+ [task]` — Full framework reload. Used after context compression or when recovering behavior.
- `/chief-status` — Shows current project state and active tasks from memory.
- `/chief-plan [goal]` — Creates an architectural plan without executing code.
- `/chief-build [task]` — Executes technical changes for an approved task or goal.
- `/chief-clean` — Deduplicates and compacts compressible memory.
- `/chief-memory` — Shows stored memory (preferences, overview, ADRs).
- `/chief-reset` — Clears temporary runtime task files only.

---

## 4. Documentation Quick Links

- [Core System Prompt](file:///home/ishaan/Work/ai-chief/core/system-prompt.md)
- [Core Configuration](file:///home/ishaan/Work/ai-chief/core/config.md)
- [System Governance](file:///home/ishaan/Work/ai-chief/AGENTS.md)
- [Installation Guide](file:///home/ishaan/Work/ai-chief/docs/installation.md)
- [Platform Compatibility](file:///home/ishaan/Work/ai-chief/docs/platforms.md)
- [Advanced: Native Multi-Agent Orchestration](file:///home/ishaan/Work/ai-chief/docs/advanced/native-agents.md)
- [Command Specifications](file:///home/ishaan/Work/ai-chief/protocols/commands.md)
- [Standard Response Formats](file:///home/ishaan/Work/ai-chief/protocols/response_format.md)
- [Activation Protocol](file:///home/ishaan/Work/ai-chief/protocols/activation.md)
