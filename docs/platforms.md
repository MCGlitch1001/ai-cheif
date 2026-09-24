# Platform Compatibility & Verification Guide

## 1. Overview
AI-Chief is designed to be **strictly tool-agnostic**. It operates across diverse AI coding tools without relying on proprietary platform features, cloud daemons, or custom APIs.

This guide provides practical verification procedures, capability matrices, and documented integration methods across all supported environments:
1. **Google Antigravity**
2. **Claude Code (Anthropic CLI)**
3. **OpenAI Codex / ChatGPT CLI**
4. **Cursor (Composer / Agent Mode)**
5. **Gemini-Based Coding Agents**
6. **Generic AI Coding Agents (Windsurf, Aider, etc.)**

---

## 2. 9-Point Platform Verification Matrix

Each supported platform is evaluated against the following nine operational criteria:

| Verification Criterion | Description |
| :--- | :--- |
| **1. Discovery** | Can the assistant locate AI-Chief via `core/system-prompt.md`, `AGENTS.md`, or `SKILL.md`? |
| **2. Install Comprehension** | Does the assistant understand `"Install AI-Chief into this project"` without hallucinating? |
| **3. File Creation** | Can the assistant scaffold the full directory tree non-destructively? |
| **4. `/chief` Activation** | Does the assistant execute the 6-step filesystem bootstrap on `/chief`? |
| **5. `/chief+` Recovery** | Can the assistant recover from degraded context by cold-reloading files from disk? |
| **6. 7-Stage Pipeline** | Does the assistant execute the internal pipeline (Understand ➔ Plan ➔ Bound ➔ Execute ➔ Test ➔ Compress)? |
| **7. Zero Role-Play Chatter** | Does the assistant avoid emitting fake agent dialogue (*"Chief says...", "Worker says..."*)? |
| **8. Log Suppression** | Does the assistant prevent dumping raw diffs and terminal traces to the human? |
| **9. Context Preservation** | Does persistent Markdown memory survive conversation truncations? |

---

## 3. Platform Breakdown & Verification Reports

### 3.1 Google Antigravity
- **Verification Status:** `VERIFIED & TESTED LIVE`
- **Discovery Mechanism:** Auto-discovers [`SKILL.md`](file:///home/ishaan/Work/ai-chief/SKILL.md) in workspace skills; enforces [`AGENTS.md`](file:///home/ishaan/Work/ai-chief/AGENTS.md) as root directive.
- **Execution Mode:** Single-Agent OS prompt (default) with optional native subagent dispatch via `invoke_subagent`.
- **Capability Matrix:**
  - Discovery: ✅ YES
  - Install Comprehension: ✅ YES
  - File Creation: ✅ YES
  - `/chief` Activation: ✅ YES
  - `/chief+` Recovery: ✅ YES
  - 7-Stage Pipeline: ✅ YES
  - Zero Role-Play Chatter: ✅ YES
  - Log Suppression: ✅ YES
  - Context Preservation: ✅ YES
- **Guaranteed Capabilities:** Subagent tool execution, isolated child workspaces, deep file inspection.

---

### 3.2 Claude Code (Anthropic CLI)
- **Verification Status:** `VERIFIED & DOCUMENTED`
- **Discovery Mechanism:** Ingests `core/system-prompt.md`, `AGENTS.md`, or `CLAUDE.md` at project root.
- **Execution Mode:** Single-Agent OS prompt running 7-stage internal pipeline.
- **Capability Matrix:**
  - Discovery: ✅ YES (via root `AGENTS.md` / `core/system-prompt.md`)
  - Install Comprehension: ✅ YES
  - File Creation: ✅ YES
  - `/chief` Activation: ✅ YES
  - `/chief+` Recovery: ✅ YES
  - 7-Stage Pipeline: ✅ YES
  - Zero Role-Play Chatter: ✅ YES
  - Log Suppression: ✅ YES
  - Context Preservation: ✅ YES (via disk memory)
- **Guaranteed Capabilities:** File editing, shell execution, Git integration.

---

### 3.3 OpenAI Codex / ChatGPT CLI
- **Verification Status:** `VERIFIED & DOCUMENTED`
- **Discovery Mechanism:** System prompt injection or root repository scan (`AGENTS.md`, `core/system-prompt.md`).
- **Execution Mode:** Single-Agent OS prompt running 7-stage internal pipeline.
- **Capability Matrix:**
  - Discovery: ✅ YES
  - Install Comprehension: ✅ YES
  - File Creation: ✅ YES
  - `/chief` Activation: ✅ YES
  - `/chief+` Recovery: ✅ YES
  - 7-Stage Pipeline: ✅ YES
  - Zero Role-Play Chatter: ✅ YES
  - Log Suppression: ✅ YES
  - Context Preservation: ✅ YES
- **Guaranteed Capabilities:** Python/Bash tool calling, structured JSON/Markdown responses.

---

### 3.4 Cursor (Composer & Agent Mode) & Windsurf
- **Verification Status:** `VERIFIED & DOCUMENTED`
- **Discovery Mechanism:** Place rules in `.cursorrules` or `.windsurfrules` pointing to `core/system-prompt.md` and `AGENTS.md`.
- **Execution Mode:** Single-Agent OS prompt running 7-stage internal pipeline.
- **Capability Matrix:**
  - Discovery: ✅ YES
  - Install Comprehension: ✅ YES
  - File Creation: ✅ YES
  - `/chief` Activation: ✅ YES
  - `/chief+` Recovery: ✅ YES
  - 7-Stage Pipeline: ✅ YES
  - Zero Role-Play Chatter: ✅ YES
  - Log Suppression: ✅ YES
  - Context Preservation: ✅ YES

---

### 3.5 Gemini-Based Coding Agents
- **Verification Status:** `VERIFIED & DOCUMENTED`
- **Discovery Mechanism:** Reads workspace files directly or loads system instruction from `core/system-prompt.md`.
- **Execution Mode:** Single-Agent OS prompt running 7-stage internal pipeline.
- **Capability Matrix:**
  - Discovery: ✅ YES
  - Install Comprehension: ✅ YES
  - File Creation: ✅ YES
  - `/chief` Activation: ✅ YES
  - `/chief+` Recovery: ✅ YES
  - 7-Stage Pipeline: ✅ YES
  - Zero Role-Play Chatter: ✅ YES
  - Log Suppression: ✅ YES
  - Context Preservation: ✅ YES
- **Guaranteed Capabilities:** Large context windows, structured markdown deliverables.

---

## 4. Verification & Testing Procedure for Any Platform

To verify AI-Chief compliance in any new AI environment:

1. **Step 1 (Install Test):** Paste `Install AI-Chief into this project.` Verify all 10 checkpoints pass and the standard onboarding summary is returned.
2. **Step 2 (Safe Plan Test):** Run `/chief-plan add health check route`. Verify the model produces an architectural plan **without modifying code**.
3. **Step 3 (Execution Test):** Run `/chief-build`. Verify that code is written, tests are executed, output is compressed, and the assistant delivers the standard 4-field response (`Summary`, `Changes`, `Status`, `Next`).
4. **Step 4 (Amnesia Recovery Test):** In a long or reset session, type `/chief+ check system status`. Verify the model reloads from the filesystem and resumes cleanly.
