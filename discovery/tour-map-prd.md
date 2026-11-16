# PRD: Tour Map

- **Status:** Draft for Discovery review (revision 2)
- **Date:** 2026-11-16
- **Upstream inputs:** `ideation/tour-map.md` (v2 brief, approved), `ideation/market-research.md`
  (2026-11-16 snapshot), `ideation/decision.md` (Pursue, 2026-11-16)
- **Product owner decisions treated as fixed:** map-first page; music, UK only; Ticketmaster
  Discovery API as the only MVP data source, with limited coverage accepted; no user accounts or
  uploads; distance from a location/postcode out of MVP; side project and learning experiment,
  no revenue target; nothing copied from Bandsintown.

**How claims are labelled in this document.** `[Evidence: …]` cites a source from the market
research or the brief. `[Assumption]` marks something not backed by a source and which could be
wrong. `[Decision]` marks a choice this PRD makes, or records from the product owner. Most
market-research items were seen only in search summaries and are marked "unverified" there. This
PRD keeps that label wherever it relies on one.

---

## 1. Overview

Tour Map is a small website where **the map is the page**. A visitor searches for a musician and
lands on a full-screen map of the UK with a pin at each of that artist's upcoming UK dates listed
on Ticketmaster. Tapping a pin shows the date, the venue and a link to buy tickets on
Ticketmaster. The data comes from the Ticketmaster Discovery API through a small server-side
service. There are no accounts and nothing to upload.

It is a side project with two purposes. The first is a **map-UX experiment**: can a map-first
tour view let a fan pick a reachable date more quickly and more easily than reading a list or
using Bandsintown's artist-page map? The second is **learning to integrate a real third-party
API** within its terms. It does not aim to win users from Bandsintown and has no revenue target
`[Decision: ideation/decision.md]`.

## 2. Problem Statement

**The problem.** Touring musicians publish their dates as a list: date, venue, town. A list
answers "when is the next show?". It does not answer the question a fan deciding where to go
actually has: **"where in the UK are they playing, and which date can I realistically get
to?"** To answer that from a list, the fan reads every entry and places each town on a mental map.
That gets slower and more error-prone as tours get longer, and it is worse for towns the fan
doesn't know well.

**What exists today, and why it doesn't settle the problem for this experiment.**
- **Artist and venue list widgets** are the common way tour dates are published (Bandsintown,
  Songkick Tourbox and Seated widgets; Seated was on 894 domains as of October 2026)
  `[Evidence: market-research.md, Indirect Alternatives]`. These are lists, not maps.
- **Bandsintown's artist pages have an event map** (added March 2022, powered by Stay22)
  `[Evidence: market-research.md, Bandsintown artist-page event map, unverified, dated 2022]`.
  The product owner's first-hand view is that the map is secondary to the list and hard to get
  from "this artist" to "the whole tour at a glance" `[Evidence: ideation/tour-map.md, first-hand,
  n=1]`. **No published source corroborates or contradicts that view**, and nobody has checked
  whether the map still exists in the same form in November 2026
  `[Evidence: market-research.md, "Published feedback…"]`. This PRD therefore treats "Bandsintown's
  map is harder to navigate" as **a hypothesis to test** (§4, G1 and G6; R4, R25), not a fact.
- **Other map-first single-artist tools exist or have existed**: Tour Radius, MusicMap, Concert
  Map, the Artist Map GitHub project, ROSTR's Live tab and Gigora. None was confirmed live in this
  session `[Evidence: market-research.md, "Other single-artist or tour-map products"]`. So a
  map-first tour view is **not a new idea**, and this PRD makes no novelty claim. What this
  project tests that those tools don't answer is a measured comparison: whether a specific set of
  map-UX choices (default framing, no clustering, a details panel, mobile full-screen) makes "find
  my reachable date" measurably quicker and easier on the same tours. Where any of those tools
  turn out to be live, R4 records them as reference points before build.

**How painful is it?** The evidence for pain is the product owner's own repeated experience
(n=1) `[Evidence: ideation/tour-map.md, "Why now"]`. No research compares map and list usability
for this task `[Evidence: market-research.md, "Map vs list usability: no sourced evidence
found"]`. `[Assumption]` Other UK gig-goers who follow touring artists share the same friction.
Two pieces of planned work are the first points at which this assumption meets data from anyone
else: the R25 screener question, which records whether each of the five testers has faced this
task in the last 12 months and how they solved it, and the G1/G6 results themselves. Both come
from the owner's friends, so they are indicative only (K11).

**Context: the data constraint is part of the problem space.** Ticketmaster does not list every
UK date. Ticketmaster says it partners with 5 of 12 major UK arenas `[Evidence: market-research.md,
UK ticketing market share, committee written evidence 2025, unverified]`. A 2026 committee report
attributes 58% of UK primary ticket sales to Live Nation (66% including affiliates), figures Live
Nation disputes `[Evidence: same section, unverified, dated 2026]`. Any tour sold partly through
See Tickets, DICE, AXS or others will be incomplete on this map. The API cannot report dates it
doesn't hold, so **the product cannot detect or show gaps**. It can only say plainly that it shows
Ticketmaster-listed dates alone (R17). The product owner has accepted this for the MVP
`[Decision: ideation/decision.md]`. For the experiment, the same constraint means the comparison
test must use only tours where every source shows the same dates (R2, R25).

## 3. Target Users

