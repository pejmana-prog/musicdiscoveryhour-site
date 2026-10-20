# Music Discovery Hour — musicdiscoveryhour.com

Static site (plain HTML/CSS/JS, no framework, no build step) for the Music Discovery Hour
brand: a music blog on Instagram (~1M followers) plus a live-performance series called
**Behind the Curtain**. The site is the public front door — aimed at fans, artists, labels,
and partners.

## Deploy pipeline

- **Push to `main` = deploy.** Netlify auto-deploys in ~30s to https://musicdiscoveryhour.com.
  No build command; publish directory is the repo root.
- Before editing, **pull latest from `main` first** — desktop and mobile Claude sessions
  both work on this repo and can race each other.
- Verify a deploy landed with `curl -s https://musicdiscoveryhour.com/ | grep <new content>`.
- Netlify also handles the `shop-signup` form (Netlify Forms) and pretty URLs
  (`/shop` → `shop.html`, `/contact` → `contact.html`).

## Files

- `index.html` — home: masthead (centered wordmark over nav links) → full-bleed video hero →
  split section (left: MDH 3x3 IG cover mosaic, right: Behind the Curtain sessions — one
  player + a list of every performance, sticky on desktop, each side titled with channel icon
  buttons) → footer (no links, "© Music Discovery Hour, Inc.")
- `shop.html` — merch coming-soon page with email signup (Netlify Forms, name `shop-signup`)
- `contact.html` — contact card (copy email / mailto to partnerships@musicdiscoveryhour.com)
- `style.css` — all styles, shared across pages
- `assets/` — hero loop video, poster, session thumbs (`session-*.jpg`, 1280x720), IG covers
  (`ig-1..9.jpg`), BTC emblem, favicons

## Brand rules (decided by the owner — do not undo)

- **No follower counts anywhere.** No "1M followers" copy, no stats strips.
- **No taglines** in the hero ("Your next favorite song…" was rejected as cheesy).
  The hero is the video, the floating nav links, nothing else.
- **No buzzy claim-y copy** ("the feed that found them first" was rejected — MDH is a music
  blog, it doesn't claim to have discovered artists). Current IG section title:
  "Artists & moments we love."
- The featured session is labeled **"Featured Session"** (not "Session 001"). Other
  sessions picked from the list are labeled "Session".
- **MDH emblem**: black circle, white lowercase Montserrat wordmark "music discovery hour."
  — implemented as inline SVG (nav on shop/contact, footer). The old "mdh" avatar is retired.
- **Behind the Curtain** is its own brand: separate IG (@behindthecurtainseries) and
  YouTube channel. MDH main IG is @musicdiscoveryhour.
- Warm cream palette (`--bg #f5f0e8`, `--ink #17130e`, gold accent) + Fraunces display
  serif + Inter UI. Keep it.

## Nav structure

Links float over the hero on `index` (`.topnav-overlay`), sit on cream on other pages
(`.topnav-page`). Three dropdowns (`.nav-drop`, shared JS at the bottom of each page):

- **Instagram ⌄** → Music Discovery Hour, Behind the Curtain
- **YouTube ⌄** → Behind the Curtain (channel UCTLJP53Q57KqO9Y9fJAC_dw)
- **TikTok ⌄** → Behind the Curtain (@btc.series)
- **Spotify ⌄** → Official Playlist, Top Songs of 2025, Top Songs of 2024,
  Region Spotlight — Brazil

Dropdowns: hover-open on desktop with an invisible bridge + 300ms grace (don't reintroduce
the hover-gap bug); tap-toggle everywhere; fixed sheet under the nav on ≤720px because the
nav row scrolls horizontally there.

## Sessions player (home, right column)

One `<button class="session-poster">` + hidden `.session-embed` (YouTube nocookie iframe,
created on first play) and a `.session-list` of `.session-item` buttons carrying
`data-video/artist/song/thumb/alt` (+ `data-featured` on the first). Clicking an item swaps the
poster/meta; if the player is already open it swaps the iframe src instead, so there is never
more than one iframe. Order = newest first; add a new performance by prepending an item,
moving `data-featured` and the `is-current`/`aria-current` markers to it, and updating the
featured poster/meta markup (and the `og:image` on all three pages, which uses the featured thumb).

## Media recipes

- **Hero loop** (Oct 2026: Raveena "Blissed Out", wide shot 2:27–2:45 of the YouTube master
  EBrhz3Px0Js, downloaded at 1440p with the DiscoveryClipper backend's yt-dlp + cookies; the
  earlier Lee Lewis loop came from the 4K master in `~/Documents/Most Recent Lee Lewis Files/`).
  It's a seamless crossfade loop: 18s segment, last 1s xfaded into the first 1s
  (`[S+1..S+18][S..S+1] xfade=fade:duration=1:offset=16` → 17s), cropped to 84% biased up
  (`crop=iw*0.84:ih*0.84:iw*0.08:0`) to drop the burned-in "behind the curtain" script in the
  bottom-left corner, 1280×720 CRF 27 → ~0.5MB. Poster = frame 0 of the loop. Pick a passage
  that is one continuous wide shot with slow movement and no lip-sync close-up (the hero is
  muted). Don't replace with a hard-cut loop — the camera moves, so a plain loop jump reads as
  a glitch.
- **Session thumbs**: a clean wide performance frame from the master (`ffmpeg -ss T -frames:v 1
  -vf scale=1280:720`), badge left in. The YouTube title cards animate artist and song
  separately, so there is no single frame with both — don't use them.
- **IG grid covers**: the designed cover card is the **very FINAL frame of the first slide**
  of each carousel (the video ends on the cover). A plain `-sseof -0.15 -frames:v 1` seek
  lands a few frames early and misses it — instead decode the tail and keep the last frame:
  `yt-dlp --playlist-items 1` (cookies from the DiscoveryClipper backend), then
  `ffmpeg -sseof -0.6 -i slide1.mp4 -vsync 0 out-%02d.jpg` and use the highest-numbered
  file. Self-host as `assets/ig-N.jpg` — never use IG embed.js (blocked/throttled) and
  never IG's API thumbnail or frame zero (wrong frames).

## Related but separate

- The DiscoveryClipper app (repo `spyqs/MDPOUR`, at `~/Projects/DiscoveryClipper`) makes the
  IG clips. Different project — never commit/push it from a session working on this site.
- Merch storefront (Fourthwall) is in progress; `shop.html` collects emails until it's live,
  then the Shop links should point at the storefront.
- Contact email `partnerships@musicdiscoveryhour.com` is a Google Workspace mailbox; its MX
  records live in **Netlify DNS** (not the registrar).
