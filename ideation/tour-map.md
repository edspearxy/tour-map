# Idea brief: Tour Map

> **Status: ready for `/ideation`.** Drafted from the product owner's description and answers;
> the remaining open questions are listed at the end deliberately.

## The problem

Touring artists — musicians, comedians, poets, spoken-word acts, any act playing a run of
venues — publish their tour dates as a **list**: date, venue, city. A list answers "when and
where is the next show?" but not the question most fans actually have: **"is there a show I
can realistically get to?"**

To answer that from a list, a fan has to read every entry, recognise each town, and judge in
their head how far it is from them. It gets harder when:

- the tour has many dates, or spans countries;
- the town names are unfamiliar (small venues, regional stops, foreign legs);
- the fan is buying for **someone else** — a gift for a friend or relative in another city —
  so the "near me" judgement has to be made from a location they don't know well.

## Positioning: artist-first, not location-first

Existing live-event discovery products (Bandsintown, Songkick) are built around **"what's on
near me?"** — they start from a location and surface whichever events happen to be there. That
serves people who want *a* night out.

This idea inverts it: **start with who you want to see, then work out visually where you can
see them.** The artist is fixed; the location is the variable. The question being answered is
not "what's on locally?" but "of every date this specific artist is playing, which ones can I
— or the person I'm buying for — actually get to?" Where an artist isn't playing anywhere
local, the list-and-location products simply show nothing, while the map still shows the
nearest options worth travelling for.

## Who has it

- **Fans of a specific touring musician** deciding whether, and where, they can see them.
- **Gift buyers** buying tickets for someone who lives elsewhere — the case where list views
  fail worst, because the buyer has no mental map of the recipient's area.

Artists, promoters and venues are **not** users in the first version.

## Why now

Personal frustration: I've repeatedly hit this myself — reading down a long tour list trying to
work out which dates are realistically reachable, for me or for someone I'm buying for. This is
a side project scratching that itch, not a bet on market timing. What makes it viable as a solo
build now is that mapping and geocoding are cheap and an AI-assisted build makes a niche tool
practical to deliver end to end.

## Rough shape of the idea

A website where you start from an artist and see their tour on a **map** instead of a list:

1. **Paste the URL of a web page listing the artist's tour dates** (e.g. the tour page on the
   artist's own site).
2. The dates are extracted from that page and each venue is plotted as a pin.
3. Enter a location — your own, or someone else's — and see which dates are near it, with
   distance.
4. Tap a pin to see the date and venue, and follow the link through to buy tickets.

**Data approach.** The MVP scrapes the tour list from a URL the user provides — no dependency on
a third-party events API or its licence. The longer-term direction combines this with
API-driven data (events/ticketing APIs) for coverage and freshness.

**Scope of the first version:** music only, UK venues first. Comedy, poetry and other live acts,
and other countries, come later.

**Commercial intent:** a side project. Affiliate ticket links are a possible later addition if it
gets traction; there is no revenue target.

## What success looks like

A fan can go from "artist's tour page" to "the nearest date to this location, with a link to buy"
in under a minute, without reading the full list.

## Open questions

- **Extraction reliability:** tour pages vary widely in structure (plain HTML, embedded
  widgets, JavaScript-rendered lists). What share of real artist tour pages can be extracted
  accurately, and how is a failed or partial extraction shown to the user?
- **Venue location accuracy:** turning a venue name and town into a correct map position,
  especially for small or ambiguously named venues.
- **Terms of use:** whether scraping a page on a user's behalf is acceptable under the source
  sites' terms.
- **Alerts and accounts** (e.g. "tell me when a date is added near X") are assumed out of the
  first version.
