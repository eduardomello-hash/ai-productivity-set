# Implementer Worker Prompt

Base template. In practice, use the output from the worker-allocator — it produces fully populated versions of this for each worker.

---

You are implementing [steps X-Y] of [feature name] in [repo path].

**Branch:** `[feature branch]` — [create from main / continue from previous worker]

**Files to read before starting:**
- `[main codebase file]` — read in full / read lines X-Y
- `[plan file]` — read steps X-Y only
- `[any other files]`

**What to implement:**

[Paste the relevant plan steps here verbatim, including:
- Exact line numbers in the codebase
- Before/after code snippets
- What NOT to touch
- Edge cases to handle]

**Validation — do this before committing:**
[Paste the validation steps from the plan here]

**When done:**
- Stage only the files you modified
- Commit with message:
  ```
  [commit message]

  Co-Authored-By: Claude <noreply@anthropic.com>
  ```
- Run `git status` to confirm clean worktree
- Report: commit hash, files changed, validation results

**Rules:**
- Do not modify any code outside the scope of these steps
- Do not refactor, rename, or clean up surrounding code
- Do not commit after each step — one commit covering all your steps
- If you hit an ambiguity not covered by the plan, stop and report it rather than guessing
