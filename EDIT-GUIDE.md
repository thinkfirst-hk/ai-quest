# Think First! AI Quest — Edit Guide

The whole game is **one file: `index.html`**. All text a non-coder might want to change
lives in the `C = { … }` object near the top of the `<script>`, between the markers
`/*__CONTENT_START__*/` and `/*__CONTENT_END__*/`. Everything below
`__CONTENT_END__` is game logic — don't touch it unless you're changing behaviour.

## 1. How text works

Every string is a bilingual pair:

```js
title: {en:"The Homework Shortcut", zh:"功課捷徑"}
```

Where the two age tiers need different wording, each language nests an `ex`
(Explorer, 8–10) and `pi` (Pilot, 11–14) variant:

```js
text: {en:{ex:"Simple version…", pi:"Fuller version…"},
       zh:{ex:"簡單版……",        pi:"完整版……"}}
```

If a string has no `ex`/`pi` split, both tiers share it. Keep `\n` inside a string to
force a paragraph break (the story box preserves line breaks).

## 2. Anatomy of a scenario

Each entry in `C.scenarios` is:

```js
{
  id:"homework", icon:"📚",
  title:{en:"…", zh:"…"},
  skills:["own","question"],   // shown on the mission-complete screen
  start:"b1",                  // first node id
  nodes:{ … }
}
```

Five node types (the first three are the original menu model; `flag`/`reveal` are the
v2.0 flag-then-check model — see §9):

| Type | Looks like | Purpose |
|---|---|---|
| story (no `type`) | `{step:"pause", text:…, sonar:…, choices:…}` | a beat + decision. `step` lights the Think-First-Loop chip. `sonar` (optional) is a Captain Sonar bubble. |
| outcome | `{type:"out", tone:"wise"/"risky"/"mixed", step:…, text:…, ask:…, next:…}` | consequence of a choice. `tone` drives the glow/shake animation and the debrief icon. `ask` is Sonar's one reflective question. Rewind is automatic. |
| end | `{type:"end", text:{…}}` | the scenario's closing reflection. Every scenario must have a node with id `end`. |
| flag | `{type:"flag", step:…, claims:{ex,pi}, next:"reveal", …}` | active-reading board: the child taps the claims that seem off, with a scripted check tool. No labelled right/wrong. See §9. |
| reveal | `{type:"reveal", step:…, out:{calibrated,overrely,overreject}, next:"end", …}` | truth board + calibrated consequence + (Pilot) reflection. |

**Choices** are either one array (both tiers) or `{ex:[…2 max…], pi:[…3–4…]}`:

```js
{t:{en:"Button label", zh:"按鈕文字"},
 tone:"wise",            // wise | risky | mixed | plain (plain = a "continue" beat)
 fx:{own:2, decide:1},   // optional: meter points (wise/mixed only by convention)
 set:{pasted:true},      // optional: story flag for later branching
 next:"c2b"}             // node id, or {ex:"end", pi:"b3"}, or {flag:"pasted", then:"end", else:"b3"}
```

Rules of thumb that keep the pedagogy intact:
* risky choices: **no `fx`** — and they automatically subtract `C.riskyPenalty`
  (default 1) from the decision node's own loop `step`, floored at 0, so a risky
  pick visibly costs a point on the bar without ever going negative. The consequence
  text + Captain Sonar's reflection stay the main teaching; the tone never punishes.
  Set `C.riskyPenalty:0` to restore the original "no meter effect" behaviour;
* every outcome reconverges eventually; the "memory" of a risky path lives in its text;
* Explorer arrays max 2 choices; keep one clearly tempting option in every decision.

## 3. Adding a 7th scenario

1. Copy any scenario block in `C.scenarios`, give it a new `id`, `icon`, `title`, `skills`, nodes.
2. Append it to the array — the Mission Map, unlock chain, meter and debrief pick it up automatically.
3. (Optional) add a matching entry to `C.checks` — check `N` is shown after scenario `N+1`
   (index-aligned: `checks[0]` follows `scenarios[0]`). With 7 scenarios and 5 checks,
   scenarios 6–7 simply have no check; add up to 6 checks if you want one after each
   non-final scenario. Each check needs **exactly one** `ok:true` option.

## 4. Adjusting age-tier wording

Search the scenario for `ex:` — those are the Explorer variants. To give a node
tier-specific text where it currently shares one string, replace
`text:{en:"…", zh:"…"}` with the nested form shown in §1. To give Explorer different
choices, convert `choices:[…]` to `choices:{ex:[…], pi:[…]}`.

## 5. Meter & licence logic (in case you need it)

