# AI Productivity Setup

A lean two-AI orchestration model for software development. One AI as architect and brain, one as worker and hands. Clear separation of roles, structured handoff documents, and a repeatable workflow for shipping complex features with minimal manual overhead.

---

## The Model

Two roles, strictly separated:

### Orchestrator (Claude Code)
- Holds full project context across the entire engagement
- Composes prompts for worker agents — never reads code or does heavy analysis itself
- Reviews and routes results from workers
- Stays lean on context window to remain effective throughout long sessions
- Makes design and sequencing decisions
- Keeps a persistent memory of project state, decisions, and user preferences

### Worker (Codex / Claude subagents)
- Reads code, writes code, runs tests, writes plans
- Operates on specific, self-contained tasks
- Each worker prompt must be fully self-contained — workers have no memory of prior sessions
- Reports back with structured output
- Stateless between tasks

**Why this split:** The orchestrator's value is context and judgment. The worker's value is execution. Mixing them degrades both — the orchestrator burns context reading code it doesn't need to fully hold, and the worker wastes cycles on coordination it shouldn't own.

---

## Document Structure

```
project/
├── context_briefs_and_plans/
│   ├── context_brief-[feature].md     # feature spec / requirements
│   ├── plan-[feature].md              # implementation plan (written by planner agent)
│   └── [feature]-handoff.md          # orchestrator bootstrap for new chat sessions
├── ai_auditing_and_reviews/
│   └── codex_output-[n].md           # raw output from worker/review agents
```

**Rule:** Orchestrator reads handoff docs and reviews. Workers read code and plans. Nothing crosses over.

---

## Workflow

```
1. Brief       → Write context_brief-[feature].md (human or orchestrator)
2. Plan        → Planner agent reads brief + codebase → writes plan-[feature].md
3. Scrutiny    → Reviewer agent reads plan + codebase → verdict doc
4. Arbitration → Human reviews verdict, settles disputes, approves plan
5. Allocate    → Allocator agent reads plan → produces N worker prompts
6. Implement   → Worker chats execute prompts sequentially (or parallel where safe)
7. Review      → Reviewer agent reads full diff → pass/fail per step
8. Fix         → Fix prompts for any failures (targeted, not full rewrites)
9. Test        → Human runs end-to-end on real data, spot-checks output
10. Merge      → Clean branch, merge to main, repo hygiene pass
```

---

## How to Use This (Start Here)

If this is your first time, here's the full loop from zero to shipped feature:

**Step 1 — Capture your idea.**
Open any AI chat (doesn't have to be your orchestrator). Explain your idea freely — what you want to build, why, any constraints you know about. Ask that chat to summarise it into a concise brief. Copy the summary.

**Step 2 — Start your orchestrator chat.**
Open Claude Code (or Claude in the IDE). This is your orchestrator — it will be your single point of contact for the entire feature. Paste your brief. The orchestrator will ask clarifying questions if needed, then compose a prompt for a planner agent.

**Step 3 — Run the planner.**
The orchestrator gives you a prompt to paste into a separate worker chat (Codex or a new Claude tab). That worker reads your codebase and writes a detailed plan. **All worker output must be saved as a `.md` file** in an agreed directory in your repo (e.g. `context_briefs_and_plans/`). Never leave output only in the chat — it will be lost when the session ends.

**Step 4 — Review and approve.**
Paste the plan output back to your orchestrator. It will compose a scrutiny prompt. Run that in another worker chat, save the output as an `.md` file, paste the verdict back to the orchestrator. You read both and decide what to approve.

**Step 5 — Implement.**
The orchestrator composes worker prompts (one per implementation batch). Each worker reads the plan, writes code, commits, and reports back. You paste results to the orchestrator.

**Step 6 — Review, test, merge.**
One final review worker reads the full diff and reports pass/fail. You run the end-to-end test yourself. Orchestrator handles the merge prep and repo cleanup.

**On context management:**
Each AI chat has a context window — a limit on how much it can hold in memory at once. Workers fill up fast because they read full codebases. Orchestrator fills up slower because it only reads docs. When any chat starts feeling slow or confused, it's usually context pressure. For workers: just start a new chat with a fresh self-contained prompt. For the orchestrator: ask it to write a handoff doc (`context_briefs_and_plans/handoff-[date].md`) before it fills up, then start a new orchestrator chat and point it at that file. No information is lost.

**The golden rule: everything important lives in a file, not in a chat.**
Plans, reviews, allocations, handoffs — all saved as `.md` files in the repo. Chats are ephemeral. The repo is the source of truth.

---

## Installed Subagents (Claude Code)

The roles above also ship as **real Claude Code subagents** in [`.claude/agents/`](.claude/agents/), installed at the user level (`~/.claude/agents/`) so they work in **every** repo you open. They are repo-agnostic — each discovers the project's test command, plans/reviews directories, and conventions at runtime.

Manage them with the `/agents` command. Invoke one by asking for it by name ("use the **planner** agent to…") or let Claude delegate automatically based on the task.

| Agent | Role | Writes code? |
|-------|------|:---:|
| `explorer` | Read-only codebase research — answers questions, locates code, maps structure | no |
| `planner` | Writes a detailed, review-ready implementation plan | no |
| `plan-reviewer` | Scrutinises a plan before any coding (High/Medium/Low + verdict) | no |
| `worker-allocator` | Splits an approved plan into batched, self-contained worker tasks | no |
| `implementer` | Executes specific plan steps in scope, runs tests, commits | **yes** |
| `fixer` | Applies targeted, minimal fixes for review findings / known bugs | **yes** |
| `debugger` | Root-causes a bug or failing test, proposes the minimal fix | no (diagnoses) |
| `code-reviewer` | Final pre-merge review of a feature branch vs its plan | no |

**The orchestrator is your main session, not a subagent.** Copy [`orchestrator.CLAUDE.md`](orchestrator.CLAUDE.md) into a project's `CLAUDE.md` (or your global `~/.claude/CLAUDE.md`) to make the session you type into behave as the architect that delegates to these agents.

**Editing an agent:** the versioned source is in this repo's [`.claude/agents/`](.claude/agents/). After changing one, re-install with:
```
cp ".claude/agents/"*.md ~/.claude/agents/
```

---

## Agent Prompts (paste-into-any-chat templates)

If you're not using Claude Code subagents (e.g. Codex or a plain chat), [`prompts/`](prompts/) holds the same roles as ready-to-paste, fill-in-the-brackets templates:

| File | Role |
|------|------|
| [`orchestrator-bootstrap.md`](prompts/orchestrator-bootstrap.md) | Bootstrap a new orchestrator chat from a handoff doc |
| [`planner.md`](prompts/planner.md) | Write a detailed implementation plan |
| [`plan-reviewer.md`](prompts/plan-reviewer.md) | Scrutinise a plan before coding |
| [`worker-allocator.md`](prompts/worker-allocator.md) | Split a plan into self-contained worker prompts |
| [`implementer.md`](prompts/implementer.md) | Implement specific steps from a plan |
| [`code-reviewer.md`](prompts/code-reviewer.md) | Review completed implementation on a feature branch |

---

## Key Principles

1. **Handoff documents are the connective tissue.** When an orchestrator chat fills up, a well-written handoff doc lets a new chat pick up instantly with zero information loss.
2. **Workers are stateless.** Every worker prompt must stand alone — include file paths, line numbers, what not to touch, and how to validate.
3. **Separation is a hard rule.** If the orchestrator starts reading code, it's doing the worker's job and burning its own context.
4. **One commit per worker.** Workers commit all their steps together, not step-by-step.
5. **Review before merge.** Always a review pass on the full diff before touching main.
