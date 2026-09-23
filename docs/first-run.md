# Your First Run with AI-Chief 🚀

This guide walks you through what happens during your very first task after installing AI-Chief.

---

## 1. Triggering Your First Task

Once installed, issue your first engineering task by typing `/chief` followed by your goal:

```text
/chief build a login system
```

---

## 2. What Happens Behind the Scenes

Notice how the work flows automatically through the 3-tier system without overwhelming your chat:

```
[User: /chief build a login system]
                 │
                 ▼
     ┌───────────────────────┐
     │ 1. Chief Receives     │ ──► Chief parses the intent, acknowledges briefly,
     │    Request            │     and creates an internal directive for Manager.
     └───────────┬───────────┘     (Chief never codes or runs terminal commands!)
                 │
                 ▼
     ┌───────────────────────┐
     │ 2. Manager Prepares   │ ──► Manager reviews memory/manager/project_state.md,
     │    Task & Bounds      │     curates target files (routes, models, controllers),
     └───────────┬───────────┘     and stages runtime/tasks/task-001.md.
                 │
                 ▼
     ┌───────────────────────┐
     │ 3. Worker Executes    │ ──► An isolated, disposable Worker writes code,
     │                       │     executes unit tests, and verifies password hashing.
     └───────────┬───────────┘     Worker formats a 5-field structured report.
                 │
                 ▼
     ┌───────────────────────┐
     │ 4. Manager Compresses │ ──► Manager strips out verbose compiler and test output,
     │    Output             │     updates memory/manager/active_tasks.md, and creates
     └───────────┬───────────┘     a 4-line status summary for Chief.
                 │
                 ▼
     ┌───────────────────────┐
     │ 5. Chief Replies      │ ──► Chief translates the summary into a clear, natural
     │    Simply             │     message under 10 sentences without code dumps.
     └───────────────────────┘
```

---

## 3. What You See in the Chat

Unlike traditional AI coding sessions that dump 200 lines of terminal output, file diffs, and debugging trial-and-error directly into your chat, you receive a clean, executive summary:

> **Chief:**
> "I have had our engineering team build the login system. We created the authentication route handling user login, securely verify passwords using bcrypt, and issue signed JWT tokens on success. All 6 automated authentication tests passed cleanly.
>
> Please ensure you add a `JWT_SECRET` variable to your `.env` file before deploying to staging. Would you like us to proceed with password reset endpoints next?"

---

## 4. Key Takeaways from Your First Run

1. **Clean Conversation:** You only communicate with Chief. No spam, no raw diffs, no wall-of-logs.
2. **Context Integrity:** Because the Worker operated in an isolated scope and Manager compressed its output, your LLM's context window remains 95% empty, preserving reasoning power for complex future tasks.
3. **Persistent State:** If you restart your IDE or close your session, your project's architectural decisions and progress remain safely written to `memory/`.
