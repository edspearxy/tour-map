# Idea Council: Feasibility lens: Tour Map

- **Reviewer lens:** Feasibility (criteria 1.1, 1.2) only
- **Date:** 2026-11-16
- **Inputs:** `ideation/tour-map.md` (idea brief), `ideation/market-research.md`
- **Corrections read:** universal `context/corrections/idea-council-feasibility.md` (no entries yet); project-local `memory/corrections/idea-council-feasibility.md` (does not exist yet)

## 1.1 Technical feasibility: 10 / 15

**Justification:** The map, pins, distance ranking and ticket links are all standard, well-understood technology. The whole MVP, though, depends on one component that hasn't been tested and could take a lot of work: generic extraction of tour dates from any URL a user pastes in. The brief raises this as an open question but doesn't answer it. The market research doesn't help, because it couldn't establish how tour pages are actually built.

**Source text:**
- Brief, rough shape: "Paste the URL of a web page listing the artist's tour dates ... The dates are extracted from that page and each venue is plotted as a pin."
- Brief, open questions: "tour pages vary widely in structure (plain HTML, embedded widgets, JavaScript-rendered lists). What share of real artist tour pages can be extracted accurately, and how is a failed or partial extraction shown to the user?"
- Brief, open questions: "Venue location accuracy: turning a venue name and town into a correct map position, especially for small or ambiguously named venues."
- Market research: "Share of artist sites using each method: no sourced evidence found. Whether these widgets render client-side (JavaScript or iframe) was not established from the sources."

**Why not 15:** The weakest part of the system is the part every user touches first, and nothing in the brief or the research checks how well it works. **Why not 5:** The capability clearly exists (headless rendering, parsers written for each widget, extraction by a language model). The question is how much effort it takes and how much of the input it covers, not whether it can be done at all. The brief names the problem directly instead of assuming it away.

## 1.2 Resourcing feasibility: 7 / 10

**Justification:** The brief names a team: one person building it as a side project with AI assistance. It never says what skills that person has or how long the build will take, so resourcing is described but thinly.

**Source text:**
- Brief, why now: "This is a side project scratching that itch ... What makes it viable as a solo build now is that mapping and geocoding are cheap and an AI-assisted build makes a niche tool practical to deliver end to end."
- Brief, commercial intent: "a side project ... there is no revenue target."

**Why not 10:** The brief gives no rough timeline and says nothing about the builder's own technical skills. It treats "AI-assisted" as standing in for both. **Why not 3:** A solo builder making a small website without a revenue target is a plausible setup. Nothing in the brief implies a team or a schedule that doesn't exist.

## Lens subtotal: 17 / 25

---

## Substance gate (Feasibility lens): 4 / 10

Scored last, separately from the subtotal.

**Trace 1: "The MVP scrapes the tour list from a URL the user provides — no dependency on a third-party events API or its licence."** *Does not hold up.*
According to the market research, tour pages are typically published through third-party widgets: "Bandsintown widget - embed codes for WordPress, Wix, Squarespace and Bandzoogle", the "Songkick Tourbox widget", and "Seated ... tour-dates widget on 894 domains". So on a typical artist page, the dates the MVP would pull out are a third-party provider's data, delivered by that provider's widget. Two consequences follow:
- **Technical:** If the widget renders client-side or inside an iframe (the research says this was "not established"), the dates are not in the HTML of the URL the user pastes. The extractor would then need a headless browser plus parsers for each widget, which means building per-provider integrations by another name.
- **Licence:** The research says the Bandsintown Data Application Terms "cover API and widgets" and that the Partner Search API page "says not to scrape data or modify content". The brief claims scraping removes the dependency on a provider's licence, but the evidence shows it only moves that dependency somewhere else.

The brief states this as a design decision. The input it comes from contradicts it.

**Trace 2: "mapping and geocoding are cheap."** *Asserted, not supported.*
The brief names no geocoding or map provider and gives no cost or usage-limit figure. The market research has nothing on geocoding or mapping. The brief's own open question about "small or ambiguously named venues" admits that the hard part of geocoding is accuracy, not cost, and nothing addresses that. The claim may well be true, but nothing in the inputs backs it up.

**Strongest counter-argument (falsification):** Most UK artist tour pages are a Bandsintown, Songkick or Seated widget. A "paste any URL" extractor will then go one of two ways:
- It fails on those pages, because the dates load client-side and aren't in the fetched HTML. The headline flow then breaks on the most common input.
- It succeeds only by rendering the page and parsing each widget. That is effectively an integration with each provider, and at least one of them forbids it.

Either way, the "no third-party dependency" MVP doesn't exist as described. The brief comes close to this in its open questions ("embedded widgets, JavaScript-rendered lists"; "whether scraping a page on a user's behalf is acceptable"), but it never links the two. It also offers no fallback for a failed extraction and no estimate of how many pages it could handle.

**Score rationale:** At least one sampled item (Trace 1) is stated in the document but contradicted by `ideation/market-research.md`, which puts this at the 4 anchor. The gate rule applies to Substance scores of 4 or below. Applying it is for synthesis to decide.

**What would move this score:** Sample about 20 real UK artist tour pages and record how each publishes its dates (static HTML, Bandsintown, Songkick, Seated, iframe, client-side JS) and whether a simple fetch returns the dates. Then restate the data approach to match what that sample shows.
