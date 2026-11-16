# Market research: Tour Map

Snapshot produced 2026-11-16. Evidence only, no judgement. Every entry is sourced. Most findings come from search-result summaries, not from reading the pages in full. Two direct fetches (corp.bandsintown.com and developer.ticketmaster.com) failed to resolve in this session, so those hosts need to be added to the session's allowed network domains before their terms can be read in full. Terms below are therefore secondary or snippet-level and are marked "verify".

## Direct Competitors

Products that show a single artist's tour on a map:

- **Tour Radius** - Product Hunt listing describes an interactive map where you search any artist to explore their full tour: dates, venues, sold-out status and every stop on the route. The listing points to tourradius.com. Pricing, data source and whether it is still live were not confirmed, and tourradius.com itself was not opened. Source: https://www.producthunt.com/p/tour-radius (accessed 2026-11-16).
- **Bandsintown artist-page event map** - Artist pages gained an event map (powered by Stay22) showing all upcoming shows. Fans can view the whole tour or select dates to see shows in a range, plus hotels near each location. Changelog dated 2022 per the search summary. Source: https://artistfeedback.bandsintown.com/changelog/event-map-on-artist-pages
  - **Product-owner correction (2026-11-16, first-hand check of the current product):** the Bandsintown map shows the location of an individual event plus nearby attractions/hotels (Stay22). It does **not** show a UK-wide map with every tour date plotted together. The 2022 changelog summary above (sourced from a search summary, not the page itself) overstates the current product. Treat as superseded for scoring.
- **ROSTR Pro "Live" tab** - Uses Bandsintown data to generate an interactive map of an artist's upcoming shows. ROSTR says it is still improving the feature. Source: https://discopro.rostr.cc/features/new/tour-data
- **MusicMap** - Enter a musician and it shows an interactive map of past and upcoming concerts. Source: https://satsumasegment.github.io/MusicMap (this is a github.io page, which suggests a hobby project; not otherwise verified).
- **Concert Map** - Shows upcoming concerts from favourite artists on an interactive world map. Source: https://alternativeto.net/software/concert-map/about
- **Artist Map (open source)** - GitHub project that explores band gigographies using the Songkick API and Mapbox. Source: https://www.github.com/d-rowe/artist-map
- **Band-specific tour map app** - The Dave Matthews Band app (iTour.dmb) offers an interactive tour map. Source: https://apps.apple.com/app/id398937827
- **Fan-built proposal** - A Reddit thread (r/bobdylan) proposes an interactive Bob Dylan tour map. Source: https://lr.in.psf.lt/r/bobdylan/comments/1f9sfur/bob_dylan_interactive_tour_map/llvl1n8/?context=3 (a proxy mirror of Reddit).

Distance from an arbitrary location (the "ranks an artist's dates by distance" question):

- Bandsintown: the search found no evidence of a distance-from-arbitrary-location filter on artist pages. The "Request a Show" prompt appears when there are no upcoming shows near the viewer's location. Source: https://www.bandsintown.com/a/72746-distance (search summary only; the page is for an unrelated band named "Distance", so it does not document the feature). Treat as no sourced evidence found for or against.
- Shazam: concerts can be browsed by artist, date, location or genre. Source: https://support.apple.com/guide/shazam/dev08d87910a/web
- Apple Music, Apple Maps and Shazam concert discovery use Bandsintown data. Source: https://venues.bandsintown.com/support/blog/apple-music-apple-maps-and-shazam-expand-concert-discovery-leveraging-bandsintown
- A Consequence concerts page lists a distance from the user's location per event. Source: https://concerts.consequence.net/ (the page was not opened; this comes from the search summary).
- JamBase launched a "Concert Finder" discovery and recommendation platform. Source: https://www.jambase.com/article/concert-finder
- Songkick, Ticketmaster, Dice, SeatGeek, Google: no sourced evidence found on single-artist map or arbitrary-location distance ranking in the searches run.

## Indirect Alternatives

- **Do nothing / read the list on the artist's own page.** Tour pages are commonly published through embedded widgets, so the list view is the status quo (see publishing methods below).
- **Bandsintown artist pages and app** - list upcoming shows, show whether there are dates in the viewer's city, and offer alerts for tour announcements and shows near the user. Source: https://www.bandsintown.com/Mack+10
- **Rush tour-date search site** - an example of a fan-run searchable tour-date listing. Source: https://www.rushisaband.com/blog/2015/12/22/4581/Rush-searchable-tour-date-listings-and-new-site-at-TourDateSearch.com (2015; likely dated).
- **Setlist.fm** - past setlists and venues. Source: https://www.setlist.fm/venues/browse/m/324.html
- **Gift vouchers instead of choosing a date** - UK theatre guide notes vouchers (e.g. via tickadoo) let the recipient choose show and date. Source: https://www.britishtheatre.com/posts/best-theatre-gift-experiences-uk

### How artist tour pages are published

