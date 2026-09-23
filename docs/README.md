# AI-Chief Documentation Index

Welcome to the AI-Chief framework documentation library (v0.3).

## 🚀 Getting Started & Guides
1. [5-Minute Quick Start](file:///home/ishaan/Work/ai-chief/docs/quick-start.md)
   - Fastest path to understanding and trying AI-Chief.
2. [Complete User Guide](file:///home/ishaan/Work/ai-chief/docs/user-guide.md)
   - In-depth manual covering activation, core lifecycle, and all commands.
3. [Persistent Context Guide](file:///home/ishaan/Work/ai-chief/docs/persistent-context.md)
   - Why chat history cannot be trusted and how AI-Chief survives conversation compression.
4. [Fallback Mode Guide](file:///home/ishaan/Work/ai-chief/docs/fallback-mode.md)
   - Operating instructions for single-agent conversation environments using structured role switching.

## 🏗️ Architecture & Protocols
5. [Architecture Whitepaper](file:///home/ishaan/Work/ai-chief/docs/architecture.md)
   - 3-tier stratification, context isolation, and token economics.
6. [Persistent Activation Protocol](file:///home/ishaan/Work/ai-chief/protocols/activation.md)
   - 6-step bootstrap sequence when `/chief` is detected.
7. [Context Recovery Protocol](file:///home/ishaan/Work/ai-chief/protocols/context_recovery.md)
   - Self-healing reload triggers and the `/chief+` command.
8. [Execution Lifecycle](file:///home/ishaan/Work/ai-chief/docs/lifecycle.md)
   - Complete 7-stage breakdown from human prompt to Chief response.
9. [Command System & Tool Integration](file:///home/ishaan/Work/ai-chief/docs/commands.md)
   - Cross-tool integration guide for Google Antigravity, Claude Code, OpenAI Codex, and Cursor.
10. [Command Protocol Specification](file:///home/ishaan/Work/ai-chief/protocols/commands.md)
    - Formal syntax and behavior matrix for `/chief`, `/chief+`, `/chief-plan`, `/chief-build`, etc.
11. [Inter-Agent Delegation Protocol](file:///home/ishaan/Work/ai-chief/protocols/delegation.md)
    - Schemas for `Manager → Worker`, `Worker → Manager`, and `Manager → Chief`.
12. [Compression Protocol](file:///home/ishaan/Work/ai-chief/protocols/compression.md)
    - Inviolable immutable boundaries and semantic compression algorithms.
13. [Memory Hygiene Rules](file:///home/ishaan/Work/ai-chief/protocols/memory_rules.md)
    - The 3-tier memory durability hierarchy and cleaner safeguards.
14. [Response Formats](file:///home/ishaan/Work/ai-chief/protocols/response_format.md)
    - Formatting standards for Chief, Manager, Worker, and Cleaner.

## 📋 Governance & Verification
15. [System Governance & AGENTS.md](file:///home/ishaan/Work/ai-chief/AGENTS.md)
    - Universal rules for all AI models operating in this repository.
16. [Release Checklist](file:///home/ishaan/Work/ai-chief/docs/release-checklist.md)
    - Readiness verification for public open-source releases.
