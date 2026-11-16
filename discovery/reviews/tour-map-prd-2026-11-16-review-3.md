# PRD Review: Tour Map (draft 3, revision 2)

- **PRD:** `discovery/tour-map-prd.md` (Status: "Draft for Discovery review (revision 2)", 2026-11-16)
- **Reviewed:** 2026-11-16 by prd-critic-agent. This is the third review today. Earlier reviews: `tour-map-prd-2026-11-16-review.md` and `-review-2.md`, read for context only.
- **Upstream inputs traced:** `ideation/tour-map.md` (v2 brief, approved), `ideation/market-research.md`, `ideation/decision.md`, plus `ideation/reviews/synthesis-2026-11-16-2.md`, which `decision.md` refers to.
- **Rubric:** `context/rubrics/prd-rubric.md` + `prd-standards.md`. The universal corrections file has no entries. There is no project-local corrections file (`memory/corrections/` holds only `.gitkeep`).
- **Method:** I scored every criterion fresh. The Substance gate uses new samples. Draft 1 traced the coverage notice and the comparison design. Draft 2 traced artist matching and "terms before Build". None of those is reused here.

## Result

| | Score |
|---|---|
| Raw total | **91/100** (band: Ship) |
| Execution readiness | **7/10** |
| Substance gate | **4/10** |
| **Final verdict** | **Revise** |

> Substance gate: verdict capped at Revise — sampled trace failed (the brief's risk that venue
> coordinates must be "present and correct" is supported only for "present". R18 says "The filter
> exists to catch bad coordinates", but it catches only null, 0,0 and outside-the-UK values. A pin
> placed in the wrong UK town passes R2, G3, R18 and the R25 step 1 eligibility check. R25 then
> scores testers' answers against distances "to each venue" without saying whose coordinates are
> used) despite a raw total of 91/100.

> Gate override: verdict capped at Revise — Substance gate scored 4/10 despite a raw total of 91/100.

Root-cause gate (Problem clarity ≤ 6): did not fire (11/15). Execution readiness gate (≤ 4): did not fire (7/10).

**What changed since review 2.** Every review-2 finding has been fixed in the text:
- R3 now gates group B: "R3 completes before any ticket in group B starts".
- R1 now tests the attractions search with full and partial names.
- R25 AC3 now has an end point: "Postponement ends at the G1 timeframe … close the experiment as inconclusive", and §4 says "An inconclusive close counts as G1 not met".
- R7 now specifies the attraction lookup, its 404 rule and its "2 upstream calls" cost.
- R6 now returns `truncated`.
- R9 now says "every upstream call counts … including both R7 calls and E-R10 retries".
- R16 now states how cancelled events are counted.
- E-R6b is now labelled `[Assumption]`, K1 carries "unverified" again, and the OSM rationale now matches the research.

The cap this time comes from a new sample, not from an old finding left unfixed.

**Gate thresholds in the rubric are inconsistent (carried from review 2).** `prd-rubric.md` line 183 still says "Problem Clarity ≤ 3" and "Execution readiness gate (≤ 3)". That conflicts with the gate sections above it and with `prd-standards.md` (≤ 6, ≤ 4, ≤ 4). I applied the standards' thresholds. Neither gate is near either threshold, so this does not change the verdict. It should go to `retro`.

---

## Criterion scores

### 1) Problem clarity — 11/15
The problem is specific and kept separate from the solution. The evidence that it is painful is still one person's experience, which the PRD states openly. This fits the "thin on evidence" anchor. The screener question and the G1/G6 results are planned work, not evidence yet.
- "It does not answer the question a fan deciding where to go actually has: **"where in the UK are they playing, and which date can I realistically get to?"**"
- "The evidence for pain is the product owner's own repeated experience (n=1)"

