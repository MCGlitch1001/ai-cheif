# Protocol: Persistent Activation System

> **Core Principle:** *The conversation is temporary. The filesystem is the source of truth.*

---

## 1. Overview
AI platforms routinely truncate old conversation turns, compress chat context, or purge system prompts over extended sessions. If an AI coding framework relies on conversation history to remember its directives, it will inevitably degrade, break character, or forget architectural constraints.

**AI-Chief decouples its intelligence from conversation state.** Every interaction is anchored to the persistent filesystem.

---

## 2. Standard Activation: `/chief [request]`

Every AI-Chief task must be initiated or prefixed with:

```text
/chief [request]
```

### Example:
```text
/chief add authentication to the application using JWT
```

---

## 3. Activation Lifecycle (Filesystem-First)

When `/chief` is detected in user input, the AI assistant **MUST** immediately execute the following 6-step bootstrap sequence before processing the request:

```
[User invokes /chief]
        │
        ▼
1. LOAD SYSTEM PROMPT ──► Read core/system-prompt.md & core/config.md
        │
        ▼
2. PARSE GOVERNANCE ────► Read SKILL.md & AGENTS.md (Root Directives)
        │
        ▼
3. RESTORE MEMORY ──────► Ingest memory/chief/preferences.md
        │                 Ingest memory/manager/project_state.md
        │                 Ingest memory/manager/decisions.md (ADRs)
        │
        ▼
4. APPLY CONFIG ────────► Enforce communication style, length, and risk limits
        │
        ▼
5. VERIFY RUNTIME ──────► Check runtime/active_session.md
        │
        ▼
6. EXECUTE PIPELINE ────► Run 7-stage internal pipeline (docs/lifecycle.md)
```

### The 6 Mandatory Steps:
1. **Load AI-Chief System Prompt:** Ingest [`core/system-prompt.md`](file:///home/ishaan/Work/ai-chief/core/system-prompt.md) and user configuration in [`core/config.md`](file:///home/ishaan/Work/ai-chief/core/config.md).
2. **Read `SKILL.md` & `AGENTS.md`:** Anchor root governance, operational constraints, and filesystem priority.
3. **Restore Required Memory Files:**
   - [`memory/chief/preferences.md`](file:///home/ishaan/Work/ai-chief/memory/chief/preferences.md) (Tone, response length, human constraints).
   - [`memory/manager/project_state.md`](file:///home/ishaan/Work/ai-chief/memory/manager/project_state.md) (Current architecture and active milestones).
   - [`memory/manager/decisions.md`](file:///home/ishaan/Work/ai-chief/memory/manager/decisions.md) (Architectural Decision Records).
4. **Apply Operational Bounds:** Adhere strictly to the communication style, sentence limits, and risk parameters in [`core/config.md`](file:///home/ishaan/Work/ai-chief/core/config.md).
5. **Verify Runtime State:** Ensure runtime staging paths (`runtime/tasks/`, `runtime/active_session.md`) are initialized and clear of stale artifacts.
6. **Process the Request Through the 7-Stage Pipeline:** Bound context, plan internally, execute modular changes, verify with tests, and emit the standard 4-field response (`Summary`, `Changes`, `Status`, `Next`).

---

## 4. Priority Rule
**The filesystem has absolute priority over conversation history.**
If past messages in the conversation contradict what is written in `core/system-prompt.md`, `AGENTS.md`, or `memory/`, the AI **MUST** discard the conversational context and strictly obey the filesystem files.
