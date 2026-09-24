# AI-Chief 🎖️

A small system-prompt layer that makes AI coding assistants more focused, concise, and consistent.

---

## The Two Products

### [Mode 1 — AI-Chief System Prompt](prompts/mode-1.md)
**Copy the prompt into your system prompt.**
- Works by itself if you don't have a system prompt.
- Works by appending to the bottom of your existing system prompt.
- Makes the AI concise, action-oriented, grounded in your files, and free of conversational fluff.

👉 **Get the prompt:** [`prompts/mode-1.md`](prompts/mode-1.md)

---

### [Mode 2 — AI-Chief Prompt Modifier](prompts/mode-2.md)
**Already have a custom system prompt?**
- Give Mode 2 your existing prompt and AI-Chief.
- It uses any LLM to intelligently merge both prompts into one clean, optimized, cohesive prompt.
- Preserves your domain rules, personality, and tool instructions while eliminating fluff and contradictions.

👉 **Get the modifier:** [`prompts/mode-2.md`](prompts/mode-2.md)

---

## Core Principle: An Add-On Layer

AI-Chief does **not** try to replace your existing AI personality or system prompt. It is an operational add-on:

```text
Existing System Prompt
         +
      AI-Chief
         =
Better Operating Behavior
```

---

## What It Fixes

| Without AI-Chief | With AI-Chief |
| :--- | :--- |
| Explains what *could* be done with partial code placeholders | Performs the requested work directly and completely |
| Dumps conversational pleasantries and meta-commentary | Delivers direct, concise, high-signal responses |
| Hallucinates architecture based on temporary chat history | Treats repository files as the authoritative source of truth |
| Spills raw chain-of-thought and internal reasoning | Keeps reasoning internal and outputs clean deliverables |

---

## Quick Example

Want to see how Mode 2 combines a custom prompt with AI-Chief?

Check out [`examples/before-after.md`](examples/before-after.md) to see a real-world prompt before and after enhancement.

For setups across Cursor, Claude, ChatGPT, and Windsurf, see [`examples/examples.md`](examples/examples.md).

---

## License

MIT License. Free for personal and commercial use.
