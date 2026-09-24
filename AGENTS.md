# AI-Chief: Project Directives & Governance

## 1. System Identity & Mission
AI-Chief is a **portable opt-in prompt mode** designed to make AI assistants more focused, concise, actionable, and consistent.

AI-Chief centers entirely around one feature: **an opt-in `/chief` prompt mode**:
- **Without `/chief`:** The AI behaves normally, following its existing system prompt, personality, tools, and instructions.
- **With `/chief`:** The AI activates AI-Chief operational directives for that message (direct execution, conciseness, internal deliberation, file/tool grounding, and zero unsolicited churn).

---

## 2. Inviolable Architectural Constraints
- **Pure Prompt Framework:** AI-Chief consists strictly of prompt files and documentation.
- **Opt-In Per Message:** Never permanently alter the assistant's personality or behavior. Activation requires explicit `/chief` prefix.
- **Zero Complex Infrastructure:** No agents, no subagents, no orchestration engines, no runtimes, no message queues, no APIs, no databases, no servers, and no background daemons.
- **No Simulated Agent Chatter:** Never simulate multi-agent conversations (e.g., *"Chief says..."*, *"Manager says..."*, *"Worker says..."*).
- **Additive & Preserving:** Preserves existing persona, domain knowledge, capabilities, safety rules, and tools seamlessly.
- **Simplicity:** The repository must remain minimal and immediately understandable within 20 seconds.

---

## 3. Repository Structure
```text
ai-chief/
├── README.md
├── prompts/
│   └── mode-1.md
├── examples/
│   └── examples.md
├── LICENSE
├── AGENTS.md
└── index.html
```
