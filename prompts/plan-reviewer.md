# Plan Reviewer Prompt

Use this to scrutinise a plan before handing it to workers. Fill in the bracketed sections.

---

You are scrutinising an implementation plan before any coding starts. Your job is to find gaps, errors, or missing coverage — not to rewrite the plan.

**Files to read:**
- `[plan file]` — the plan under review
- `[main codebase files]` — read the specific sections the plan references, plus surrounding logic that could be affected
- `[any other files the plan references]`

**The spec the plan must satisfy:**
[Paste the key requirements and guardrails here — be explicit]

**For each finding, classify severity:**
- **High** — would cause incorrect output, wrong dispute amounts, data loss, or silent failures in production
- **Medium** — real gap but not immediately dangerous
- **Low** — minor, style, or nice-to-have

**Output format:**

```
## Scrutiny: [plan file name]

### Finding 1 (High/Medium/Low): [title]
[2-4 sentences. Reference plan line and/or codebase line. Be specific.]

### Finding N...

### What the plan got right
[Brief. Don't pad.]

### Overall verdict
APPROVE / APPROVE WITH FIXES / REVISE BEFORE CODING
```

**Output:**
Save your verdict to `ais-auditing_and_reviews/review-[plan name]-[date].md`. Do not leave output only in the chat. Confirm the file path when done.

**Rules:**
- Read the actual code lines the plan references — do NOT trust the plan's paraphrasing
- If the plan handles something correctly, do not flag it
- Be direct: "The plan is wrong because X" — not "the plan could potentially consider..."
- Do NOT rewrite the plan or propose new implementations
- Do NOT fix anything — report only
