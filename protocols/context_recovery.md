# Protocol: Context Recovery & Self-Healing

## 1. Purpose
This protocol defines how AI-Chief detects context corruption, hallucinated roles, or silent prompt compression by the underlying LLM platform, and how the system safely self-heals by triggering a complete framework reload.

---

## 2. Trigger Symptoms (When Recovery is Required)

The AI assistant or the human user should trigger Context Recovery if any of the following conditions occur:

1. **Missing Instructions:** The AI cannot locate or recall rules regarding Chief, Manager, or Worker constraints.
2. **Unclear Agent Role:** The model begins attempting to write code while speaking to the user, or dumps terminal logs in the chat.
3. **Conflicting Behavior:** The AI acts on hallucinated previous conversation history that contradicts `memory/` or `AGENTS.md`.
4. **Compressed Context:** The host platform (Claude Code, ChatGPT, Antigravity, Cursor) emits a context compression warning, drops earlier turns, or reaches high token thresholds (>100k tokens).
5. **Stale Runtime:** `runtime/current_agent.md` indicates an out-of-sync agent state.

---

## 3. The Recovery Command: `/chief+`

When any symptom is detected:
- The AI **must proactively request** that the user issue `/chief+`, OR
- The human user issues `/chief+ [request]`.

```text
/chief+ [request]
```

### Example:
```text
/chief+ refactor the database layer
```

---

## 4. Full Framework Reload Procedure

When `/chief+` is invoked, the AI must bypass all active chat memory and perform a cold-start reload of the filesystem:

```
                  ┌────────────────────────┐
                  │ User invokes: /chief+  │
                  └───────────┬────────────┘
                              │
                              ▼
                  ┌────────────────────────┐
                  │ 1. Purge Ephemeral     │ ──► Clear runtime/tasks/*.md
                  │    Scratchpad          │
                  └───────────┬────────────┘
                              │
                              ▼
                  ┌────────────────────────┐
                  │ 2. Force Reload Core   │ ──► SKILL.md & AGENTS.md
                  │    Directives          │
                  └───────────┬────────────┘
                              │
                              ▼
                  ┌────────────────────────┐
                  │ 3. Force Reload Agent  │ ──► agents/chief.md
                  │    Definitions         │     agents/manager.md
                  └───────────┬────────────┘     agents/worker.md
                              │
                              ▼
                  ┌────────────────────────┐
                  │ 4. Force Reload All    │ ──► protocols/activation.md
                  │    Protocols           │     protocols/commands.md
                  └───────────┬────────────┘     protocols/delegation.md
                              │                  protocols/memory_rules.md
                              ▼
                  ┌────────────────────────┐
                  │ 5. Resync All Memory   │ ──► memory/chief/preferences.md
                  │    State               │     memory/manager/project_state.md
                  └───────────┬────────────┘     memory/manager/decisions.md
                              │                  memory/project/overview.md
                              ▼
                  ┌────────────────────────┐
                  │ 6. Reset Current Agent │ ──► runtime/current_agent.md -> Chief
                  │    & Resume Task       │     Process request through Chief
                  └────────────────────────┘
```

### Reload Manifest:
The model must explicitly reload the following files from disk:
- **Root Directives:** [`SKILL.md`](file:///home/ishaan/Work/ai-chief/SKILL.md), [`AGENTS.md`](file:///home/ishaan/Work/ai-chief/AGENTS.md)
- **Agent Personas:** [`agents/chief.md`](file:///home/ishaan/Work/ai-chief/agents/chief.md), [`agents/manager.md`](file:///home/ishaan/Work/ai-chief/agents/manager.md), [`agents/worker.md`](file:///home/ishaan/Work/ai-chief/agents/worker.md)
- **Protocols:** All files in [`protocols/`](file:///home/ishaan/Work/ai-chief/protocols/)
- **Memory Banks:** All files in [`memory/`](file:///home/ishaan/Work/ai-chief/memory/)
- **Runtime:** Reset [`runtime/current_agent.md`](file:///home/ishaan/Work/ai-chief/runtime/current_agent.md) to `Chief`

---

## 5. Automated Recovery Prompt

If the AI notices it has violated Chief guidelines (e.g., almost printed code to the human), it must pause and self-correct with this notice:

> *"I detected that my context state was degraded by recent conversation compression. I have reloaded my core directives from the filesystem via `/chief+` and am back in Chief mode. How would you like me to proceed with your request?"*
