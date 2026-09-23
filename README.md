# AI-Chief 🎖️ (v0.3)

> **A portable, tool-agnostic AI agent orchestration framework and communication layer between humans and AI workers.**

[![Version](https://img.shields.io/badge/version-v0.3.0-blue.svg)](https://github.com/your-username/ai-chief)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](file:///home/ishaan/Work/ai-chief/LICENSE)
[![Zero Dependencies](https://img.shields.io/badge/dependencies-0%20(Pure%20Markdown)-brightgreen.svg)](#installation-methods)
[![Architecture](https://img.shields.io/badge/architecture-3--Tier%20Stratified-orange.svg)](file:///home/ishaan/Work/ai-chief/docs/architecture.md)

---

## 1. What is AI-Chief?

**AI-Chief** is a lightweight, zero-dependency orchestration framework that bridges the gap between human developers and AI coding agents.

Rather than having a single AI model attempt to chat with you, navigate your codebase, write code, run builds, debug failures, and dump thousands of lines of terminal output into a single conversation, AI-Chief stratifies work into three specialized tiers:
- **Chief Agent (Tier 1):** Your personal communication partner. Speaks naturally, concisely (<10 sentences), and empathetically. **Chief never writes code, inspects repos, or dumps logs.**
- **Manager Agent (Tier 2):** The permanent intelligence and context controller. Understands your request, curates bounded context for workers, orchestrates execution, compresses verbose technical outputs, and maintains Git-versioned Markdown memory.
- **Worker Agent (Tier 3):** Disposable execution engines. Spawned with an atomic task, inspect files, code, run tests, debug, and submit a structured report before self-terminating.
- **Cleaner Agent (Auxiliary):** Performs memory maintenance and garbage collection to keep context windows lean.

---

## 2. Why Does AI-Chief Exist?

Monolithic AI coding agents suffer from two systemic issues:
1. **Context Pollution & Token Exhaustion:** When an agent reads multiple large files and runs test suites that dump hundreds of lines of output, the LLM's context window degrades rapidly. The agent loses track of early instructions, begins hallucinating, and becomes prohibitively expensive.
2. **Poor User Experience:** Developers are forced to sift through walls of raw diffs, build logs, and internal trial-and-error churn just to understand if a feature works.

**AI-Chief solves this by design:**
- **Zero API/Plugin Lock-in:** AI-Chief works through clear instructions, markdown memory, and standardized protocols—not proprietary plugins, binary CLIs, or cloud databases.
- **Context Firewalling:** Workers only receive the exact files needed. Raw logs are absorbed and compressed by Manager before reaching Chief.
- **Human-First Communication:** The human only interacts with Chief, receiving polished, high-signal updates.

```
                  ┌──────────────────────┐
                  │      Human User      │
                  └──────────┬───────────┘
                             │ Natural Conversation (<10 sentences)
                             ▼
                  ┌──────────────────────┐
                  │     Chief Agent      │
                  │   (Human Interface)  │
                  └──────────┬───────────┘
                             │ User Intent / Delegated Goal
                             ▼
                  ┌──────────────────────┐
                  │    Manager Agent     │◄────────┐
                  │  (Context & Memory)  │         │
                  └──────────┬───────────┘         │
                             │ Task / Context / Boundaries
                             ▼                     │
                  ┌──────────────────────┐         │
                  │   Disposable Worker  │         │
                  │  (Execution / Tools) │         │
                  └──────────┬───────────┘         │
                             │ Detailed Raw Output │
                             ▼                     │
                  ┌──────────────────────┐         │
                  │ Manager Compression  ├─────────┘
                  │  & Memory Updating   │
                  └──────────┬───────────┘
                             │ High-Signal Status (STATUS, DONE, IMPORTANT, NEXT)
                             ▼
                  ┌──────────────────────┐
                  │     Chief Agent      │
                  └──────────┬───────────┘
                             │ Concise Human Update
                             ▼
                  ┌──────────────────────┐
                  │      Human User      │
                  └──────────────────────┘
```

---

## 3. Installation Methods

AI-Chief requires **no node modules, no python environments, and no hosted servers**. Choose the method that best matches your setup:

### Method 1: Clone Repository into a Project
Clone AI-Chief directly into your existing project or workspace:
```bash
git clone https://github.com/your-username/ai-chief.git .ai-chief
```

### Method 2: Copy `ai-chief` Folder into an Existing Project
Copy the `ai-chief/` directory directly into the root of your existing codebase:
```bash
cp -r /path/to/ai-chief /path/to/your-project/ai-chief
```

### Method 3: Point an AI Tool to `SKILL.md`
If you are using an AI coding tool that supports skill or rule loading (Google Antigravity, Claude Code, Cursor, Windsurf):
- Point your assistant to [`SKILL.md`](file:///home/ishaan/Work/ai-chief/SKILL.md) or [`AGENTS.md`](file:///home/ishaan/Work/ai-chief/AGENTS.md).
- In Antigravity: Placed automatically in your workspace skills.
- In Claude Code / Cursor: Reference `@SKILL.md` or `@AGENTS.md` in your instructions.

> [!NOTE]
> AI-Chief operates entirely through structured instructions, file-based memory, and text protocols. It requires zero third-party integrations or API keys to function.

---

## 4. How to Activate

Once installed in your project, activate AI-Chief in your chat interface:

```text
/chief
```

The AI assistant will:
1. Load directives from [`SKILL.md`](file:///home/ishaan/Work/ai-chief/SKILL.md) and [`agents/chief.md`](file:///home/ishaan/Work/ai-chief/agents/chief.md).
2. Adopt the Chief persona.
3. Greet you concisely and ask how it can coordinate the engineering team for you.

---

## 5. Example Workflow

### 1. You Issue a Command:
```text
/chief Add authentication using JWT to our Express app
```

### 2. Chief Acknowledges & Delegates:
Chief confirms your request in 2 sentences and internally alerts Manager:
```markdown
USER REQUEST: Add authentication using JWT to Express app
CONSTRAINTS: Production Express server, TypeScript
PRIORITY: NORMAL
```

### 3. Manager Plans & Bounds Context:
Manager inspects [`memory/manager/project_state.md`](file:///home/ishaan/Work/ai-chief/memory/manager/project_state.md), identifies `src/server.ts`, and writes `runtime/tasks/task-001.md`:
```markdown
TASK: Implement JWT authentication middleware
CONTEXT: Express 4.x with TypeScript. Secret stored in process.env.JWT_SECRET.
FILES: src/middleware/auth.ts, src/server.ts, test/auth.test.ts
EXPECTED RESULT: 401 on missing token; valid token attaches user. Tests pass.
LIMITATIONS: Use jsonwebtoken; do not install passport.
```

### 4. Worker Executes & Reports:
A disposable Worker is instantiated. It creates `auth.ts`, modifies `server.ts`, runs `npm test`, and outputs:
```markdown
COMPLETED: Implemented JWT authentication middleware and wired to protected routes.
CHANGED: src/middleware/auth.ts (NEW), src/server.ts (MODIFIED), test/auth.test.ts (NEW)
TESTS: npm test test/auth.test.ts passed (5/5 assertions green).
ISSUES: None. Handled token expiration explicitly.
NEXT STEPS: Wire user registration and login endpoints.
```
*(Worker self-terminates)*

### 5. Manager Compresses & Syncs Memory:
Manager updates [`memory/manager/active_tasks.md`](file:///home/ishaan/Work/ai-chief/memory/manager/active_tasks.md) and formats a 4-line digest for Chief:
```markdown
STATUS: SUCCESS
DONE: JWT authentication middleware implemented and validated with 5 unit tests.
IMPORTANT: Requires JWT_SECRET to be configured in .env.
NEXT: Implement user registration and login endpoints.
```

### 6. Chief Delivers the Final Answer:
> **Chief:** *"I've had the engineering team implement JWT authentication middleware and hook it into your protected routes. All 5 test suites passed cleanly. Before we move on to login endpoints, please ensure `JWT_SECRET` is defined in your `.env` file. Would you like us to proceed with user login routes next?"*

---

## 6. Command System

AI-Chief includes 7 built-in text commands:

| Command | Usage | Description |
| :--- | :--- | :--- |
| `/chief` | `/chief [request]` | Activates AI-Chief mode, loads `SKILL.md`, and initiates work. |
| `/chief-status` | `/chief-status` | Returns a concise overview of current project state and task list. |
| `/chief-plan` | `/chief-plan [goal]` | Decomposes a goal into a proposed task list without executing code. |
| `/chief-build` | `/chief-build [task]` | Authorizes Manager to dispatch Worker agents to execute changes. |
| `/chief-clean` | `/chief-clean` | Activates Cleaner Agent to deduplicate and compact memory. |
| `/chief-memory` | `/chief-memory` | Displays stored preferences, project overview, and key ADR decisions. |
| `/chief-reset` | `/chief-reset` | Clears temporary task files in `runtime/tasks/`. |

---

## 7. Memory Architecture

```
Permanent (Never compressed or deleted)
├── Agent instructions (agents/)
├── User preferences (memory/chief/preferences.md)
└── Architecture decisions (memory/manager/decisions.md)

Compressible (Periodically compacted by Cleaner & Manager)
├── Old conversations (memory/chief/conversation_state.md)
├── Worker reports (absorbed by Manager)
└── Completed tasks / logs (memory/manager/active_tasks.md)

Temporary (Ephemeral staging)
└── Staging task files (runtime/tasks/) — safe to purge via /chief-reset
```

---

## 8. Directory Structure

```
ai-chief/
├── AGENTS.md                                # Root directives, governance, and command mapping
├── README.md                                # Full framework overview, lifecycle, and command table
├── SKILL.md                                 # Skill guide with command triggers and agent contracts
│
├── agents/                                  # Agent personas and operational contracts
│   ├── chief.md                             # Human interface (<10 sentences, non-coding)
│   ├── manager.md                           # Context controller & permanent memory keeper
│   ├── worker.md                            # Ephemeral execution engine (code, tools, tests)
│   └── cleaner.md                           # Memory hygiene, deduplication, and compaction
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
├── runtime/                                 # Runtime simulation & task staging
│   ├── active_session.md                    # Active session state & telemetry
│   ├── current_agent.md                     # Active agent tracking for Fallback Mode
│   ├── message_queue.md                     # Simulated inter-agent message buffer
│   └── tasks/                               # Ephemeral staging for worker task files
│       └── README.md
│
├── protocols/                               # Rigid inter-agent communication specifications
│   ├── commands.md                          # Portable command protocol (/chief, /chief-plan, etc.)
│   ├── delegation.md                        # Manager ↔ Worker and Manager ↔ Chief contracts
│   ├── compression.md                       # Output compression algorithms and boundaries
│   ├── memory_rules.md                      # Permanent, Compressible, Temporary storage rules
│   └── response_format.md                   # Exact output schemas across all tiers
│
├── templates/                               # Operational Markdown templates
│   ├── task.md                              # Manager → Worker task definition template
│   ├── worker_report.md                     # Worker → Manager execution report template
│   └── memory_update.md                     # ADR and task status update templates
│
├── config/
│   └── settings.md                          # Model tiers, token limits, and thresholds
│
└── docs/                                    # Documentation library
    ├── README.md                            # Documentation index
    ├── quick-start.md                       # 5-minute onboarding guide
    ├── user-guide.md                        # Comprehensive user guide
    ├── lifecycle.md                         # 7-stage execution lifecycle guide
    ├── commands.md                          # User commands & cross-tool integration guide
    ├── fallback-mode.md                     # Single-agent fallback operating instructions
    ├── architecture.md                      # Deep architectural whitepaper
    └── release-checklist.md                 # v0.3 public release checklist
```

---

## 9. Operating Modes

- **Subagent Mode:** Used in environments supporting background subagents (Google Antigravity, custom harnesses). Workers execute concurrently or in isolated threads.
- **Fallback Mode:** Used in single-agent environments (standard CLI / Web chat). Chief, Manager, and Worker operate through structured cognitive role switches tracked in [`runtime/current_agent.md`](file:///home/ishaan/Work/ai-chief/runtime/current_agent.md). See [`docs/fallback-mode.md`](file:///home/ishaan/Work/ai-chief/docs/fallback-mode.md).

---

## 10. License

MIT License. Free for personal and commercial use.