* `C.target` (= 4) is the points needed to fill one meter segment.
* Wise/mixed `fx` add points; a risky choice subtracts `C.riskyPenalty` (= 1) from
  its decision node's loop `step`, clamped to `0…`. A small "▼ STEP −1" cue shows on
  that risky outcome; rewinding restores the points (the snapshot is taken before the
  deduction).
* The licence's **strongest skill** = highest score; **training focus** = lowest
  (ties resolve in loop order). Sonar Checks give +1 to their step when answered
  right first try.
* Progress persists in `localStorage` under the key `tfq_v1`; if storage is blocked
  the game silently runs in-memory. Bump the key if you ship breaking content changes.

## 6. Pilot Profiles (v1.1)

The end screen offers a **Pilot Profile** badge (Autopilot Rider 自動波機師 · Ground Crew
地勤隊長 · Test Pilot 試飛員 · Wingmate 僚機夥伴 · Captain 機長). All profile text lives in
`C.profiles` — kid-facing fields (`name`, `line`, `strength`, `focus`) are warm and
identity-positive; `diag` and `qs` (2 tailored adult questions) appear **only** on the
Debrief Sheet. The "snapshot, not forever" line is `C.ui.snapshot`.

Classification is deterministic and documented in the comment above `computeProfile()`
in the engine. It reads the skill totals plus two optional choice tags you can use when
writing scenarios: `axis:"trust"` (leaned into AI without checking) and `axis:"guard"`
(pushed AI away even when useful). Wise choices in missions 5–6 count as people-first
signals. If you add scenarios, tag the clearly over-trusting / over-rejecting choices
the same way and the profiles keep working. Captain requires balance across all five
loop steps and is checked first; ambiguous data falls back to Test Pilot or Wingmate,
never Captain.

## 7. Sound & motion (v1.2, from the designer's asset kit)

* **Sounds** are fully synthesized in the inline `TFSFX` object (no audio files): `tap,
  wise, risky, mixed, tick, sonar, correct, stamp, rewind, unlock, fanfare`. They're
  wired to choices (by tone), meter gains, Sonar Checks, mission stamps, map unlocks,
  rewinds and the licence fanfare. The 🔊/🔇 pill in the top bar toggles `S.muted`
  (persisted). Audio starts only on user gestures, and every call no-ops if WebAudio
  is unavailable.
* **Motion classes** are `tf-` prefixed in the stylesheet (pop, stamp, bob, hop,
  wiggle, enter, unlock, twinkle, typing, confetti). All are disabled automatically
  by the global `prefers-reduced-motion` guard.
* **Typing dots**: give any story node `typing:true` and its message appears after a
  short chatbot-style typing animation (skipped under reduced motion; shown once per
  visit). Currently on the Luna (M2) and Kiki (M6) openings.
