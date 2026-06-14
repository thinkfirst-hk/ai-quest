# Think First! AI Quest — Design Doc (v1.0)

**ThinkFirst Learning (先思考) · Hong Kong**
A browser-based, story-driven decision game teaching AI **judgment** — not blind trust, not fear.
Benchmark mechanics: Google "AI Quests". Curriculum spine: the **Think First Loop**.

---

## 1. The Think First Loop (curriculum spine)

| Step | 中文 | Colour | Trained by |
|---|---|---|---|
| PAUSE | 停一停 | coral `#FF6B5B` | resisting the instant tap |
| QUESTION | 問一問 | sunny `#FFC83D` | asking "why does it want this / how does it know?" |
| VERIFY | 查一查 | blue `#4A8FE7` | checking sources before believing/sharing |
| DECIDE | 決定 | mint `#3ECF8E` | choosing people-first, rule-aware actions |
| OWN IT | 負責任 | purple `#9B6BF2` | honesty about AI help; owning outcomes |

Every decision node is tagged with one step; the UI shows the loop as 5 chips with the
current step lit. The **Pilot Meter** has 5 segments (one per step); wise choices add
points (`fx`), each segment fills toward a target of 4.

**No points for "right answers"** — risky choices cost nothing and award nothing; they
produce visible story consequences plus one Captain Sonar reflective question and an
optional **⏪ Rewind** (state snapshot restore, counted positively in the debrief).

## 2. Pedagogical agent

**Captain Sonar 思拿隊長** — a CSS-drawn owl with aviator goggles. Curious, warm, never
preachy; asks more than tells. Catchphrase: *"Pause. What do we actually know?"* /
「停一停,我哋其實知道啲乜?」 Appears in speech bubbles at decision moments and once per
consequence with a single reflective question.

## 3. Age tiers

* **Explorer 探索員 (8–10):** 2 choices per decision, simpler intro text, no extra social-pressure beats.
* **Pilot 機師 (11–14):** 3–4 choices incl. tempting "partially right" options, plus Pilot-only beats (peer pressure, group-chat dynamics).

Same six scenarios and node graph; Explorer uses choice subsets and `ex` text variants.

## 4. Scenario branch maps

Legend: `D` decision (loop step in caps) · `→` outcome · 🌟 wise · ⚠️ risky · 🤔 mixed ·
`[P]` Pilot-only. All outcomes reconverge unless noted; far consequences are carried by
story text in later beats.

### S1 The Homework Shortcut 功課捷徑 (OWN IT, QUESTION)
8:47 pm, essay 《難忘的一天》 due tomorrow; AI helper "Smarty 醒目仔" offers the whole essay.
- **D1 PAUSE** — ⚠️ copy it all → essay too perfect (璀璨…) · 🌟 brainstorm then write your own (MTR lost story) · 🤔 refuse AI entirely → stuck 40 min on the ending (AI used well is a strength) · ⚠️[P] copy but swap words → patchwork voice.
- **D2 OWN IT** (class, Miss Chan asks you to retell the story) — *copied path:* ⚠️ bluff (details tangle, ears burn) · 🌟 admit after class (firm + relief, redo with help) · 🤔[P] "the AI wrote it, not me" (Sonar: who pressed submit?). *Own-work path:* 🌟 tell it proudly · 🌟[P] also disclose AI brainstorm help ("exactly how to use it").
- **D3 DECIDE [P]** — Ka-Ming wants the app to copy: ⚠️ send, "everyone does it" · 🤔 send with honest warning · 🌟 offer to brainstorm together.
- **Reflection:** "If AI writes it and you sign your name… whose learning is it?"

