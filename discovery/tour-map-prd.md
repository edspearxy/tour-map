# PRD: Tour Map

- **Status:** Draft for Discovery review (first draft)
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
tour view let a fan pick a reachable date more quickly than reading a list or using Bandsintown's
artist-page map? The second is **learning to integrate a real third-party API** within its terms.
It does not aim to win users from Bandsintown and has no revenue target
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
  map is harder to navigate" as **a hypothesis to test** (§4, G1; R4, R25), not a fact.
- **Other map-first single-artist tools exist or have existed**: Tour Radius, MusicMap, Concert
  Map, the Artist Map GitHub project, ROSTR's Live tab and Gigora. None was confirmed live in this
  session `[Evidence: market-research.md, "Other single-artist or tour-map products"]`. So a
  map-first tour view is **not a new idea**, and this PRD makes no novelty claim. What this
  project tests that those tools don't answer is a measured comparison: whether a specific set of
  map-UX choices (default framing, no clustering, a details panel, mobile full-screen) makes "find
  my reachable date" measurably quicker on the same tours. Where any of those tools turn out to be
  live, R4 records them as reference points before build.

**How painful is it?** The evidence for pain is the product owner's own repeated experience
(n=1) `[Evidence: ideation/tour-map.md, "Why now"]`. No research compares map and list usability
for this task `[Evidence: market-research.md, "Map vs list usability: no sourced evidence
found"]`. `[Assumption]` Other UK gig-goers who follow touring artists share the same friction.
The validation test (§4) is the first point at which this assumption meets data from anyone else.

**Context: the data constraint is part of the problem space.** Ticketmaster does not list every
UK date. Ticketmaster says it partners with 5 of 12 major UK arenas `[Evidence: market-research.md,
UK ticketing market share, committee written evidence 2025, unverified]`. A 2026 committee report
attributes 58% of UK primary ticket sales to Live Nation (66% including affiliates), figures Live
Nation disputes `[Evidence: same section, unverified, dated 2026]`. Any tour sold partly through
See Tickets, DICE, AXS or others will be incomplete on this map. The API cannot report dates it
doesn't hold, so **the product cannot detect or show gaps**. It can only say plainly that it shows
Ticketmaster-listed dates alone (R17). The product owner has accepted this for the MVP
`[Decision: ideation/decision.md]`.

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

**Experiment owner: the product owner, as the learner.** The project exists to learn about map-UX
and API integration. Success for this user is a clear, recorded answer to G1 and a working
integration that stays within the API terms (G2–G5).

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
| G2 | The data source can support the experiment at all | Number of sampled UK tours where Ticketmaster lists every date on the artist's own official list | Unknown. No source measures Discovery API tour completeness `[Evidence: market-research.md]` | ≥3 fully covered tours found from up to 10 checked | Before any map UI ticket starts (R2) |
| G3 | Dates that are returned actually reach the map | % of returned UK events with valid venue coordinates (placed as pins) | Unknown. No source on UK venue coordinate population `[Evidence: market-research.md]` | ≥95% across the tours used in R2 | Measured during R2, re-checked at the R25 test |
| G4 | Real use by the owner | Number of real ticket decisions (own or someone else's) where the owner used Tour Map to pick the date | 0 (product doesn't exist) | ≥3 | Within 3 months of going live |
| G5 | Integration stays within API limits | Days on which upstream Ticketmaster calls exceed 4,500 (guard threshold, R9), and number of HTTP 429 responses received | 0 (no integration yet) | 0 days over threshold; ≤5 × 429 responses per week | Continuously from going live, reviewed at 4 weeks |

**How each metric is measured**
- **G1:** moderated sessions run by the product owner using the protocol in R25. Each trial is
  timed by screen recording, from the moment the artist's page has finished loading (artist
  search is excluded so all three conditions start equal) until the tester states a date out loud.
  The correct answer for each task is worked out beforehand by straight-line distance from the
  named town to each venue. Results go in a results sheet saved in the project repository
  (`validation/g1-results.*`, format chosen at Build).
