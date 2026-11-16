# PRD Review: Tour Map (draft 2, revision 1)

- **PRD:** `discovery/tour-map-prd.md` (Status: "Draft for Discovery review (revision 1)", 2026-11-16)
- **Reviewed:** 2026-11-16 by prd-critic-agent (second review the same day; first review: `tour-map-prd-2026-11-16-review.md`)
- **Upstream inputs traced:** `ideation/tour-map.md` (v2 brief, approved), `ideation/market-research.md`, `ideation/decision.md`
- **Rubric:** `context/rubrics/prd-rubric.md` + `prd-standards.md`. The universal corrections file has no entries. No project-local corrections file exists (`memory/corrections/` holds only `.gitkeep`).
- **Method:** scored fresh against the rubric. The Substance gate uses new samples, not the draft-1 traces.

## Result

| | Score |
|---|---|
| Raw total | **89/100** (band: Ship with fixes) |
| Execution readiness | **7/10** |
| Substance gate | **4/10** |
| **Final verdict** | **Revise** |

> Substance gate: verdict capped at Revise — sampled trace failed (`decision.md`'s "read the full
> terms before Build" is named in R3 but not enforced: the §6 sequencing constraint gates only
> groups C–E. That lets R5–R10, including R8 caching and R10 tiles, the two requirements that
> depend most on the terms, be built before R3 is done. The artist-matching trace rests on a
> citation to a market-research section that does not exist) despite a raw total of 89/100.

> Gate override: verdict capped at Revise — Substance gate scored 4/10 despite a raw total of 89/100.

Root-cause gate (Problem clarity ≤ 6): not triggered (11/15). Execution readiness gate (≤ 4): not triggered (7/10).

Note on gate thresholds: the "Verdict bands" paragraph of `prd-rubric.md` (line 183) gives the gates as "Problem Clarity ≤ 3" and "Execution readiness gate (≤ 3)". That conflicts with the rubric's own gate sections (≤ 6, ≤ 4) and with `prd-standards.md` (≤ 6, ≤ 4, ≤ 4). This review applies the standards' thresholds. Neither gate is near either threshold here, so the conflict does not change this verdict. It should be fixed through `retro`.

---

## Criterion scores

### 1) Problem clarity — 11/15
The problem is specific and separate from the solution, and the PRD is honest about where its evidence comes from. The pain still rests on one person's experience, which matches the "thin on evidence" anchor. The new R25 screener question is planned work, not evidence yet. The PRD text has not changed in substance on this point, so the score stays at 11, as in the first review.
- "It does not answer the question a fan deciding where to go actually has: **"where in the UK are they playing, and which date can I realistically get to?"**"
- "The evidence for pain is the product owner's own repeated experience (n=1)"
- "Two pieces of planned work are the first points at which this assumption meets data from anyone else: the R25 screener question … and the G1/G6 results themselves."

### 2) Target user + context (JTBD) — 10/10
The PRD names a segment, gives a concrete scenario and a JTBD. Phones and laptops are now handled consistently with R25. The gift-buyer segment is dropped, which `decision.md` allows ("treat as secondary or drop").
- "**Primary segment: UK gig-goers deciding which date of a specific musician's UK tour to attend.**"
- "**Devices:** the product is responsive and serves both phones and laptops (R11–R20). The comparison test (R25) runs on phones only…"

### 3) Evidence quality — 7/10
Labelling is mostly careful. The draft-1 fixes are in: Google billing is now "reportedly … unverified", and the phone-only choice is labelled `[Decision]` with a cite to the brief's "Purpose". Spot checks match `market-research.md`: 894 domains, 5 of 12 arenas, 58%/66%, £7.6bn/£318, YouGov 10%, MapTiler suspension, the size × page cap. The sourcing is still inconsistent in three places:
- **A citation to a source that doesn't exist.** E-R6b says: "No automatic filtering of tributes, because the API offers no reliable flag `[Evidence: market-research.md, artist matching risk]`". `market-research.md` has no "artist matching risk" section and says nothing about tribute flags. Artist matching appears only in the brief's Risks list, which does not make this claim. This is an unlabelled assumption presented as evidence.
- **A dropped "unverified" label.** K1 says "Medium (5 of 12 major arenas `[Evidence]`)". §2 cites the same figure as "unverified", and the PRD's own rule is that it "keeps that label wherever it relies on one".
- **An overstated claim.** R10 rejects OSM "because its policy discourages application use". The research says identifying User-Agent/Referer, attribution, "heavy use needs permission", blocking possible, and "recommends running your own tile server". It does not say the policy discourages application use.
- Fix: re-label E-R6b as `[Assumption]`, or cite something real. Put "unverified" back on K1. Reword the OSM rationale to match the source.

(R4 cites `synthesis-2026-11-16-2.md`, which is outside the three inputs given to this review. `decision.md` refers to it, so I have not marked the PRD down for it, but I could not verify it.)

