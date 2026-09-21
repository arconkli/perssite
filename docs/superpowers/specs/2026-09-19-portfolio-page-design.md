# /portfolio — design spec

Date: 2026-09-19. Status: approved for build (autonomous session; Andrew's direction: "my style × Apple, with beautiful animations showcasing my projects").

## Goal

An unlisted page at `andrewconklin.com/portfolio` (reachable by link, kept out of search engines with noindex meta tags and X-Robots-Tag headers, not in the sitemap) that shows, with tons of verified detail, what Andrew has built and done: Bounty_OS, Soleil Clusters, Soleil Pictures, his growth-marketing record, on-camera work, music, and the smaller builds. Audience: hiring managers, founders, investors, collaborators. Every number must trace to a repo, a database export, a document on disk, or a live page (see "Fact sources").

## Non-goals

- No CMS, framework, or build step. One HTML file plus an `assets/portfolio/` folder, like the rest of the site.
- No revenue, GMV, or open-rate claims. No degree claim. Bounty_OS is described as an event-driven webhook + scheduled-job + polling architecture, never "websockets".
- The unlisted pitch page (`/everyonesocial`) and the private film-rankings app are not linked.

## Files

| File | Change |
|---|---|
| `portfolio.html` | New page. Served at `/portfolio` by Cloudflare Pages clean URLs (verified: `/about` and `/everyonesocial` already resolve this way). |
| `assets/portfolio/*.webp, *.mp4` | Optimised imagery: 14 webp images and one 8-second mp4 (2.1 MB total, largest single file 856 KB). |
| `index.html`, `about.html` | Add a "Portfolio" link to the top-right nav. |
| `_headers` | X-Robots-Tag noindex for `/portfolio` and `/portfolio.html`, mirroring the pitch page. Not added to the sitemap. |

## Visual system: "Andrew × Apple"

**From Andrew's existing pages:** Anybody variable font on the width axis (condensed headlines, wide lowercase micro-labels), monochrome ink on warm off-white, hairline rules, JetBrains Mono ledgers with tabular numerals, the SVG-threshold text morph from the homepage, the lowercase marquee ticker, slow custom easing (`cubic-bezier(.16,1,.3,1)`).

**From Apple product pages:** one idea per screen; very large display type with short lines; generous vertical rhythm (120–160 px between chapters); full-bleed product imagery in device frames; alternating light and dark bands; sticky "feature scroll" sections where the product visual stays pinned and swaps as feature copy scrolls past; numbers that count up as they enter; muted autoplaying product clips; a slim sticky chapter nav with scroll-spy.

**Tokens.** Light page `#F5F4F0` with ink `#111214`; dark bands `#0B0B0C` with ink `#EDEAE5`. One signal colour per product, used sparingly: Bounty_OS green `#4ADE80`, Clusters amber `#E39A2D`. Body text is `system-ui` (San Francisco on Apple devices, which is the Apple half of the brief); display is Anybody; numbers are JetBrains Mono.

**Motion inventory** (all vanilla JS, no libraries; everything static under `prefers-reduced-motion: reduce`):

1. Hero word morph (founder / builder / marketer / actor / producer / musician).
2. Fade-up reveals on scroll via IntersectionObserver, staggered by `--d`.
3. Sticky showcases: for Bounty_OS and Clusters the visual column is `position: sticky`; each feature block, as it crosses the viewport centre, crossfades the pinned visual to its own image (3 images per product).
4. Autoplay-muted product clips that play only while visible (IntersectionObserver play/pause), with webp posters.
5. Count-up on every ledger value with a `data-count` attribute, once, when visible.
6. Subtle parallax on full-bleed imagery (transform only, rAF-throttled, ±6%).
7. Poster rail for the film slate: horizontal scroll-snap, hover tilt with perspective.
8. Sticky chapter nav with scroll-spy highlighting.
9. Footer marquee.

## Page structure

0. **Chapter nav** (sticky): ANDREW CONKLIN · 01–08 links · email.
1. **Hero**: "I build the whole thing." + morph line + 2-sentence positioning + headshot + a four-number ledger (2 products live · 1,700+ commits in 2026 · 4.95M creator views tracked · 10 film credits).
2. **Index**: numbered chapter list.
3. **01 Bounty_OS** (dark, green signal): what it is; the block model with the worked example; sticky showcase (home → docs explorer → mobile dashboard); "the machine" feature list; ledger "traction, production DB 28 Jun 2026"; ledger "the build"; links.
4. **02 Soleil Clusters** (light, amber signal): what it is; sticky showcase (wide canvas → collaboration → phone) plus the 8-second establishing clip; engineering feature list (Yjs CRDT over PartyKit Durable Objects, 50-endpoint API, own OAuth 2.1 server, 33-tool MCP server on the official registry, native shells, Scout, pgvector, Workers AI SEO loop, attribution, docs-parity CI, backups); ledgers "the build" and "in production"; press line (FilmPlatforms, 27 Jul 2026); links.
5. **03 Soleil Pictures** (dark, cinematic): studio line; poster rail (YAHWEH, Deep Dive, Lost Time) with loglines and stage; the development paper trail (business plan, marketing plan, storyboards, short script co-written 2025); links.
6. **04 Growth record** (light): WorldStar HipHop, freelance, consulting, Georgia Entertainment, Swoon Esports — as a dated ledger.
7. **05 On camera** (dark): 10-credit table, training, still from Noise Complaint, IMDb link.
8. **06 Music** (light): Keekoh, It All Falls on Soleil Records, output cadence (typographic; the master video is a lyric video, so no loop).
9. **07 Other builds** (light): card grid — Project Genesis, Rocklords, cast-and-crew database, Create OS, investor-research pipeline, this site's film app, this page.
10. **08 Contact + colophon** (dark): email headline; links; "how this page was verified" line and date.
11. **Ticker**.

## Fact sources (what may be cited, and how)

- **Bounty_OS traction**: `bounty-app-3-gem/RESUME_DATA.md` production export dated 2026-06-28. Always labelled with that date. Pitch-page figures ("533+ in a month") are not used; the DB-derived +282% (394 in Feb 2026) is.
- **Bounty_OS build**: counts re-verified in the repo on 2026-09-19 (134,798 lines TS/TSX; 39 edge functions; 122 migrations; 55 tables / 203 functions / 132 policies / 9 pg_cron jobs as declared in migrations; 40 routes; 41 docs pages; 599 commits, single author).
- **Clusters build**: repo counts on 2026-09-20 (1,764 commits, 1,690 since 2026-05-01, 651 in the last 90 days; 171,247 non-generated lines + 32,713 CSS; 358 migrations; 245 RLS policies; 21 edge functions; 50 REST endpoints + 18 OAuth endpoints from `docsiteSurface.json`; 33 MCP tools + 3 prompts; 1,381 unit tests per `npm test` run on 2026-09-19; 195 Playwright specs; 64 docs pages; 8 changelog editions). MCP Registry and npm entries confirmed live.
- **Clusters production**: pitch page figures verified 2026-09-01 (180,159 live edit ops; 5,054 lifecycle emails delivered; search impressions 1,146 → 5,793 over six weeks); users 113 / MAU 86 from the 2026-06-28 fact base, labelled with that date.
- **Growth record**: 2026 résumé + `Spooki x Duke Recap.csv` totals row (48 videos, 956,650 views, $7,750, CPV $0.0081, best $0.0016) + LinkedIn export dates. The "$0.013 cost-per-lead" line is dropped (the sheet computes cost per like).
- **Credits**: acting résumé dated 2026-02-23 (10 credits) plus Lifeline (lead) and the festival selections for Lifeline and Noise Complaint, both from Andrew directly (2026-09-20). Not independently confirmed on IMDb (blocked); presented as credits, not awards.
- **Music**: Soleil Records distribution report (artist Keekoh, It All Falls, activity from Nov 2024); track-folder counts on disk.
- **Other builds**: repo counts on 2026-09-20 (Genesis 14 crates / 24,912 lines / 275 tests / 90 systems, 78 CDDA-fork commits; Rocklords 18,107 lines / 126 tests / 39 server modules; Future Studio 90 ledger entries / 147 assertions; investor pipeline 53 tests; film app 4,067 lines / 57 commits).

## Known discrepancies, and the choice made

| Topic | Sources disagree | Page says |
|---|---|---|
| Soleil Pictures founding | site "2023"; LinkedIn/résumé 2024; LLC Aug 2024; logo files 2022 | "co-founded in 2024" |
| Clusters start | app in production ~March 2026 (Andrew); first tracked commit 2026-05-03 | "in production since spring 2026"; ledger shows 1,690 commits since May |
| Clusters tests | pitch page 2,203; `npm test` 1,381 unit + 195 e2e specs | "1,381 unit tests and 195 end-to-end specs" |
| MCP tools | code/registry 33; one web page read said 47 | 33 |
| Atlanta move | about page 2022; IMDb 2023 | year omitted |
| WorldStar CPL | résumé "cost-per-lead"; sheet = cost per like | omitted |
| Claude Code usage | pitch page 171 sessions; surviving files 47 | no numbers; "built solo, with Claude Code as a daily collaborator" |

## Accessibility and performance

- Semantic sections with real headings; skip link; visible focus; all imagery has alt text; videos are decorative (`aria-hidden`, muted, `playsinline`, no audio track) with a poster.
- Contrast ≥ 4.5:1 for text on both bands (checked against the tokens above).
- Fonts via Google Fonts with `font-display: swap` and system fallbacks; images lazy-loaded below the fold with width/height set; total page weight under 4 MB including clips.
- JSON-LD: `ProfilePage` → `Person` (sameAs: IMDb, Instagram, LinkedIn, GitHub, Wikidata) with `hasPart` for the two `SoftwareApplication`s and the `Organization`.

## Testing

- HTML validity (no unclosed tags, unique ids), every internal anchor resolves, every external link returns 2xx/3xx.
- Playwright screenshots at 390, 820, 1440 px, light and dark bands, plus `prefers-reduced-motion` to confirm content is fully visible with motion off.
- A fact-check pass: extract every number from the page and match it to the source table above.

## Addendum, 2026-09-21: sample video in the growth record

Andrew asked for his "Georgia Entertainment Sample Vid" (youtu.be/RxR0G8qX9dY, unlisted, 1:58, uploaded
2024-08-22, vertical 1080x1920) to go on the page: "a sample video I made for a potential client, kinda shows
the videos I can make." First placed inside the Georgia Entertainment ledger row as a 15rem phone-shaped
poster; he liked the poster but said the row's spacing felt "off and weird" (the row became ~35rem tall with a
two-line paragraph and a void). Fix, chosen by a three-proposal judge panel over "shrink to a thumbnail +
dialog player": the figure moved out of the row into `div.recwrap` around the ledger. At 72rem and up it is a
15rem sticky exhibit column beside the whole list (top = nav height + 1.25rem), its mono label
"sample · georgia entertainment" on the same line as the first entry name; 46 to 72rem it sits below the list
with the caption beside the phone on its base line; under 46rem it stacks. The Georgia row is a one-line
paragraph again. Nothing from YouTube loads until the play button is pressed; the press builds the iframe the
same way the EveryoneSocial page does (youtube.com/embed with `origin`,
`referrerpolicy=strict-origin-when-cross-origin`), and `_headers` sends the matching Referrer-Policy for
/portfolio so the player's origin check passes. The poster is a real frame from the last second of the video
(caption-free), `assets/portfolio/sample-georgia.webp`, 720x1280. Caption copy states only what is known:
aug 2024, 1:58, vertical, sound on, "Cut to pitch a potential client: the kind of video I make.", and the
YouTube link.

## Addendum, 2026-09-21: Soleil Pictures chapter

Andrew: "get the actual posters from our soleil pictures website... make the logo actually fit properly and be
the correct size AND remove the paper trail." The posters are now the three files soleilpictures.com/projects
serves (`assets/YAHWEH_mock.PNG` 1518x2048, `assets/Deep_Dive_poster_correct.jpg` 3300x5100,
`assets/losttime-poster.png` 1536x2048), re-encoded to 1200px-wide webp and shown at their own aspect ratios
(fixed card height, auto width; on phones the height also respects the viewport width so nothing letterboxes).
The earlier YAHWEH image was a different design. The logo had been rendering at 256x592 because an inline
`width` left the HTML `height` attribute in force; it is re-encoded from
`~/Documents/soleilpictures/soleil_pictures_logo.png` (alpha kept) and sits at 14rem beside the chapter links
as the chapter's sign-off. The paper-trail ledger is removed.
