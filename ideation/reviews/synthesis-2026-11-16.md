# Idea Council Synthesis: Tour Map

- **Date:** 2026-11-16
- **Step:** idea-synthesis-agent (compiles the four lenses; does not score anything itself)
- **Inputs:** `ideation/reviews/feasibility-2026-11-16.md`, `ideation/reviews/market-2026-11-16.md`, `ideation/reviews/uservalue-2026-11-16.md`, `ideation/reviews/risk-2026-11-16.md`
- **Standards:** `context/rubrics/idea-standards.md` (plugin)
- **Corrections read:** universal `context/corrections/idea-synthesis-agent.md` (no entries yet); project-local `memory/corrections/idea-synthesis-agent.md` (does not exist yet)

> **This is not the decision.** The verdict below is a suggestion derived mechanically from the standards. The product owner decides, having read all four lens reviews as well as this readout.

---

## 1. Lens subtotals side by side

| Lens | Criterion | Score | One-line justification (from the lens review) |
|---|---|---|---|
| Feasibility | 1.1 Technical feasibility | 10 / 15 | Map, pins and ranking are standard; the whole MVP rests on untested "extract tour dates from any URL", which the brief raises but doesn't answer. |
| | 1.2 Resourcing feasibility | 7 / 10 | Solo side project with AI assistance is plausible, but no timeline and no statement of the builder's skills. |
| | **Subtotal** | **17 / 25** | |
| Market / Competitive | 2.1 Competitive landscape awareness | 5 / 15 | The "artist-first map" differentiator is contradicted by the research (Bandsintown artist-page event map, Tour Radius, MusicMap and others); none are named in the brief. |
| | 2.2 Market timing / signal | 7 / 10 | Sourced UK live-music market and ticket-gifting figures, but nothing for this specific job or the remote gift-buyer segment. |
| | **Subtotal** | **12 / 25** | |
| User-Value / JTBD | 3.1 Job-to-be-done strength | 10 / 15 | The job ("is there a show I can realistically get to?") is real and stated before the solution. Pain and frequency rest only on the author's own experience, and UK-only scope leaves out the cases where it hurts most. |
| | 3.2 Value differentiation vs. status quo | 7 / 10 | A believable reason to switch from reading a list. The case against the named competitors rests on a description of them that the research contradicts. The lens calls this score "generous". |
| | **Subtotal** | **17 / 25** | |
| Risk | 4.1 Named risks | 10 / 15 | Technical risks (extraction, geocoding) and a legal risk (scraping terms) are named and specific. No business or adoption risk is named, and the competitive risk is asserted away. |
| | 4.2 Reversibility / cheap validation path | 7 / 10 | Validation needs are recognised in the open questions, but no concrete cheap step is proposed. Low reversal cost comes from circumstance, not from the plan. |
| | **Subtotal** | **17 / 25** | |

## 2. Total

**17 + 12 + 17 + 17 = 63 / 100**

---

## 3. Substance gate: every lens, and the council minimum

| Lens | Substance | Sampled trace(s) that failed |
|---|---|---|
| Feasibility | **4 / 10** | Trace 1: "no dependency on a third-party events API or its licence". The research shows tour pages are mostly Bandsintown, Songkick or Seated widgets, and Bandsintown says "not to scrape data". The dependency is moved, not removed, and the dates may not even be in the fetched HTML. (Trace 2, "mapping and geocoding are cheap", is asserted with no support.) |
| Market / Competitive | **4 / 10** | Trace 1: "Bandsintown and Songkick are location-first and show nothing when the artist isn't local". The research shows a Bandsintown artist-page event map of the whole tour since 2022. (Trace 2, the remote gift buyer, is only partly supported.) |
| User-Value / JTBD | **4 / 10** | Trace 1: same positioning claim ("simply show nothing"), contradicted by the Bandsintown event map. Trace 2: the gift buyer as "the case where list views fail worst" has no supporting evidence, and the brief doesn't engage the voucher alternative. |
| Risk | **4 / 10** | Trace 1: same data-approach claim as Feasibility (licence moved, not removed; the long-term API key is also tied to a single artist). Trace 2: same positioning claim as Market and User-Value. |

