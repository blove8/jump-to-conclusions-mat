# Redesign — JTC Mat

*Agent: Designer. Date: 2026-04-26.*

## Where I think this site is on the curve

The visual identity is genuinely strong — the brown/gold mat with engraved bevels, the Fraunces italic verdict card with `INITECH · INTEROFFICE · CONFIDENTIAL` watermark and `Filed {DATE} · {TIME}` stamp, the noise overlay, the oxblood "Reproduce" stamp at -1.5°. Brand is 80% landed. The highest-leverage push isn't visual polish — it's **commitment to the bureaucratic-document conceit beyond the single memo card**, and **fixing the spatial flow** so the verdict you just generated isn't ejected to a small card sitting under a 4:3 mat that ate the viewport.

---

## Direction A — "The Form is the Site" *(SHELVED — keep for future reference)*

### Thesis
Lean all the way into the bit: every surface on the page is a numbered, stamped, carbon-copy Initech form.

### What changes
- **Verdict card becomes a multi-part NCR triplicate** — white/yellow/pink layered with 1px offset, perforated edges. Visible top sheet is the verdict; faded yellow + pink peek under it as a stack of "previous filings."
- **Form-numbered everything**: kicker becomes `FORM JTC-1 · REV. 1.99`. Extend `Fig. 1`, `No. 27B/6` to dock (`AUTH. PANEL`), verdict (`MEMO 47-B`), share button (`STAMP & FILE`).
- **The "Filed" timestamp** stops being decoration and becomes the stack identifier — `MEMO #001`, `#002`, etc.
- **Custom conclusions** become "FORM JTC-2: PROPOSE NEW CONCLUSION" — submitting routes through a fake approval queue ("PENDING REVIEW BY R&D · 0:03 ELAPSED") before injecting.

### What stays untouched
- The mat itself, bevels, gradients, active-tile pulse, hop animation
- Fraunces italic for verdicts, JetBrains Mono for kickers, brown/gold/oxblood palette
- Hero treatment with Smykowski tagline citation

### Effort / risk
**Medium.** Triplicate stack is ~80 lines CSS + small JS array push. Form-renaming is copy. Approval queue is `setTimeout` with fake progress label.
**Risk:** Going too on-the-nose. Restraint required — pick 4–5 hero form numbers, not 12.

---

## Direction B — "Mat-First, Verdict-In-Place" *(SHELVED — keep for future reference)*

### Thesis
The mat is the hero; right now the verdict is exiled below it. Collapse them: the verdict overlays the mat tile that was hit, and the rest of the mat dims behind it like a Polaroid being developed on top of the tile.

### What changes
- **Landed verdict animates into a card that pins to/over the active tile**, anchored at the tile location with a curled-paper-being-laid-over-it transform. Card is small (~60% of mat width), at -2° rotation, has a pinned-thumbtack pseudo-element.
- **Verdict card below disappears** or shrinks into a one-line "ledger" strip: `JUMP #003 · 14:32 · "Blame The Intern" · [share]`. Ledger grows downward as you keep jumping — earned scroll-for-jokes.
- **The dock loses ×5 and ⚙** (move into "more" overflow). Dock becomes: `[🔔] [JUMP] [🗣]`. Three-element dock reads as one decision tool.
- **Per-tile hit counts get promoted** from 0.52rem corner pills to embossed gold dots along the tile edge.

### What stays untouched
- Audio plumbing, narrator, intern-bias, chaos shake
- Mat geometry, hop animation
- Smykowski citation

### Effort / risk
**Small-to-medium.** Pinned card is positioning + transform. Ledger strip is `<ol>` with append-on-jump.
**Risk:** Pinned cards on small screens (320–375px) eat the whole mat. Need breakpoint fallback. Also: hiding ⚙ behind overflow is a measurable discoverability hit.

---

## Direction C — "Office Space Diorama" *(CHOSEN — implementing)*

### Thesis
Stop being a web tool, become a *web shrine*. The page above the fold is a stylized cubicle wall — the mat is hung on the wall like a corkboard, the verdict prints on a dot-matrix tear-feed printer below it, and the dock is a beige Compaq keyboard.

### What changes
- **Mat reframed as a wall artifact**: keep the mat exactly as-is, but place it inside a `<figure>` with a pushpin in each corner (CSS), slight outward shadow as if mounted, on a faint cubicle-fabric texture background (existing noise SVG, recolored to gray-beige).
- **Verdict prints from a dot-matrix printer** under the mat — perforated tractor-feed paper sliding down on each jump (CSS `transform: translateY` from -100% to 0), Courier/JetBrains Mono text, intentional ink-fade gradient. Could add a subtle dot-matrix `brrt` sound if narrator+sound on.
- **Dock becomes a low-fi keyboard strip** with chunky beige buttons and visible keycaps. JUMP becomes the spacebar. `⚙` becomes a labeled `F1` key. ×5 becomes `×5 ↻`.
- **Second screen below the fold**: scroll past the cubicle and find the rest of the office — "Bulletin Board" with previous verdicts as tacked-up sheets, "About" panel as Initech employee handbook, footer "Copyright 1999, all rights reserved" (already half-true: `Est. 1999` on line 1112).

### What stays untouched
- The conclusions list itself (lines 1127–1140) — the writing is the joke
- All accessibility scaffolding (`aria-live`, `aria-pressed`, focus-visible)
- The mat geometry. Don't recolor or reshape

### Why it lands better
A leans into forms. B leans into spatial flow. **C is the share-bait maximalist** — page becomes a destination people screenshot the *whole page* of, not just the verdict. Office Space film is iconic; the page can earn its place in the meme-stack.

### Effort / risk
**Large.** Cubicle frame, printer animation, keyboard dock, second screen — 300–500 added lines.
**Risk:** Easiest direction to over-do and end up looking like a 2007 deviantArt skin. The current minimalist-with-strong-typography aesthetic has restraint; C trades that for commitment to the bit.

---

## Surgical wins (regardless of direction)

1. **Promote per-tile hit counts to actual heatmap.** Tint tile background by hit-count (small amber glow) or make count a real number in JetBrains Mono at 0.7rem corner badge.
2. **Custom conclusion needs a managed list.** Bordered list inside Settings showing "YOUR CUSTOM TILES (2/4 max)" with each removable. Add "Replace which tile?" select instead of random.
3. **Collapse Settings + Config into one sheet with two tabs.** Form 7B and Form 27B/6 become tabs inside one sheet.
4. **Load Space Grotesk or remove the references.** Lines 436 and 768.
5. **Share text should be the verdict alone, not narrative.** `"X" — Initech Decision Engine` with deep-link (`?v=X` or `#blame-the-intern`).
6. **Kicker version evolves.** After 10 jumps `v1.99-RC1`. After 25 `v1.99 SP2`. After 50 `v2000`.
7. **Memo card needs a verdict number.** Replace timestamp-only with `MEMO #003 · CC: ALL HANDS`.
8. **The footer is dead space.** Grow into tiny FAQ-as-bureaucracy or shrink to nothing.

## Things explicitly NOT to change

- Mat geometry, gradients, bevels, active-tile red pulse (lines 257–365)
- Smykowski citation under tagline (lines 996–999)
- Fraunces italic verdict typography (lines 640–653) with opacity-and-translateY entrance
- JetBrains Mono ALL-CAPS metadata kickers
