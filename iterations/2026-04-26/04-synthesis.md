# Synthesis — JTC Mat (3-agent pass)

*Date: 2026-04-26. Agents: general-overview, bug-iteration, redesign.*
*Path chosen: 3 — Direction **C** (Office Space Diorama) + bug top 5. A and B retained for future reference in `03-redesign.md`.*

## Where the three agents converged

| Item | Flagged by | Why it matters |
|---|---|---|
| Custom-conclusion is half-designed (overwrites random tile, no list/manage UI, leaves stale stats) | overview, bugs, redesign | Bug + UX + brand opportunity in one fix. Direction C wraps it as Form JTC-2 with fake approval queue |
| Settings + Config sheet split is confusing | overview, bugs, redesign | Collapse into one sheet with two tabs; resolves dual-listener bug on `resetBtn` |
| `Space Grotesk` referenced but never loaded | overview, bugs, redesign | One-line fix |
| No streak / verdict-history surfacing despite `blameCounts` existing | overview, redesign | Direction C: bulletin-board section with tacked-up past verdicts |
| Memo card could push parody harder | overview, redesign | Direction C: dot-matrix tractor-feed printer |

## Bug pass top 5 (implementing alongside Direction C)

1. Custom-conclusion stale state (kills 3 user-visible bugs at once)
2. Share button copy regression
3. Load `Space Grotesk` or remove from font stacks
4. `updateStatsLine` tie-breaking
5. Multi-codepoint emoji handling

## Direction C implementation phases

1. **Foundation** — bug fixes top 5; Settings/Config tab collapse; Space Grotesk decision
2. **Cubicle frame** — mat in pinned `<figure>`, cubicle-fabric background, pushpins
3. **Dot-matrix printer** — verdict prints on perforated tractor-feed paper below mat
4. **Keyboard dock** — beige Compaq keyboard with chunky keycaps; JUMP = spacebar, ⚙ = F1
5. **Second screen / office** — scroll-down: bulletin board (past verdicts), employee handbook (about), copyright 1999 footer

Each phase = one commit on `redesign/diorama-c`. Preview after each phase via raw.githack.com.