### 4) Success metrics — 15/15
Draft 1's gap is closed against the brief. Its success statement ("within seconds … quicker and easier to navigate") now maps to G7, G1 and G6. Each goal has a KPI, a baseline (measured at the same time as the comparators, which is right for a comparative test), a target, a timeframe and a measurement method.
- G1: "Tour Map median ≥30% lower than **each** comparator; Tour Map faster than both comparators for ≥4 of 5 testers; Tour Map correct-answer rate no lower than either comparator | Test run within 4 weeks of the MVP going live"
- G6: "Tour Map median ease rating higher than **each** comparator's, **and** ≥4 of 5 testers rank Tour Map easiest"
- G7: "≤10 seconds `[Assumption: A8]`". The PRD labels this as its own reading of the brief's "within seconds".
- The brief's validation path ("use the map for real ticket decisions") is covered by G4: "≥3 | Within 3 months of going live", measured by "the product owner's own dated log".

### 5) Scope + non-goals — 12/12
The in-scope list is crisp, §9 lists non-goals explicitly, and the v1 boundary is stated. Each deferral matches the brief and `decision.md`. The brief's "affiliate ticket links possibly later" is deferred with a reason, and postcode distance is post-MVP as decided.
- "**No data source other than the Ticketmaster Discovery API**, and no scraping `[Decision: ideation/decision.md, brief]`"
- "Each of these is deferred deliberately. None starts unless G1 is met."

### 6) Requirements quality — 11/15
The draft-1 fixes are in: exact bounding box, a defined R9 refusal response, R19 split into (a)–(e), and an Option G default for Q2. Most requirements are now precise. Two still can't be built as written:
- **R7 cannot meet its own outputs with the one call it specifies.** "`GET /api/artists/{attractionId}/events` calls the Discovery events endpoint with that `attractionId`…" It must also return "`attractionName`" and satisfy "AC3: Given an unknown attractionId, then the endpoint returns HTTP 404". An events query only returns events, so for an artist with 0 events (E-R7a, which needs "{artist}" in its message) it has no name to return, and it cannot tell an unknown ID from a known artist with no UK dates. That needs a second upstream lookup, and the PRD never specifies one or counts it against R9's budget.
- **R6 can't trigger E-R6a.** R6 "returns at most 10 candidates" and R6 AC1 fixes the response as "a JSON array of ≤10 objects". E-R6a requires the browser to show "Showing the top 10 matches…" when "Upstream returns more than 10 attractions". Nothing in the response tells the browser there were more than 10. R7 has a `truncated` flag for the same purpose; R6 has nothing equivalent.
- Minor: it is not specified whether E-R10's single retry counts towards R9's per-second and daily counters. It is also not specified whether cancelled or postponed events (E-R7e) count in R16's "N UK dates".
- Fix: specify the R7 attraction lookup, including its call cost and its 404 rule. Add a `truncated`/`total` field to R6. State how retries and cancelled events are counted.

### 7) Acceptance criteria — 10/10
Every major requirement now has a pass/fail test, including the three draft-1 gaps. R19 has measurable gesture ACs ("`window.scrollX` and `window.scrollY` stay 0 and no `scroll` event fires on `document`"). R22 has a timed drill ("elapsed time from starting step 1 to completing step 4 is no more than 30 minutes"). R9 has queue, refusal and reset ACs (AC2, AC4). Some sub-clauses still have no test. These are optional, non-blocking fixes: R15's date format and "not time-zone converted"; R14's 0-pin and max-zoom-12 cases; R23's "no stored IP addresses or search terms"; and E-R10's one-retry behaviour.

