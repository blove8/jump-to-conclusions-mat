# Bug Iteration — JTC Mat

*Agent: general-purpose. Date: 2026-04-26. Method: static code review.*

## Validation of candidates

1. **Avatar positioning math** — **PARTIAL / by-design.** Line 1498 comment explicitly says "overhangs the tile's upper-left corner" — intentional pivot from original's centered placement. Cannot confirm visual correctness without browser. **P2** if visual review confirms it looks off; otherwise P3.
2. **Custom conclusion overwrites tile destructively** — **REAL.** `index.html:1761-1764`. **P1.**
3. **Share button copy regression** — **REAL.** Initial label `↗ Reproduce` (1024); fallback restore says `Share this take` (1194). Also `shareBtn.textContent = 'Copied ✓'` (1193) wipes the `<span>` icon. **P1.**
4. **Duplicate `.zone` CSS** — **REAL.** `line-height` at 290 + 302; `white-space: pre-line` at 291 + 306. **P3.**
5. **Two `resetBtn` listeners** — **REAL but harmless today.** Order-fragile. **P3.**
6. **`updateStatsLine` ties favor lowest index** — **REAL.** Line 1272. **P2.**
7. **Multi-codepoint emoji breaks layout** — **REAL.** `cfgPersonEmoji maxlength="4"` (1102) lets ZWJ-joined glyphs in. `loadConfig` (1245) `.slice(0, 4)` can split a grapheme mid-codepoint. **P2.**
8. **Narrator priming utterance** — **REAL.** `primeAudio` bound to first gesture regardless of toggle (1372-1374). **P3.**
9. **Resize during auto-×5 race** — **REAL.** `resize` listener (1642) calls `placePerson` mid-hop. **P2.**
10. **`Space Grotesk` not loaded** — **REAL.** Referenced at 436, 768; `<link>` (26) loads only Fraunces, Inter, JetBrains Mono. **P2.**
11. **`stampVerdictDate` locale** — **REAL.** Lines 1165-1166. **P3.**
12. **Hardcoded `internIndex = 2`** — **REAL.** Line 1529. **P3** alone, but couples with new bug below into P2.

## Additional bugs found

### P1

- **Custom conclusion leaves stale `data-hit` badge and `blameCounts`.** A tile previously hit 5 times keeps showing the "5" badge under new text. Same root as candidate #2.
- **Custom conclusion can overwrite the currently active tile without clearing `.active` or `currentZone`.** The orange highlight stays under new text.

### P2

- **Custom-conclusion injection silently breaks intern-bias mode** when `indexToReplace` randomly hits index 2.
- **`addCustomBtn` accepts duplicates** — same input twice replaces two random tiles with identical text.
- **`speechSynthesis` voices race.** First load in Chrome/iOS returns `[]`. No `voiceschanged` listener.
- **`runJump` `setInterval` not cleared on early errors.** No `try/catch` around interval body.
- **`narrate()` `setTimeout(60ms)` race** — rapid clicks can produce double-speak.

### P3

- Resize listener has no debounce.
- `audioCtx` never closed.
- `.btn-share` `transform: rotate(-1.5deg)` + focus-visible may produce clipped focus ring.
- `<dialog>` Esc close path doesn't reset `currentZone` / focus return.
- `localStorage` poisoned values can produce `NaN` after `clamp`.
- Reduced-motion preference disables transition but `runJump` still uses real interval timing.

## Recommended fix priority (top 5)

1. **Custom-conclusion stale state** (Candidate #2 + new P1 stale-badge / stale-active) — single tight code path fixes three user-visible bugs at once.
2. **Share button copy regression** (Candidate #3) — one-line wording fix; ships inconsistent UI on first share.
3. **Load `Space Grotesk` or remove from font stack** (Candidate #10) — silent visual regression.
4. **Tie-breaking in `updateStatsLine`** (Candidate #6) — small change that meaningfully improves stats correctness.
5. **Multi-codepoint emoji handling** (Candidate #7) — use `Array.from(str).slice(0, 1)` for grapheme-aware truncation.
