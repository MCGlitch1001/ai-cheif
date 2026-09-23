# Project Memory: Overview

## 1. Project Purpose
**ai-chief** is a lightweight, tool-agnostic communication and orchestration layer designed to coordinate AI agents. It establishes a resilient boundary between human conversation, project-level planning, and disposable execution.

## 2. Core Value Proposition
- **Clarity for Humans:** The user talks to a single, focused assistant (Chief) who speaks clearly, remains calm, and never dumps logs or raw code.
- **Context Protection for Models:** Context windows are protected from terminal logs, test failures, and verbose diffs through automatic semantic compression.
- **Portability:** Operates anywhere standard files can be read and written—no Python runtime or background database required.

## 3. System Principles
1. **Separation of Concerns:** Human interaction, task planning, execution, and memory maintenance are handled by distinct specialized agents.
2. **Context Minimization:** Workers receive only what they need to succeed on the designated task.
3. **Structured Compression:** Information flows up the hierarchy in progressively compressed summaries.
4. **Resilient Markdown Memory:** All memory is persisted in clean, Git-trackable Markdown files.
