# General Overview — JTC Mat

*Agent: general-purpose. Date: 2026-04-26.*

## What I found this thing actually is

A single-file, self-contained "decision engine" parody in the Office Space / Initech voice — a 4×3 mat of preloaded corporate-cop-out conclusions (`Blame The Intern`, `Reorg Will Fix It`, etc.) where a stick-figure avatar hops between tiles and lands on a verdict. Audience is internet/tech humor; the page is meant to be screenshot- and share-bait, with the verdict styled as an interoffice memo. State is more advanced than it first looks: persistent config in localStorage, a custom-conclusion injector, narrator TTS, hop-physics tuning, and a 5x auto-jump mode.

## Architecture & code structure

- One file, ~1833 lines: `<style>` lines 27–987, markup 989–1124, `<script>` 1126–1831. No build step, no external deps beyond Google Fonts.
- Logical "modules" inside the script, loosely separated by blank lines: state (1199–1228), config persistence (1230–1260), audio priming/boing (1277–1426), TTS (1428–1468), confetti (1470–1482), zone build + jump engine (1485–1633), sheet/dock wiring (1659–1735), reset + custom conclusion (1737–1776), config UI (1778–1830).
- Config layer is the clean part: `CONFIG_DEFAULTS` (1207) + `CONFIG_SCHEMA` (1219) + generic render/bind (`renderConfigInputs`/`bindConfigInputs`) is data-driven and easily extended.
- Markup uses two `<dialog class="sheet">` elements as bottom sheets — Settings and Config (Form 7B / Form 27B/6). Good native-platform reach.
- Lots of explanatory comments (1289–1294, 1358–1359, 1458–1460, 1567–1571, 1706–1707, etc.) that read like deliberate post-mortem notes on iOS Safari quirks.

## Visual / brand

- Coherent identity: warm-amber `--accent`, oxblood `--stamp`, dark `#0e0c11` base; mat uses brown/gold gradient with engraved-feel inset shadows (262–273). Radial backgrounds (82–84) and overlaid SVG-noise body::before (104–113) sell a "70s office document" texture.
- Typography is the main lift — Fraunces italic display for the verdict, JetBrains Mono for ALL-CAPS metadata kickers, Inter for UI. The Fraunces italic + memo card pull off the "interoffice memo" conceit well (lines 605–614, "INITECH · INTEROFFICE · CONFIDENTIAL" footer stamp).
- The mat itself (`.mat-wrapper::before`/`::after`, "Fig. 1 — The Mat™", "No. 27B/6", lines 245–254) is a small but landed Easter egg.
- Hero h1 has `aria-label` overriding the visual `<br>` split — the trademark is wrapped in `aria-hidden` (994). Conscious choice.

## Interaction model

- One central button: **JUMP!** in the fixed dock. Avatar hops `hopCountBase + 0..3` random tiles (chaos +4) at `hopIntervalMs` per hop, then lands on a chosen tile (1573–1619). Conclusion fades up in the memo card, with a `Re:` line and a filed-date timestamp.
- Secondary actions live in dock: sound toggle, narrator toggle (mirrored to checkboxes inside Settings sheet — see `bindDockToggle` 1719–1735), `×5` auto-jump, and `⚙` settings.
- Two sheets: **Settings** (chaos / narrator / sound / intern-bias toggles + custom-conclusion injector + reset) and a deeper **Config** (8 sliders/inputs persisted to localStorage).
- Selection has memory: tile-cooldown (don't repeat within N turns, 1518–1524), intern-bias chance (1531–1534), confetti on streak or random. Replays land differently.
- Empty/default state: "Awaiting your first hot take." → after first jump, "Re: Your most pressing question" appears, share button reveals, jump button text changes to JUMP AGAIN.

## Areas that warrant a deeper look

### For the bug-iteration pass

- Avatar positioning math: `getZoneLandingPosition` (1495–1505) uses `zoneRect.left - matRect.left - 4` and the `.person` has `transform: translate(0,0)` (375), yet original used `translate(-50%, -50%)` for centering.
- Custom conclusion overwrites a random existing tile (1761) — destructive, no undo, can clobber a tile the user just blamed. Doesn't invalidate `blameCounts`/`lastLandedTurnByIndex`.
- Share button copy regression: `shareVerdict` fallback (1194) sets innerHTML to "Share this take", but the initial label is "Reproduce" (1024).
- `.zone` has duplicated rules: `line-height` declared twice (290, 302) and `white-space: pre-line` twice (291, 306).
- Two listeners on `resetBtn` registered in different places (1697 closes sheet; 1737 actually resets).
- `updateStatsLine` picks first-found max (1272) — ties always favor the lowest index.
- Avatar-emoji width assumption: `.person { font-size: 1.95rem }` and the `-4px` offsets assume a single-codepoint emoji; multi-char input will overflow the tile.
- `narrate()` may double-speak: priming utterance ('go', 1360) plays at volume 0.01.
- Auto-×5: `runAutoJumps` (1625) does not catch in-progress sheet open or page resize; `resize` listener (1642) calls `placePerson` mid-hop, can snap the avatar.
- `pulse-jump` ring restarts on reset.

### For the redesign pass

- Hierarchy below the mat is thin — no second screen, no "About," no tongue-in-cheek FAQ.
- Memo card visual could be pushed harder — faux-typewriter, faded carbon copy, perforated edge.
- Dock is busy on small screens: 5 controls in a fixed bar.
- Settings vs Tune split — two sheets with overlapping affordances.
- Kicker reads "v1.99" — opportunity for an evolving version pun.
- No visible streak/leaderboard feedback.
- Original has CRT/scanline vibe absent in the new one.
- The conclusions list is small (12) — gets repetitive fast.

## Surprises / oddities

- Heavy iOS-Safari plumbing for what's a joke site: `buildSilentWavDataUri` (1295–1316) hand-encodes a WAV header to unlock the iOS audio session. Suggests the author actually demos this on iPhones.
- Avatar offset comment contradicts the original implementation. Original used centered `translate(-50%,-50%)`. The new file changed positioning model entirely.
- `stampVerdictDate` uses `toLocaleDateString` with empty locales array (1166).
- `internIndex = 2` is hard-coded (1529).
- `addCustomBtn` silently mutates the conclusions array but has no UI to list current conclusions or remove a custom one.
- `Space Grotesk` is referenced in `.btn-jump` (line 436) and `.config-value` (768) font-family stacks but never loaded.
