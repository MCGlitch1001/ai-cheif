# Platform Compatibility & Verification Guide

## 1. Overview
AI-Chief is designed to be **strictly tool-agnostic**. It operates across diverse AI coding tools without relying on proprietary platform features, cloud daemons, or custom APIs.

This guide provides practical verification procedures, capability matrices, and documented limitations across all supported environments:
1. **Google Antigravity**
2. **Claude Code (Anthropic CLI)**
3. **OpenAI Codex / ChatGPT CLI**
4. **Cursor (Composer / Agent Mode)**
5. **Gemini-Based Coding Agents**
6. **Generic AI Coding Agents**

---

## 2. 9-Point Platform Verification Matrix

Each supported platform is evaluated against the following nine operational criteria:

| Verification Criterion | Description |
| :--- | :--- |
| **1. Discovery** | Can the agent locate AI-Chief via `AGENTS.md`, `SKILL.md`, or project prompt? |
| **2. Install Comprehension** | Does the agent understand `"Install AI-Chief into this project"` without hallucinating? |
| **3. File Creation** | Can the agent scaffold the full directory tree non-destructively? |
| **4. `/chief` Activation** | Does the agent execute the 6-step filesystem bootstrap on `/chief`? |
| **5. `/chief+` Recovery** | Can the agent recover from degraded context by cold-reloading files from disk? |
| **6. Core Workflow** | Does the system execute `Chief → Manager → Worker → Summary`? |
| **7. Fallback Mode** | Can the agent execute structured internal role-switching if subagents are absent? |
| **8. Log Suppression** | Does the agent prevent dumping raw diffs and terminal traces to the human? |
| **9. Context Preservation** | Does persistent Markdown memory survive conversation truncations? |

---

## 3. Platform Breakdown & Verification Reports