**Council Substance score: 4 / 10** (the lowest; here every lens scored 4).

**The gate fires (≤ 4).** Two separate claims in the brief failed traces against `ideation/market-research.md`:
- **The data-approach claim** ("scraping removes third-party/licence dependency") failed in the **Feasibility** and **Risk** lenses.
- **The positioning claim** ("incumbents are location-first and show nothing") failed in the **Market**, **User-Value** and **Risk** lenses.

Both are load-bearing. One is the reason given for the MVP's architecture, and the other is the brief's main differentiator.

---

## 4. Disagreements and tensions between lenses

This is the main content of the synthesis. No scores are changed here. Each item names a place where one lens's finding undercuts another lens's score.

**D1. Feasibility 1.1 (10/15) vs. Feasibility's own Substance trace, Market and Risk on scraping terms.**
Feasibility scores 1.1 at 10 because extraction is "a question of effort and coverage, not whether it can be done at all", with per-widget parsers given as a viable route. Market's counter-argument and Risk's Trace 1 point out that at least one of those widget providers (Bandsintown) says not to scrape its data. Feasibility's own Substance trace says the same thing ("at least one of them forbids it"). So the route that justifies 1.1 is the route other findings say may not be allowed. Whether licence permission belongs in "technical feasibility" is a reading of the rubric. Either way, 1.1 is credited for a path that three findings, including Feasibility's own, call into question.

**D2. Market 2.1 (5/15) vs. User-Value 3.2 (7/10): the same evidence, scored at different severity.**
Both lenses rest their score on the same contradiction (the Bandsintown artist-page map and Tour Radius against "simply show nothing"). Market took the lower anchor ("differentiation isn't tied to a real alternative"). User-Value gave 7 and says in its own text that this is "generous", because the rubric treats a section that is contradicted by its source as missing. User-Value kept the points for the comparison against "do nothing / read the list". The two criteria measure different things, so this is not necessarily an error. But the competitor finding cost Market about two-thirds of 2.1 and cost User-Value much less of 3.2. The product owner should know the 17/25 User-Value subtotal leans on a score its own reviewer flagged as generous.

**D3. User-Value 3.1 (10/15, job is real) vs. Market and Risk (the job is already largely served).**
User-Value credits the job as real and stated independently of the solution, which is why the root-cause gate does not fire. Market's counter-argument and Risk's falsification argue that the core job is already done, free, by Bandsintown and the surfaces using its data (Apple Music, Maps, Shazam). That leaves only "distance from a location the user types in" for remote gift buyers, and that slice has no sourced evidence. User-Value's own counter-argument makes the same point. This doesn't contradict 3.1, which measures job strength rather than whether the job is already served. It does mean the job that remains unserved is much narrower than the one 3.1 credited.

**D4. User-Value's scope finding not reflected in Feasibility or Risk.**
Only User-Value noticed that the brief's pain conditions ("spans countries", "foreign legs") fall outside the UK-only first version. Feasibility and Risk score the UK-first MVP without considering that it may leave out the cases where the job hurts most. Risk 4.1 doesn't count this as an unnamed adoption risk, though it fits under the gap Risk did flag (no business or adoption risk named).

**D5. The side-project framing is treated differently by each lens.**
- Feasibility 1.2 gives credit for a solo builder with no revenue target being a plausible setup.
- Risk 4.2 explicitly refuses credit for low reversal cost "by circumstance" and scores only the validation path the brief proposes.
- User-Value adds a note, without changing its score, that a side project may lower the bar for "someone would change behaviour".
- Market 2.2 quotes "not a bet on market timing" and still scores the signal as written.

These choices aren't inconsistent, since each follows its own criterion. But the product owner should know the council didn't apply one shared view of how much the hobby framing should count.

**D6. The geocoding claim was examined by one lens only.**
Feasibility's Trace 2 found "mapping and geocoding are cheap" unsupported (no provider, no cost, no limits). Risk 4.1 gave credit for naming venue-location accuracy as a risk but didn't test the cost claim. No other lens examined it. It is a small gap, but it is a claim the brief's "why now" depends on.