- **"Clearly quicker", defined** `[Decision]`: all three conditions must hold. (a) The Tour Map
  median time across all trials is at least 30% lower than the Bandsintown-map median **and** at
  least 30% lower than the list median. (b) At least 4 of the 5 testers have a lower personal
  median on Tour Map than on both comparators. (c) The share of correct answers on Tour Map is no
  lower than on either comparator. `[Assumption]` 30% is set high enough to be noticeable above
  stopwatch noise with five testers. It is a judgement call, not statistical significance. If the
  Bandsintown map no longer exists (R4), G1 is judged against the list alone and the PRD outcome
  records that.
- **Consequence if G1 fails** `[Decision, from the brief's stop rule]`: no post-MVP work (§8)
  starts. The product owner decides between closing the experiment and trying one documented UX
  variation and re-running R25.
- **G2:** a manual comparison recorded in the R2 ticket. For each tour, list the dates on the
  artist's own official tour page and the events the Discovery API returns, then mark the tour
  "fully covered" or "partial" with the missing dates named.
- **G3:** server-side counters (events returned, events placed, events unplaced) logged per
  artist request (R7, R18). Read from logs for the R2 tours.
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

**Sequencing constraint** `[Decision]`: R1–R4 complete before any ticket in groups C–E starts.
R1 and R2 have stop conditions that can end or change the build.

### A. Pre-build validation (gates the build)

**R1. Confirm UK access on a standard Discovery API key.**
Register a standard Ticketmaster Discovery API v2 key. Using it, request events with
`countryCode=GB` and `classificationName=music` for 3 artists known to have upcoming UK
Ticketmaster dates. Record the request URLs, event counts and whether venue `location.latitude`
and `location.longitude` are present.
- *Why:* research could not confirm GB results on a standard key, and the separate International
  Discovery API reportedly no longer takes new key requests `[Evidence: market-research.md,
  unverified]`.
- **AC1:** Given a standard key, when the 3 requests are made, then each returns ≥1 GB event and
  the results are recorded in the R1 ticket. → Pass: the build proceeds.
- **AC2:** Given a standard key, when any request returns no GB events for an artist known to
  have them, or returns an access error, then the result is recorded and no group C–E ticket
  starts until the product owner has decided what to do (the default is to stop).

**R2. Coverage check on real UK tours (G2, G3).**
Choose up to 10 current or announced UK music tours. The first is the tour in the brief's example
(Cardiff Utilita Arena, London Eventim Apollo, Bristol Hippodrome, Halifax Piece Hall). For each
tour, compare the Discovery API's GB events for the artist against the artist's official tour
page. Record each tour as "fully covered" or "partial", list any missing dates, and count events
without usable coordinates.
- **AC1:** Given the check is complete, when ≥3 tours are fully covered, then those tours become
  the R25 test tours and the build proceeds.
- **AC2:** Given 10 tours have been checked, when fewer than 3 are fully covered, then the build
  of groups C–E does not start. `[Decision, made in advance]` The default outcome is **stop**.
  The product owner may override this in writing in the R2 ticket with one of: switch data source
  (needs a revised PRD), or run R25 on tours where Ticketmaster is partial **and** the Bandsintown
  map shows the same set of dates. Hand-entered data is not an option, because it would break
  the "no uploads" and single-source decisions.
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
For each tour chosen in R2, open the artist's Bandsintown page on a phone and on a laptop.
Record, with dated screenshots and short notes: whether an event map exists; how it is reached
from the artist page; its default framing; whether every date is visible as a pin; and the number
of dates it shows. Also try to open Tour Radius, MusicMap and Concert Map and record whether each
is live, with a screenshot if so.
- *Why:* this turns "hard to navigate" from recollection into a recorded baseline, and confirms
  the comparator exists `[Evidence: synthesis-2026-11-16-2.md, fixes 4–5]`.
- **AC1:** Given R4 is complete, then the ticket holds dated screenshots for every R2 test tour on
  both device types, plus a live/not-live status for the three named tools.
- **AC2:** Given the Bandsintown event map no longer exists, then R25 runs with two conditions
  (Tour Map vs list) and G1 is judged on that basis.

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
`classificationName=music`. It returns at most 10 candidates, each with `attractionId`, `name`
and, if present, genre. The query must be 2–100 characters after trimming.
- **AC1:** Given q="Coldplay", when called, then the response is a JSON array of ≤10 objects,
  each with non-empty `attractionId` and `name`.
