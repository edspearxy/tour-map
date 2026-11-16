# Market research: Tour Map (v2, map-first artist tour view, manual data, map-UX experiment)

Snapshot produced 2026-11-16, overwriting the v1 snapshot. Evidence only, no judgement.

Access limits this session: direct page fetches failed (host did not resolve, so presumably outside the session's allowed network domains) for artistfeedback.bandsintown.com, tourradius.com, satsumasegment.github.io, alternativeto.net, discopro.rostr.cc, www.mapbox.com, www.maptiler.com, operations.osmfoundation.org and mapsplatform.google.com. Only github.com was fetched directly. Everything else below comes from search-result summaries. It is labelled "unverified" where it matters, and the dated claims carry a year. To verify competitor liveness and provider terms from their own pages, those hosts need allowing.

## Direct Competitors

### Bandsintown artist-page event map
- Changelog (March 2022): artist pages gained an event map powered by Stay22, showing all upcoming shows. Events are organised by date and location, and fans can expand events for details. It also surfaces lodging near each show. It extends an existing event-page tool onto artist pages. Source: https://artistfeedback.bandsintown.com/changelog/event-map-on-artist-pages (search summary only, unverified, dated 2022; page not opened).
- A urlscan.io capture of a Bandsintown artist page shows requests to stay22.com, consistent with the integration. This is not documentation of how the map behaves today. Source: https://urlscan.io/result/37d99451-a947-41e4-b322-cd2f4b7d58e9 (search summary only).
- Whether the map still exists in the same form in November 2026, how it frames the tour by default, and whether artists can turn it off: no sourced evidence found. The map was not viewed first-hand in this session.

### Published feedback on the Bandsintown map's usability (for or against "hard to navigate")
- No sourced evidence found that singles out Bandsintown's artist-page map as hard to use. A search for Trustpilot/Reddit/review complaints about it returned nothing map-specific. No official Bandsintown acknowledgment of map usability complaints was found.
- Closest items, none of which is a map-specific complaint about Bandsintown:
  - A June 2024 SmartCustomer review says event listings showed date, artist and venue but not city or state. A single anonymous review, not about the map. Source: https://www.smartcustomer.com/reviews/bandsintown.com (search summary only).
  - A PopMatters piece reportedly says a map feature for browsing venues is tough to use. The snippet does not name Bandsintown or the page, so attribution is unconfirmed. Source: https://popmatters.com/186308-band-on-tour-2495609904.html (search summary only, unverified).
  - Digital Music News critiques Bandsintown's mobile experience (a user landing on the app home page instead of the artist's concert page). It does not mention the map. Source: https://www.digitalmusicnews.com/?p=46120 (the article dates from around 2015 per the related URL).
  - ROSTR says that on its own Bandsintown-fed map, large tours "can get pretty messy" and that it planned filter controls. This describes ROSTR's map, not Bandsintown's. Source: https://discopro.rostr.cc/features/new/tour-data (search summary only).
  - Competitor marketing, not independent: Gigora says Bandsintown coverage depends on artist opt-in. Source: https://gigora.live/bandsintown-alternative
- The product owner's first-hand view is therefore not corroborated or contradicted by any published source found.

### Other single-artist or tour-map products (liveness status)
| Product | What sources say | Liveness today |
|---|---|---|
| Tour Radius | Product Hunt listing describes an interactive map where you search any artist to see the full tour (dates, venues, sold-out status, route). Points to tourradius.com. https://www.producthunt.com/p/tour-radius | Unverified, listing seen in an earlier run (2026-11-16 snapshot, undated listing). tourradius.com did not resolve in this session, and a search for it returned nothing from that domain. |
| MusicMap | Enter a musician, see an interactive map of past and upcoming concerts. https://satsumasegment.github.io/MusicMap | Unverified. The page did not resolve for fetch in this session, but a search did return the URL. Appears to be a github.io hobby project. Search summary only, undated. |
| Concert Map | Web tool showing upcoming shows from favourite artists on an interactive world map. A summary claims 100,000+ events and listening-history import. https://alternativeto.net/software/concert-map/about | Unverified. AlternativeTo page seen via search summary only, undated. Whether the product itself is live: no sourced evidence found. |
| Artist Map (d-rowe / d-buckner) | GitHub repo "explore band gigographies", built with Mapbox, React, Redux, Express, Songkick API; 63 commits; public, not archived; links a demo at artistmap.herokuapp.com. https://www.github.com/d-rowe/artist-map | Repo page fetched 2026-11-16. The page showed no commit dates, so recency is unknown. The demo was not opened. Heroku-hosted demo and Songkick API dependency: liveness unverified. |
| ROSTR Pro "Live" tab | Uses Bandsintown data for an interactive map of an artist's upcoming shows. https://discopro.rostr.cc/features/new/tour-data | Unverified, search summary only, undated. |
| Gigora | Competitor site says it offers an interactive world map of upcoming shows, filterable by country or city. https://gigora.live/bandsintown-alternative | Unverified, vendor self-description via search summary. |
| Dave Matthews Band app (iTour.dmb) | App Store listing with interactive tour map. https://apps.apple.com/app/id398937827 | Not re-checked in this run. Unverified. |

