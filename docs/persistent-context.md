# Persistent Context: Surviving Chat Truncation

> **Core Law:** *The conversation is temporary. The filesystem is the source of truth.*

---

## 1. Why Chat History Cannot Be Trusted

Almost all autonomous AI coding failures trace back to a single assumption: **that the LLM's conversation history is permanent.**

In reality:
1. **Host-Level Summarization:** Platforms like ChatGPT, Claude Code, and Cursor quietly compress, truncate, or sliding-window past turns once conversations exceed token thresholds.
2. **Instruction Drift:** Initial instructions (e.g. "Do not dump raw logs", "Always check tests", "Use TypeScript only") vanish from the active context window over long sessions.
3. **Hallucinated State:** An assistant with degraded context begins guessing what files exist or what architecture was agreed upon earlier.
4. **Session Volatility:** If your IDE restarts, your terminal crashes, or you switch from Claude Code to Antigravity, pure conversational context is lost completely.

---

## 2. How AI-Chief Survives

AI-Chief treats every conversation turn as ephemeral. It maintains its intelligence and architectural continuity through **three persistent pillars**:

```
┌─────────────────────────────────────────────────────────────┐
│ 1. PERSISTENT INSTRUCTIONS (core/system-prompt.md, AGENTS) │
│    Reloaded on every /chief or /chief+ call                 │
├─────────────────────────────────────────────────────────────┤
│ 2. PERSISTENT MEMORY (memory/manager/*.md, memory/chief/)   │
│    Project state, ADR decisions, and user preferences       │
│    stored as human-readable, Git-versioned Markdown         │
├─────────────────────────────────────────────────────────────┤
│ 3. THE RECOVERY PROTOCOL (/chief+)                          │
│    Forces a complete cold-start filesystem reload           │
│    whenever context compression or behavioral drift occurs  │
└─────────────────────────────────────────────────────────────┘
```

### Surviving Context Limits
Instead of stuffing thousands of tokens of test logs, file listings, and diffs into the chat, AI-Chief runs changes internally, verifies with tests, and updates [`memory/manager/project_state.md`](file:///home/ishaan/Work/ai-chief/memory/manager/project_state.md) and [`memory/manager/decisions.md`](file:///home/ishaan/Work/ai-chief/memory/manager/decisions.md).

### Surviving Conversation Compression
Because memory lives on disk, host platform compression only affects temporary dialogue. The project's milestones, user preferences, and architectural constraints remain safely preserved on the filesystem.

### Surviving Platform Switches
You can start a project in Google Antigravity, switch to Claude Code in the terminal, and later inspect it in Cursor. Each tool reads [`core/system-prompt.md`](file:///home/ishaan/Work/ai-chief/core/system-prompt.md) and [`AGENTS.md`](file:///home/ishaan/Work/ai-chief/AGENTS.md) and picks up right where the other left off.

---

## 3. Workflow Comparison

### ❌ The Fragile Monolithic Workflow
```text
Turn 1:   User gives complex system instructions & preferences.
Turn 50:  Assistant is doing well.
Turn 200: Context limit hit. Platform silently compresses early turns.
Turn 350: Initial rules disappear. Assistant begins hallucinating patterns.
Turn 500: Total failure. Assistant overwrites core files and dumps raw logs.
```

### ✅ The Resilient AI-Chief Workflow
```text
Task 1:   User: "/chief add user database models"
          -> AI reads core/system-prompt.md & memory/ -> Bounds context -> Executes -> Memory synced.

Task 50:  User: "/chief add password hashing"
          -> AI re-verifies memory/manager/decisions.md -> ADRs respected -> Output in 4-field schema.

Task 200: Host platform truncates conversation history.
          User: "/chief add password reset endpoints"
          -> AI detects /chief -> Re-reads filesystem -> Behaves identically to Turn 1!

Task 500: Complex multi-file refactor causes confusion.
          User: "/chief+ refactor auth into separate service"
          -> AI forces full reload -> Pristine context restored -> Task completed flawlessly.
```

---

## 4. The Two Golden Rules for Users

1. **Prefix Every Substantive Request with `/chief`:**
   ```text
   /chief [your task here]
   ```
   This triggers the 6-step bootstrap in [`protocols/activation.md`](file:///home/ishaan/Work/ai-chief/protocols/activation.md), ensuring the model never operates on stale assumptions.

2. **Use `/chief+` Whenever Behavior Drifts:**
   If the AI ever writes unstructured churn in chat, dumps terminal logs, or forgets a previous decision, simply issue:
   ```text
   /chief+ [task]
   ```
   This immediately forces a cold-start reload of all framework files, resetting the assistant back to peak precision.
