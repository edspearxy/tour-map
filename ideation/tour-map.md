# Idea brief: Tour Map (v2 — map-first artist tour view)

> **v2, 2026-11-16.** Supersedes v1 (parked — see `decision.md`). v1 proposed scraping tour
> pages and positioned against location-first discovery; the council found the scraping claim
> unsupported, and the product owner confirmed Bandsintown already offers an artist tour map.
> v2 narrows the idea to a map-first view of a single artist's tour, fed by manually uploaded
> data. Previous version is in git history.

## The problem

Touring musicians publish their dates as a **list**: date, venue, city. A list answers "when
is the next show?" but not the question a fan actually has: **"where in the country can I see
them, and which date is realistically reachable?"** — for themselves, or for someone they're
buying tickets for. Answering that from a list means reading every entry and placing each town
on a mental map.

Bandsintown already offers an event map on its artist pages, but the map is secondary to the
list, sits inside a wider discovery product, and is not the thing you land on.

## Who has it

- **Fans of a specific touring musician** working out which date they can get to.
- **Gift buyers** buying tickets for someone who lives elsewhere.

## Why now

Personal frustration: I've repeatedly read down long tour lists trying to work out which dates
are reachable. This is a side project and a learning build, not a market-timing bet.

## Rough shape of the idea

A website where **the map is the page**. Pick an artist and you land on a map of the UK with a
pin at every tour date — e.g. Cardiff Utilita Arena, London Eventim Apollo, Bristol Hippodrome,
Halifax Piece Hall — rather than a list with a map tucked behind it.

1. Choose an artist.
2. See the whole UK tour as pins on a full-screen map.
3. Tap a pin for date, venue and a link to buy tickets.

**Data for the MVP: manual upload.** Tour dates are uploaded by hand (e.g. a spreadsheet/CSV of
artist, date, venue, town, ticket link) for a small set of artists. No scraping and no
third-party events API, so no licence or terms-of-use dependency. Automated sources (e.g.
Ticketmaster's Discovery API) are a later step once the map experience is proven.

**Scope:** music, UK only, a handful of hand-curated artists.

**Commercial intent:** side project; affiliate ticket links possibly later. No revenue target.

**Not a copy.** Inspired by the idea of an artist tour map; it uses none of Bandsintown's name,
branding, design or data.

## What success looks like

Landing on an artist's page, a fan can identify the nearest or most convenient date within
seconds, without reading a list.

## Validation path

Upload 3–5 real UK tours by hand, use the map for real ticket decisions (my own and gifts), and
show it to a few friends who go to gigs. If the map-first view isn't clearly quicker than the
list, stop before investing in automated data.

## Risks

- **Market:** Bandsintown covers most of the job; this is unlikely to win users from it, which
  is acceptable for a personal/learning project.
- **Data freshness:** manually uploaded dates go stale when tours add, move or cancel dates.
  Acceptable for a handful of artists; the trigger to automate.
- **Venue location accuracy:** each venue needs a correct map position; with manual upload this
  can be captured (or checked) at upload time.

## Open questions

- Who uploads — only me as site owner, or any visitor (e.g. paste a CSV to see their own map)?
- Does "near a location" (enter a postcode, see distances) belong in the MVP or after it?
