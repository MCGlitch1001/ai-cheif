# Your First Run with AI-Chief 🚀

This guide walks you through what happens during your very first task after installing AI-Chief.

---

## 1. Triggering Your First Task

Once installed, issue your first engineering task by typing `/chief` followed by your goal:

```text
/chief build a login system using JWT
```

---

## 2. What Happens Behind the Scenes

Notice how the work flows automatically through AI-Chief's 7-stage internal execution pipeline without conversational noise:

```
[User: /chief build a login system using JWT]
                 │
                 ▼
     ┌───────────────────────┐
     │ 1. Parse Intent       │ ──► Identifies requirements: login endpoint, password
     │                       │     hashing with bcrypt, signed JWT token generation.
     └───────────┬───────────┘
                 │
                 ▼
     ┌───────────────────────┐
     │ 2. Context & Config   │ ──► Reads core/config.md, user preferences, and ADRs
     │                       │     from memory/ to ensure alignment with existing stack.
     └───────────┬───────────┘
                 │
                 ▼
     ┌───────────────────────┐
     │ 3. Bound Context      │ ──► Isolates only relevant target files: auth routes,
     │                       │     user controller, and unit test suites.
     └───────────┬───────────┘
                 │
                 ▼
     ┌───────────────────────┐
     │ 4. Plan Internally    │ ──► Formulates atomic implementation sequence.
     └───────────┬───────────┘
                 │
                 ▼
     ┌───────────────────────┐
     │ 5. Execute Code       │ ──► Writes clean, modular code with defensive error
     │                       │     handling for missing passwords or invalid logins.
     └───────────┬───────────┘
                 │
                 ▼
     ┌───────────────────────┐
     │ 6. Verify with Tests  │ ──► Runs test suites internally, verifying 200 OK on valid
     │                       │     login and 401 Unauthorized on invalid passwords.
     └───────────┬───────────┘
                 │
                 ▼
     ┌───────────────────────┐
     │ 7. Deliver Response   │ ──► Emits clean 4-field standard response.
     └───────────────────────┘
```

---

## 3. What You See in the Chat

Unlike traditional AI coding sessions that dump walls of raw terminal logs, messy diffs, or simulated agent churn (*"Chief says...", "Worker says..."*), you receive a clean 4-field update:

```markdown
Summary:
Implemented the login system with secure password verification via bcrypt and signed JWT token issuance.

Changes:
- src/routes/auth.ts (New POST /api/login endpoint)
- src/controllers/auth.ts (Password comparison and JWT signing logic)
- tests/auth.test.ts (Unit tests validating successful login and rejection of bad credentials)

Status:
Completed

Next:
Ensure `JWT_SECRET` is defined in your `.env` file, then run `npm test` to verify your environment.
```

---

## 4. Key Takeaways from Your First Run

1. **Zero Conversational Churn:** No fake dialogues, no intermediate tool spam, no wall-of-logs.
2. **Context Integrity:** Because context is bounded and reasoning is kept internal, your LLM's context window remains clean, preserving reasoning power for future tasks.
3. **Persistent State:** If you restart your IDE or close your session, project decisions and task logs remain safely written to `memory/`.
