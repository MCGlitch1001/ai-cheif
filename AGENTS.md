# AI-Chief: Project Directives & Governance

## 1. System Identity & Mission
AI-Chief is a **portable system-prompt framework** designed to make AI assistants more focused, concise, actionable, and consistent.

AI-Chief is centered entirely around **TWO simple products**:
1. **Mode 1 — AI-Chief System Prompt (`prompts/mode-1.md`):** A lightweight, self-contained additive instruction layer. It can be used alone or appended to an existing system prompt.
2. **Mode 2 — AI-Chief Prompt Modifier (`prompts/mode-2.md`):** A prompt template used with an LLM to merge an existing system prompt with AI-Chief into a single, cohesive, upgraded prompt.

---

## 2. Inviolable Architectural Constraints
- **Pure Prompt Framework:** AI-Chief consists strictly of prompt files.
- **Zero Complex Infrastructure:** No agents, no subagents, no orchestration engines, no runtimes, no message queues, no APIs, no databases, no servers, and no background daemons.
- **No Simulated Agent Chatter:** Never simulate multi-agent conversations (e.g., *"Chief says..."*, *"Manager says..."*, *"Worker says..."*).
- **Additive by Design:** AI-Chief is an add-on layer that improves operating behavior while preserving the user's existing domain rules, tools, and persona.
- **Simplicity:** The repository must remain minimal and immediately understandable within 30 seconds.

---

## 3. Repository Structure
```text
ai-chief/
│
├── README.md
│
├── prompts/
│   ├── mode-1.md
│   └── mode-2.md
│
├── examples/
│   ├── before-after.md
│   └── examples.md
│
├── LICENSE
└── AGENTS.md
```
