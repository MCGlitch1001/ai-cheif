# AI-Chief Configuration Settings

> **PORTABLE SETTINGS:** Customize framework behavior across different AI environments without code changes.

---

## 1. Agent Model Recommendations

| Agent | Role | Recommended Model Tier | Example Providers |
| :--- | :--- | :--- | :--- |
| **Chief** | Human Communication | Fast / High-EQ / Lightweight | Claude 3.5 Haiku, Gemini Flash, GPT-4o-mini |
| **Manager** | Planning & Context Control | High Reasoning / Long Context | Claude 3.7 Sonnet (Thinking), Gemini Pro, GPT-4o |
| **Worker** | Code & Tool Execution | High Code Accuracy / Fast Tools | Claude 3.7 Sonnet, OpenAI Codex, DeepSeek Coder |
| **Cleaner** | Memory Compaction | Fast / Token Efficient | Claude 3.5 Haiku, Gemini Flash |

---

## 2. Thresholds & Limits

- **Chief Max Output Length:** `10 sentences` (strictly enforced in conversational turns).
- **Cleaner Trigger Task Count:** `15 completed tasks` in `memory/manager/active_tasks.md`.
- **Cleaner Max Memory Lines:** `1000 lines` across all `memory/` files.
- **Worker Concurrency Limit:** `1` (sequential by default; can be raised to `3` if orchestrator supports parallel subagents).

---

## 3. Directory Paths

- **Core Agents:** `agents/`
- **Memory Root:** `memory/`
  - Chief Memory: `memory/chief/`
  - Manager Memory: `memory/manager/`
  - Project Memory: `memory/project/`
- **Runtime Staging:** `runtime/tasks/`
- **Protocols:** `protocols/`
- **Templates:** `templates/`
- **Documentation:** `docs/`

---

## 4. Environment Integrations

- **Google Antigravity:** Managed via `SKILL.md` and native subagents (`invoke_subagent`).
- **Claude Code:** Invoked via custom prompts or project instructions referencing `agents/chief.md`.
- **OpenAI Codex / ChatGPT CLI:** Instructions loaded from `AGENTS.md` and `agents/`.
- **Cursor / Windsurf:** Project rules loaded via `.cursorrules` or `.windsurfrules`.