- **AC2:** Given q is shorter than 2 or longer than 100 characters after trimming, then the
  endpoint returns HTTP 400 and makes no upstream call.
- **AC3:** Given the upstream returns no attractions, then the endpoint returns HTTP 200 with an
  empty array.

**R7. Artist events endpoint.**
`GET /api/artists/{attractionId}/events` calls the Discovery events endpoint with that
`attractionId`, `countryCode=GB`, `classificationName=music`, upcoming events only, sorted by date
ascending, page size 200, first page only. For each event it returns: `eventId`, `localDate`,
`localTime` (nullable), `venueName`, `city`, `latitude` and `longitude` (parsed to numbers,
nullable), `ticketUrl`, and a `placeable` flag (see R18). It also returns `attractionName` and
`truncated: true` if the upstream reports more than 200 events.
- *Why the page cap:* the API caps size × page below 1,000 `[Evidence: market-research.md,
  unverified]`. `[Assumption]` 200 is the maximum page size and no UK tour exceeds it. R1
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
  rule]`. `[Assumption]` Ticketmaster's terms allow short-lived caching. R3 confirms this; if they
  don't, the TTL is set to 0.
- **AC1:** Given the same request twice within the TTL, then the upstream is called once.
- **AC2:** Given TTL=0, then every request calls the upstream.

**R9. Upstream rate guard.**
The server sends no more than 5 upstream requests per second (excess requests are queued for up
to 2 seconds, then refused). It keeps a daily upstream-call counter that resets at 00:00 UTC.
From 4,500 calls in a day, it stops making new upstream calls: cached responses are still served,
and uncached requests get HTTP 503 with `reason: "daily-limit"`. It logs the daily count and the
number of upstream 429 responses (G5).
- *Why:* free-tier limits of 5,000 calls/day and 5/second `[Evidence: market-research.md,
  unverified]`; the terms allow throttling of heavy non-user-driven use `[Evidence: same, secondary,
  unverified]`. The 500-call margin leaves room for R1/R2 manual checks on the same key.
  `[Decision]` No calls are made except in response to a visitor action: no pre-fetching or
  background crawling.
- **AC1:** Given 20 simultaneous uncached requests, then no more than 5 upstream calls start in
  any one-second window.
- **AC2:** Given the counter is at 4,500, when an uncached request arrives, then HTTP 503 with
  `reason: "daily-limit"` is returned and no upstream call is made. When a cached request arrives,
  then it is served.

**R10. Map library and tile provider.** `[Decision: resolves "map tile provider choice and
licence" in ideation/decision.md]`
Use **MapLibre GL JS** (BSD-3-Clause) with **MapTiler Cloud's free plan** for tiles and style,
with MapTiler and OpenStreetMap attribution shown on the map at all times.
- *Why this choice:* MapLibre is open source `[Evidence: market-research.md, npm metadata]`.
  MapTiler's free plan suspends maps for the rest of the month when exceeded instead of charging,
  so a side project carries no surprise bill. It is limited to non-commercial use, which fits an
  MVP with no revenue or affiliate links `[Evidence: market-research.md, MapTiler terms via
  search summary, official numbers not read]`. Alternatives rejected: the OSM standard tile
  server, because its policy discourages application use, needs permission for heavy use and can
  block without notice; Mapbox, because GL JS is no longer open source and sources disagree on
  whether a card is needed; Google Maps Platform, because it needs a billing account
  `[Evidence: market-research.md, Map provider costs and terms]`.
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
The landing page shows a full-screen map framed on the UK (bounds roughly 49.8°N 8.7°W to
60.9°N 1.8°E) with a search box over it. Submitting (Enter key or Search button) calls R6. There
is no type-ahead, so each search costs one API call.
- **AC1:** Given 0 results, then the message "No artist found for '{q}'. Check the spelling or
  try another name." is shown and the search box keeps the query.
- **AC2:** Given exactly 1 result, then the browser navigates to `/artist/{attractionId}` with no
  extra step.
- **AC3:** Given 2–10 results, then a selectable list of artist names (with genre where present)
  is shown, and choosing one navigates to `/artist/{attractionId}`.
- **AC4:** Given a query shorter than 2 characters, then the Search button is disabled and no
  request is sent.

**R12. Shareable artist URL.**
`/artist/{attractionId}` renders the artist map directly when opened from a link, a bookmark or a
reload, with no search needed.
- *Why:* R25 test sessions open each tour directly, and fans share links.
- **AC1:** Given a valid attractionId URL opened in a new browser session, then the artist map
  renders with the same pins as when reached through search.
- **AC2:** Given an invalid or unknown attractionId, then the §7 E-R12 state is shown.

### D. Artist map

**R13. One pin per placeable venue; every placeable date represented.**
Each event with `placeable: true` is shown on the map. Events at the **identical** venue
coordinates (for example, three nights at one venue) share one pin with a count badge ("3").
Events at different coordinates always get separate pins. **No clustering** of different
coordinates in the MVP.
- *Why:* product-owner decision of "a pin per tour date". Separate pins at identical coordinates
  would sit exactly on top of each other and hide all but one, so they are grouped and the badge
  keeps every date accounted for. Clustering is left out because research suggests it hides data
  and adds clicks when points are few, and is meant for many points in one region
  `[Evidence: market-research.md, Map UX patterns, search summaries]`. Clustering is kept as a
  post-MVP variation (§8).
- **AC1:** Given a tour of N placeable events across V distinct coordinate pairs, then exactly V
  pins are rendered and the badge counts sum to N.
- **AC2:** Given two venues 2km apart in London, then two pins are rendered at all zoom levels.

**R14. Default framing fits the whole tour.**
On load, the map fits the bounds of all pins with 48px padding (plus the height of the header and
notice), with maximum zoom 12. With 1 pin: centre on it at zoom 10. With 0 pins: show the R11 UK
framing.
- **AC1:** Given pins in Cardiff, London, Bristol and Halifax, then on first render all four are
  inside the visible map area at 360×640 and at 1366×768, with no pin under the header or notice.
- **AC2:** Given 1 pin, then the map is centred on it at zoom 10.

**R15. Pin details panel.**
Tapping or clicking a pin, or pressing Enter on a focused pin, opens a details panel listing each
date at that pin, earliest first. Each date shows: the date as "Sat 14 Mar 2027" (the event's
local date, not time-zone converted), the start time as "19:30" if known, the venue name, the
town, and a link "Tickets on Ticketmaster" to that event's `ticketUrl`, which opens in a new tab
(`rel="noopener"`). Only one panel is open at a time. It closes with its close button, a tap on
empty map, or the Escape key. On viewports narrower than 768px the panel is a bottom sheet that
covers no more than 50% of the height. On wider viewports it is a side panel.
- **AC1:** Given a pin with 3 dates, when tapped, then 3 entries are shown in date order, each with
  its own ticket link to the right `ticketUrl`.
- **AC2:** Given an event with no time, then the time is left out (no "00:00" or "TBC" invented).
- **AC3:** Given a panel is open, when another pin is tapped, then the first panel closes and the
  second opens.

**R16. Artist header.**
A header shows the artist name (`attractionName`), "N UK dates on Ticketmaster" (N = all returned
events, placeable or not), and a "Search again" control that returns to the landing page.
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
An event is unplaceable if its coordinates are null, non-numeric, exactly 0,0, or outside the
UK bounding box in R11. Unplaceable events are left off the map and listed behind a control
reading "N dates not shown on the map". Opening it shows the same fields and ticket link as R15.
The control appears only when N ≥ 1.
- **AC1:** Given 1 event with null coordinates, then it doesn't appear as a pin, the control reads
  "1 date not shown on the map", and opening it shows that event with its ticket link.
- **AC2:** Given all events are placeable, then the control isn't rendered.

**R19. Mobile behaviour.**
The map fills the viewport and the page itself does not scroll, so the map can't trap a page
scroll. One-finger pan and pinch-zoom work on touch screens. Each pin's tap target is at least
44×44 CSS px.
- *Why:* small embedded maps that capture swipes are a known mobile problem; a full-page map avoids
  it without cooperative gestures `[Evidence: market-research.md, Google 2016 blog]`.
- **AC1:** Given the latest iOS Safari and Android Chrome at 360×640, then pan, pinch-zoom, pin
  tap and the R15 bottom sheet all work, and no page-level scroll happens.

**R20. Keyboard and screen-reader access to pins.**
Pins can be reached by keyboard (Tab, in date order) and each has an accessible name of the form
"Sat 14 Mar 2027, Eventim Apollo, London" (or "3 dates at Eventim Apollo, London" for grouped
pins). The search box, results list, details panel and R18 control are all operable by keyboard.
- **AC1:** Given keyboard-only use, then a user can search, pick an artist, move between pins,
  open a panel and reach a ticket link without a mouse.

### E. Compliance, attribution and privacy

**R21. Data attribution, independence statement and removal contact.**
Every page shows "Event data from Ticketmaster" (or the exact attribution form R3(d) requires)
and a link to an About panel. The About panel states: Tour Map is an independent hobby project,
not affiliated with or endorsed by Ticketmaster or Bandsintown; dates come only from Ticketmaster;
and a contact email address for corrections and removal requests.
- **AC1:** Given any page, then the Ticketmaster attribution and the About link are visible, and
  the About panel contains all three statements and a working `mailto:` link.

**R22. Content removal within 24 hours.** `[Decision: implements the 24-hour removal duty found
in research]`
A server-side denylist (configuration file or environment setting) of Ticketmaster `eventId`s and
`attractionId`s is applied to all R6 and R7 responses. A runbook in the repository covers: add
the ID, deploy, purge the cache. The steps take no more than 30 minutes in total.
- *Why:* Ticketmaster-requested removals must happen within 24 hours `[Evidence: market-research.md,
  terms snippet, unverified, dated 2023]`.
- **AC1:** Given an eventId on the denylist, then it is absent from R7 responses immediately after
  deploy, including responses that were previously cached.
- **AC2:** Given an attractionId on the denylist, then R6 never returns it and R7 returns 404 for
  it.

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

**R25. Run the side-by-side comparison test (G1).** `[Decision: resolves "design the side-by-side
comparison on fully covered tours, and define clearly quicker" in ideation/decision.md]`
Protocol:
1. **Tours:** 3 tours marked fully covered in R2. Before testing, confirm the Bandsintown map
   (R4) shows the same number of dates as Tour Map for each. Leave out any tour where it doesn't.
   This keeps data completeness from confounding the result.
2. **Testers:** 5 people who go to gigs (the brief's friends). `[Assumption]` Friends may be
   biased towards the owner's product. Each tester's result is recorded separately so one
   outlier is visible.
3. **Tasks:** for each tour, a named UK town that is not a venue town (for example, "Which date is
   nearest to Shrewsbury?"). The correct answer is worked out beforehand (G1 method).
4. **Conditions:** Tour Map (`/artist/{id}`), Bandsintown artist page (map), artist's official
   list page. Each tester does every tour in every condition. Condition order rotates between
   testers, and each condition uses a different town for the same tour, to limit learning
   effects.
5. **Device:** the tester's own phone, because the job happens on phones (§3).
6. **Timing and correctness:** as in §4 G1, recorded per trial.
- **AC1:** Given the sessions are complete, then the results sheet holds a time and correct/wrong
  result for every tester × tour × condition, and the G1 verdict ("clearly quicker" met or not)
  is stated with the three conditions (a)–(c) shown.
- **AC2:** Given fewer than 3 eligible tours or fewer than 5 testers, then the test is
  postponed rather than run with a smaller set, and the reason is recorded.

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
| E-R6a | Upstream returns more than 10 attractions | The first 10 in upstream relevance order are shown with the hint "Showing the top 10 matches. Add more of the name to narrow it down." |
| E-R6b | Name matches a tribute act or a same-named act | Shown in the R11 picker list with genre where available. `[Assumption]` the visitor can tell which act is which by name and genre. No automatic filtering of tributes, because the API offers no reliable flag `[Evidence: market-research.md, artist matching risk]`. |
| E-R6c | Exactly 1 match that turns out to be the wrong act | The visitor uses "Search again" (R16). The R11 auto-jump is accepted at this cost. |
| E-R7a | Artist has 0 upcoming GB events on Ticketmaster | Map shows the UK framing (R14) with header "0 UK dates on Ticketmaster" and the message "Ticketmaster doesn't list any upcoming UK dates for {artist}. Dates sold elsewhere won't appear here. Check the artist's official site." The R17 notice stays. Never "not touring". |
| E-R7b | Upstream reports more than 200 events | The first 200 by date are shown, with a line under the header: "Showing the first 200 dates." |
| E-R7c | Past dates in the response | Filtered out by the server (R7 AC1). |
| E-R7d | Event with no `ticketUrl` | The date is shown without a ticket link and with the text "No ticket link available". |
| E-R7e | Cancelled or postponed event status present in the data | `[Assumption]` the API marks status. Such events are shown with "Cancelled" or "Postponed" next to the date and no ticket link. To be confirmed in R1. |
| E-R9 | Upstream rate limit or daily limit reached | Daily limit: uncached requests get 503 `daily-limit`, and the browser shows "Tour Map has reached its daily data limit. Try again tomorrow." Upstream 429: treated as E-R10. |
| E-R10 | Upstream timeout (8s), 5xx or 429 | One retry after 1 second. If that fails, the endpoint returns 502 and the browser shows "Couldn't load tour dates from Ticketmaster just now." with a "Try again" button that re-requests once per tap. No automatic retry loops. |
| E-R10b | API key revoked or invalid (upstream 401/403) | No retry. The browser shows the E-R10 message. The server logs "auth-failure" at error level so the owner can see it. Recovery is a manual key change. |
| E-R10c | Map tiles fail to load or the MapTiler free plan is suspended for the month | Pins and panels still render on a plain background. A banner reads "Map background unavailable. Dates are still shown." `[Assumption]` MapLibre can render markers without tiles; confirmed at Design. |
| E-R12 | Unknown or denylisted attractionId in the URL | Landing page with the message "We couldn't find that artist. Try searching by name." |
| E-R13 | Several venues very close together (for example, in London) | Pins stay separate (R13). At the default framing they may overlap visually. The visitor zooms in. Measured as part of R25; clustering is a post-MVP variation (§8). |
| E-R14 | All events at one venue | Treated as 1 pin (R14 AC2), with the badge showing the date count. |
| E-R18 | Every event is unplaceable | The map shows the UK framing with no pins. The R18 control lists all dates, and the header still counts them. |
| E-R19 | Device offline | The browser shows "You're offline. Reconnect and try again." The search button stays active and retries when tapped. |
| E-R19b | Location permission | Not requested. The MVP never asks for the visitor's location (§9), so there is no permission-denied state. |
| E-R22 | Removal request arrives | R22 runbook: denylist, deploy, cache purge, all within 24 hours of receipt. |
| E-limits | Search query input | Trimmed. 2–100 characters (R6 AC2). Special characters are URL-encoded and passed through. |

## 8. Post-MVP / Future Considerations

Each of these is deferred deliberately. None starts unless G1 is met (§4).

- **Distance from a location or postcode.** `[Decision: product owner]` Post-MVP. It needs a
  geocoding provider (more terms and cost) and possibly location permission, and it tests a
  different question from the core map-UX one.
- **Clustering and other density variations** (for example, clustering above a zoom level, or
  expanding a cluster on hover). Deferred so the MVP measures a no-clustering baseline first;
  research suggests clustering trades data visibility for tidiness `[Evidence: market-research.md]`.
- **Affiliate ticket links.** Deferred. They would make the site commercial, which conflicts with
  MapTiler's free plan and needs a fresh Ticketmaster terms review (R10).
- **Additional data sources** (See Tickets, DICE, AXS and others) to reduce coverage gaps.
  Deferred. Each brings its own terms and access process; the Bandsintown API is single-artist
  and needs written consent `[Evidence: market-research.md, Other event data terms]`, and is in
  any case excluded by the no-Bandsintown-data decision.
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
  `[Decision: product owner]`.
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
| 5 tester volunteers and 3 fully covered tours | R25 | Tours depend on R2; testers are the owner's friends |
| Product owner time to run R1–R4 and R25 | Gate items | Side-project time, no committed schedule |
| Tracker with one issue per requirement | Squad traceability | Per the project's working model |

### Risks

| # | Risk | Type | Likelihood | Impact | Mitigation |
|---|---|---|---|---|---|
| K1 | Ticketmaster covers too few tours fully to run a fair test | Data | Medium (5 of 12 major arenas `[Evidence]`) | Experiment can't answer G1 | R2 runs before any UI work, with a stop default decided in advance |
| K2 | **Strongest case against this plan:** the G1 test measures data completeness or clutter, not map UX, because a tester finds the date faster on whichever map shows fewer or different pins | Validity | High without controls | G1 result is meaningless | R25 uses only fully covered tours with matched date counts on both maps; correct answers are fixed beforehand; condition order rotates |
| K3 | Fans read missing dates as "no show near me" and draw the wrong conclusion | User harm / trust | Medium | Wrong decision by a fan | R17 permanent notice and wording bans; E-R7a empty-state wording |
| K4 | Terms conflict, for example caching banned, a required attribution form, or the replication clause read as covering this site | Legal | Unknown until R3 | Rework, or the experiment stops | R3 before build; R8 TTL configurable to 0; no purchase or price features (§9) |
| K5 | API key revoked, or terms changed without notice | Legal / technical | Low–Medium | Site stops working | E-R10b error state; R9 keeps usage well under limits; no background calls |
| K6 | Standard key returns no GB events | Technical | Unknown | No data source | R1 first, with a stop default |
| K7 | Artist search returns the wrong act | Product | Medium | Wrong tour shown | R11 picker for 2+ matches; artist name shown in the header (R16) |
| K8 | UK venues lack coordinates | Data | Unknown | Dates missing from the map | R18 unplaced-dates list; G3 measured in R2 |
| K9 | Map tile free plan exceeded, or provider terms change | Technical / cost | Low at hobby traffic `[Assumption]` | Map background lost for the month | Suspension rather than charge (no bill); E-R10c fallback |
| K10 | Bandsintown map changed or removed, so there's no comparator | Validity | Unknown | G1 loses one comparison | R4 records it; R25 falls back to list-only (R4 AC2) |
| K11 | Friendly testers are biased | Validity | Medium | G1 result overstates the benefit | Per-tester results recorded; ≥4 of 5 condition; finding is reported as indicative, not conclusive |
| K12 | The idea isn't new (Tour Radius, MusicMap and others) | Market | Known | None on a learning project | No novelty claim; R4 records any live tools as reference |

### Assumptions (if wrong, the plan changes)

- A1: Fans who follow touring artists share the owner's list-reading friction (n=1 today). G1 is
  the first test of it.
- A2: Ticketmaster's terms allow short-lived server-side caching and a single-purpose map view
  that links out for purchase (R3).
- A3: The maximum events page size is 200 and no UK tour exceeds it (R1, R7).
- A4: `countryCode=GB` covers events in Northern Ireland (R1 checks one if available).
- A5: A 30% median difference with 5 testers is a reasonable bar for "clearly quicker" on a
  hobby experiment. It is not statistically significant.
- A6: MapTiler free-plan limits are enough at hobby traffic (R3(h) confirms the official numbers).
- A7: The visitor can tell acts apart by name and genre in the picker.

## 11. Open Questions

| # | Question | Who answers | Needed by |
|---|---|---|---|
| Q1 | What do the full Ticketmaster terms say on attribution, caching, display rules and commercial use? | Product owner via R3 | Before Build |
| Q2 | Does the product owner accept grouping same-venue dates into one pin with a count badge (R13) as meeting "a pin per tour date"? | Product owner | Discovery decision |
| Q3 | Does the API mark cancelled or postponed events, and how (E-R7e)? | R1 | Before R7 |
| Q4 | Which hosting platform provides the server-side function and secret store? | Design gate | Design |
| Q5 | Does Bandsintown's artist-page map still exist in the form described in 2022? | R4 | Before R25 |
| Q6 | What contact email address is published for removal requests (R21)? | Product owner | Before R21 |
