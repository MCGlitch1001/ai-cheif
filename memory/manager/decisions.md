# Manager Memory: Architectural Decision Records (ADRs)

> **IMMUTABLE / APPEND-ONLY ZONE:** Preserves engineering decisions, architectural trade-offs, and critical constraints. Do not compress or delete.

---

### ADR-001: Zero-Dependency Pure Markdown Architecture
- **Date:** Initialization
- **Status:** ACCEPTED
- **Context:** The orchestration framework must be ultra-portable across any AI agent environment (Google Antigravity, Claude Code, OpenAI Codex, Cursor) without requiring Python packages, databases, Docker containers, or background daemon processes.
- **Decision:** All memory, state tracking, and protocol definitions must use standard Markdown files. Task state is tracked by writing and reading discrete `.md` files.
- **Consequences:**
  - Zero setup friction.
  - Highly inspectable and version-controlled via standard Git.
  - Requires agents to read/write structured markdown rather than query an API.

---

### ADR-002: Disposable Worker Pattern with Strict Context Boundaries
- **Date:** Initialization
- **Status:** ACCEPTED
- **Context:** Monolithic coding agents accumulate conversation logs, shell outputs, and file contents, causing context degradation and token waste.
- **Decision:** Worker Agents are strictly disposable and have no persistent memory across tasks. Manager feeds only the necessary files and expected criteria. Manager absorbs raw Worker output, compresses it, and updates persistent memory.
- **Consequences:**
  - Prevents context window saturation.
  - Protects human and Manager context from low-level debugging noise.
  - Enables clean task isolation.

---

### ADR-003: Chief Agent Strict Non-Execution Constraint
- **Date:** Initialization
- **Status:** ACCEPTED
- **Context:** Mixing human interaction with code generation leads to chaotic, noisy chat logs and frequent hallucinations.
- **Decision:** Chief Agent is prohibited from writing code blocks, running terminal commands, or exposing internal protocol tags to the user. Chief delegates exclusively to Manager.
- **Consequences:**
  - Human user enjoys a clean, executive-level conversation flow.
  - Eliminates accidental execution of unauthorized code.