* **Designer SVGs (v1.3)**: 35 artworks from the asset kit are inlined in the `ART`
  registry (engine, just below the transient guards) and placed with the `art(name,
  cls, width)` helper. Captain Sonar now uses the six pose SVGs context-sensitively
  (thinking at decisions, celebrate on wise outcomes, worried on risky — never angry,
  point on Sonar Checks, wink for the profile reveal). Scene backdrops show on each
  mission's first node (`SCENE_ART`, index-aligned), cast avatars appear in the
  header strip (`CAST`) and as the typing-dots avatar (`CHAT_AVATAR`). UI art:
  stamp-complete on mission end, seal-licence on the licence, lock/unlock on the map,
  sticker-wise / sticker-star rewards, sticker-rewind-hero + icon-rewind framing
  rewind as re-thinking, icon-loop-ring on home/debrief. Avatar `clipPath` ids are
  namespaced per avatar (the kit README's collision warning). Only
  `sticker-thinking-cap` was left out (unused). To swap art, replace the string in
  `ART` — same key, any viewBox-based SVG.

## 8. Self-test checklist (run after every edit)

Automated (what was run before shipping v1.0 — all passing):

- [x] JS parses under `'use strict'` (`node --check`), zero console output
- [x] every `{en/zh}` leaf has both languages; every tier variant has both `ex` and `pi`
- [x] every `next` reference resolves; every node reachable on Pilot tier; `end` reachable on both tiers
- [x] Explorer never sees more than 2 choices
- [x] every choice has label + tone + next; `fx` keys are valid loop steps
- [x] 5 Sonar Checks, exactly one correct option each
- [x] 16 full headless playthroughs (en/zh × Explorer/Pilot × 4 choice strategies), including a rewind in every scenario and a wrong-then-right answer on every check — all reach the licence
- [x] all five skills reach the meter target on a wise-choices run
- [x] risky choices deduct 1 from their loop step, floor at 0, show the cue once, and rewind restores the points
- [x] language toggle mid-game preserves screen, progress and meter
- [x] Debrief Sheet lists every logged choice, a meter snapshot and exactly 3 discussion questions
- [x] file size ≈ 100 KB (< 500 KB target), no external requests of any kind
- [x] all five Pilot Profiles reachable (Captain + Autopilot + Ground Crew on both tiers; scripted runs), classification deterministic
- [x] profile recomputes correctly after ⏪ rewind (trust/risky signals removed with the undone choice)
- [x] bilingual walk covers all profile strings; kid screen never shows the adult diagnostic; Debrief shows diagnostic + exactly 2 tailored questions

Manual (2 minutes in a browser):

- [ ] open `index.html` by double-clicking (file://) — loads with no console errors
- [ ] phone-width check: all buttons comfortably tappable (≥44 px)
- [ ] pick a risky choice → outcome shakes; wise → glows; reduced-motion OS setting disables both
- [ ] press ⏪ Rewind on an outcome → same decision reappears, meter restored
- [ ] finish mission 1 → Sonar Check appears; wrong answer → gentle retry, no fail
- [ ] toggle 中文/EN mid-scenario → same node, nothing lost
- [ ] complete all 6 → licence shows name, strongest skill + training focus
- [ ] Debrief Sheet → Print preview: buttons/topbar hidden, sheet prints clean
- [ ] refresh the page mid-mission → resumes where you were

## 9. The flag-then-check model (v2.0) — and how to apply it to the other scenarios

Mission 4 ("The Confident Wrong Answer") is the flagship of a new interaction model
that **makes the child perform the judgment** instead of picking it from a labelled
menu. It is two reusable node types, driven entirely by data in the content object:

**`type:"flag"`** — the AI answer rendered as tappable **claim chips**. Each claim:

```js
{ status:"correct" | "error" | "cant",      // truth (never shown until the reveal)
  text:{en,zh},
  check:{ truth:{en,zh},                     // what the check tool concludes
          sources:[ {title:{en,zh}, snip:{en,zh}} ] } }
```

Put the claims under `claims:{ ex:[…], pi:[…] }` (Explorer: one clear error + correct
claims; Pilot: a subtler error + a `cant` claim so they must tell *wrong* from
*unverifiable*). The board also carries plain bilingual labels (`assistant`, `topic`,
`intro`, `sonar`, `instab`, `checkBtn`, `lockBtn`, `toolTitle`, `srcLbl`, `checkedTag`,
`flaggedLbl`, `toolBack`) — all editable, no logic.

**`type:"reveal"`** — three consequence variants the engine picks by calibration:

```js
out:{ calibrated:{tone:"wise",  text:{en,zh}, ask:{en,zh}},
      overrely:  {tone:"risky", text:{en,zh}, ask:{en,zh}},   // missed it
      overreject:{tone:"mixed", text:{en,zh}, ask:{en,zh}} }   // flagged everything
```

plus `truthLbl`, `yourFlags`, and (Pilot) `reflectQ`/`reflectPh` for the one-line
free-text reflection.

**Scoring is calibration, not suspicion** (in `computeCalib`/`lockFlags`): caught the
error **and** didn't over-flag → wise `+question/+verify`; flagged accurate/`cant`
claims → `axis:"guard"` (over-rejection); missed it → `axis:"trust"` (over-reliance).
This feeds the existing meter, the Pilot-Profile classifier (Ground Crew vs Autopilot)
and the Debrief — no separate scoring system. The adult Debrief gains a one-line
diagnostic (`flagshipDebriefHTML`) and a copy-paste **JSON export** for pilot data;
each session record is `S.flagship[scenarioId] = {caughtError, overFlagged,
usedCheckTool, checkedError, bucket, reflection}`.

**To convert another scenario:** replace its `start` with a `board` (`type:"flag"`)
+ a `reveal` node, write the `claims[]`/`check` data and the three reveal variants,
keep `skills`/`id`/`end`. Nothing else changes — the map, meter, profile, debrief,
rewind/undo, language toggle, age tiers and print all work automatically. All board
state lives in `S.fb` (serialisable), so the language toggle and refresh are safe
mid-board, and the commit is snapshotted so ⏪ Rewind recomputes the meter.

**A/B toggle:** the original multiple-choice Mission 4 is kept at `C._classicM4`.
Load the game with `?m4=classic` (or run `TFQm4('classic')` in the console; `TFQm4('flag')`
to switch back) to play the old version in the same build for pilot comparison.

Self-test (automated, all passing): error catchable on both tiers; flag-everything
scores as over-rejection and missing it as over-reliance; calibrated catch lights
QUESTION/VERIFY; check tool reachable; language toggle mid-flag/mid-check keeps
selections; ⏪ rewind from the reveal restores the board and undoes the meter; Debrief
shows the diagnostic + JSON export; the other five scenarios still play end-to-end.