### 2) Target user + context (JTBD) — 10/10
The PRD names a segment, gives a scenario and a job-to-be-done. It drops the gift-buyer segment, which `decision.md` allows ("treat as secondary or drop").
- "**Primary segment: UK gig-goers deciding which date of a specific musician's UK tour to attend.**"
- "**Job-to-be-done:** "When a tour is announced, help me see at a glance where in the UK it goes, so that I can pick the date I can actually reach…""

### 3) Evidence quality — 10/10
I spot-checked the PRD's citations against `market-research.md`. All of these match, and each keeps its "unverified" label where the research gives one:
- 894 domains
- 5 of 12 arenas
- 58% / 66%
- £7.6bn / £318
- YouGov 10%
- MapTiler suspension instead of charging, and its non-commercial clause
- size × page below 1,000
- partial words don't match on events
- the International API not taking new keys
- the Google 2016 gesture blog
- the clustering sources

The three review-2 sourcing faults are fixed. R4 now cites `synthesis-2026-11-16-2.md`, and fixes 4–5 there match what R4 does. Assumptions are labelled throughout (A1–A11, plus inline `[Assumption]` tags).
- "No automatic filtering of tributes. `[Assumption]` the API has no reliable tribute flag; the research did not cover this."
- "Medium (5 of 12 major arenas `[Evidence: market-research.md, unverified]`)"
- "because its policy requires an identifying User-Agent and Referer, needs permission for heavy use, can block without notice and recommends running your own tile server"

### 4) Success metrics — 15/15
Each goal has a KPI, a baseline, a target, a timeframe and a measurement method. The brief's success statement maps onto G1, G6 and G7. The "real ticket decisions" part of the brief's validation path maps onto G4.
- G1: "Tour Map median ≥30% lower than **each** comparator; Tour Map faster than both comparators for ≥4 of 5 testers; Tour Map correct-answer rate no lower than either comparator | Test run within 4 weeks of the MVP going live"
- "G4: the product owner's own dated log (date, artist, decision made), kept in the repository."

### 5) Scope + non-goals — 12/12
The in-scope list is crisp. Non-goals are explicit in §9, and each deferral in §8 has a reason and matches the brief and `decision.md`.
- "**No data source other than the Ticketmaster Discovery API**, and no scraping `[Decision: ideation/decision.md, brief]`"
- "Each of these is deferred deliberately. None starts unless G1 is met."

### 6) Requirements quality — 11/15
Requirements are numbered, mostly constraint-based, and now include the R7 lookup and the R6 `truncated` flag. Two still leave the person building them guessing:
- **R13 Option S contradicts its own AC.** The requirement says "at least 44 CSS px between neighbouring pin centres". AC4 requires that "no two pins' 44×44 CSS px tap targets overlap" and that "each pin's centre is within 100 CSS px of the venue's true position". Two 44 px squares whose centres are 44 px apart on a diagonal still overlap; they need about 62 px between centres. On a single ring, 62 px spacing pushes the radius past 100 px at about 10 dates. Even 44 px spacing does so at 15 dates. So for a multi-night residency of 10 or more nights, AC4 cannot pass as written. This only bites if Q2 is answered "Option S".
- **R7's two calls against R9's 2-second refusal.** R9 says "If it has not started its upstream call 2 seconds after it arrived, it is **refused**". R7 makes two calls in sequence: "First, the Discovery attraction lookup … Second, the events endpoint". The PRD does not say whether the 2-second rule applies to the second call. It also does not say what the endpoint returns if the lookup succeeds and the events call is then refused. A builder has to guess, and the first call has already been spent.
- Fix:
  - For Option S, state the minimum spacing as non-overlapping 44×44 boxes, and either let the ring grow (drop the 100 px bound for N above a stated number) or use more than one ring.
  - State that R9's queue applies per R7 request (both calls) or per call, and what is returned if the second call is refused.

