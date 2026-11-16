# Idea brief: Tour Map

> **Status: DRAFT for product-owner review.** Drafted from a three-sentence description. Sections
> marked *[to confirm]* are assumptions, not stated intent — confirm, correct or delete them
> before `/ideation` runs, because the council will score this text as written.

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

## Who has it

- **Fans of touring acts** deciding whether, and where, to see a show. *[to confirm: primary
  segment — live-music fans first, or all live performance from day one?]*
- **Gift buyers** buying tickets for someone who lives elsewhere — the case where list views
  fail worst, because the buyer has no mental map of the recipient's area.

*[to confirm: are artists / promoters / venues users too — e.g. an embeddable map for an
artist's own site — or strictly out of scope for now?]*

## Why now

*[to confirm — no "why now" was given in the original description. Candidates to choose from
or replace: tour lists keep getting longer as acts route more secondary-market dates; ticket
platforms optimise for selling a single event, not for comparing a tour; maps and geocoding
are now cheap enough for a solo build. If none of these is genuinely true for you, say so —
the council will treat an unsupported "why now" as a weakness either way.]*

## Rough shape of the idea

A website where you pick an artist and see their upcoming tour on a **map** instead of (or
alongside) a list:

1. Find an artist.
2. Their upcoming dates are plotted as pins at each venue.
3. Enter a location — your own, or someone else's — and see which dates are near it, with
   distance.
4. Tap a pin to see the date and venue, and follow through to buy tickets.

*[to confirm:]*

- **Where tour data comes from** — a third-party events API, the artist's own site, or
  manual/user entry. This is likely the deciding feasibility question.
- **Sign-up / alerts** — e.g. "tell me when this artist adds a date within 50 miles" — in or
  out of the first version?
- **Geography** — UK first, or global from launch?
- **Business model** — personal project, affiliate ticket links, or something else?

## What success looks like

*[to confirm]* A fan can go from "artist name" to "the nearest show to this location, with a
link to buy" in under a minute, without reading the full list.
