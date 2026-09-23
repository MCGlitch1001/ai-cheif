# Protocol: Response Formats & Output Standards

## 1. Overview
To ensure seamless multi-agent collaboration, every agent tier in AI-Chief has a strictly enforced response schema. Adhering to these formats prevents misinterpretation, hallucination, and conversational clutter.

---

## 2. Chief Agent Response Standard (Human-Facing)

### Format: Natural Conversational Markdown
- **Length:** Maximum 10 sentences (default 2 to 5 sentences).
- **Tone:** Professional, clear, courteous, and confident.
- **Prohibitions:**
  - Zero raw code blocks (` ```...``` `).
  - Zero command-line execution dumps or stack traces.
  - Zero internal agent tags (`STATUS:`, `DONE:`, `TASK:`, etc.).
  - Zero unsolicited meta-explanations of agent mechanics.

### Template:
```markdown
[Direct answer or milestone confirmation]. [Key technical highlight explained simply]. [Important constraint or choice requiring user feedback, if any]. [Next planned step].
```

### Example:
> I have set up the database migrations and verified that the user schema compiles cleanly. All 12 initial validation tests passed without issue. Before we proceed to wiring the API endpoints, please let me know if you would like role-based permissions enabled by default.

---

## 3. Manager Agent Response Standard (Internal to Chief)

### Format: Structured Status Block
Manager communicates only with Chief using the 4-field status protocol.

### Template:
```markdown
STATUS: [SUCCESS | IN_PROGRESS | BLOCKED | FAILED]
DONE: [1-2 sentence summary of verified deliverables]
IMPORTANT: [Key risks, architectural choices, or missing credentials]
NEXT: [Immediate next technical step]
```

---

## 4. Worker Agent Response Standard (Internal to Manager)

### Format: Technical Execution Report
Worker communicates only with Manager using the 5-field execution protocol.

### Template:
```markdown
COMPLETED: [Specific deliverables completed]
CHANGED: [Files created, modified, or removed with paths]
TESTS: [Verification commands run and exact results]
ISSUES: [Errors, unhandled edge cases, or missing dependencies encountered]
NEXT STEPS: [Logical follow-up implementation steps]
```

---

## 5. Cleaner Agent Response Standard (Internal Audit)

### Format: Maintenance Audit Report
Cleaner produces an audit log for Manager upon completing memory pruning.

### Template:
```markdown
CLEANUP AUDIT:
- INSPECTED: [List of files scanned]
- PRUNED ENTRIES: [Count and summary of duplicate or obsolete entries removed]
- ARCHIVED TASKS: [List of completed tasks consolidated or moved]
- PRESERVED: [Verification that preferences, rules, and ADRs remain intact]
- STATUS: [COMPLETE]
```
