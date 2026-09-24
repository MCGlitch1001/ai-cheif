# AI-Chief 🎖️

> **A portable, lightweight AI operating system prompt and disciplined internal execution framework.**

[![License](https://img.shields.io/badge/license-MIT-green.svg)](file:///home/ishaan/Work/ai-chief/LICENSE)
[![Zero Dependencies](https://img.shields.io/badge/dependencies-0%20(Pure%20Markdown)-brightgreen.svg)](#install-ai-chief)
[![Architecture](https://img.shields.io/badge/architecture-Single--Agent%20OS%20Prompt-orange.svg)](file:///home/ishaan/Work/ai-chief/docs/architecture.md)

---

## 1. What is AI-Chief?

**AI-Chief** is a lightweight, zero-dependency AI operating system prompt and execution framework that bridges the gap between human developers and AI coding assistants.

AI-Chief is **one single AI assistant** operating with an internal execution framework. It is **NOT** a collection of fake agents talking to each other. You will never see simulated role tags like *"Chief says..."*, *"Manager says..."*, or *"Worker says..."*, and you will never be bombarded with intermediate terminal churn.

Instead, AI-Chief runs a structured 7-stage internal operating pipeline:
1. **Understand user intent:** Isolate core goals, edge cases, and constraints.
2. **Context check:** Inspect configuration and persistent Markdown memory on disk.
3. **Bound files:** Identify the minimal file set needed, avoiding context pollution.
4. **Plan internally:** Formulate an atomic execution sequence.
5. **Execute cleanly:** Make modular, defensive code changes.
6. **Verify:** Validate with tests, syntax checkers, or linters.
7. **Summarize:** Deliver a clean, predictable human-friendly response.

```
                    ┌────────────────────────┐
                    │       Human User       │
                    └───────────┬────────────┘
                                │ Command (/chief, /chief-build, etc.)
                                ▼
                    ┌────────────────────────┐
                    │ AI-Chief System Prompt │ (core/system-prompt.md)
                    └───────────┬────────────┘
                                │
                                ▼
                    ┌────────────────────────┐
                    │   Internal Execution   │
                    │       Framework        │ (7-Stage Pipeline)
                    │  (Understand ➔ Plan    │
                    │   ➔ Bound ➔ Execute    │
                    │   ➔ Test ➔ Compress)   │
                    └───────────┬────────────┘
                                │ High-Signal Response
                                ▼
                    ┌────────────────────────┐
                    │     Human Response     │ (Summary / Changes / Status / Next)
                    └────────────────────────┘
```

---

## 2. Why AI-Chief?

Standard AI coding agents suffer from two major failure modes:
1. **Context Bloat & Forgetting:** Over long sessions, conversation history overflows. The model forgets constraints, hallucinates previously discussed rules, or drifts off course.
2. **Conversational Churn:** The model floods the chat with internal monologues, half-baked reasoning, massive stack traces, and messy diffs.

**AI-Chief solves both:**
- **The Filesystem Rule:** *The conversation is temporary. The filesystem is the source of truth.* Instructions, architectural decisions (ADRs), and user preferences live permanently in Markdown files on disk.
- **High-Signal Output Schema:** All technical deliverables conclude with a 4-field standard response: `Summary`, `Changes`, `Status`, and `Next`.
- **Zero API/Plugin Lock-in:** 100% portable Markdown. Works seamlessly in Claude Code, Google Antigravity, OpenAI Codex, Cursor, Windsurf, or Gemini CLI.

---

## 3. Install AI-Chief

Installing AI-Chief is completely automated and non-destructive.

### 1-Line Installation Flow:
1. **Paste this single prompt into your AI coding tool:**
   ```text
   Install AI-Chief into this project.
   ```
2. **AI configures AI-Chief:** The assistant reads [`install.md`](file:///home/ishaan/Work/ai-chief/install.md), scaffolds `core/`, `memory/`, and `protocols/`, runs a 10-point self-audit, and responds:
   ```
   ✅ AI-Chief installed successfully.

   AI-Chief is ready.

   Start:
    /chief <your task>

   Commands:
    /chief-reset — reset temporary session data
    /chief-clean — clean and organize memory
    /chief-status — view project status
    /chief-memory — view saved memory
    /chief-plan — plan without executing
    /chief-build — execute approved tasks
   ```
3. **Alternatively, point your assistant directly to the system prompt:**
   ```text
   Load AI-Chief system prompt from core/system-prompt.md
   ```

*(See [`docs/installation.md`](file:///home/ishaan/Work/ai-chief/docs/installation.md) for full setup instructions.)*

---

## 4. How to Use

### Normal Activation:
```text
/chief add JWT authentication to our Express routes
```

AI-Chief runs its internal pipeline and replies using the standard format:

```markdown
Summary:
Implemented JWT authentication middleware and wired verification across protected API endpoints.

Changes:
- src/middleware/auth.ts (New JWT verification middleware)
- src/routes/api.ts (Protected routes with auth middleware)
- tests/auth.test.ts (Unit tests validating token parsing and expiration)

Status:
Completed

Next:
Run `npm test` to verify your test suite, and ensure `JWT_SECRET` is set in your `.env`.
```

---

## 5. Command System

AI-Chief includes built-in portable text commands:

| Command | Usage | Description |
| :--- | :--- | :--- |
| `/chief` | `/chief [request]` | Normal activation. Executes request through the 7-stage internal pipeline. |
| `/chief+` | `/chief+ [request]` | Full framework reload. Forces cold-start resync when context is degraded or compressed. |
| `/chief-status` | `/chief-status` | Returns a concise overview of current project state and task list. |
| `/chief-plan` | `/chief-plan [goal]` | Decomposes a goal into an architectural plan without executing code. |
| `/chief-build` | `/chief-build [task]` | Executes planned or requested technical modifications. |
| `/chief-clean` | `/chief-clean` | Deduplicates and compacts compressible memory files. |
| `/chief-memory` | `/chief-memory` | Displays stored preferences, project overview, and key ADR decisions. |
| `/chief-reset` | `/chief-reset` | Clears temporary task staging files in `runtime/tasks/`. |

---

## 6. Directory Structure

```
ai-chief/
├── core/                                    # Core AI Operating System Prompt & Config
│   ├── system-prompt.md                     # Main single-agent OS prompt
│   └── config.md                            # Editable user settings (tone, max sentences, risk)
│
├── AGENTS.md                                # Root directives, governance, and command mapping
├── README.md                                # Full framework overview and documentation
├── SKILL.md                                 # Skill entry point and discovery rules
├── LICENSE                                  # MIT License
├── install.md                               # Automated 1-line installation specification
│
├── memory/                                  # Pure Markdown persistent memory
│   ├── chief/
│   │   ├── preferences.md                   # Immutable user preferences & tone
│   │   └── conversation_state.md            # Compressible conversation milestones
│   ├── manager/
│   │   ├── project_state.md                 # Architecture, stack, and active milestone
│   │   ├── decisions.md                     # Immutable Architectural Decision Records (ADRs)
│   │   └── active_tasks.md                  # Task registry (active, queued, completed)
│   └── project/
│       └── overview.md                      # Global project goals and domain context
│
├── runtime/                                 # Ephemeral runtime staging
│   ├── active_session.md                    # Active session state & telemetry
│   └── tasks/                               # Ephemeral staging for task files
│       └── README.md
│
├── protocols/                               # Communication & execution specifications
│   ├── activation.md                        # Persistent activation & 6-step bootstrap protocol
│   ├── context_recovery.md                  # Self-healing reload triggers & /chief+
│   ├── commands.md                          # Portable command protocol specifications
│   ├── response_format.md                   # Standard 4-field output schema
│   ├── compression.md                       # Output compression algorithms and boundaries
│   ├── memory_rules.md                      # 3-tier memory durability hierarchy rules
│   └── delegation.md                        # Task specifications & delegation schemas
│
├── templates/                               # Operational Markdown templates
│   ├── task.md                              # Task definition template
│   └── memory_update.md                     # ADR and task status update templates
│
├── config/
│   └── settings.md                          # Model tiers, token limits, and thresholds
│
└── docs/                                    # Documentation library
    ├── README.md                            # Documentation index
    ├── installation.md                      # 1-line installation guide & safety rules
    ├── platforms.md                         # Cross-platform compatibility matrix
    ├── first-run.md                         # First run experience & walkthrough
    ├── quick-start.md                       # 5-minute onboarding guide
    ├── user-guide.md                        # Comprehensive user guide
    ├── persistent-context.md                # Context survival & recovery guide
    ├── lifecycle.md                         # 7-stage internal execution pipeline
    ├── commands.md                          # User commands & cross-tool integration guide
    ├── architecture.md                      # Architectural whitepaper
    └── advanced/
        └── native-agents.md                 # Optional native multi-agent orchestration
```

---

## 7. Optional Native Multi-Agent Orchestration

While AI-Chief operates out-of-the-box as a single unified assistant, platforms featuring **native background subagent processes** (e.g., Google Antigravity with `invoke_subagent`) can optionally distribute internal pipeline stages across isolated subagents.

See [`docs/advanced/native-agents.md`](file:///home/ishaan/Work/ai-chief/docs/advanced/native-agents.md) for full architectural patterns and implementation details.

---

## 8. License

MIT License. Free for personal and commercial use.