### 7) Acceptance criteria — 10/10
Every major requirement has a pass/fail test, including the process requirements: R1 AC1–AC3, R2 AC1–AC3, R22 AC3's timed drill, and R25 AC1–AC4. Some sub-clauses still have no test. These are optional, non-blocking fixes: R14's 0-pin case and maximum zoom 12; R15's "Sat 14 Mar 2027" format; R23's "no stored IP addresses or search terms"; and E-R10's single retry.
- "**AC4:** Given the counter is at 4,500 and the server clock passes 00:00 UTC (tested with an injected clock), when an uncached request arrives at 00:00:01 UTC, then the upstream call is made and the counter reads 1."

### 8) Edge cases + failure states — 8/8
The PRD covers errors, empty states, permissions, limits, timeouts and retries, each with exact on-screen wording.
- "E-R10 | Upstream timeout (8s), 5xx or 429 | One retry after 1 second, counted by R9 like any other call."
- "E-R19b | Location permission | Not requested."
- "E-R18 | Every event is unplaceable | The map shows the UK framing with no pins."

### 9) Dependencies + risks + assumptions — 4/5
Dependencies, risks K1–K14, assumptions A1–A11 and open questions Q1–Q6 are all explicit. One upstream risk loses half its content on the way in. The brief says "**Venue location accuracy:** relies on Ticketmaster's venue coordinates being present and correct." The PRD's version is only "K8 | UK venues lack coordinates". No risk, assumption or open question covers coordinates that are present but wrong. That makes it a hidden assumption, which is the "incomplete" anchor.
- Fix: widen K8 (or add a K15/A12) to cover wrong-but-plausible coordinates, with the mitigation from the Substance fix below.

**Total: 11 + 10 + 10 + 15 + 12 + 11 + 10 + 8 + 4 = 91/100**

---

## Execution readiness — 7/10
Broadly buildable. Endpoints, parameters, response shapes, UI wording, limits, sequencing and stop rules are concrete. Q2 and Q6 are open, but each has a default or a deadline. Hosting is deferred to Design ("Chosen at the Design gate"), which is legitimate. An engineer would still ask three things:
- how R9's 2-second refusal applies to R7's second call;
- how Option S should lay out a large residency;
- for R25, which coordinates "straight-line distance from the named town to each venue" uses.

These are minor clarifications, so 7, not 10.

## Substance gate — 4/10 (scored last)

**Trace 1 — the brief's Risks: "Venue location accuracy: relies on Ticketmaster's venue coordinates being present and correct."**
The "present" half traces well, end to end:
- R1 records "whether venue `location.latitude` and `location.longitude` are present".
- R2 counts "events without usable coordinates".
- G3 targets "≥95%".
- R7 AC2 parses string coordinates to numbers. The research says the v1 docs describe them as strings and a third-party sample shows numbers, so this is grounded.
- R18 handles "null, non-numeric, exactly 0,0, or outside the UK bounding box".
- K8 and E-R18 cover the risk and the empty state.

The "correct" half is named but not supported:
- The PRD claims the job is done: "The filter exists to catch bad coordinates, not to decide what counts as the UK." But the filter catches only gross errors. A venue geocoded to the wrong UK town, or to a city centroid, passes it.
- No other step looks at where a pin actually sits. R2 compares dates against the official page and counts missing coordinates. G3 measures "valid venue coordinates (placed as pins)". R25 step 1 matches the three date sets "by date and town", and the town is the text `city` field, not the pin's position.
- This hits the experiment directly. R25 step 4 fixes each correct answer "by straight-line distance from the named town to each venue in that session's verified date set" without saying whose coordinates are used:
  - If it uses Ticketmaster's coordinates, a wrong pin makes the "correct" answer wrong. A list-condition tester who knows real geography is then marked down.
  - If it uses real venue locations, a Tour Map tester who trusts the pin is marked down, against G1(c).
  - Either way G1 can measure data error rather than map UX. That is the same kind of confound K2 calls "**Strongest case against this plan**". K2's mitigations control which dates are shown, but not where they are shown.