Carried from v1 (not re-verified): Bob Dylan interactive tour map proposal thread on Reddit (via proxy mirror). Source: https://lr.in.psf.lt/r/bobdylan/comments/1f9sfur/bob_dylan_interactive_tour_map/llvl1n8/?context=3

Distance from an arbitrary location (post-MVP feature): no change from v1. No sourced evidence found of a Bandsintown filter for it. Shazam supports browsing concerts by artist, date, location or genre (https://support.apple.com/guide/shazam/dev08d87910a/web). Apple Music, Apple Maps and Shazam use Bandsintown data (https://venues.bandsintown.com/support/blog/apple-music-apple-maps-and-shazam-expand-concert-discovery-leveraging-bandsintown).

## Indirect Alternatives

- **Do nothing / read the list.** Tour pages are commonly published through embedded list widgets. Sources in v1: Bandsintown widget (https://artists.bandsintown.com/support/widget-installation, https://wordpress.org/plugins/bandsintown/), Songkick Tourbox (https://support.songkick.com/hc/en-us/sections/360002601014-Tour-dates-widget), Seated widget on 894 domains per a third-party tracker as of October 2026 (https://technologychecker.io/technology/seated). Share of artist sites using each: no sourced evidence found.
- **Bandsintown artist pages and app**: lists, alerts, dates near the viewer. https://www.bandsintown.com/Mack+10
- **Setlist.fm** (past setlists and venues): https://www.setlist.fm/venues/browse/m/324.html
- **Fan-run tour-date search pages** (Rush example, 2015, likely dated): https://www.rushisaband.com/blog/2015/12/22/4581/Rush-searchable-tour-date-listings-and-new-site-at-TourDateSearch.com
- **Gift vouchers instead of choosing a date**: https://www.britishtheatre.com/posts/best-theatre-gift-experiences-uk
- **Generic map builders** (non-music, for context only): Portland Open Studios runs an interactive map for an artist studio tour (https://portlandopenstudios.com/2024-artists); Allinmap offers multi-stop tour maps (https://www.allinmap.app/en/event-maps/tour). Both via search summary only.

## Market Signal

### Map UX patterns and research (all from search summaries, not read in full)
- **Clustering when few points:** a vendor help page says clustering suits many locations in one region and is generally unnecessary for a few dispersed points (https://support.metalocator.com/en/articles/1659899-map-marker-clustering). Studio Wolf says not to cluster when the exact point matters to the user, and notes clusters sit at the centroid of their points (https://studiowolf.com/blog/to-cluster-or-not-to-cluster). A developer essay argues clustering hides data and adds clicks, zooms and pans (https://dev.to/markosmilja/beyond-marker-clustering-1jmj). A PeerJ preprint says practitioners over-use clustering to fix performance without resolving perceived performance (https://peerj.com/preprints/27858). Write-ups describe turning clustering off at a set zoom level, and expanding a cluster on hover (https://medium.com/homes-engineering/map-marker-clusters-c90b2f192324, search summary only). No Nielsen Norman Group material on clustering was found.
- **Default framing / fit-bounds:** Leaflet's reference documents `fitBounds(bounds, options?)` (https://leafletjs.com/reference.html). No MapLibre `fitBounds` source was found in this run. No sourced usability research on default framing for small point sets was found.
- **Mobile map interaction:** Google's blog says an embedded map captures swipes and can trap mobile users. Its fix is cooperative gesture handling (one finger scrolls the page, two fingers pan the map). It also made the fullscreen control visible by default on mobile because user studies found small embedded maps hard to use (https://maps-apis.googleblog.com/2016/11/smart-scrolling-comes-to-mobile-web-maps.html, dated 2016). Cooperative gesture options are documented for Google (https://developers.google.com/maps/documentation/javascript/examples/interaction-cooperative) and Mapbox GL JS (https://docs.mapbox.com/mapbox-gl-js/example/cooperative-gestures.md).
- **Map vs list usability:** no sourced evidence found. The search returned no study comparing the two.

### Map provider costs and terms for a small hobby site (secondary sources unless stated; check live pages)
- **Mapbox GL JS:** sources agree on 50,000 free map loads per month; a load is counted when a Map object initialises, and pan/zoom do not add loads. Overage is reported at about $5 per 1,000 loads, though tiers differ between sources. Whether a credit card is needed is disputed between sources. Official pricing page seen only as a partial snippet. Sources: https://www.mapbox.com/pricing/, https://docs.mapbox.com/accounts/guides/pricing/, https://www.woosmap.com/blog/mapbox-pricing (2026), https://storerocket.io/learn/mapbox-pricing. Mapbox GL JS moved to a non-open-source licence in December 2020 (https://www.geoapify.com/mapbox-gl-new-license-and-6-free-alternatives).
- **MapTiler Cloud:** third-party listings give the free plan as 100,000 API requests and 5,000 API sessions per month; storage figures conflict. MapTiler's own terms say the Free Plan is limited to non-commercial use plus R&D, with on-screen attribution required. Per its docs, going over on the free plan suspends maps for the rest of the month rather than charging. Sources: https://www.maptiler.com/cloud/terms/, https://frontdeskreview.com/software/maps-api/maptiler/ (updated June 2026), https://freetier.co/directory/products/maptiler. Official numbers not read.
- **OpenStreetMap standard tile server (tile.openstreetmap.org):** the policy (per search summary and secondary sources) requires a valid identifying User-Agent (library defaults not recommended) and a Referer for websites. It requires visible attribution, and heavy use needs prior permission. Blocking without notice is possible, and the policy recommends running your own tile server. Attribution wording and caching details were uncertain. Sources: https://operations.osmfoundation.org/policies/tiles/, https://wiki.openstreetmap.org/wiki/Blocked_tiles, https://docs.fleaflet.dev/tile-servers/using-openstreetmap-direct
- **Google Maps Platform:** since 1 March 2025 the $200 monthly credit was replaced by a free monthly threshold per Core Services SKU: 10,000 billable events for Essentials SKUs, 5,000 Pro, 1,000 Enterprise. What counts as an event varies by SKU. Billing account required (not stated in the summary; verify). Sources: https://developers.google.com/maps/billing-and-pricing/march-2025, https://developers.google.com/maps/billing-and-pricing/faq
- **Leaflet and MapLibre GL JS:** MapLibre GL JS is a free, open-source fork of mapbox-gl-js. Package metadata lists BSD-3-Clause. Sources: https://www.npmjs.com/package/maplibre-gl, https://classic.yarnpkg.com/en/package/maplibre-gl. Leaflet is described as open source (docs: https://leafletjs.com/reference.html). Leaflet's licence text was not retrieved. Both libraries need a tile or style source from a provider above.

### UK live music demand and ticket gifting (carried from v1, not re-verified)
- UK live music consumer spend about £7.6bn in 2025, up 12.9% (https://accessaa.co.uk/uk-live-music-sector-reaches-record-7-6bn-spend/); prior year £6.68bn (https://completemusicupdate.com/uk-live-music-spend-reached-6-68-billion-last-year-but-challenges-remain/).
- Average UK fan spent £318 on live tickets over 12 months (https://www.musicweek.com/public_html/index.php/live/read/gen-z-music-fans-spending-425-a-year-on-live-music/092807).
- YouGov (Feb 2023): 10% of respondents bought tickets as presents (https://yougov.com/en-gb/articles/46168-reaching-consumers-who-buy-tickets-as-gifts).
- Share of gig tickets bought for someone in a different city: no sourced evidence found.

### Event data terms (relevant to the "later" automation step; v1 findings, snippet-level, verify)
- Bandsintown API: key tied to a single artist; written consent needed (https://corp.bandsintown.com/data-applications-terms, https://help.artists.bandsintown.com/en/articles/7053475-api-and-app-id).
- Ticketmaster Discovery API: 5,000 calls/day and 5 requests/second default (https://developer.ticketmaster.com/products-and-docs/apis/getting-started/).
- Songkick API: keys processed manually, terms unverified (https://groups.google.com/g/songkick-api). Setlist.fm API: free non-commercial only (https://www.setlist.fm/help/api-terms). SeatGeek API terms updated 17 March 2025 (https://seatgeek.com/api-terms).
