# Level Up: Next Moves for the AI Productivity Setup

Current gameplan for improving the orchestration model. Ordered roughly by impact vs effort.

---

## 1. Claude Projects — Persistent Orchestrator Context

**What:** Claude Projects lets you attach files and write persistent instructions that load into every chat automatically. The orchestrator's bootstrap doc, key decisions, and user preferences persist without manual pasting.

**Why it matters:** Right now every new orchestrator chat starts cold and needs a handoff doc. With Projects, the core context is always there. Handoff docs become lighter — just "here's what's in progress" rather than the full system explanation.

**Effort:** Low — set it up once, paste in the README + preferences, done.

---

## 2. GitHub MCP Server — Direct Repo Operations

**What:** An MCP (Model Context Protocol) server that gives Claude direct access to GitHub: read diffs, push commits, open PRs, comment on issues — all without copy-pasting.

**Why it matters:** Currently the orchestrator tells you what to do and you run the git commands. With GitHub MCP, it does it directly. Cuts the copy-paste loop for commit/push/PR operations.

**Effort:** Medium — install the MCP server, configure Claude Code settings. One-time setup.

---

## 3. Claude Code Subagents — Collapse Into One Session

**What:** Claude Code can spawn worker subagents inline using the Agent tool. Instead of opening separate Codex/Claude tabs, the orchestrator chat launches workers directly and gets their results back in the same session.

**Why it matters:** Eliminates the copy-paste loop entirely for worker coordination. The orchestrator composes a prompt, fires the agent, reads the result, moves on — all in one window.

**Effort:** Low — already available in Claude Code. Just use the Agent tool instead of opening new tabs.

---

## 4. Local Models via Ollama — Preserve Quota for Real Work

**What:** Run Hermes 3 70B (or similar) locally via Ollama for throwaway tasks: summarising agent output, formatting documents, quick lookups, boilerplate generation.

**Why it matters:** Claude Pro has usage limits. A lot of orchestration overhead (reformatting, summarising, minor edits) doesn't need Claude quality. Local models handle it for free, preserving quota for complex reasoning.

**Good local models:**
- `nous-hermes-3` — strong instruction following, good for structured output
- `qwen2.5-coder` — good for code-specific tasks
- `mistral` — fast, good for quick questions

**Effort:** Low — install Ollama, pull a model, done.

---

## 5. GitHub Actions — Automated Test Runs

**What:** CI pipeline that runs the test suite automatically on every push to a feature branch.

**Why it matters:** Right now you manually run pytest after workers commit. With CI, you see test results in the PR without doing anything. Workers can commit, you check the GitHub status badge, move on.

**Effort:** Medium — write a workflow file once. For Python projects it's ~15 lines.

---

## 6. Cline (VS Code Extension) — IDE-Integrated Workers

**What:** VS Code extension that uses Claude (or other models) to read and edit files directly in the editor, with full codebase context.

**Why it matters:** Useful as an alternative to Codex for workers that need to do surgical edits to large files. Lives in the editor so file navigation is instant.

**When to use:** When the task is edit-heavy and the worker needs to navigate the file interactively rather than read-then-write.

**Effort:** Low — install extension, point at Claude API key.

---

## 7. Structured Output Format for Workers

**What:** Standardise the format workers use to report back — especially for reviewers and planners. JSON or a strict markdown schema.

**Why it matters:** Right now orchestrator reads free-form worker output and extracts what it needs. Structured output makes routing and decision-making faster and less error-prone, especially when using subagents.

**Effort:** Low — add output format specs to the prompt templates.

---

## What's Already Working Well (Don't Break It)

- Handoff documents between orchestrator chats
- Hard separation: orchestrator doesn't read code
- Plan → scrutiny → allocate → implement → review flow
- One commit per worker
- Self-contained worker prompts
