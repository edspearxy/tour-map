# PRD Review: Tour Map (draft 1)

- **PRD:** `discovery/tour-map-prd.md` (Status: "Draft for Discovery review (first draft)", 2026-11-16)
- **Reviewed:** 2026-11-16 by prd-critic-agent
- **Upstream inputs traced:** `ideation/tour-map.md` (v2 brief, approved), `ideation/market-research.md`, `ideation/decision.md`
- **Rubric:** `context/rubrics/prd-rubric.md` + `prd-standards.md`. Universal corrections file read (no entries). No project-local corrections file exists.

## Result

| | Score |
|---|---|
| Raw total | **81/100** (band: Ship with fixes) |
| Execution readiness | **7/10** |
| Substance gate | **4/10** |
| **Final verdict** | **Revise** |

> Substance gate: verdict capped at Revise — sampled trace failed (the side-by-side comparison
> design carried from `decision.md` contradicts itself: R2 AC2's override path runs R25 on
> Ticketmaster-partial tours, which R25 step 1 and K2 rule out and which re-introduces the
> completeness confound against the official-list comparator; and E-R13's "Measured as part of
> R25" has no measurement behind it in R25) despite a raw total of 81/100.

> Gate override: verdict capped at Revise — Substance gate scored 4/10 despite a raw total of 81/100.

Root-cause gate (Problem clarity ≤ 6): not triggered (11/15). Execution readiness gate (≤ 4): not triggered (7/10).

---

## Criterion scores

### 1) Problem clarity — 11/15
Specific and clearly separated from the solution, and honest about its evidence, but the pain rests on one person's experience. That is the "thin on evidence" anchor, not "evidence-backed".
- Specific: "It does not answer the question a fan deciding where to go actually has: **"where in the UK are they playing, and which date can I realistically get to?"**"
- Evidence base: "The evidence for pain is the product owner's own repeated experience (n=1)" and "No research compares map and list usability for this task".
- Separated from solution and from novelty: "a map-first tour view is **not a new idea**, and this PRD makes no novelty claim."

### 2) Target user + context (JTBD) — 10/10
Named segment, a concrete scenario and a JTBD. The brief's unevidenced gift-buyer segment is dropped, as `decision.md` allowed ("treat as secondary or drop").
- "**Primary segment: UK gig-goers deciding which date of a specific musician's UK tour to attend.**"
- "**Job-to-be-done:** "When a tour is announced, help me see at a glance where in the UK it goes, so that I can pick the date I can actually reach…""
- "**Gift buyers: dropped as a target segment for the MVP.** `[Decision]`"

### 3) Evidence quality — 7/10
Labelling is disciplined and the citations I checked match `market-research.md` (Seated 894 domains, 5 of 12 arenas, 58%/66%, MapTiler suspension, synthesis fixes 4–5). The PRD breaks its own stated rule in places, though: "This PRD keeps that label wherever it relies on one".
- R10 states as fact "Google Maps Platform, because it needs a billing account". The research says "Billing account required (not stated in the summary; verify)". The PRD drops the unverified flag on a point it uses to reject an option.
- R25 step 5: "the tester's own phone, because the job happens on phones (§3)". §3 says "on a phone or laptop", so the justification has no source and no `[Assumption]` label.
- Fix: put "unverified" back on the Google billing claim, and either label the phone-only premise an assumption or make it consistent with §3.

### 4) Success metrics — 11/15
G1–G5 each give a KPI, baseline, target, timeframe and measurement method ("Tour Map median ≥30% lower than **each** comparator; Tour Map faster than both comparators for ≥4 of 5 testers…"). Against the brief, though, the core success definition is only partly measured:
- Brief: "a fan can identify the nearest or most convenient date **within seconds** … people find this one quicker **and easier to navigate**."
- G1 measures relative speed and correctness only. No metric or target covers "easier to navigate" (no post-trial preference or ease rating in R25), and none covers the absolute "within seconds".
- Fix: add a per-trial ease or preference measure to R25 and G1, or record a decision that the brief's "easier" is dropped. State whether an absolute time target applies.

### 5) Scope + non-goals — 12/12
There is a crisp in-scope list (R1–R26 grouped by area), an explicit §9 non-goals list and a stated v1 boundary. Deferrals match the brief and decision.md (postcode distance, affiliate links, other sources).
- "**No data source other than the Ticketmaster Discovery API**, and no scraping"
- "**No clustering** (R13)." / "Each of these is deferred deliberately. None starts unless G1 is met (§4)."

### 6) Requirements quality — 11/15
The requirements are numbered, mostly constraint-based and testable, and each is sized to one tracker issue. A few are vague or underspecified in ways that matter:
- R11 "bounds **roughly** 49.8°N 8.7°W to 60.9°N 1.8°E". R18 then uses this as a hard filter: "outside the UK bounding box in R11" makes an event unplaceable. A hard filter can't use "roughly".
- R9 "excess requests are queued for up to 2 seconds, **then refused**". The status code, body and browser behaviour for a refused request are not defined anywhere, and §7 E-R9 covers only the daily limit and upstream 429.
- R19 is compound and its outcome is untestable: "One-finger pan and pinch-zoom work on touch screens."
- R13 depends on an unresolved owner decision: "Does the product owner accept grouping same-venue dates into one pin … as meeting "a pin per tour date"?" (Q2).
- Fix: give exact bounds. Specify R9's refusal response and UI. Split R19 into gestures with observable outcomes. Close Q2.

### 7) Acceptance criteria — 7/10
Every requirement has Given/When/Then criteria and most are sharp (R5 AC1, R13 AC1, R17 AC2). Some major requirements lack a real pass/fail test:
- R19 AC1: "pan, pinch-zoom, pin tap and the R15 bottom sheet all **work**". This is a "should work" criterion on the phone behaviour that the brief makes a core experiment topic ("how the map reads on a phone").
- R22, the 24-hour removal duty carried from `decision.md`, says "The steps take no more than 30 minutes in total", but its ACs test only the denylist's effect. Nothing tests the timing or the runbook, which is the compliance requirement.
- R9: the 2-second queue/refusal and the 00:00 UTC daily reset have no AC.
- Fix: give R19 measurable criteria (for example, no `scroll` event on `document` during a pan gesture). Add a timed runbook-drill AC to R22. Add queue/refusal and reset ACs to R9.

### 8) Edge cases + failure states — 8/8
Errors, empty states, permissions, limits, timeouts and retries are all covered: "E-R10 | Upstream timeout (8s), 5xx or 429 | One retry after 1 second… No automatic retry loops." "E-R19b | Location permission | Not requested." "E-R18 | Every event is unplaceable". Non-blocking nit: the E-R10 copy "Couldn't load tour dates from Ticketmaster just now." is also what a failed artist *search* shows, and it is wrong for that context.

### 9) Dependencies + risks + assumptions — 4/5
The dependencies, risks (K1–K12), assumptions (A1–A7) and open questions (Q1–Q6) are thorough. One dependency that the core experiment rests on is hidden: R2's "fully covered" status is a pre-build snapshot of "current or announced UK music tours". R25 runs after build, with "Product owner time … Side-project time, no committed schedule". Nothing names the risk that test tours finish, or that their coverage changes, before R25 runs.
- Fix: add a risk and dependency: "R2 test tours must still be upcoming and still fully covered at R25". Re-run the coverage comparison at R25 time.

**Total: 11 + 10 + 7 + 11 + 12 + 11 + 7 + 8 + 4 = 81/100**

---

## Execution readiness — 7/10
Broadly buildable. Endpoints, parameters, response fields, UI copy and limits are concrete enough to estimate (R6, R7, R9, R15, R17). Clarifications still needed before or during build: Q2 (pin grouping), R11's "roughly" bounds, R9's refusal behaviour, Q6 (removal email), and hosting (deferred to Design, which is legitimate: "Chosen at the Design gate"). These are minor, which puts it at 7, not 10.

## Substance gate — 4/10 (scored last)

**Trace 1 — `decision.md`: "State plainly in the product that only Ticketmaster-listed dates are shown (don't claim gaps are detectable)."** Traces end to end. The context paragraph ("**the product cannot detect or show gaps**") leads to R17 (exact notice text, non-dismissable, visible at 320px), R17 AC2 (banned strings), R16 ("N UK dates on Ticketmaster"), E-R7a ("Never "not touring""), R21 and K3. It holds up.

**Trace 2 — `decision.md`: "Design the side-by-side comparison with Bandsintown on fully covered tours, and define 'clearly quicker'."** "Clearly quicker" is well defined (§4 (a)–(c)), and R25's protocol and K2 directly answer the synthesis's fix 2. The trace breaks at the fallback:
- R2 AC2 lets the owner "run R25 on tours where Ticketmaster is partial **and** the Bandsintown map shows the same set of dates."
- R25 step 1 requires "3 tours marked fully covered in R2", and K2's mitigation says "R25 uses only fully covered tours". So the override cannot be run under R25 as written.
- If the override were run, the third condition, "artist's official list page", would show dates that Tour Map does not. That brings back the exact confound K2 calls "**Strongest case against this plan**", this time against the list comparator. The correct answer, "worked out beforehand", could then be a date missing from Tour Map.
- A related claim is made but not supported. E-R13 says close-together London pins are "Measured as part of R25", but R25's tasks ("a named UK town that is not a venue town") and its recorded fields (time, correct or wrong) contain no overlap or density measure. The brief names "pin density and clustering where dates bunch up (e.g. London)" as one of the experiment's questions.

**Falsify — strongest concrete argument that the PRD is wrong:** the experiment's control for data completeness is set at the wrong time. R2 marks tours "fully covered" before build. R25 runs after an unscheduled side-project build. By then, upcoming-only filtering (E-R7c) will have removed past dates, and other agents may have added dates to the official list. R25 step 1 re-checks only that "the Bandsintown map … shows the same number of dates as Tour Map". It never re-checks Tour Map against the official list, which is the third condition. So the matched-data guarantee in K2 can silently fail for the list comparator, and G1 would then measure completeness, not map UX. The document anticipates half of this (the Bandsintown re-check, and R25 AC2 postponement if fewer than 3 tours are eligible) but not the list side or tour expiry.

Score: one of two sampled items does not hold up (an internal contradiction in the comparison design that `decision.md` asked the PRD to resolve, plus an unsupported "measured" claim), so the score is 4. The strongest counter-argument is only partly anticipated. Scoring down when in doubt also gives 4, not 7.

---

## What to fix, in priority order (Revise)

1. **Substance gate (caps the verdict).** Make R2 AC2's override consistent with R25 and K2. Either remove the partial-coverage option, or define how the list condition is matched (for example, a list of only the Ticketmaster-listed dates) and how the correct answer is fixed. Re-verify all three conditions' date sets at R25 time. Either add a density or overlap measure to R25 or remove "Measured as part of R25" from E-R13.
2. **Criterion 4, Success metrics (−4).** Measure or explicitly drop the brief's "easier to navigate" and "within seconds".
3. **Criterion 6, Requirements quality (−4).** Exact R11 bounds, R9 refusal response, split and measurable R19, close Q2.
4. **Criterion 1, Problem clarity (−4).** The pain evidence is n=1. Little can be fixed before G1, but a handful of documented fan accounts, or the R4 baseline record, would strengthen it. Lowest-effort fix: none needed for the gate, since it is not triggered.
5. **Criterion 3, Evidence quality (−3).** Put "unverified" back on the Google billing claim. Label or reconcile R25's "the job happens on phones".
6. **Criterion 7, Acceptance criteria (−3).** Measurable R19 AC, timed R22 runbook AC, R9 queue/refusal and reset ACs.
7. **Criterion 9, Dependencies + risks (−1).** Name the tour-expiry and coverage-drift risk between R2 and R25.