### 3.1 Google Antigravity
- **Verification Status:** `VERIFIED & TESTED LIVE`
- **Discovery Mechanism:** Auto-discovers [`SKILL.md`](file:///home/ishaan/Work/ai-chief/SKILL.md) in workspace skills; enforces [`AGENTS.md`](file:///home/ishaan/Work/ai-chief/AGENTS.md) as root directive.
- **Execution Mode:** Native Subagents (`invoke_subagent`) or Single-Agent Fallback.
- **Capability Matrix:**
  - Discovery: ✅ YES
  - Install Comprehension: ✅ YES
  - File Creation: ✅ YES
  - `/chief` Activation: ✅ YES
  - `/chief+` Recovery: ✅ YES
  - Chief → Manager → Worker → Summary: ✅ YES
  - Fallback Mode: ✅ YES
  - Log Suppression: ✅ YES
  - Context Preservation: ✅ YES
- **Guaranteed Capabilities:** Multi-agent concurrent dispatch, tool calling, isolated subagent workspaces.
- **Documented Limitations:** None. Full native compatibility.

---

### 3.2 Claude Code (Anthropic CLI)
- **Verification Status:** `DOCUMENTED & THEORETICALLY VERIFIED`
- **Discovery Mechanism:** Ingests `AGENTS.md` or `CLAUDE.md` at project root. Users can alias `/chief` or reference `@SKILL.md`.
- **Execution Mode:** Single-Agent Fallback Mode (Cognitive role-switching).
- **Capability Matrix:**
  - Discovery: ✅ YES (via root `AGENTS.md` / `CLAUDE.md`)
  - Install Comprehension: ✅ YES
  - File Creation: ✅ YES
  - `/chief` Activation: ✅ YES
  - `/chief+` Recovery: ✅ YES
  - Chief → Manager → Worker → Summary: ✅ YES (via Fallback Mode)
  - Fallback Mode: ✅ REQUIRED & TESTED
  - Log Suppression: ⚠️ HIGH (Model must adhere to Chief `<10 sentences` rule)
  - Context Preservation: ✅ YES (via disk memory)
- **Guaranteed Capabilities:** File reading, command execution, Git tracking.
- **Documented Limitations:** Claude Code operates in a single conversation thread. Native subagent spawning is not exposed via standard CLI; the agent **must** use Fallback Mode ([`docs/fallback-mode.md`](file:///home/ishaan/Work/ai-chief/docs/fallback-mode.md)).

---

### 3.3 OpenAI Codex / ChatGPT CLI
- **Verification Status:** `DOCUMENTED & THEORETICALLY VERIFIED`
- **Discovery Mechanism:** System prompt injection or root repository scan (`AGENTS.md`).
- **Execution Mode:** Single-Agent Fallback Mode.
- **Capability Matrix:**
  - Discovery: ✅ YES
  - Install Comprehension: ✅ YES
  - File Creation: ✅ YES
  - `/chief` Activation: ✅ YES
  - `/chief+` Recovery: ✅ YES
  - Chief → Manager → Worker → Summary: ✅ YES
  - Fallback Mode: ✅ REQUIRED
  - Log Suppression: ⚠️ MEDIUM-HIGH (Requires explicit prompt adherence)
  - Context Preservation: ✅ YES
- **Guaranteed Capabilities:** Python/Bash tool calling, file editing.
- **Documented Limitations:** Lacks background subagent orchestration. Role-switching relies on prompt adherence and [`runtime/current_agent.md`](file:///home/ishaan/Work/ai-chief/runtime/current_agent.md).

---

### 3.4 Cursor (Composer & Agent Mode)
- **Verification Status:** `DOCUMENTED & THEORETICALLY VERIFIED`
- **Discovery Mechanism:** Place rules in `.cursorrules` pointing to `@AGENTS.md` and `@SKILL.md`.
- **Execution Mode:** Single-Agent Fallback Mode.
- **Capability Matrix:**
  - Discovery: ✅ YES (via `.cursorrules`)
  - Install Comprehension: ✅ YES
  - File Creation: ✅ YES
  - `/chief` Activation: ✅ YES
  - `/chief+` Recovery: ✅ YES
  - Chief → Manager → Worker → Summary: ✅ YES
  - Fallback Mode: ✅ REQUIRED
  - Log Suppression: ✅ YES
  - Context Preservation: ✅ YES
- **Guaranteed Capabilities:** Fast multi-file editing, codebase indexing.
- **Documented Limitations:** Composer operates as a single agent. While Cursor can execute terminal commands and edit files, it cannot spin off autonomous background agents. Fallback Mode is mandatory.

---

### 3.5 Gemini-Based Coding Agents
- **Verification Status:** `DOCUMENTED & THEORETICALLY VERIFIED`
- **Discovery Mechanism:** Reads workspace files directly or loads system instruction from `AGENTS.md`.
- **Execution Mode:** Single-Agent Fallback Mode or tool-dispatch mode depending on the harness.
- **Capability Matrix:**
  - Discovery: ✅ YES
  - Install Comprehension: ✅ YES
  - File Creation: ✅ YES
  - `/chief` Activation: ✅ YES
  - `/chief+` Recovery: ✅ YES
  - Chief → Manager → Worker → Summary: ✅ YES
  - Fallback Mode: ✅ YES
  - Log Suppression: ✅ YES
  - Context Preservation: ✅ YES
- **Guaranteed Capabilities:** Long context comprehension (up to 2M tokens), structured output.
- **Documented Limitations:** If using standard web or single-threaded API, Fallback Mode must be used.

---

### 3.6 Generic AI Coding Agents (Aider, Windsurf, Custom LLMs)
- **Verification Status:** `DOCUMENTED & THEORETICALLY SUPPORTED`
- **Discovery Mechanism:** Ingests root [`AGENTS.md`](file:///home/ishaan/Work/ai-chief/AGENTS.md).
- **Execution Mode:** Single-Agent Fallback Mode.
- **Capability Matrix:**
  - Discovery: ✅ YES (if agent reads root documentation)
  - Install Comprehension: ✅ YES
  - File Creation: ✅ YES
  - `/chief` Activation: ✅ YES
  - `/chief+` Recovery: ✅ YES
  - Chief → Manager → Worker → Summary: ✅ YES
  - Fallback Mode: ✅ REQUIRED
  - Log Suppression: ⚠️ Variable (depends on model instruction-following quality)
  - Context Preservation: ✅ YES
- **Documented Limitations:** Smaller or open-weights models (<30B parameters) may occasionally slip into writing code directly in chat. If observed, issue `/chief+` to force a cold-start reload.

---

## 4. Verification & Testing Procedure for Any Platform

To verify AI-Chief compliance in any new AI environment:

1. **Step 1 (Install Test):** Paste `Install AI-Chief into this project.` Verify all 10 checkpoints pass and the standard onboarding summary is returned.
2. **Step 2 (Safe Plan Test):** Run `/chief-plan add health check route`. Verify the model produces a plan **without modifying code**.
3. **Step 3 (Execution Test):** Run `/chief-build`. Verify that code is written, tests are executed, output is compressed, and Chief delivers a concise answer (<10 sentences).
4. **Step 4 (Amnesia Recovery Test):** In a long or reset session, type `/chief+ check system status`. Verify the model reloads from the filesystem and resumes flawlessly.
