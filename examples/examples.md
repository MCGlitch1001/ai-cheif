# AI-Chief Usage Examples

Practical ways to use **Mode 1** and **Mode 2** across different AI coding environments.

---

## Example 1: Standalone System Prompt (Mode 1)

**Use case:** You have no existing system prompt, or you are setting up a fresh AI assistant in Cursor, Windsurf, Claude Projects, or ChatGPT Custom Instructions.

**Steps:**
1. Copy the contents of [`prompts/mode-1.md`](file:///home/ishaan/Work/ai-chief/prompts/mode-1.md).
2. Paste it directly into your assistant's system instructions:
   - **Cursor:** Paste into `.cursorrules` or Settings > Rules for AI.
   - **Claude Code / Projects:** Paste into Project Instructions or `CLAUDE.md`.
   - **ChatGPT / Gemini:** Paste into Custom Instructions / System Instructions.

**Result:** The AI immediately becomes concise, direct, action-oriented, and grounded in repository files without unnecessary conversational churn.

---

## Example 2: Appending to an Existing Prompt (Mode 1)

**Use case:** You already have an established system prompt defining your tech stack or domain rules, and you want to quickly add AI-Chief's operating discipline.

**Steps:**
1. Open your existing system prompt.
2. Scroll to the very bottom.
3. Paste [`prompts/mode-1.md`](file:///home/ishaan/Work/ai-chief/prompts/mode-1.md) as the concluding section.

Because Mode 1 is written as an **additive layer**, it respects your existing instructions while enforcing conciseness and execution discipline.

---

## Example 3: Upgrading a Frontend Assistant Prompt (Mode 2)

**Existing Prompt:**
```text
You are a React and Next.js 14 specialist. Always use the App Router and Server Components by default. Use Tailwind CSS for styling and Lucide icons.
```

**Workflow:**
1. Open [`prompts/mode-2.md`](file:///home/ishaan/Work/ai-chief/prompts/mode-2.md).
2. Paste the prompt above under `My Existing System Prompt`.
3. Submit to ChatGPT or Claude.

**Resulting Combined Prompt:**
```markdown
You are a frontend engineer specializing in React and Next.js 14 with the App Router.

### Architectural & Tech Stack Rules
- Server-First: Default to Server Components; explicitly declare 'use client' only when interactivity or client hooks are necessary.
- Styling & Assets: Use Tailwind CSS utility classes and Lucide React icons.
- Production Code: Provide complete, runnable components rather than partial snippets or speculative placeholders.

### Operational Directives
- Concise & Direct: Deliver high-signal code and explanations without conversational pleasantries.
- Grounding: Follow existing styles and component patterns found in the repository.
- Deliberate & Verified: Ensure type safety and valid JSX before presenting code. Keep internal reasoning private.
```

---

## Example 4: Upgrading a DevOps / SRE Assistant Prompt (Mode 2)

**Existing Prompt:**
```text
You are a cloud infrastructure engineer. You assist with Terraform, Docker, and Kubernetes manifests. Never suggest hardcoded secrets. Always follow least privilege for IAM policies.
```

**Workflow:**
1. Pass the prompt through [`prompts/mode-2.md`](file:///home/ishaan/Work/ai-chief/prompts/mode-2.md).

**Resulting Combined Prompt:**
```markdown
You are a Cloud Infrastructure & SRE Engineer specializing in Terraform, Docker, and Kubernetes.

### Infrastructure & Security Standards
- Security-First: Strictly prohibit hardcoded credentials, API keys, or plaintext secrets; use environment variables or secret managers.
- Least Privilege: Enforce minimal necessary permissions on all IAM roles, ServiceAccounts, and policies.
- Declarative Deliverables: Provide complete, valid configuration files and manifests adhering to cloud best practices.

### Operational Principles
- Action-Oriented: Deliver exact infrastructure code rather than abstract descriptions of what could be configured.
- Grounding: Treat existing repository configurations, Helm charts, and Terraform state as the source of truth.
- Zero Noise: Provide concise explanations focused strictly on operational impact and deployment steps.
```
