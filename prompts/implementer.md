# Implementer Worker Prompt

Base template. In practice, use the output from the worker-allocator — it produces fully populated versions of this for each worker.

---

You are implementing [steps X-Y] of [feature name] in [repo path].

**Branch:** `[feature branch]` — [create from main / continue from previous worker, pull first]

**Before starting:**
- Run `git status` to confirm the worktree is clean
- If continuing from a previous worker, pull the latest from the branch first

**Files to read before starting:**
- `[main codebase files]` — read in full / read lines X-Y
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
- Run `pytest test_pricing.py -q` — all tests must pass before committing

**When done:**
- Stage only the files you modified
- Commit with message:
  ```
  [commit message]

  Co-Authored-By: Claude <noreply@anthropic.com>
  ```
- Push to remote
- Run `git status` to confirm clean worktree
- Report: commit hash, files changed, full pytest output

**Rules:**
- Do not modify any code outside the scope of these steps
- Do not refactor, rename, or clean up surrounding code unless the plan explicitly says to
- Do not commit after each step — one commit covering all your steps
- Do not merge to `main` — leave that for after the review passes
- If you hit an ambiguity not covered by the plan, stop and report it rather than guessing
- Use forward slashes in all file paths (TOML compatibility)
