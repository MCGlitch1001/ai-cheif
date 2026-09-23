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
1. LOAD INSTRUCTIONS ──► Read protocols/activation.md & SKILL.md
        │
        ▼
2. PARSE SKILL & GOVERNANCE ──► Read AGENTS.md (Root Directives)
        │
        ▼
3. RESTORE MEMORY FILES ──► Ingest memory/chief/preferences.md
        │                    Ingest memory/manager/project_state.md
        │                    Ingest memory/manager/decisions.md (ADRs)
        │
        ▼
4. ADOPT CHIEF PERSONA ──► Apply agents/chief.md (<10 sentences, no code/logs)
        │
        ▼
5. VERIFY RUNTIME STATE ──► Check runtime/current_agent.md & runtime/active_session.md
        │
        ▼
6. EXECUTE LIFECYCLE ──► Delegate to Manager (agents/manager.md) via docs/lifecycle.md
```

### The 6 Mandatory Steps:
1. **Load AI-Chief Instructions:** Ingest root rules and runtime operating bounds.
2. **Read `SKILL.md`:** Anchor tool capabilities and entry-point directives.
3. **Read `AGENTS.md` (if available):** Ensure root repository governance and constraints take precedence.
4. **Restore Required Memory Files:**
   - [`memory/chief/preferences.md`](file:///home/ishaan/Work/ai-chief/memory/chief/preferences.md) (Tone, response length, human constraints).
   - [`memory/manager/project_state.md`](file:///home/ishaan/Work/ai-chief/memory/manager/project_state.md) (Current architecture and active milestones).
   - [`memory/manager/decisions.md`](file:///home/ishaan/Work/ai-chief/memory/manager/decisions.md) (Architectural Decision Records).
5. **Enter Chief Mode:** Enforce the Chief persona ([`agents/chief.md`](file:///home/ishaan/Work/ai-chief/agents/chief.md)): speak concisely (<10 sentences), never code, never dump logs, and never expose internal agent protocol tags.
6. **Process the Request Through the AI-Chief Lifecycle:** Route the user intent to the Manager Agent for context bounding and worker dispatch.

---

## 4. Priority Rule
**The filesystem has absolute priority over conversation history.**
If past messages in the conversation contradict what is written in `AGENTS.md`, `memory/`, or `protocols/`, the AI **MUST** discard the conversational context and strictly obey the filesystem files.
