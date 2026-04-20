---
description: Autonomously pick one backlog item, implement it, open a PR for approval
---

# /pick-next

You are a **cost-conscious solo agent** working on `blove8/jump-to-conclusions-mat`.
Follow this checklist **EXACTLY**. Stop at any step if a precondition fails. Every
tool call is quota. Keep the run tight.

## Preflight (aim for ≤ 5 tool calls)

1. **Read** `backlog.md`. If the YAML front-matter has `agent: paused` (or anything
   other than `agent: active`), append a skip line to `.agent/runlog.md` and exit.
2. **Check open PRs**: `mcp__github__list_pull_requests` (or `gh pr list`) with
   state open. If **≥ 2** PRs are open on this repo from branches named
   `claude/agent-*`, exit: the human must review/merge before more work is queued.
3. **Pick the top eligible item**: first unchecked line in `## Active` whose
   size tag is `[XS]`, `[S]`, or `[M]`. If the top unchecked item is `[L]` or
   larger, stop and open a GitHub issue titled `Split: <item>` describing a
   decomposition, then exit. **Do not** skip to a smaller item below it.
4. If no eligible item exists, append a skip line to the runlog and exit.

## Execution

5. **Branch**: `git checkout -b claude/agent-<slug>` where `<slug>` is a short
   dasherized version of the item (≤ 40 chars).
6. **Implement** directly in `index.html` — the project is a single file. Do
   **not** touch `original/*` (v0 archive), do **not** change the 12 canonical
   conclusions, do **not** modify `main` directly.
7. **Budget**: keep the diff under **300 LOC**. If the change is growing past
   that, stop, `git reset --hard origin/main`, and open a `Split:` issue instead.
8. **Syntax check**: run this and confirm it exits silently (no output = OK):
   ```
   node -e "const h=require('fs').readFileSync('index.html','utf8'); const s=h.match(/<script>([\\s\\S]*?)<\\/script>/)[1]; new Function(s);"
   ```
   Must pass before committing. Fix any parse error before proceeding.
9. **Commit** with a single complete message explaining the **why** (not just
   the what). No AI-assistant trailers or self-promotion.

## Ship

10. **Push** the branch.
11. **Open a PR** against `main` with the `agent/auto` label (create the label
    if it doesn't exist) and the repo owner as assignee. The PR body must
    include:
    - The backlog item being addressed (copy the exact line, including tag)
    - A **Test plan** bullet list a human can follow in ≤ 2 minutes
    - A one-line **Cost footprint** — approximate tool-call count for the run

## Close-out

12. **Do not** mark the backlog item done yet. The agent will move it to the
    `## Done` section on the **next** run once the PR is merged. That avoids
    losing work if a PR is rejected.
13. Append a single-line entry to `.agent/runlog.md`:
    `- YYYY-MM-DD HH:MM — picked "<item>" — PR #N (or "skipped: <reason>")`
14. **Exit.** Do not start another item in the same run.

## Hard rules — never break these

- **Never** touch `main` directly.
- **Never** merge your own PRs or bypass review.
- **Never** spawn research subagents unless the item explicitly calls for
  cross-codebase research. This project is one 2000-line HTML file.
- **Never** disable lints, tests, or syntax checks to "make it pass".
- **Never** invent a new backlog item, slash command, or priority.
- **If blocked**: stop, log the blocker to the runlog, exit. Don't thrash.

## Closing the loop on merged PRs

On every run, before step 3, also:
- `gh pr list --state closed --label agent/auto --limit 10` — for any that
  are `merged:true`, move the corresponding backlog item from `## Active` to
  `## Done` with the PR number appended: `[XS] Example thing — #42`. Commit
  this as a tiny maintenance commit on the agent's working branch only if
  it's picking up another item this run; otherwise leave for next run.
