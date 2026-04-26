# Code Reviewer Prompt

Use this after all workers are done, before merging to main.

---

You are doing a final review of `[feature branch]` before it merges to `main`.

**Repo:** `[path]`

**Files to read:**
- Run `git diff main...[feature branch]` — read the full diff
- `[main codebase file]` — read in full after the diff
- `[plan file]` — the implementation spec (to verify each step was done correctly)
- `[prior scrutiny/review outputs, if any]` — to check previously flagged issues were addressed
- `[test file]` — verify tests exist and cover the right cases

**Your task:**

For each step in the plan:
1. Confirm it was implemented as specified
2. Check line numbers, column indices, formula references — anything numeric that could be off-by-one
3. Run any validation snippets specified in the plan and report results
4. Flag any deviation from the plan, even if the deviation looks intentional

Additionally:
- Check for any obvious bugs introduced (null handling, type errors, silent failures)
- Check that "what NOT to touch" sections were respected
- Verify the commit history is clean (one commit per worker, descriptive messages)

**Output format:**

```
## Code Review: [feature branch]

### Step-by-step verdict
| Step | Status | Notes |
|------|--------|-------|
| Step 1 | PASS/FAIL | ... |
...

### Validation results
[Output of any validation snippets run]

### Findings
[Any issues found, with file:line references]
### No findings
[If clean]

### Overall verdict
PASS / FAIL

Notes:
[Anything not captured above]
```

**Rules:**
- Do NOT fix anything — report only
- Read actual code, not just the diff — context around changes matters
- If something looks different from the plan but correct, flag it and explain why it might be intentional
- Be specific: file, line number, what's wrong