**D7. The long-term API fallback is undercut, and no criterion scores it.**
Risk's Trace 1 notes that a Bandsintown API key "is tied to a single artist unless Bandsintown authorises otherwise", which weakens the brief's longer-term API direction. Feasibility doesn't mention this, and no criterion score reflects it. If scraping is ruled out on licence grounds (D1), the obvious fallback also has a constraint nobody accounted for.

**Agreement worth noting.**
Feasibility and Risk independently proposed the same cheap next step: sample about 20–30 real UK artist tour pages and record how each publishes its dates and whether a plain fetch returns them. Risk adds a second cheap check: whether the existing Bandsintown artist-page map already answers the gift-buyer question.

**Caveat on correlated evidence.**
All four Substance failures trace back to one input, `ideation/market-research.md`, and mostly to two items in it: the widget-prevalence and terms evidence, and the Bandsintown event-map entry, which is sourced to a changelog "dated 2022". The research itself marks Tour Radius's live status as unconfirmed. Four lenses agreeing here is less independent than it looks. If the Bandsintown map entry turned out to be stale or wrong, the positioning failure in three lenses would weaken together. The data-approach failure doesn't depend on that entry. This doesn't change the gate, but re-checking that entry is a cheap way to make the result more robust.

---

## 5. Gates

| Gate | Fired? | Basis |
|---|---|---|
| Root-cause gate (3.1 ≤ 5, **or** User-Value subtotal ≤ 10/25) | **No** | 3.1 = 10/15 and User-Value subtotal = 17/25. Confirmed by the User-Value reviewer's explicit flag ("Root-cause gate triggered by this lens: No"). |
| Substance gate (council minimum ≤ 4) | **Yes** | Council minimum is 4/10, and every lens scored 4. The failed traces are named in section 3. |

---

## 6. Suggested verdict (a suggestion, not a decision)

- **Band from total alone:** 63/100 falls in **45–64: Refine & re-submit**. It is two points below the "Pursue with caveats" threshold of 65.
- **Substance cap:** the Substance gate fired, so the suggestion is capped at "Revise" whatever the total. `idea-standards.md` names the cap "Revise", but the Ideation band table has no "Revise" band. The nearest equivalent is "Refine & re-submit", which is how this synthesis reads it. The cap doesn't change the band here, but it applies on its own grounds. Even if the scores moved two points upward on re-review, the suggestion would stay at Refine & re-submit until the two failed claims are fixed in the brief.

**Suggested verdict: Refine & re-submit** (from the total band, and separately from the Substance cap).

**What re-submission would need to address, drawn from the lenses:**
1. Restate the data approach to match the widget and terms evidence. Feasibility and Risk suggest sampling about 20 UK tour pages first.
2. Rewrite the positioning against the alternatives that actually exist (the Bandsintown artist-page map, Tour Radius and others). State the narrower differentiator directly, if it survives: distance from any location the user enters, for remote gift buyers. Also address the voucher alternative.
3. Name the business and adoption risks, including competitor overlap and the extra step of pasting a URL.
4. Propose at least one concrete, cheap validation step.
5. Consider whether a UK-only first version leaves out the pain cases the brief itself puts forward.

**The decision is the product owner's.** The ideation skill must present all four lens reviews alongside this readout. The project's side-project, no-revenue framing (flagged in the User-Value review) is a legitimate factor the owner may weigh differently from how the rubric does.

---

## Process notes

- The Market lens flagged that rubric criterion 2.1 names its source as `idea/market-research.md`. The file that exists is `ideation/market-research.md`, so this looks like a typo in the rubric. It is passed on here as a possible `retro` item.
- The Substance cap is labelled "Revise" in `idea-standards.md`, which doesn't match the Ideation band names (see section 6). This is also a possible `retro` item.
- No project-local corrections existed for this agent, and nothing was appended. Entries are added only when a human corrects a synthesis.