- **Bandsintown widget** - embed codes for WordPress, Wix, Squarespace and Bandzoogle; the WordPress plugin is set up with an app_id. Sources: https://artists.bandsintown.com/support/widget-installation, https://wordpress.org/plugins/bandsintown/
- **Songkick Tourbox widget** - pasted into site HTML or embed element; the widget is standardised with limited customisation; the buy button goes straight to the ticket vendor. Sources: https://support.songkick.com/hc/en-us/sections/360002601014-Tour-dates-widget, https://support.wix.com/en/article/embedding-songkick-tour-dates-on-your-wix-site
- **Seated** - a tracking site reports Seated's tour-dates widget on 894 domains as of October 2026, mostly touring acts. This is a third-party count, and Seated's own docs were not found. Source: https://technologychecker.io/technology/seated
- **SeatGeek tour dates WordPress plugin** - listed as old, last updated about 4,305 days ago on one listing. Source: https://wordpress.org/plugins/seatgeek-tour-dates/
- Share of artist sites using each method: no sourced evidence found. Whether these widgets render client-side (JavaScript or iframe) was not established from the sources.

## Market Signal

### UK live music
- LIVE reports UK live music consumer spend of an estimated £7.6bn in 2025, up 12.9% year on year. Source: https://accessaa.co.uk/uk-live-music-sector-reaches-record-7-6bn-spend/
- An earlier figure of £6.68bn is reported for the prior year. Source: https://completemusicupdate.com/uk-live-music-spend-reached-6-68-billion-last-year-but-challenges-remain/
- PRS for Music collected £101.4m from live music in 2025, up 13.2%. Source: https://www.pulse.bot/entertainment/news/prs-for-music-pays-record-1014-million-in-livemusic-royalties-in-2025-5180641b-6d47-47d1-8297-f7a434488e17/
- UK Music tourism: sources conflict on jobs supported (74,000 vs 72,000 FTE) and cite a 26% revenue jump; the original UK Music "Music by Numbers" report was not retrieved. Source: https://vip-booking.com/news/ReadNews/General/9487/uk-music-tourism-reaches-record-high
- Average UK fan spent £318 on live tickets over 12 months (18 to 24 year olds £425). Source: https://www.musicweek.com/public_html/index.php/live/read/gen-z-music-fans-spending-425-a-year-on-live-music/092807
- Price pressure: 51% of Britons say ticket prices stopped them attending a gig at least once in five years (YouGov 2022). Source: https://readdork.com/news/yougov-poll-ticket-prices-2022

### Ticket gifting
- YouGov (February 2023): 10% of respondents said they bought tickets as presents (food and drink 44%, clothes and shoes 42%). Source: https://yougov.com/en-gb/articles/46168-reaching-consumers-who-buy-tickets-as-gifts
- Cadbury "Gift a First Gig" campaign with Academy Music Group and Ticketmaster. Source: https://sponsorship.org/we-are-the-great-british-gig-nation-and-cadbury-is-here-to-celebrate-it/
- No sourced evidence found on the share of gig tickets bought for someone in a different city.

### Events APIs and terms (all marked verify; official pages not fully read)
- **Bandsintown** - Data Application Terms (last updated 25 Feb 2019 per summary) cover API and widgets. Each API key is tied to a single artist unless Bandsintown authorises otherwise. A third-party summary says access needs written consent and a personal app ID. The Partner Search API page says not to scrape data or modify content. Sources: https://corp.bandsintown.com/data-applications-terms, https://help.artists.bandsintown.com/en/articles/7053475-api-and-app-id, https://artists.bandsintown.com/support/partner-search-api
- **Ticketmaster Discovery API** - default 5,000 calls per day and 5 requests per second; higher limits case by case after checks on terms, branding and representation of data. Terms (updated 27 June 2023) allow rate limiting or blocking of calls not primarily in response to direct user actions. Sources: https://developer.ticketmaster.com/products-and-docs/apis/getting-started/, https://developer.ticketmaster.com/support/terms-of-use/. Third-party reviewers note the docs and FAQ disagree on a per-second limit.
- **Songkick API** - keys are processed manually. An aggregator listing says free for non-commercial and paid for commercial use, which is unverified; the forum posts are from 2011. Sources: https://groups.google.com/g/songkick-api, https://saas-finder-site.pages.dev/tool/songkick
- **SeatGeek Platform API** - API/SDK Terms last updated 17 March 2025: limited, revocable, non-transferable licence. Source: https://seatgeek.com/api-terms
- **Setlist.fm API** - free for non-commercial projects only; commercial use needs contact with setlist.fm. Reported (secondary) requirements: direct server calls and a followable link back. Sources: https://www.setlist.fm/help/api-terms, https://api.setlist.fm/docs/1.0/data.html
- **Dice**: no sourced evidence found on a public events API.
- **Scraping terms of the widget providers** (Seated, Songkick Tourbox, Bandsintown widget pages): no sourced evidence found beyond the Bandsintown items above. The Songkick and Seated terms were not retrieved.
- Third-party scraper actors exist for Bandsintown and Ticketmaster (Apify), which shows scraping is offered as a service but is not evidence about legality or terms. Sources: https://apify.com/hoholabs/bandsintown-scraper, https://apify.com/primeparse/ticketmaster-event-scraper