**Primary segment: UK gig-goers deciding which date of a specific musician's UK tour to attend.**
- **Scenario:** a musician they follow announces, or is partway through, a UK tour of roughly
  5–30 dates. The fan has the tour listing open on a phone or laptop and needs to pick the date
  they can get to (for example, near home, near family, or on a weekend they're free).
- **Job-to-be-done:** "When a tour is announced, help me see at a glance where in the UK it goes,
  so that I can pick the date I can actually reach and get to the ticket link without reading the
  whole list."
- **Evidence:** first-hand only (n=1). `[Assumption]` the scenario is common among UK gig-goers.
  Market context: UK live music consumer spend was about £7.6bn in 2025, and the average UK fan
  spent £318 on live tickets over 12 months `[Evidence: market-research.md, UK live music demand,
  carried from v1, not re-verified]`. This shows the activity is widespread. It does not show that
  this friction exists.
- **Devices:** the product is responsive and serves both phones and laptops (R11–R20). The
  comparison test (R25) runs on phones only, for the reasons given there; laptop use is built
  but not measured in the MVP.

**Experiment owner: the product owner, as the learner.** The project exists to learn about map-UX
and API integration. Success for this user is a clear, recorded answer to G1, G6 and G7, and a
working integration that stays within the API terms (G2–G5).

**Gift buyers: dropped as a target segment for the MVP.** `[Decision]` The brief named people
buying tickets for someone who lives elsewhere. The only related evidence is that 10% of YouGov
respondents (February 2023) bought tickets as presents `[Evidence: market-research.md]`. There is
no evidence on how many of those buy for someone in another city
`[Evidence: market-research.md, "no sourced evidence found"]`. The MVP has no gift-specific
requirements and gift buyers are not recruited as testers. The primary flow serves them anyway if
they turn up.

## 4. Goals & Success Metrics

| # | Goal | Metric | Baseline | Target | Timeframe |
|---|---|---|---|---|---|
| G1 | Map-first view is clearly quicker for the core job (the experiment's question) | Median time to identify the tour date nearest a named UK town, per condition (Tour Map / Bandsintown artist-page map / artist's own list) | **Not yet measured.** Bandsintown and list times are recorded in the same test sessions (R25). The Bandsintown map's existence and behaviour are recorded first (R4). | "Clearly quicker" (defined below): Tour Map median ≥30% lower than **each** comparator; Tour Map faster than both comparators for ≥4 of 5 testers; Tour Map correct-answer rate no lower than either comparator | Test run within 4 weeks of the MVP going live |
| G6 | Map-first view is easier to navigate (the brief's "easier to navigate") | (i) Per-trial ease rating, 1–7, per condition; (ii) each tester's end-of-session ranking of the three conditions from easiest to hardest | **Not yet measured.** Recorded for all conditions in the same R25 sessions | Tour Map median ease rating higher than **each** comparator's, **and** ≥4 of 5 testers rank Tour Map easiest | Same sessions as G1 |
| G7 | A fan can find their date "within seconds" (the brief's absolute bar) | Median time across all Tour Map trials | **Not yet measured** (R25) | ≤10 seconds `[Assumption: A8]` | Same sessions as G1 |
| G2 | The data source can support the experiment at all | Number of sampled UK tours where Ticketmaster lists every date on the artist's own official list | Unknown. No source measures Discovery API tour completeness `[Evidence: market-research.md]` | ≥3 fully covered tours found from up to 10 checked | Before any map UI ticket starts (R2) |
| G3 | Dates that are returned actually reach the map | % of returned UK events with valid venue coordinates (placed as pins) | Unknown. No source on UK venue coordinate population `[Evidence: market-research.md]` | ≥95% across the tours used in R2 | Measured during R2, re-checked on each R25 session day (R25 step 1) |
| G4 | Real use by the owner | Number of real ticket decisions (own or someone else's) where the owner used Tour Map to pick the date | 0 (product doesn't exist) | ≥3 | Within 3 months of going live |
| G5 | Integration stays within API limits | Days on which upstream Ticketmaster calls exceed 4,500 (guard threshold, R9), and number of HTTP 429 responses received | 0 (no integration yet) | 0 days over threshold; ≤5 × 429 responses per week | Continuously from going live, reviewed at 4 weeks |

G-numbers are stable identifiers, not a priority order. G6 and G7 sit next to G1 because the
three together make up the brief's success statement: "a fan can identify the nearest or most
convenient date within seconds … people find this one quicker and easier to navigate"
`[Evidence: ideation/tour-map.md, "What success looks like"]`.

**How each metric is measured**
- **G1:** moderated sessions run by the product owner using the protocol in R25. Each trial is
  timed by screen recording, from the moment the artist's page has finished loading (artist
  search is excluded so all three conditions start equal) until the tester states a date out loud.
  The correct answer for each task is fixed before the session, by straight-line distance from the
  named town to each venue in that session's verified date set (R25 steps 1 and 4). Results go in
  a results sheet saved in the project repository (`validation/g1-results.*`, format chosen at
  Build).
- **"Clearly quicker", defined** `[Decision]`: all three conditions must hold. (a) The Tour Map
  median time across all trials is at least 30% lower than the Bandsintown-map median **and** at
  least 30% lower than the list median. (b) At least 4 of the 5 testers have a lower personal
  median on Tour Map than on both comparators. (c) The share of correct answers on Tour Map is no
  lower than on either comparator. `[Assumption]` 30% is set high enough to be noticeable above
  stopwatch noise with five testers. It is a judgement call, not statistical significance. If the
  Bandsintown map no longer exists (R4), G1 is judged against the list alone and the PRD outcome
  records that.
- **G6:** immediately after each trial the tester answers "How easy was it to find that date?" on
  a 1–7 scale (1 = very hard, 7 = very easy). After all trials, the tester ranks the three
  conditions from easiest to hardest. Both go in the same results sheet. `[Decision]` A single
  post-task rating plus a final ranking is used because it adds under a minute per tester and
  needs no questionnaire licence.
- **G7:** computed from the G1 Tour Map trial times. No extra data collection.
- **Overall result and consequences** `[Decision, from the brief's stop rule]`:
  - The experiment's success statement is met when G1, G6 and G7 are all met.
  - **If G1 fails:** no post-MVP work (§8) starts. The product owner decides between closing the
    experiment and trying one documented UX variation and re-running R25. This is the brief's stop
    rule, which names speed only.
  - **If G1 is met but G6 or G7 is not:** the shortfall is recorded in the R25 write-up. No
    post-MVP work starts until the owner records a decision either to accept the result or to try
    one documented UX variation aimed at the missed measure and re-run R25.
  - **If R25 can't run in full within the G1 timeframe:** R25 AC3 applies. A reduced run is judged
    as above and reported as indicative. An inconclusive close counts as G1 not met, so no
    post-MVP work starts.
- **G2:** a manual comparison recorded in the R2 ticket. For each tour, list the dates on the
  artist's own official tour page and the events the Discovery API returns, then mark the tour
  "fully covered" or "partial" with the missing dates named.
- **G3:** server-side counters (events returned, events placed, events unplaced) logged per
  artist request (R7, R18). Read from logs for the R2 tours and for the R25 tours on each session
  day.
- **G4:** the product owner's own dated log (date, artist, decision made), kept in the repository.
- **G5:** the server's daily upstream-call counter and 429 count (R9), written to the application
  log once a day.

`[Decision]` No third-party analytics or cookies are used (R23). Metrics come from moderated
sessions, server counters and the owner's log only.

## 5. Core User Flow (MVP)

1. Visitor opens the site. The landing page is a full-screen map of the UK with a search box
   over it (R11).
2. Visitor types an artist name and submits it.
3. The server queries Ticketmaster's attractions endpoint for music attractions matching the name
   (R6).
   - 0 matches: a "no artist found" message; the visitor can search again.
   - 1 match: go straight to step 5.
   - 2–10 matches: the visitor picks the right act from a list of names (this handles tribute
     acts and similar names).
4. Visitor picks an artist.
5. The browser navigates to the artist's map page (`/artist/{attractionId}`, R12). The server
   fetches that artist's upcoming GB music events (R7).
6. The map fits itself to show every pin (R14). The header shows the artist name and "N UK dates
   on Ticketmaster" (R16). The coverage notice is visible (R17).
7. Visitor scans the pins and taps the one nearest to where they can get to.
8. The details panel opens: date(s), time if known, venue, town and a "Tickets on Ticketmaster"
   link (R15).
9. Visitor taps the ticket link. Ticketmaster's event page opens in a new tab. Tour Map's
   involvement ends here: no purchase, seat choice or price display happens in Tour Map.

## 6. Key Requirements (MVP Scope)

**Traceability** `[Decision]`: the product owner works in a squad model where every piece of
development is traced to one tracker issue. Each requirement below (R1–R26) is written to be
delivered and verified as **one tracker issue**. Validation work (R1–R4, R25) is tracked the same
way as code, because its outcomes gate the build. The group headings below are scope areas, not
tickets. Requirement IDs are stable across revisions so issue links don't break.

**Sequencing constraint** `[Decision]`: R3 completes before any ticket in group B starts, because
group B holds the requirements that depend most on the terms (R8 caching, R10 tile provider and
attribution). R1–R4 complete before any ticket in groups C–E starts. R1 and R2 have stop
conditions that can end or change the build.

**UK bounding box** `[Decision]`, used by R11 (landing framing) and R18 (placeability filter):
latitude **49.80 to 60.90** (°N) and longitude **−8.70 to 1.80** (8.70°W to 1.80°E), both
ranges inclusive. This covers Great Britain, Northern Ireland, Shetland and the Isles of Scilly.
The Channel Islands fall outside it. The Isle of Man falls inside it, so any Isle of Man event
that the API returns under `countryCode=GB` is placed on the map. The filter exists to catch bad
coordinates, not to decide what counts as the UK.

### A. Pre-build validation (gates the build)

**R1. Confirm UK access on a standard Discovery API key.**
Register a standard Ticketmaster Discovery API v2 key. Using it, request events with
`countryCode=GB` and `classificationName=music` for 3 artists known to have upcoming UK
Ticketmaster dates. Record the request URLs, event counts, whether venue `location.latitude`
and `location.longitude` are present, the maximum accepted page size, and whether and how
event status (cancelled, postponed) is reported. For the same 3 artists, also make the R6
attractions search (`keyword` + `classificationName=music`) once with the full name and once
with a partial name (first word only), and the R7 attraction lookup for one known and one
made-up ID. Record which searches return the artist and what the made-up ID returns.
- *Why:* research could not confirm GB results on a standard key, and the separate International
  Discovery API reportedly no longer takes new key requests `[Evidence: market-research.md,
  unverified]`. Research also found no source confirming how `classificationName=music` combines
  with `keyword` on attractions, and partial words don't match on events `[Evidence:
  market-research.md, "How artists are searched", unverified]`.
- **AC1:** Given a standard key, when the requests are made, then each events request returns ≥1
  GB event, each full-name attractions search returns that artist in its first 10 results, and
  the results (including the partial-name outcomes) are recorded in the R1 ticket. → Pass: the
  build proceeds.
- **AC2:** Given a standard key, when any events request returns no GB events for an artist known
  to have them, any full-name attractions search does not return the artist, or any request
  returns an access error, then the result is recorded and no group C–E ticket starts until the
  product owner has decided what to do (the default is to stop).
- **AC3:** Given R1 is complete, then the ticket records the maximum page size accepted and the
  event status field (or its absence), answering A3 and Q3.

**R2. Coverage check on real UK tours (G2, G3).**
Choose up to 10 current or announced UK music tours. The first is the tour in the brief's example
(Cardiff Utilita Arena, London Eventim Apollo, Bristol Hippodrome, Halifax Piece Hall). Prefer
tours whose last date is at least 3 months after the check, so they are still running when R25
takes place `[Assumption: A9]`. For each tour, compare the Discovery API's GB events for the
artist against the artist's official tour page. Record each tour as "fully covered" or
"partial", list any missing dates, and count events without usable coordinates.
- **AC1:** Given the check is complete, when ≥3 tours are fully covered, then **every** fully
  covered tour (not only the first 3) is recorded as an R25 candidate tour with its last date,
  and the build proceeds.
- **AC2:** Given 10 tours have been checked, when fewer than 3 are fully covered, then the build
  of groups C–E does not start. `[Decision, made in advance]` The default outcome is **stop**.
  The only override the product owner may record in writing in the R2 ticket is **switching data
  source**, which needs a revised PRD. Running the comparison on partially covered tours is
  **not** an option: on such a tour the artist's official list shows dates that Tour Map does
  not, so the list condition in R25 would measure data completeness, not map UX (K2).
  Hand-entered data is not an option either, because it would break the "no uploads" and
  single-source decisions.
- **AC3:** The share of events with valid coordinates across all checked tours is recorded (G3).

**R3. Read and record the full terms before build.**
Read, in full and from the providers' own pages: the Ticketmaster Developer General Terms of Use
and any Ticketmaster branding/attribution guidelines, plus the chosen map tile provider's terms
(R10). Record the exact wording for each item below, and say whether the requirements in this
PRD comply:
(a) no replicating or replacing Ticketmaster.com's essential user experience; (b) removal of
content within 24 hours on request; (c) terms changing without notice and key revocation;
(d) attribution and branding; (e) caching and storage of API data; (f) display restrictions;
(g) commercial versus non-commercial use, including affiliate links; (h) the map provider's
free-plan limits, attribution and non-commercial clause.
- *Why:* (a)–(c) were found by research in search summaries only. (d)–(g) were not found at all
  `[Evidence: market-research.md, Ticketmaster terms, unverified, dated 2023]`.
  `[Decision: ideation/decision.md]` requires the full terms to be read before Build.
- **AC1:** Given the terms have been read, then the R3 ticket contains a quote and a
  comply/conflict note for each of (a)–(h), with the date read.
- **AC2:** Given any item conflicts with a requirement here (for example, caching forbidden,
  which affects R8), then the affected requirement is changed through a PRD revision before its
  ticket starts.

**R4. Dated baseline record of Bandsintown's artist-page map and existing map tools.**
For each R2 candidate tour, open the artist's Bandsintown page on a phone and on a laptop.
Record, with dated screenshots and short notes: whether an event map exists; how it is reached
from the artist page; its default framing; whether every date is visible as a pin; the dates it
shows; and whether close-together venues (for example, two London venues) overlap at its default
framing. Also try to open Tour Radius, MusicMap and Concert Map and record whether each is live,
with a screenshot if so.
- *Why:* this turns "hard to navigate" from recollection into a recorded baseline, and confirms
  the comparator exists `[Evidence: synthesis-2026-11-16-2.md, fixes 4–5]`.
- **AC1:** Given R4 is complete, then the ticket holds dated screenshots for every R2 candidate
  tour on both device types, plus a live/not-live status for the three named tools.
- **AC2:** Given the Bandsintown event map no longer exists, then R25 runs with two conditions
  (Tour Map vs list) and G1 and G6 are judged on that basis.

### B. Data service (server-side)

**R5. All Ticketmaster calls go through the project's own server; the key never reaches the
browser.** `[Decision: resolves "browser vs server-side" in ideation/decision.md]`
The API key is held as a server-side secret (environment variable or the host's secret store).
The browser calls only the project's own endpoints (R6, R7).
- *Why:* a key in the browser can be copied and used up against the 5,000/day limit, and the key
  can be revoked for misuse `[Evidence: market-research.md, rate limits and revocation,
  unverified]`. A server is also needed for caching (R8) and rate guarding (R9).
- **AC1:** Given the deployed site, when all JavaScript bundles are searched and network traffic
  is captured across a full search and map view, then the API key string appears nowhere and no
  request goes from the browser to `app.ticketmaster.com`.

**R6. Artist search endpoint.**
`GET /api/artists?q={name}` calls the Discovery attractions endpoint with `keyword={name}` and
`classificationName=music`. It returns a JSON object `{"results": [...], "truncated": bool}`:
`results` holds at most 10 candidates, each with `attractionId`, `name` and, if present, genre;
`truncated` is true if the upstream reports more than 10 attractions (E-R6a). The query must be
2–100 characters after trimming.
- **AC1:** Given q="Coldplay", when called, then `results` holds ≤10 objects, each with non-empty
  `attractionId` and `name`, and `truncated` is a boolean.
- **AC2:** Given q is shorter than 2 or longer than 100 characters after trimming, then the
  endpoint returns HTTP 400 and makes no upstream call.
- **AC3:** Given the upstream returns no attractions, then the endpoint returns HTTP 200 with an
  empty `results` array and `truncated: false`.

**R7. Artist events endpoint.**
`GET /api/artists/{attractionId}/events` makes two upstream calls. First, the Discovery
attraction lookup (`/discovery/v2/attractions/{attractionId}`) supplies `attractionName`; if it
reports the ID as not found, the endpoint returns HTTP 404 and makes no second call. Second, the
events endpoint with that `attractionId`, `countryCode=GB`, `classificationName=music`, upcoming
events only, sorted by date ascending, page size 200, first page only. For each event it returns:
`eventId`, `localDate`, `localTime` (nullable), `venueName`, `city`, `latitude` and `longitude`
(parsed to numbers, nullable), `ticketUrl`, and a `placeable` flag (see R18). It also returns
`attractionName` and `truncated: true` if the upstream reports more than 200 events. An
uncached request therefore costs 2 upstream calls against R9.
- *Why the lookup:* an events query alone can't name an artist with 0 UK dates (E-R7a) or tell
  an unknown ID from a known artist with no UK dates. `[Assumption]` the lookup reports an unknown
  ID as not found; R1 checks this.
- *Why the page cap:* the API caps size × page below 1,000 `[Evidence: market-research.md,
  unverified]`. `[Assumption: A3]` 200 is the maximum page size and no UK tour exceeds it. R1
  confirms the maximum.
- **AC1:** Given an attractionId with UK events, when called, then every returned event has a
  GB venue, a date on or after today (UK time), and events are in ascending date order.
- **AC2:** Given an event whose coordinates arrive as strings, then they are returned as numbers.
  Given they are missing or unparseable, then they are returned as `null` with `placeable: false`.
- **AC3:** Given an unknown attractionId, then the endpoint returns HTTP 404.

**R8. Server-side response cache.**
Responses from R6 and R7 are cached by request parameters with a time-to-live of 60 minutes by
default, set by configuration. A value of 0 turns caching off.
- *Why:* repeated visits during testing and sharing shouldn't each spend API calls. A third-party
  guide advises reusing results `[Evidence: market-research.md, freeapihub, not an official
  rule]`. `[Assumption: A2]` Ticketmaster's terms allow short-lived caching. R3 confirms this; if
  they don't, the TTL is set to 0.
- **AC1:** Given the same request twice within the TTL, then the upstream is called once.
- **AC2:** Given TTL=0, then every request calls the upstream.

**R9. Upstream rate guard.**
- **Per-second limit:** the server starts no more than 5 upstream requests in any one-second
  window. A request that can't start straight away waits in a queue. If it has not started its
  upstream call 2 seconds after it arrived, it is **refused**: the endpoint returns HTTP 503 with
  the JSON body `{"reason": "busy"}` and the header `Retry-After: 5`, and no upstream call is made
  for it. The browser then shows the E-R9b state.
- **Daily limit:** the server keeps a daily upstream-call counter that resets to 0 at 00:00 UTC.
  From 4,500 calls in a day it stops making new upstream calls: cached responses are still
  served, and uncached requests get HTTP 503 with the JSON body `{"reason": "daily-limit"}` (no
  `Retry-After` header). The browser then shows the E-R9a state.
- **What counts:** every upstream call counts towards both limits, including both R7 calls and
  E-R10 retries.
- **Logging:** it logs the daily count and the number of upstream 429 responses (G5).
- *Why:* free-tier limits of 5,000 calls/day and 5/second `[Evidence: market-research.md,
  unverified]`; the terms allow throttling of heavy non-user-driven use `[Evidence: same, secondary,
  unverified]`. The 500-call margin leaves room for R1/R2 manual checks on the same key.
  `[Decision]` No calls are made except in response to a visitor action: no pre-fetching or
  background crawling.
- **AC1:** Given 20 simultaneous uncached requests, then no more than 5 upstream calls start in
  any one-second window.
- **AC2:** Given 20 simultaneous uncached requests, then every request either starts its upstream
  call within 2 seconds of arrival or receives HTTP 503 with body `{"reason": "busy"}` and header
  `Retry-After: 5`, and no refused request causes an upstream call.
- **AC3:** Given the counter is at 4,500, when an uncached request arrives, then HTTP 503 with
  body `{"reason": "daily-limit"}` is returned and no upstream call is made. When a cached request
  arrives, then it is served.
- **AC4:** Given the counter is at 4,500 and the server clock passes 00:00 UTC (tested with an
  injected clock), when an uncached request arrives at 00:00:01 UTC, then the upstream call is
  made and the counter reads 1.
- **AC5:** Given the browser receives 503 `busy` or 503 `daily-limit`, then it shows the
  matching E-R9b or E-R9a message and makes no automatic retry.

**R10. Map library and tile provider.** `[Decision: resolves "map tile provider choice and
licence" in ideation/decision.md]`
Use **MapLibre GL JS** (BSD-3-Clause) with **MapTiler Cloud's free plan** for tiles and style,
with MapTiler and OpenStreetMap attribution shown on the map at all times.
- *Why this choice:* MapLibre is open source `[Evidence: market-research.md, npm metadata]`.
  MapTiler's free plan suspends maps for the rest of the month when exceeded instead of charging,
  so a side project carries no surprise bill. It is limited to non-commercial use, which fits an
  MVP with no revenue or affiliate links `[Evidence: market-research.md, MapTiler terms via
  search summary, official numbers not read]`. Alternatives rejected: the OSM standard tile
  server, because its policy requires an identifying User-Agent and Referer, needs permission for
  heavy use, can block without notice and recommends running your own tile server
  `[Evidence: market-research.md, Map provider costs and terms, search summary]`; Mapbox,
  because GL JS is no longer open source and sources disagree on whether a card is needed
  `[Evidence: same]`; Google Maps Platform, because it reportedly needs a billing account
  `[Evidence: same section; the billing account is not stated in the source summary, unverified]`.
  If R3(h) or a later check showed MapTiler unsuitable, the Google option would be re-checked
  rather than assumed ruled out.
- **Effect of future affiliate links** `[Decision]`: adding affiliate ticket links would make the
  site commercial. That would break MapTiler's free-plan non-commercial clause and may change the
  site's position under Ticketmaster's terms (clause not yet found, R3(g)). Affiliate links are
  therefore out of the MVP (§9). Adding them later needs a paid or commercial-permitted tile plan
  and a fresh terms review.
- **AC1:** Given any map view, then the provider attribution required by R3(h) is visible without
  any interaction at viewport widths of 320px and above.
- **AC2:** Given the tile provider returns errors (including plan suspension), then the behaviour
  in §7 E-R10c applies.

### C. Search and navigation

**R11. Map-first landing page with search.**
The landing page shows a full-screen map fitted to the UK bounding box defined at the top of §6,
with a search box over it. Submitting (Enter key or Search button) calls R6. There is no
type-ahead, so each search costs one API call.
- **AC1:** Given 0 results, then the message "No artist found for '{q}'. Check the spelling or
  try another name." is shown and the search box keeps the query.
- **AC2:** Given exactly 1 result, then the browser navigates to `/artist/{attractionId}` with no
  extra step.
- **AC3:** Given 2–10 results, then a selectable list of artist names (with genre where present)
  is shown, and choosing one navigates to `/artist/{attractionId}`.
- **AC4:** Given a query shorter than 2 characters, then the Search button is disabled and no
  request is sent.
- **AC5:** Given the landing page at 360×640 and at 1366×768, then the map's visible bounds
  contain all four corners of the UK bounding box.

**R12. Shareable artist URL.**
`/artist/{attractionId}` renders the artist map directly when opened from a link, a bookmark or a
reload, with no search needed.
- *Why:* R25 test sessions open each tour directly, and fans share links.
- **AC1:** Given a valid attractionId URL opened in a new browser session, then the artist map
  renders with the same pins as when reached through search.
- **AC2:** Given an invalid or unknown attractionId, then the §7 E-R12 state is shown.

### D. Artist map

**R13. Every placeable date represented on the map; no clustering.**
Each event with `placeable: true` is shown on the map. Events at **different** coordinates always
get separate pins at every zoom level. There is **no clustering** of different coordinates in the
MVP.

How events at **identical** coordinates (for example, three nights at one venue) are shown depends
on the product owner's answer to Q2. Both options are specified so the ticket is testable either
way:
- **Option G (grouped):** the events share one pin with a count badge ("3"). Pins for a single
  event have no badge.
- **Option S (separate):** each event gets its own pin. Pins that share coordinates are laid out
  on screen in a ring centred on the venue, with at least 44 CSS px between neighbouring pin
  centres, at every zoom level.

`[Decision]` If Q2 has not been answered when the R13 ticket starts, Option G is built. The R13
ticket records which option was built. R14, R15, R20 and E-R14 follow whichever option applies.
- *Why:* product-owner decision of "a pin per tour date". Separate pins at identical coordinates
  would sit exactly on top of each other and hide all but one unless grouped (G) or spread (S).
  Clustering is left out because research suggests it hides data and adds clicks when points are
  few, and is meant for many points in one region `[Evidence: market-research.md, Map UX
  patterns, search summaries]`. Clustering is kept as a post-MVP variation (§8), informed by the
  R25 density record.
- **AC1 (both options):** Given two venues 2km apart in London, then two separate pins are
  rendered at every zoom level from 5 to 18.
- **AC2 (both options):** Given 30 events at 30 distinct coordinate pairs within one region, then
  30 pins are rendered at every zoom level from 5 to 18 (no clustering).
- **AC3 (Option G):** Given a tour of N placeable events across V distinct coordinate pairs, then
  exactly V pins are rendered, the badge counts sum to N, and single-event pins carry no badge.
- **AC4 (Option S):** Given a tour of N placeable events, then exactly N pins are rendered; for
  events sharing coordinates, no two pins' 44×44 CSS px tap targets overlap at any zoom level from
  5 to 18, and each pin's centre is within 100 CSS px of the venue's true position.

**R14. Default framing fits the whole tour.**
On load, the map fits the bounds of all pins with 48px padding (plus the height of the header and
notice), with maximum zoom 12. If every pin shares one coordinate pair: centre on it at zoom 10.
With 0 pins: show the UK bounding-box framing (R11).
- **AC1:** Given pins in Cardiff, London, Bristol and Halifax, then on first render all four are
  inside the visible map area at 360×640 and at 1366×768, with no pin under the header or notice.
- **AC2:** Given all events at one coordinate pair, then the map is centred on it at zoom 10.

**R15. Pin details panel.**
Tapping or clicking a pin, or pressing Enter on a focused pin, opens a details panel listing each
date at that pin (several under R13 Option G, one under Option S), earliest first. Each date
shows: the date as "Sat 14 Mar 2027" (the event's local date, not time-zone converted), the start
time as "19:30" if known, the venue name, the town, and a link "Tickets on Ticketmaster" to that
event's `ticketUrl`, which opens in a new tab (`rel="noopener"`). Only one panel is open at a
time. It closes with its close button, a tap on empty map, or the Escape key. On viewports
narrower than 768px the panel is a bottom sheet that covers no more than 50% of the height. On
wider viewports it is a side panel.
- **AC1:** Given a pin carrying 3 dates (Option G), when tapped, then 3 entries are shown in date
  order, each with its own ticket link to the right `ticketUrl`. Given a pin carrying 1 date
  (either option), then 1 entry is shown with its ticket link.
- **AC2:** Given an event with no time, then the time is left out (no "00:00" or "TBC" invented).
- **AC3:** Given a panel is open, when another pin is tapped, then the first panel closes and the
  second opens.

**R16. Artist header.**
A header shows the artist name (`attractionName`), "N UK dates on Ticketmaster" (N = all returned
events, placeable or not, excluding any marked cancelled under E-R7e; postponed events are
counted), and a "Search again" control that returns to the landing page.
- **AC1:** Given 12 returned events, 11 placeable, then the header reads "12 UK dates on
  Ticketmaster".

**R17. Coverage notice (always visible).** `[Decision: resolves "state plainly that only
Ticketmaster-listed dates are shown" in ideation/decision.md]`
On every artist map, a notice reads exactly: **"Only dates listed on Ticketmaster are shown. This
tour may have other dates sold elsewhere. Check the artist's official site for the full list."**
It is visible without scrolling or tapping at all viewport widths of 320px and above, and it
can't be dismissed. Tour Map never claims a tour is complete, never claims to detect missing
dates, and never says an artist "isn't touring" or "has no dates".
- **AC1:** Given any artist map at 320px, 768px and 1366px width, then the full notice text is
  visible on first render.
- **AC2:** Given a search of the whole UI string set, then none of "complete", "all dates", "not
  touring" or "no dates" is used about tour coverage.

**R18. Dates that can't be placed on the map.**
An event is unplaceable if its coordinates are null, non-numeric, exactly 0,0, or outside the UK
bounding box defined at the top of §6 (boundary values count as inside). Unplaceable events are
left off the map and listed behind a control reading "N dates not shown on the map". Opening it
shows the same fields and ticket link as R15. The control appears only when N ≥ 1.
- **AC1:** Given 1 event with null coordinates, then it doesn't appear as a pin, the control reads
  "1 date not shown on the map", and opening it shows that event with its ticket link.
- **AC2:** Given all events are placeable, then the control isn't rendered.
- **AC3:** Given events at (51.5072, −0.1276), (49.80, −6.30), (48.8566, 2.3522) and
  (55.0, −9.0), then the first two are placeable and the last two are unplaceable.

**R19. Mobile viewport and touch gestures.**
On touch devices the map fills the viewport and the page itself does not scroll, so the map can't
trap a page scroll. The requirement is five separately checkable constraints: (a) the document
never scrolls; (b) a one-finger drag pans the map; (c) a two-finger pinch zooms the map, not the
page; (d) each pin's tap target is at least 44×44 CSS px; (e) tapping a pin opens the R15 bottom
sheet. All ACs run on the latest iOS Safari and the latest Android Chrome at 360×640, on a real
device or the vendor's touch emulation.
- *Why:* small embedded maps that capture swipes are a known mobile problem; a full-page map avoids
  it without cooperative gestures `[Evidence: market-research.md, Google 2016 blog]`.
- **AC1 (a):** Given an artist map, when a one-finger drag of 200 CSS px is made in each of the
  four directions, then `window.scrollX` and `window.scrollY` stay 0 and no `scroll` event fires
  on `document`.
- **AC2 (b):** Given the same drags at a fixed zoom level, then a pin visible before each drag
  moves on screen by 200 CSS px ±10 px in the drag direction.
- **AC3 (c):** Given a two-finger pinch-out on the map, then the map's zoom level increases by at
  least 0.5 and `window.visualViewport.scale` stays 1.
- **AC4 (d):** Given any rendered pin, then its interactive element's bounding box is at least
  44×44 CSS px.
- **AC5 (e):** Given a pin is tapped, then within 500 ms the bottom sheet is open showing that
  pin's date(s), and its height is no more than 50% of the viewport height.

**R20. Keyboard and screen-reader access to pins.**
Pins can be reached by keyboard (Tab, in date order) and each has an accessible name of the form
"Sat 14 Mar 2027, Eventim Apollo, London" (or, under R13 Option G, "3 dates at Eventim Apollo,
London" for a grouped pin). The search box, results list, details panel and R18 control are all
operable by keyboard.
- **AC1:** Given keyboard-only use, then a user can search, pick an artist, move between pins,
  open a panel and reach a ticket link without a mouse.
- **AC2:** Given any pin, then its accessible name matches the form above for the R13 option
  built.

### E. Compliance, attribution and privacy

**R21. Data attribution, independence statement and removal contact.**
Every page shows "Event data from Ticketmaster" (or the exact attribution form R3(d) requires)
and a link to an About panel. The About panel states: Tour Map is an independent hobby project,
not affiliated with or endorsed by Ticketmaster or Bandsintown; dates come only from Ticketmaster;
and a contact email address for corrections and removal requests (address to be supplied by the
product owner, Q6).
- **AC1:** Given any page, then the Ticketmaster attribution and the About link are visible, and
  the About panel contains all three statements and a working `mailto:` link to the address
  recorded in Q6.

**R22. Content removal within 24 hours.** `[Decision: implements the 24-hour removal duty found
in research]`
A server-side denylist (configuration file or environment setting) of Ticketmaster `eventId`s and
`attractionId`s is applied to all R6 and R7 responses. A runbook in the repository covers, in
order: add the ID to the denylist, deploy, purge the cache, and verify the item is gone from the
live site. Following the runbook takes no more than 30 minutes, which leaves the rest of the 24
hours as margin for noticing the request.
- *Why:* Ticketmaster-requested removals must happen within 24 hours `[Evidence: market-research.md,
  terms snippet, unverified, dated 2023]`.
- **AC1:** Given an eventId on the denylist, then it is absent from R7 responses immediately after
  deploy, including responses that were previously cached.
- **AC2:** Given an attractionId on the denylist, then R6 never returns it and R7 returns 404 for
  it.
- **AC3:** Given the runbook file exists in the repository with the four steps above, when the
  product owner runs a timed drill on the live site using a real eventId (removed from the
  denylist again afterwards), then start and finish timestamps are recorded in the R22 ticket and
  the elapsed time from starting step 1 to completing step 4 is no more than 30 minutes.

**R23. No accounts, uploads, cookies or third-party analytics.**
The site has no sign-in, no file or data upload, sets no cookies and loads no third-party analytics
or advertising scripts. Server logs hold aggregate counters (G3, G5) and request errors, with no
stored IP addresses or search terms linked to a visitor.
- **AC1:** Given a full search and map session, then the browser has stored no cookies, and the
  only third-party requests are to the map tile provider (R10).

**R24. No Bandsintown name, branding, design or data.**
The product name, logo, colours, icons and layout are original. The only use of the Bandsintown
name is the "not affiliated" statement in R21. No request is ever made to any Bandsintown domain
or API.
- **AC1:** Given the built site, then a search of the source and assets finds "Bandsintown" only
  in the R21 statement, and captured network traffic shows no requests to `*.bandsintown.com`.
- **AC2:** Given the visual design is reviewed at the Design gate alongside the R4 screenshots,
  then no layout, palette or iconography is reproduced.

### F. Validation

**R25. Run the side-by-side comparison test (G1, G6, G7).** `[Decision: resolves "design the
side-by-side comparison on fully covered tours, and define clearly quicker" in
ideation/decision.md]`
Protocol:
1. **Tour eligibility, re-verified on each session day.** The candidate tours are those recorded
   as fully covered in R2 AC1. On the day of each session, before the first trial, the owner
   records three date sets for each candidate tour: (i) the dates Tour Map shows at
   `/artist/{id}`; (ii) the dates the Bandsintown artist-page map shows; (iii) the **upcoming**
   dates on the artist's official list page. A tour is eligible for that session only if:
   - all three sets are identical, matched by date and town;
   - every Tour Map date is placeable (no R18 control shown), so the map is what is tested; and
   - at least 4 dates remain upcoming `[Assumption: A10]`.

   Ineligible tours are left out of that session, with the reason recorded. This catches tours
   that have finished or shrunk since R2, dates added to or removed from any of the three
   sources, and changes to the Bandsintown map. If the Bandsintown map no longer exists (R4 AC2),
   set (ii) is skipped.
2. **Replenishing tours.** If fewer than 3 tours are eligible, the owner may check further current
   UK tours using the R2 comparison method and the step 1 rule, recorded in the R25 ticket, until
   3 are eligible or 10 more have been checked.
3. **Testers:** 5 people who go to gigs (the brief's friends). `[Assumption]` Friends may be
   biased towards the owner's product (K11). Each tester's result is recorded separately so one
   outlier is visible. Before the first trial each tester is asked, and their answer recorded
   word for word: "In the last 12 months, have you tried to work out which date of a UK tour you
   could get to? If so, how did you do it?"
4. **Tasks and towns.** For each tour, three named UK towns that are not venue towns (for example,
   "Which date is nearest to Shrewsbury?"). Each town is chosen so that:
   - by straight-line distance, its nearest upcoming venue is at least 10% closer than the
     second-nearest, so the correct answer is unambiguous; and
   - no already-played date on the official list is nearer to it than the correct answer, so the
     list condition is not penalised for showing past dates.

   The correct answer is fixed against that session's verified date set (step 1).
   **Density task:** where a tour has two or more pins whose tap targets overlap at Tour Map's
   default framing at 360×640 (step 8), at least one of that tour's three towns is chosen so that
   its correct answer is one of the overlapping pins.
5. **Conditions and rotation.** The conditions are: Tour Map (`/artist/{id}`); the Bandsintown
   artist page (map); and the artist's official list page. Each tester does every tour in every
   condition, using a different town for each condition. Condition order, and which town goes
   with which condition, both rotate between testers so that each town is used with each
   condition at least once across the 5 testers.
6. **Device:** the tester's own phone. `[Decision]` The brief names "how the map reads on a
   phone" as one of the experiment's questions `[Evidence: ideation/tour-map.md, "Purpose"]`.
   The phone is also the smaller screen, where pin density matters most. `[Assumption: A11]` A
   result on phones is not assumed to carry over to laptops. §3's laptop use is not tested in the
   MVP, and the G1/G6/G7 results are reported as phone-only.
7. **Per-trial measures:**
   - time and correctness, as in §4 G1;
   - the ease rating, as in §4 G6;
   - for Tour Map trials only, taken from the screen recording: the number of pin taps before
     the tester answers, and whether they zoomed in before answering.

   After all trials, each tester ranks the three conditions from easiest to hardest (G6).
8. **Density record.** On each session day, for each eligible tour, the owner takes a screenshot
   of Tour Map's default framing at 360×640. They record the number of pins whose 44×44 tap
   targets overlap another pin's.
- **AC1:** Given the sessions are complete, then the results sheet holds a time, a
  correct/wrong result and an ease rating for every tester × tour × condition, plus each tester's
  ranking and screener answer. The write-up states the G1 verdict with conditions (a)–(c) shown,
  and the G6 and G7 verdicts.
- **AC2:** Given any session, then a step 1 record exists for that day showing the three date
  sets for each tour used, made before the first trial. Any trial on a tour without a matching
  record for that day is excluded from analysis, and the exclusion is noted.
- **AC3:** Given that fewer than 3 tours are eligible after step 2, or fewer than 5 testers are
  available, then the test is postponed, and the reason is recorded. Postponement ends at the G1
  timeframe (4 weeks after going live). If the full test has not run by then, the owner records
  one of two outcomes in the R25 ticket within a week: (i) run with what is available (at least
  1 eligible tour and 3 testers), with the shortfall recorded as a limitation and G1(b) and the
  G6 ranking condition requiring all but one tester; or (ii) close the experiment as
  inconclusive. §4 treats both as defined outcomes.
- **AC4:** Given the sessions are complete, then the results sheet holds the step 8 overlap count
  per tour. For every Tour Map trial whose correct answer is an overlapping pin, it holds the
  time, the correctness, the pin-tap count and the zoom-before-answer flag. The write-up states a
  descriptive density finding, as input to the post-MVP clustering decision (§8): whether
  overlap trials were slower or less accurate than the tester's other Tour Map trials. There is
  no pass/fail threshold. If no eligible tour has overlapping pins, that is recorded, and the
  density question stays open.

### Non-functional constraint applying across groups C–D

**R26. Load time.** From submitting a search with a single match (or opening `/artist/{id}`) to
pins rendered: median ≤3 seconds over 5 runs, uncached, with Chrome DevTools "Fast 4G" throttling,
for a tour of ≤50 events.
- **AC1:** Given the conditions above, then the median of 5 measured runs is ≤3.0s, recorded in
  the ticket.

## 7. Edge Cases & Failure States

| ID | Situation | Behaviour |
|---|---|---|
| E-R1/R2 | Pre-build stop conditions met | No group C–E ticket starts; the product owner decision is recorded in the R1/R2 ticket (R1 AC2, R2 AC2). |
| E-R6a | Upstream reports more than 10 attractions (R6 `truncated: true`) | The first 10 in upstream relevance order are shown with the hint "Showing the top 10 matches. Add more of the name to narrow it down." |
| E-R6b | Name matches a tribute act or a same-named act | Shown in the R11 picker list with genre where available. `[Assumption: A7]` the visitor can tell which act is which by name and genre. No automatic filtering of tributes. `[Assumption]` the API has no reliable tribute flag; the research did not cover this. |
| E-R6c | Exactly 1 match that turns out to be the wrong act | The visitor uses "Search again" (R16). The R11 auto-jump is accepted at this cost. |
| E-R7a | Artist has 0 upcoming GB events on Ticketmaster | Map shows the UK framing (R14) with header "0 UK dates on Ticketmaster" and the message "Ticketmaster doesn't list any upcoming UK dates for {artist}. Dates sold elsewhere won't appear here. Check the artist's official site." The R17 notice stays. Never "not touring". |
| E-R7b | Upstream reports more than 200 events | The first 200 by date are shown, with a line under the header: "Showing the first 200 dates." |
| E-R7c | Past dates in the response | Filtered out by the server (R7 AC1). |
| E-R7d | Event with no `ticketUrl` | The date is shown without a ticket link and with the text "No ticket link available". |
| E-R7e | Cancelled or postponed event status present in the data | `[Assumption]` the API marks status. Such events are shown with "Cancelled" or "Postponed" next to the date and no ticket link. To be confirmed in R1 (AC3). |
| E-R9a | Daily limit reached (R9) | Uncached requests get 503 `daily-limit`, and the browser shows "Tour Map has reached its daily data limit. Try again tomorrow." No retry button and no automatic retry. |
| E-R9b | Request refused by the per-second queue (R9) | 503 `busy` with `Retry-After: 5`. The browser shows "Tour Map is busy right now. Try again in a few seconds." with a "Try again" button that re-requests once per tap. No automatic retry. |
| E-R10 | Upstream timeout (8s), 5xx or 429 | One retry after 1 second, counted by R9 like any other call. If that fails, the endpoint returns 502. During artist search (R6) the browser shows "Couldn't search Ticketmaster just now."; on an artist map (R7) it shows "Couldn't load tour dates from Ticketmaster just now." Both have a "Try again" button that re-requests once per tap. No automatic retry loops. |
| E-R10b | API key revoked or invalid (upstream 401/403) | No retry. The browser shows the E-R10 message for the request type. The server logs "auth-failure" at error level so the owner can see it. Recovery is a manual key change. |
| E-R10c | Map tiles fail to load or the MapTiler free plan is suspended for the month | Pins and panels still render on a plain background. A banner reads "Map background unavailable. Dates are still shown." `[Assumption]` MapLibre can render markers without tiles; confirmed at Design. |
| E-R12 | Unknown or denylisted attractionId in the URL | Landing page with the message "We couldn't find that artist. Try searching by name." |
| E-R13 | Several venues very close together (for example, in London) | Pins stay separate (R13 AC1). At the default framing their tap targets may overlap, and the visitor zooms in. Whether this slows the core job is measured by the R25 density task and density record (R25 steps 4, 7 and 8; AC4). Clustering is a post-MVP variation (§8). |
| E-R14 | All events at one venue | Under R13 Option G: 1 pin with a badge showing the date count. Under Option S: one pin per date in a ring around the venue. In both cases the map centres on the venue at zoom 10 (R14 AC2). |
| E-R18 | Every event is unplaceable | The map shows the UK framing with no pins. The R18 control lists all dates, and the header still counts them. |
| E-R19 | Device offline | The browser shows "You're offline. Reconnect and try again." The search button stays active and retries when tapped. |
| E-R19b | Location permission | Not requested. The MVP never asks for the visitor's location (§9), so there is no permission-denied state. |
| E-R22 | Removal request arrives | R22 runbook: denylist, deploy, cache purge, verify, all within 24 hours of receipt (drilled in R22 AC3). |
| E-limits | Search query input | Trimmed. 2–100 characters (R6 AC2). Special characters are URL-encoded and passed through. |

## 8. Post-MVP / Future Considerations

Each of these is deferred deliberately. None starts unless G1 is met. Where G6 or G7 is missed,
the owner's recorded decision under §4 must exist first.

- **Distance from a location or postcode.** `[Decision: product owner]` Post-MVP. It needs a
  geocoding provider (more terms and cost) and possibly location permission, and it tests a
  different question from the core map-UX one.
- **Clustering and other density variations** (for example, clustering above a zoom level, or
  expanding a cluster on hover). Deferred so the MVP measures a no-clustering baseline first. The
  R25 density finding (AC4) is the input to this decision. Research suggests clustering trades
  data visibility for tidiness `[Evidence: market-research.md]`.
- **Affiliate ticket links.** Deferred. They would make the site commercial, which conflicts with
  MapTiler's free plan and needs a fresh Ticketmaster terms review (R10).
- **Additional data sources** (See Tickets, DICE, AXS and others) to reduce coverage gaps.
  Deferred. Each brings its own terms and access process. The Bandsintown API is single-artist
  and needs written consent `[Evidence: market-research.md, Other event data terms]`, and it is
  excluded anyway by the no-Bandsintown-data decision.
- **Laptop comparison test.** Deferred. R25 measures phones only (A11).
- **Tours outside the UK and non-music events.** Deferred. Scope was fixed as music, UK only.
- **Gift-buyer features** (for example, a recipient's town). Deferred. The segment is unevidenced
  (§3).

## 9. Non-Goals (MVP)

- **No user accounts, sign-in, favourites, alerts or uploads** `[Decision: product owner]`.
- **No distance, postcode or "near me" features, and no location permission request**
  `[Decision: product owner]`.
- **No ticket purchase, checkout, seat selection, price display or availability display.** Tour
  Map links out to Ticketmaster and stops there. Prices and availability are excluded to stay
  clearly away from replicating Ticketmaster's essential experience (R3(a)) and to avoid showing
  stale data.
- **No claim of complete tours and no gap detection** (R17).
- **No data source other than the Ticketmaster Discovery API**, and no scraping
  `[Decision: ideation/decision.md, brief]`.
- **No Bandsintown name, branding, design or data** beyond the R21 non-affiliation statement
  `[Decision: product owner]`. (R25 testers view Bandsintown's public site directly as a
  comparator; Tour Map itself never requests or shows its data.)
- **No affiliate links, ads or revenue features** (R10).
- **No events outside the UK, and no non-music events.**
- **No clustering** (R13).
- **No native mobile app.** The MVP is a responsive website only.
- **Not competing with Bandsintown for users.** There are no acquisition, SEO or growth goals.

## 10. Dependencies, Risks & Assumptions

### Dependencies

| Dependency | Needed for | Status |
|---|---|---|
| Ticketmaster Discovery API v2 standard key, GB access | R1, R5–R9 | Not yet confirmed (R1) |
| Ticketmaster Developer terms, full text | R3, then R8, R10, R21, R22 | Not yet read; the developer site didn't resolve during research |
| MapTiler Cloud free plan and terms | R10 | Not yet read from the official site |
| MapLibre GL JS | R10, R13–R20 | Open source, BSD-3-Clause per package metadata |
| Hosting that supports a server-side function and secret storage | R5, R8, R9, R22 | Chosen at the Design gate |
| 5 tester volunteers | R25 | The owner's friends; not yet asked |
| **3 test tours that are still upcoming, still fully covered and still identical across Tour Map, Bandsintown and the official list on each R25 session day** | R25, G1, G6, G7 | Candidates come from R2 AC1; re-verified on each session day (R25 step 1); topped up by R25 step 2 if too few remain (K13) |
| Product owner time to run R1–R4 and R25 | Gate items | Side-project time, no committed schedule. This is why test-tour eligibility is re-checked at test time, not assumed from R2 |
| Product owner answer to Q2 (same-venue pins) | R13, R15, R20 | Open; Option G is built by default if unanswered when R13 starts |
| Product owner contact email for removal requests (Q6) | R21 | Open |
| Tracker with one issue per requirement | Squad traceability | Per the project's working model |

### Risks

| # | Risk | Type | Likelihood | Impact | Mitigation |
|---|---|---|---|---|---|
| K1 | Ticketmaster covers too few tours fully to run a fair test | Data | Medium (5 of 12 major arenas `[Evidence: market-research.md, unverified]`) | Experiment can't answer G1 | R2 runs before any UI work, with a stop default decided in advance and no partial-coverage override |
| K2 | **Strongest case against this plan:** the G1 test measures data completeness or clutter, not map UX, because a tester finds the date faster in whichever condition shows fewer or different dates | Validity | High without controls | G1 result is meaningless | Partial-coverage tours are never tested (R2 AC2). On each session day all three conditions' date sets must be identical (R25 step 1). Towns are chosen so no past date on the list is nearer than the answer (R25 step 4). Correct answers are fixed against the verified set. Condition order and town assignment rotate (R25 step 5) |
| K3 | Fans read missing dates as "no show near me" and draw the wrong conclusion | User harm / trust | Medium | Wrong decision by a fan | R17 permanent notice and wording bans; E-R7a empty-state wording |
| K4 | Terms conflict, for example caching banned, a required attribution form, or the replication clause read as covering this site | Legal | Unknown until R3 | Rework, or the experiment stops | R3 before build; R8 TTL configurable to 0; no purchase or price features (§9) |
| K5 | API key revoked, or terms changed without notice | Legal / technical | Low–Medium | Site stops working | E-R10b error state; R9 keeps usage well under limits; no background calls |
| K6 | Standard key returns no GB events | Technical | Unknown | No data source | R1 first, with a stop default |
| K7 | Artist search returns the wrong act | Product | Medium | Wrong tour shown | R11 picker for 2+ matches; artist name shown in the header (R16) |
| K8 | UK venues lack coordinates | Data | Unknown | Dates missing from the map | R18 unplaced-dates list; G3 measured in R2 and on each R25 session day |
| K9 | Map tile free plan exceeded, or provider terms change | Technical / cost | Low at hobby traffic `[Assumption: A6]` | Map background lost for the month | Suspension rather than charge (no bill); E-R10c fallback |
| K10 | Bandsintown map changed or removed, so there's no comparator | Validity | Unknown | G1 and G6 lose one comparison | R4 records it; R25 step 1 re-checks it on each session day; R25 falls back to list-only (R4 AC2) |
| K11 | Friendly testers are biased | Validity | Medium | G1 and G6 results overstate the benefit | Per-tester results recorded; ≥4 of 5 conditions; finding reported as indicative, not conclusive |
| K12 | The idea isn't new (Tour Radius, MusicMap and others) | Market | Known | None on a learning project | No novelty claim; R4 records any live tools as reference |
| K13 | **Test tours expire or drift between R2 and R25.** The build is unscheduled side-project time, so R2's tours may have finished, lost dates to upcoming-only filtering, or gained dates on the official list or Bandsintown that Ticketmaster doesn't have | Validity / schedule | High over a multi-month build `[Assumption]` | Too few eligible tours, or a G1 that silently measures completeness | R2 prefers tours running ≥3 months past the check and keeps every fully covered tour as a candidate (R2 AC1); R25 step 1 re-verifies all three date sets on each session day; R25 step 2 tops up with newly checked tours; R25 AC2 excludes unverified trials; R25 AC3 postpones up to the G1 timeframe, then forces a recorded outcome |
| K14 | Close-together venues (for example, London) overlap at default framing, so Tour Map's no-clustering choice slows the core job | Product / validity | Unknown | Tour Map slower on bunched tours | Measured, not assumed: R25 density task and density record (AC4) feed the post-MVP clustering decision |

### Assumptions (if wrong, the plan changes)

- A1: Fans who follow touring artists share the owner's list-reading friction (n=1 today). The
  R25 screener and G1/G6 are the first tests of it.
- A2: Ticketmaster's terms allow short-lived server-side caching and a single-purpose map view
  that links out for purchase (R3).
- A3: The maximum events page size is 200 and no UK tour exceeds it (R1, R7).
- A4: `countryCode=GB` covers events in Northern Ireland (R1 checks one if available).
- A5: A 30% median difference with 5 testers is a reasonable bar for "clearly quicker" on a
  hobby experiment. It is not statistically significant.
- A6: MapTiler free-plan limits are enough at hobby traffic (R3(h) confirms the official numbers).
- A7: The visitor can tell acts apart by name and genre in the picker.
- A8: "Within seconds" in the brief is read as a Tour Map median of 10 seconds or less (G7). The
  brief gives no number. The product owner can change the figure before R25 runs.
- A9: The build and R25 can finish within about 3 months of R2, so tours chosen to run 3 months
  past the check are still running. If this is wrong, R25 step 2 tops up the tours.
- A10: A tour with at least 4 upcoming dates still makes "pick the nearest date" a real task.
- A11: Phone results are not assumed to carry over to laptops; R25 measures phones only.

## 11. Open Questions

| # | Question | Who answers | Needed by |
|---|---|---|---|
| Q1 | What do the full Ticketmaster terms say on attribution, caching, display rules and commercial use? | Product owner via R3 | Before Build |
| Q2 | Should dates at the same venue share one pin with a count badge (R13 Option G), or each get their own pin spread in a ring (Option S)? Which reading of "a pin per tour date" does the product owner intend? | Product owner | Before the R13 ticket starts; Option G is built if unanswered |
| Q3 | Does the API mark cancelled or postponed events, and how (E-R7e)? | R1 (AC3) | Before R7 |
| Q4 | Which hosting platform provides the server-side function and secret store? | Design gate | Design |
| Q5 | Does Bandsintown's artist-page map still exist in the form described in 2022? | R4, re-checked in R25 step 1 | Before R25 |
| Q6 | What contact email address is published for removal requests (R21)? | Product owner | Before the R21 ticket starts |
