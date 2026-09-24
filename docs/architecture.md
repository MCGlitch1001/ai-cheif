# Architecture Whitepaper: AI-Chief

## 1. Abstract
As autonomous coding tools grow in popularity, monolithic assistants that attempt to chat with the developer, navigate large codebases, write code, run builds, debug failures, and dump thousands of lines of terminal output into a single conversation thread inevitably degrade. The failure modes include context saturation, forgotten system instructions, hallucinated edits, and conversational noise.

**AI-Chief** introduces an ergonomic, tool-agnostic AI operating system architecture. It transforms any AI coding assistant into a structured, disciplined software engineer governed by a persistent single-agent operating system prompt ([`core/system-prompt.md`](file:///home/ishaan/Work/ai-chief/core/system-prompt.md)), an internal 7-stage execution lifecycle, and filesystem-based persistent memory.

AI-Chief is **one single AI assistant** operating with an internal execution framework. It does **NOT** simulate fake multi-agent dialogues (e.g., *"Chief says..."*, *"Manager says..."*, *"Worker says..."*), ensuring that developers receive clear, high-signal technical deliverables.

---

## 2. Core Architecture: The Single-Agent Operating System

```
                    ┌────────────────────────┐
                    │       Human User       │
                    └───────────┬────────────┘
                                │ Command (/chief, /chief-build, etc.)
                                ▼
                    ┌────────────────────────┐
                    │ AI-Chief System Prompt │ (core/system-prompt.md)
                    └───────────┬────────────┘
                                │
                                ▼
                    ┌────────────────────────┐
                    │   Internal Execution   │
                    │       Framework        │ (7-Stage Pipeline)
                    │  (Understand ➔ Plan    │
                    │   ➔ Bound ➔ Execute    │
                    │   ➔ Test ➔ Compress)   │
                    └───────────┬────────────┘
                                │ High-Signal Response
                                ▼
                    ┌────────────────────────┐
                    │     Human Response     │ (Summary / Changes / Status / Next)
                    └────────────────────────┘
```

---

## 3. The 7-Stage Internal Pipeline

Rather than exposing messy tool output, intermediate reasoning, and test dumps, AI-Chief processes tasks internally across 7 deterministic phases:

1. **Understand Request:** Analyze user intent, extract requirements, and identify implicit constraints.
2. **Context Check:** Ingest [`core/config.md`](file:///home/ishaan/Work/ai-chief/core/config.md) and persistent memory from [`memory/`](file:///home/ishaan/Work/ai-chief/memory/) (`preferences.md`, `decisions.md`, `project_state.md`).
3. **Context Bounding:** Determine strictly the minimal set of files needed for the task to avoid prompt bloat.
4. **Internal Planning:** Formulate an atomic, minimal-dependency execution roadmap. (If `/chief-plan` was triggered, record tasks and stop without editing code).
5. **Modular Execution:** Write defensive, modular code changes adhering to repository conventions.
6. **Verification & Testing:** Validate changes using compiler checks, linters, and unit test suites.
7. **Semantic Compression:** Filter raw test outputs and compiler passes into the clean 4-field response format.

---

## 4. Context Protection & Token Economics

In a standard unstructured agent interaction:
- **Turn 1:** 2,000 tokens (System prompt + user request)
- **Turn 2:** 15,000 tokens (Greedy repository search + 6 files read)
- **Turn 3:** 30,000 tokens (Compiler stack trace + messy diff attempt)
- **Turn 10:** 80,000+ tokens (Context degradation, high latency, loss of early constraints)

In **AI-Chief**:
- **Bounded Ingestion:** Context is bounded strictly to target files.
- **The Filesystem Rule:** *The conversation is temporary. The filesystem is the source of truth.* Stale conversational context is discarded on demand via `/chief+`, loading fresh instructions from disk.
- **Predictable Output:** Responses are constrained to fewer than 10 sentences or the 4-field summary schema, preventing conversational token inflation.

---

## 5. Portability & Zero-Dependency Philosophy

AI-Chief requires:
- No database engine (PostgreSQL, SQLite, ChromaDB)
- No backend server process, daemon, or microservice
- No Node.js / Python framework installation
- No proprietary API bindings or keys

Any system capable of reading and editing files (Claude Code, OpenAI Codex, Google Antigravity, Cursor, Windsurf, or Gemini CLI) can run AI-Chief natively.

---

## 6. Advanced Extension: Native Multi-Agent Orchestration

On platforms featuring native background subagent processes (such as Google Antigravity with `invoke_subagent`), AI-Chief can optionally elevate internal pipeline stages into isolated child processes.

See [`docs/advanced/native-agents.md`](file:///home/ishaan/Work/ai-chief/docs/advanced/native-agents.md) for architectural patterns and integration guidelines.
