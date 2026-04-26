# Planner Agent Prompt

Use this to ask an agent to write a detailed implementation plan. Fill in the bracketed sections.

---

You are writing a detailed implementation plan for [feature name]. This will be reviewed before any coding starts, so it must be precise and complete.

**Files to read:**
- `[main codebase file]` — read it in full
- `[context brief file]` — the feature spec and requirements
- `[README or docs file]` — system documentation for context
- `[existing plan file, if any]` — use as reference for format and level of detail

**Your task:**

Write a step-by-step implementation plan for [brief description of feature].

Each step must include:
- What to do (precise, not vague)
- Exact file and line number(s) where the change goes
- Before/after code snippets where the change modifies existing code
- What NOT to touch
- Edge cases to handle
- Validation: how to confirm this step is correct before moving on

**Additional requirements:**
[Add any feature-specific constraints, design decisions, or guardrails here]

**Output:**
Write the plan to `[agreed output directory]/plan-[feature].md`. Do not leave output only in the chat — it must be saved as a file in the repo. Confirm the file path when done.

**Plan format:**
- Numbered steps
- Risk assessment table at the end (step, risk level, reason)
- Implementation order with dependency notes (which steps must be sequential, which can be parallel)
- Steps clearly separated so each can be handed to a separate worker chat