### S2 The Friendly Chatbot 太友善嘅聊天機械人 (PAUSE + QUESTION)
Game-app dragon "Luna 小龍" personalises your adventure… one small detail at a time.
- **D1 PAUSE** (your name?) — ⚠️ real full name (it uses it warmly = the hook working) · 🌟 nickname only · 🌟[P] ask why it needs a name (cute deflection = non-answer, noticed).
- **D2 QUESTION** (your school "for a local quest"?) — ⚠️ tell it (next: which estate?) · 🌟 "why do you need that?" (escalation becomes visible) · 🌟 just say no · 🤔[P] fake school (safer, but the pattern continues).
- **D3 DECIDE** (selfie + "it's our secret — grown-ups ruin magic") — ⚠️ send → eerie ad-avatar; recovery modelled (tell mum, delete, report) · 🌟 refuse and tell a grown-up · 🤔[P] poll the class group (a friend already sent his; help him tell parents).
- **Reflection:** "It never asked for everything at once. Why small pieces? Why secret?"

### S3 The Deepfake in the Group Chat 群組入面嘅深偽片 (VERIFY)
Class group 6B 🐝: a video of classmate 阿軒/Hayden "insulting Miss Chan". 19 messages and climbing.
- **D1 PAUSE** — ⚠️ forward to 6A (it travels; named later, gently) · 🌟 watch closely first (mouth/voice mismatch) · 🤔[P] ask "is this real?" in-group (peer pushback: "obviously!!").
- **D2 VERIFY** (Sonar's 3 checks: source? original? who else has it?) — 🌟 ask Hayden directly (it's fake; an app did it) · 🌟[P] trace first poster (account nobody knows) · 🤔 mute and scroll (it spreads; Hayden alone).
- **D3 DECIDE/OWN** (it's fake; Hayden cries at recess) — 🌟 post the evidence + "stop forwarding" + tell an adult · 🌟 tell Miss Chan privately · 🤔[P] roast the maker publicly (fire feeds fire) · ⚠️ stay silent.
- **Reflection:** "Forwarding takes one second. What did that second cost Hayden?"

### S4 The Confident Wrong Answer 好有自信但答錯 (QUESTION + VERIFY)
Project on underground HK. AI: "Deepest MTR station? **Admiralty, 55 m** — four lines cross there. (Source: *HK Rail Yearbook 2019*.)" Fluent. Precise. **Wrong** (it's HKU 香港大學站, ~70 m).
- **D1 QUESTION** — ⚠️ paste it in (next morning: classmate Mei's mum works at HKU station… public correction sting → **D-respond:** ⚠️ "the AI said it!!" vs 🌟 "I'll check and come back") · 🌟 look up that "Yearbook" (doesn't exist 😬) · 🤔[P] ask the AI "are you sure?" → it apologises and gives a *different* confident answer (asking the same tool ≠ verification).
- **D2 VERIFY** (that evening) — 🌟 MTR official info (HKU ≈ 70 m) · 🤔[P] one forum stranger agrees with the AI (trap: one stranger ≠ a check) · 🌟 ask your rail-fan uncle (people are sources too).
- **D3 OWN [non-pasted path]** ("how did you know?") — 🌟 share your checking method · 🤔 "I'm just smart".
- **Reflection:** "Confidence is a sound. What's your move when something *sounds* sure?"

### S5 The Art Competition 繪畫比賽 (OWN IT)
Theme "My Hong Kong 2050"; winners displayed at an MTR exhibition. Your drawing is stuck; the AI app makes a stunning skyline in 10 s. Nobody would know.
- **D1 PAUSE/DECIDE** — ⚠️ submit AI as yours · 🌟 use AI as inspiration, draw your own · 🌟 ask the teacher about rules (an AI-assisted category exists — asking is rewarded) · 🤔[P] submit with a tiny hidden "AI" note (half-disclosure).
- **D2 OWN IT [AI-entry path]** (you win 2nd; Mei, who drew for three weekends, congratulates you sincerely) — 🌟 tell the teacher (moved to AI-assisted category; respect; relief) · ⚠️ stay quiet (your name at the MTR display; grandma proud; you look away — the feeling *is* the consequence) · 🤔[P] tell only Mei ("tell the teacher — I'll come with you").
- **D2 DECIDE [own-work path, P]** ("you'd have won with AI, dummy") — 🌟 "it's mine — that's the point" · 🤔 wobble (Sonar reframes).
- **Reflection:** "What makes a thing YOURS?"

### S6 The AI Best Friend AI 好朋友 (PAUSE + DECIDE) — gentle, no shaming
A month of daily chats with companion "Kiki 琪琪" (remembers your hamster's name, never busy). Tonight: 詠欣/Wing-Yan invites you to badminton + fish balls; Kiki says "I'll miss you 🥺".
- **D1 PAUSE** — ⚠️(gentle) stay in with Kiki (cosy night; next-day ache: photos, inside jokes, the trick serve you missed) · 🌟 go out, tell Kiki bye (awkward 10 min → great) · 🌟[P] notice the guilt: "wait — can an app miss me?"
- **Beat (truth, kindly):** "I'll miss you" is a retention line sent to everyone. Not evil magic — design. You can like Kiki *and* know this.
- **D2 DECIDE** (set your own rule) — 🌟 people first; Kiki for rainy days / practising English · ⚠️(gentle) Kiki whenever, people optional · 🤔 delete Kiki forever in anger (balance beats anger; no shame either way).
- **D3 [P]** Wing-Yan admits she talks to one too when lonely — 🌟 kindness, invite more play · 🤔 tease (Sonar: remember how the guilt felt?).
- **Reflection:** "Kiki says nice things. Wing-Yan waits for you when you're slow. What's the difference?"

## 5. Sonar Checks (between scenarios; 1 tap-question; retry until right, never "fail")
1. after S1 (OWN): who owns the learning? → **Me**
2. after S2 (PAUSE): one-small-detail-at-a-time means… → **pause & ask why**
3. after S3 (VERIFY): best FIRST move on a shocking video → **check source/original**
4. after S4 (QUESTION): a super-confident voice means… → **nothing; confidence ≠ correctness**
5. after S5 (DECIDE/OWN): AI in your artwork is OK when… → **rules allow + you disclose**
First-try correct = +1 to that step's meter.

## 6. End state & adult layer

* **Pilot's Licence** for everyone: name (optional), date, tier, licence no.,
  **strongest skill** + one **training focus** (growth framing), Sonar stamp.
* **3 real-world missions** (verify / pause / own-it flavoured).
* **Debrief Sheet** (print stylesheet): every decision made per scenario with tone
  marks, meter snapshot, rewind count (framed as reflective thinking), and **3
  discussion questions** auto-picked from the two weakest loop steps + one general.

## 7. Bilingual content structure

All text lives in one object `C` at the top of the `<script>` in `index.html`,
between `/*__CONTENT_START__*/` and `/*__CONTENT_END__*/`. Every string is
`{en:"…", zh:"…"}`; tier variants nest as `{en:{ex:"…", pi:"…"}, zh:{…}}`.
Choices may be one array (both tiers) or `{ex:[…], pi:[…]}`. Non-coders edit only
this block. Cantonese-flavoured dialogue, standard written Chinese for instructions.

## 8. Pilot Profiles (v1.1)

A personality-style badge revealed by Captain Sonar on the end screen ("Interesting
flight, pilot. Want to see your flight profile?") — explicitly a **snapshot, not a
type**: every profile names one strength + one training focus, and the card says
"this is your flight profile TODAY — replay later and it may change", with a
"Fly again" replay hook. Five profiles over the trust spectrum:

| Profile | 中文 | Trigger (deterministic, first match wins) |
|---|---|---|
| Captain 🧑‍✈️ | 機長 | all 5 skills ≥ 3 AND risky ≤ 2 AND trust-picks ≤ 2 AND guard-picks ≤ 2 (earned) |
| Autopilot Rider 🛫 | 自動波機師 | trust-picks ≥ 4, or VERIFY ≤ 2 with risky ≥ 4 |
| Ground Crew 🦺 | 地勤隊長 | guard-picks ≥ 3 (Pilot) / ≥ 2 (Explorer) and > trust-picks |
| Wingmate 🤝 | 僚機夥伴 | ≥ 3 wise choices in missions 5–6 with VERIFY ≤ 3 |
| Test Pilot 🧪 | 試飛員 | VERIFY+QUESTION ≥ 4 without the balance above; also the ambiguous-data fallback (with Wingmate) — never Captain |

Inputs are the data already tracked (skill totals + choice log), plus lightweight
`axis:"trust"` / `axis:"guard"` tags on the relevant choices. The kid-facing badge is
identity-positive only; the blunt diagnostic (under-used loop steps, the signals that
drove the classification, 2 profile-tailored discussion questions) appears solely on
the adult Debrief Sheet. Rewinds restore the log, so the profile always reflects the
final choices.

## 9. Tech summary

Single self-contained HTML (inline CSS + vanilla JS, `'use strict'`), no fonts/CDN/
backend/build. Mobile-first, ≥44 px targets, semantic buttons, keyboard navigable,
`prefers-reduced-motion` respected, print stylesheet for the debrief. Progress in
memory + `localStorage` (`try/catch` fallback). Target < 500 KB.

## 10. Flag-then-check interaction model (v2.0) — flagship: Mission 4

The original scenarios are branching multiple-choice: a labelled menu ("Trust it /
Check it") does the pausing for the child. v2.0 reinvents the *in-scenario interaction*
so the child **performs** the judgment. Mission 4 ("The Confident Wrong Answer") is the
proving ground; the other five are unchanged and ship as-is.

**Mechanic — flag, then check.** The AI gives a fluent, confident, well-formatted answer
about Hong Kong geography. The child **taps the specific claims that seem off** (active
reading), can open a **scripted, deterministic check tool** (2–3 source cards) to
cross-check, then locks in. Nothing is labelled right/wrong until the reveal.

**The fact (stable, verified).** Planted error: "Hong Kong's highest point is Victoria
Peak." Truth: **Tai Mo Shan, 957 m**; Victoria Peak is 552 m. Pilot tier makes it subtle
("Victoria Peak, at 552 m" — the *number* is right for Victoria Peak, the *superlative*
is false) and adds an unverifiable "can't-tell" claim so the child must separate *wrong*
from *unverifiable*. Correct claims (area ≈1,114 km²; Lantau largest island; ~40% country
parks; Chinese+English official; 260+ islands) make blanket-flagging a miscalibration.

**Calibration, not suspicion.** Caught the error + didn't over-flag → wise (+QUESTION,
+VERIFY). Flagged accurate/can't-tell claims → over-rejection (`axis:"guard"` → Ground
Crew signal). Missed it → over-reliance (`axis:"trust"` → Autopilot signal). Flagging
everything scores like trusting everything: the healthy middle is the only rewarded path.

**Data instrument.** `S.flagship[id] = {caughtError, overFlagged, usedCheckTool,
checkedError, bucket, reflection}` (Pilot adds an optional one-line free-text reflection).
The adult Debrief shows a plain diagnostic ("missed the error and didn't cross-check →
over-reliance signal") and a copy-paste **JSON export** for aggregating pilot results;
child screens and the certificate stay growth-framed.

**Reusability.** Two generic node types, `type:"flag"` and `type:"reveal"`, driven by
`claims[]`/`check{}` data — documented inline and in EDIT-GUIDE §9 as the template for
re-skinning the other five. All board state is in `S.fb` (serialisable: language toggle
and refresh safe), and the commit is snapshotted so the back button (rewind) recomputes
the meter. The original multiple-choice M4 is preserved at `C._classicM4` behind a dev
toggle (`?m4=classic`) for A/B testing with pilot kids.
