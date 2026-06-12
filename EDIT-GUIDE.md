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

Three node types:

| Type | Looks like | Purpose |
|---|---|---|
| story (no `type`) | `{step:"pause", text:…, sonar:…, choices:…}` | a beat + decision. `step` lights the Think-First-Loop chip. `sonar` (optional) is a Captain Sonar bubble. |
| outcome | `{type:"out", tone:"wise"/"risky"/"mixed", step:…, text:…, ask:…, next:…}` | consequence of a choice. `tone` drives the glow/shake animation and the debrief icon. `ask` is Sonar's one reflective question. Rewind is automatic. |
| end | `{type:"end", text:{…}}` | the scenario's closing reflection. Every scenario must have a node with id `end`. |

**Choices** are either one array (both tiers) or `{ex:[…2 max…], pi:[…3–4…]}`:

```js
{t:{en:"Button label", zh:"按鈕文字"},
 tone:"wise",            // wise | risky | mixed | plain (plain = a "continue" beat)
 fx:{own:2, decide:1},   // optional: meter points (wise/mixed only by convention)
 set:{pasted:true},      // optional: story flag for later branching
 next:"c2b"}             // node id, or {ex:"end", pi:"b3"}, or {flag:"pasted", then:"end", else:"b3"}
```

Rules of thumb that keep the pedagogy intact:
* risky choices: **no `fx`** — the consequence text is the teaching, never a penalty;
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
* The licence's **strongest skill** = highest score; **training focus** = lowest
  (ties resolve in loop order). Sonar Checks give +1 to their step when answered
  right first try.
* Progress persists in `localStorage` under the key `tfq_v1`; if storage is blocked
  the game silently runs in-memory. Bump the key if you ship breaking content changes.

## 6. Self-test checklist (run after every edit)

Automated (what was run before shipping v1.0 — all passing):

- [x] JS parses under `'use strict'` (`node --check`), zero console output
- [x] every `{en/zh}` leaf has both languages; every tier variant has both `ex` and `pi`
- [x] every `next` reference resolves; every node reachable on Pilot tier; `end` reachable on both tiers
- [x] Explorer never sees more than 2 choices
- [x] every choice has label + tone + next; `fx` keys are valid loop steps
- [x] 5 Sonar Checks, exactly one correct option each
- [x] 16 full headless playthroughs (en/zh × Explorer/Pilot × 4 choice strategies), including a rewind in every scenario and a wrong-then-right answer on every check — all reach the licence
- [x] all five skills reach the meter target on a wise-choices run
- [x] language toggle mid-game preserves screen, progress and meter
- [x] Debrief Sheet lists every logged choice, a meter snapshot and exactly 3 discussion questions
- [x] file size ≈ 100 KB (< 500 KB target), no external requests of any kind

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
