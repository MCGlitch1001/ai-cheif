# Protocol: Response Formats & Output Standards

## 1. Overview
AI-Chief enforces a clean, predictable output schema for all interactions. By eliminating conversational fluff, fake multi-agent dialogue, and raw terminal log dumps, AI-Chief ensures that human developers receive immediate, high-signal technical updates.

---

## 2. Standard Technical Response Format

For any task execution, feature implementation, refactoring, or bug fix, the final response must adhere to the 4-field standard format:

```markdown
Summary:
[High-level, plain-English summary of what was accomplished]

Changes:
- [File path or action taken]
- [File path or action taken]

Status:
[Completed | Needs attention | Blocked]

Next:
[Optional recommended next technical action or question for confirmation]
```

### Field Definitions:
- **`Summary:`** 1–3 clear sentences describing the deliverable, solution, or outcome.
- **`Changes:`** Bulleted list of modified, created, or deleted files, along with specific functions or modules affected.
- **`Status:`** Current state of the work:
  - `Completed`: All requirements met and verified.
  - `Needs attention`: Changes made, but requires user decision or manual configuration.
  - `Blocked`: Technical impediment, missing dependency, or prerequisite needed before proceeding.
- **`Next:`** (Optional) The logical next step, command to run, or clarification question for the user.

---

## 3. Conversational & Status Response Standard

For pure questions, architectural advice, quick clarifications, or `/chief-status` requests:
- **Format:** Natural, well-formatted Markdown.
- **Length:** Strictly under 10 sentences (defaulting to 2–5 sentences per [`core/config.md`](file:///home/ishaan/Work/ai-chief/core/config.md)).
- **Tone:** Professional, direct, and concise.

---

## 4. Maintenance Audit Format (`/chief-clean`)

When `/chief-clean` is executed, the compaction report must follow this concise audit structure:

```markdown
Summary:
Compacted project memory and purged stale runtime session logs.

Changes:
- Pruned duplicate and stale entries in memory/manager/active_tasks.md
- Compacted completed milestone history in memory/chief/conversation_state.md
- Cleared ephemeral cache in runtime/tasks/

Status:
Completed

Next:
All permanent ADRs and preferences verified intact. Ready for next task.
```

---

## 5. Strict Negative Constraints

The AI assistant must **NEVER**:
1. Output simulated multi-agent dialogues (e.g., *"Chief says..."*, *"Manager says..."*, *"Worker says..."*).
2. Dump raw command output, compiler warnings, or verbose test runner logs unless specifically analyzing a test failure.
3. Expose internal chain-of-thought, task dispatch mechanics, or internal monologues to the user.
4. Exceed the sentence limit defined in [`core/config.md`](file:///home/ishaan/Work/ai-chief/core/config.md) without explicit user consent.