- `market-research.md` says the risk is unexamined: "Whether UK venues carry populated coordinates: no sourced evidence found". It has nothing at all on accuracy.

The brief's risk is restated, not supported. That is the 4 anchor.

**Trace 2 — the brief's validation path: "Pick 3–5 real UK tours … asking them to find a given date on this map and on Bandsintown's. If the map-first view isn't clearly quicker than both the list and Bandsintown's map, stop before investing further. Check early how many of those tours Ticketmaster covers in full."**
This traces end to end:
- "Check early" is R2, sequenced before groups C–E. It has a stop default decided in advance: "The default outcome is **stop**".
- "Real UK tours" are R2's up to 10 tours, beginning with the brief's example tour, and become R25 candidates.
- "Find a given date … on this map and on Bandsintown's" is R25. It has three conditions with rotation, and towns chosen so the answer is unambiguous and past dates don't penalise the list condition.
- "Clearly quicker than both" is G1, which tests (a)–(c) against "**each** comparator".
- "Stop before investing further" is §4's rule ("If G1 fails: no post-MVP work (§8) starts") and §8 ("None starts unless G1 is met").
- The end state that failed in review 2 is now closed: "An inconclusive close counts as G1 not met".
- "Real ticket decisions (my own and gifts)" is G4.

This holds up against all three inputs.

**Falsify — the strongest concrete argument that the PRD is wrong:** each tester does "every tour in every condition". After the first condition, the tester already knows where that tour's venues are. Later conditions are then faster whatever the interface, and Tour Map's whole claimed advantage is giving the tester exactly that picture of the tour. Rotation is the standard control, and the PRD has it ("Condition order, and which town goes with which condition, both rotate between testers"; K2's mitigation repeats it). But 5 testers cannot balance 6 orders of 3 conditions, so at least one condition goes first only once. G1(b) is judged per tester, so whichever tester happens to get Tour Map first or last can decide whether 4 of 5 is reached. The PRD never names this carry-over effect. It reduces it (a different town for each condition, rotation) but does not answer it. Examples of an answer: a different tour for each condition, or a stated rule that the analysis compares only first-condition trials across testers. So the counter-argument is partly anticipated, not resolved.

**Score.** Trace 2 traces cleanly. Trace 1 does not: the brief's "correct" half is claimed by R18's wording but not supported anywhere, and it feeds straight into the validity of G1. The strongest counter-argument is only partly answered. That is below the 7 anchor ("Sampled items trace cleanly"). Under "score down when in doubt" it is 4, so the gate fires.

---

## What to fix, in priority order (Revise)

1. **Substance gate (caps the verdict).** Support the "correct" half of venue location accuracy. For example:
   - (a) In R2 and R25 step 1, check that each pin lies within a stated distance of the venue's named town, and make "every pin within N km of its town" an R25 eligibility condition.
   - (b) In R25 step 4, state which coordinates the correct-answer distances use.
   - (c) Widen G3 or K8 to cover wrong-but-plausible coordinates.

   Optional but cheap: name the carry-over effect in K2 and state how R25 limits it.
2. **Criterion 6, Requirements quality (−4).**
   - Make R13 Option S's spacing rule match AC4 (non-overlapping boxes need about 62 px diagonally), and remove the 100 px conflict for large residencies.
   - State how R9's 2-second queue applies to R7's two calls, and what is returned if the second call is refused.
3. **Criterion 1, Problem clarity (−4).** The pain evidence is still n=1. It can't be fixed before R25 runs. Not blocking, since the root-cause gate did not fire.
4. **Criterion 9, Dependencies + risks + assumptions (−1).** Add the wrong-coordinates risk or assumption, as in item 1(c).

This was the last automatic revision round, so this review goes back to the product owner. The Substance finding takes a few lines to fix. It is still a real gap in the inputs to the experiment, not a wording issue.
