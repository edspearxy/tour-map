# Idea Council: Feasibility lens (v2 brief)

- **Artefact:** `ideation/tour-map.md` (v2, 2026-11-16) + `ideation/market-research.md` (v2 snapshot, 2026-11-16)
- **Reviewer:** idea-council-feasibility
- **Date:** 2026-11-16 (second same-day review; the v1 review was not read)
- **Corrections read:** universal file (no entries yet); project-local file (doesn't exist)

## 1.1 Technical feasibility: 10 / 15

**Justification:** The map side is clearly buildable with tools that already exist. The MVP's whole data layer, though, depends on one source whose fitness for UK tours is unproven: coverage, artist matching and venue coordinates. That is "buildable, but leans on at least one significant unproven component".

**Evidence that it's buildable:**
- Brief: "A website where **the map is the page**... 1. Choose an artist. 2. See the whole UK tour as pins on a full-screen map. 3. Tap a pin for date, venue and a link to buy tickets."
- Brief: "pull their upcoming UK events (date, venue, venue coordinates, ticket link) from Ticketmaster's public, free-tier API... No scraping."
- Market research: "MapLibre GL JS is a free, open-source fork of mapbox-gl-js. Package metadata lists BSD-3-Clause." It also notes that Leaflet "documents `fitBounds(bounds, options?)`", which covers the brief's default zoom and framing.
- Market research: "venue objects include a `location` with `latitude` and `longitude`". Ticketmaster also has an official tutorial, "Locate Events on a Map", and there are hobby repos (Gig Guru, ureynrey) that combine the Discovery API with maps. There is precedent for this build.
- Scope is small and plausible for a side project. Brief: "No user accounts or uploads: visitors only search and view", and "Distance from a location... after the MVP, not in it."

**Why not 15. The unproven component is the data source:**
- **UK coverage is partial by structure, and nobody has measured it.** Market research: "Ticketmaster's own submission says it partners with five of twelve major UK arenas; the other seven sell through other providers", and "Share of Discovery API music events in UK, or tour-date completeness versus artists' own listings: no sourced evidence found." The brief admits the gap ("tours sold mainly through other ticket agents... will show gaps"), but admitting a gap doesn't show the data is usable.
- **UK access isn't confirmed.** Market research: "No result confirmed GB specifically for attractions" (the only GB example comes from a third-party scraper). The "International Discovery API" is "reported as no longer accepting new API key requests". The brief relies on standard Discovery v2 returning UK events, and no source in the research confirms that.
- **Artist matching:** "the `keyword` parameter matches an event's name, and partial words do not match". The brief names the tribute-band risk ("a name search can return the wrong act or tribute bands") but says nothing about how the user would pick between several attraction matches.
- **Coordinates:** "Whether UK venues carry populated coordinates: no sourced evidence found." The brief lists this as a risk.
- **Smaller gaps that add effort.** These are unaddressed in the brief but not blockers:
  - No map library or tile provider is chosen.
  - The research shows MapTiler's free plan "is limited to non-commercial use", which conflicts with the brief's "affiliate ticket links possibly later".
  - The OSM tile policy "requires a valid identifying User-Agent... heavy use needs permission".
  - The brief doesn't say whether API calls run in the browser (exposing the key) or through a server-side cache. That choice matters under "5,000 calls/day, 5/second" and the reported rule on "large volumes of calls not primarily in response to direct user actions".

## 1.2 Resourcing feasibility: 7 / 10

**Justification:** The brief implies a solo builder, the product owner, working on a side project, and that is plausible for a scope this small. But it names no skills and no timeline, so resourcing is "gestured at but thin".

**Quoted source text:**
- Brief: "This is a side project, not a market-timing bet."
- Brief: "I've repeatedly read down long tour lists...". This first-person framing implies the author is the builder.
- Brief: "Choosing a real third-party API is deliberate: integrating one is itself part of the learning." This suggests the API-integration skills are still being learned, not already held.
- Neither the brief nor the research gives a timeline or effort estimate of any kind. It isn't scored 3, because nothing in the brief implies a team or skillset that doesn't exist: a single-page map over one API fits one person.

## Lens subtotal: 17 / 25

## Substance gate (Feasibility lens): 7 / 10

**Trace 1: "pull their upcoming UK events (date, venue, venue coordinates, ticket link) from Ticketmaster's public, free-tier API, used within its published terms."**
- I followed this claim into the research. The fields exist in the schema, but only per unverified secondary summaries.
- UK availability and coordinate population have no supporting evidence.
- The full terms were never read ("developer.ticketmaster.com did not resolve... the full terms text was not read").
- The brief's Risks section lists the terms as covering "attribution, caching, display rules". The research found "no sourced evidence" of those clauses, and the rate limits it quotes are carried from v1 and marked unverified.
- The brief does hedge: the terms "must be read before build", and coordinates are listed as a risk.
- **Result:** the claim holds as a hedged intention. It doesn't hold as an established fact. That is borderline, but it is honestly labelled as unverified, so it doesn't drop to 4.

**Trace 2: "Pick an artist and you land on a map of the UK with a pin at every tour date."**
- In the research, the attractions endpoint supports name search and the events endpoint takes `attractionId`. So the artist-to-events path exists.
- "Every tour date" doesn't hold for any tour that sells partly outside Ticketmaster. Ticketmaster partners with 5 of 12 major arenas.
- The brief concedes this under "Known limit" and "Data coverage". Its own text is therefore consistent, but the headline promise in "Rough shape" overstates what the data can deliver.

**Falsify. The strongest specific argument against the idea:**
- The experiment's success test can't give a valid answer with this data source.
- The brief's success measure is "side by side with Bandsintown's map for the same tour, people find this one quicker and easier to navigate". Its validation asks friends "to find a given date on this map and on Bandsintown's".
- If Ticketmaster returns only part of a tour, which the research makes likely, the Tour Map shows fewer pins than Bandsintown for the same tour.
- A sparser map is quicker to scan for the wrong reason, so the map-UX comparison it exists to make is confounded by data completeness. Worse, the "given date" a tester is asked to find may simply be missing.
- The brief comes close to this ("check early how many of those tours Ticketmaster covers in full"; coverage gaps "could make the map look wrong rather than just sparse"). But the side-by-side test never controls for it, for example by limiting the comparison to fully covered tours or matching the pin sets.
- The brief engages the coverage risk; it doesn't engage the specific threat that coverage poses to the validity of its own measurement.

**Score rationale:** Both sampled items trace to supporting text, with hedges the brief itself makes. The strongest counter-argument isn't anticipated in a form that answers it. That fits the anchor at 7: right as far as it goes, and untested.
