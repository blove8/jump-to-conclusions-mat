---
agent: paused
notes: |
  Flip `agent: paused` to `agent: active` to let the loop pick work.
  Keep 3-6 items here so the agent always has options. Sizes:
    [XS] < 30 LOC   [S] < 100 LOC   [M] < 300 LOC
  Agent refuses [L] and above — it'll open a "Split:" issue instead.
---

# Decision Engine — Backlog

Open items, newest at top. Agent picks the TOP unchecked `[XS]`/`[S]`/`[M]` item per run.

## Active

- [ ] [XS] Metrics panel: add a "Top voice URI" row pulled from `config.narratorVoiceURI` so we can see which voice people actually pick
- [ ] [S] Canvas PNG: add a small serial-number corner stamp ("No. 27B/6") under the brand block to match the mat's diagram stamp
- [ ] [S] Archive sheet: each entry gets a "Reproduce ↗" action that copies the permalink URL for that verdict to clipboard
- [ ] [S] Keyboard shortcuts: `J` = JUMP, `/` = focus ask input, `?` = open help sheet listing all shortcuts
- [ ] [M] Wear & grime: faint SVG coffee-ring / scuff overlay on tiles, opacity tied to `blameCounts[i]` (caps at ~0.35), cleared on Reset

## Done

(PRs migrate here after merge — agent records them here on the next run)

## Deferred / needs design input

- Moonshot "Drift" — tiles slowly rewrite over time. Needs explicit greenlight.
- WebLLM / local model for bespoke verdict generation. Gated behind size + opt-in.