### 8) Edge cases + failure states — 8/8
Errors, empty states, permissions, limits, timeouts and retries are all covered. The draft-1 copy issue is fixed: "During artist search (R6) the browser shows "Couldn't search Ticketmaster just now."; on an artist map (R7) it shows "Couldn't load tour dates…"". Other examples: "E-R19b | Location permission | Not requested", "E-R18 | Every event is unplaceable", "E-R10b | API key revoked or invalid (upstream 401/403) | No retry." (E-R6a can't be triggered as specified. That is scored under criterion 6.)

### 9) Dependencies + risks + assumptions — 5/5
The draft-1 gap is closed. K13 names tour expiry and drift, and the dependency table now lists "3 test tours that are still upcoming, still fully covered and still identical … on each R25 session day". Dependencies, risks K1–K14, assumptions A1–A11 and open questions Q1–Q6 are explicit, and each has an owner and a needed-by point.

**Total: 11 + 10 + 7 + 15 + 12 + 11 + 10 + 8 + 5 = 89/100**

---

## Execution readiness — 7/10
Broadly buildable. Endpoints, parameters, response shapes, UI copy, limits and gesture checks are all concrete. An engineer would still have to ask about the R7 attraction lookup (where `attractionName` and the 404 come from), the missing R6 truncation signal, whether retries count against R9, and whether R5–R10 may start before R3. Hosting is deferred to Design ("Chosen at the Design gate"), which is legitimate. These are minor clarifications, so the score is 7, not 10.

## Substance gate — 4/10 (scored last)

**Trace 1 — brief, Risks: "Artist matching: a name search can return the wrong act or tribute bands."**
In the PRD this runs through R6 (attractions with `keyword` + `classificationName=music`), the R11 picker "with genre where present", E-R6b, E-R6c ("Search again"), K7 and A7. The product behaviour is coherent. The support behind it does not hold up against `market-research.md`:
- E-R6b's justification cites "`[Evidence: market-research.md, artist matching risk]`". That section does not exist, and the research makes no claim about tribute flags.
- The research flags the exact query R6 depends on as unconfirmed: "No source confirmed how [`classificationName=music`] combines with keyword on attractions", and "No result confirmed GB specifically for attractions". It also notes that keyword partial words don't match on events ("Mado" will not find "Madonna"). R1, the pre-build API gate, tests only *events* requests ("request events with `countryCode=GB` and `classificationName=music` for 3 artists"). It never tests the attractions search that is the entry point to the core flow. If that combination returns nothing, every search shows "No artist found" with no hint why.

**Trace 2 — `decision.md`: "Ticketmaster terms actually found by research: no replicating Ticketmaster's core experience, 24-hour removal on request, terms change without notice; read the full terms before Build."**
The three clauses trace well: R3 (a)–(c) with quote plus a comply/conflict note per item, §9 "No ticket purchase … price display" against (a), R22 denylist + runbook + timed drill against (b), K5/E-R10b against (c). The "before Build" half is named but not enforced:
- R3 is headed "Read and record the full terms before build" and cites "`[Decision: ideation/decision.md]` requires the full terms to be read before Build."
- The sequencing constraint says only: "R1–R4 complete before any ticket in groups C–E starts." Group B (R5–R10) is not gated.
- Group B contains the requirements most exposed to the terms. R8 caching rests on "`[Assumption: A2]` Ticketmaster's terms allow short-lived caching. R3 confirms this". R10's AC1 tests "the provider attribution required by R3(h)", which can't be tested before R3 exists. K4's own example is "caching banned".
- R3 AC2 assumes the gate exists ("changed through a PRD revision **before its ticket starts**"), but nothing in §6 prevents the R8 or R10 ticket starting first. The decision item is restated, not supported.

**Falsify — the strongest concrete argument that the PRD is wrong:** the experiment can end without an outcome. R25's eligibility is the conjunction of several conditions on each session day:
- Ticketmaster full coverage (5 of 12 arenas);
- the Bandsintown map showing an identical set (the research reports Bandsintown coverage "depends on artist opt-in");
- the official list matching;
- every date placeable;
- ≥4 dates still upcoming.

K13 rates drift likelihood "High over a multi-month build". When too few tours qualify, R25 step 2 tops up "until 3 are eligible or 10 more have been checked". After that, AC3 says the test "is postponed rather than run with a smaller set", with no limit, no re-try rule and no terminal decision. Meanwhile G1's timeframe is "Test run within 4 weeks of the MVP going live". §4 defines outcomes only for "If G1 fails" and "If G1 is met but G6 or G7 is not". A postponed or never-run G1 is neither. §8 says "None starts unless G1 is met", so the project waits indefinitely. The brief's stop rule ("If the map-first view isn't clearly quicker … stop before investing further") never fires, and the experiment's main question goes unanswered without anyone deciding to stop. The document anticipates the drift (K13, step 2, AC2, AC3) but not this end state.

**Score.** Neither sample traces cleanly. Trace 2's carried decision item is named but not enforced by the PRD's own sequencing. Trace 1's support includes a citation to a section that does not exist, and it skips a research-flagged unknown on the core query. The strongest counter-argument is partly anticipated but not answered. This is below the 7 anchor ("sampled items trace cleanly"), and scoring down when in doubt gives 4. All three findings take only a few lines each to fix.

---

## What to fix, in priority order (Revise)

1. **Substance gate (caps the verdict).**
   - (a) Extend the §6 sequencing constraint so that R3 (at least) completes before any group B ticket starts, or before R8 and R10 specifically. Alternatively, record a `[Decision]` explaining why group B may proceed first, and say what happens to built work if R3 finds a conflict.
   - (b) Add an attractions-search check to R1: `keyword` + `classificationName=music` for the same 3 artists, recording whether full and partial names match.
   - (c) Give R25 AC3 a terminal rule. For example: if R25 has not run within N weeks of going live, the owner records stop, a data-source change, or a revised protocol, and §4 treats that as a defined outcome.
2. **Criterion 1, Problem clarity (−4).** The pain evidence is still n=1. It can't be fully fixed before R25. Not blocking, since the gate is not triggered.
3. **Criterion 6, Requirements quality (−4).** Specify R7's attraction lookup (source of `attractionName`, 404 rule, cost against R9). Add a truncation or total signal to R6 for E-R6a. State whether retries count against R9 and whether cancelled events count in R16.
4. **Criterion 3, Evidence quality (−3).** Re-label or properly source E-R6b's "no reliable flag". Put "unverified" back on K1's arena figure. Reword the OSM "discourages application use" rationale to match the research.
