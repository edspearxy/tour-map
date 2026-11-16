# Ideation decision: Tour Map

## Current decision (v2 brief)

- **Verdict:** Pursue
- **Date:** 2026-11-16
- **Decided by:** product owner (overriding the council's suggested "Refine & re-submit")
- **Council readout:** 71/100 (Feasibility 17, Market 17, User-Value 17, Risk 20); root-cause gate
  did not fire; Substance gate fired (min 4/10 — Market, User-Value, Risk). See
  `reviews/synthesis-2026-11-16-2.md`.

### Rationale
- The product is a map-UX and API-integration learning experiment. The product owner accepts
  Ticketmaster's limited UK coverage for the MVP: "ticketmaster can be used for MVP so is not an
  issue for limited data coverage."

### Carried into Discovery for the PRD to resolve
- State plainly in the product that only Ticketmaster-listed dates are shown (don't claim gaps
  are detectable).
- Design the side-by-side comparison with Bandsintown on fully covered tours, and define
  "clearly quicker".
- Ticketmaster terms actually found by research: no replicating Ticketmaster's core experience,
  24-hour removal on request, terms change without notice; read the full terms before Build.
- Map tile provider choice and licence; effect of any future affiliate links on free-tier terms.
- Confirm a standard API key returns UK (GB) events; decide browser vs server-side API calls
  (key exposure, rate limits).
- Gift-buyer "lives elsewhere" segment is unevidenced — treat as secondary or drop.

## History

### v1 brief — Parked (2026-11-16)

- **Verdict:** Park
- **Date:** 2026-11-16
- **Decided by:** product owner
- **Council readout:** 63/100, suggested "Refine & re-submit"; Substance gate fired (all four lenses 4/10). See `reviews/synthesis-2026-11-16.md`.

##### Notes
- The product owner checked Bandsintown first-hand and concluded that "the Bandsintown site solves the problem" — the whole-tour, artist-first map view this idea proposed already exists. This confirms the council's market finding (Bandsintown artist-page event map).
- An earlier product-owner correction disputing that finding was withdrawn and has been removed from `market-research.md` and `memory/corrections/`.
- The unresolved data-source problem (scraping widget-based tour pages vs. provider terms) was not pursued.
- Nothing downstream runs. To reopen, revise the brief around a differentiator Bandsintown does not cover and re-run `/ideation`.

##### Retro candidates (toolkit, not this project)
- `idea-rubric.md` criterion 2.1 references `idea/market-research.md`; the file is `ideation/market-research.md`.
- `idea-standards.md` names the Substance cap "Revise", which is not an Ideation band name.
