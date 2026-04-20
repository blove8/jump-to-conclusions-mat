# .agent

Operational state for the solo autonomous agent that works on this repo.

- `runlog.md` — append-only log of every run (picked / skipped / errored)

See also:
- `../backlog.md` — the pause flag + active backlog
- `../.claude/commands/pick-next.md` — the slash command the agent follows

## Operator playbook

**Turn the agent on**
1. Edit `../backlog.md` — flip front-matter to `agent: active`.
2. Open a Claude Code session on the web against this repo.
3. Run `/loop 6h /pick-next`. Leave the session open.

**Turn it off**
- Flip `../backlog.md` back to `agent: paused`. The next loop tick will exit
  silently. Close the session when you're ready.

**Review a PR the agent opened**
- PR will be labeled `agent/auto`. Merge like any other PR. The agent will
  move the backlog line to `## Done` on its next run.

**If the agent is looping on the same failure**
- Check `runlog.md` — each run logs a reason for skip/stop.
- Pause it, fix the offending backlog item (likely too vague or too large),
  resume.
