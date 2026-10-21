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

- `index.html` — home: masthead (lowercase wordmark over nav links; icon-only on phones) →
  full-bleed video hero → ONE section: the Behind the Curtain sessions carousel (swipeable
  scroll-snap track, arrows on desktop, tap a card to play it in place, one at a time) →
  footer (no links, "© Music Discovery Hour, Inc."). The MDH Instagram posts grid was
  removed Oct 2026 on purpose (posts go stale and don't drive the business) — don't re-add it.
  To add a session: copy an `<li class="sess-card">` (newest first) and its 1280x720 thumb.
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
  blog, it doesn't claim to have discovered artists).
- **Keep it simple.** Sessions carousel cards show just artist + song — no "Featured
  Session" / "Session 001" badges (gold labels were rejected).
- **MDH emblem**: black circle, white lowercase Montserrat wordmark "music discovery hour."
  (the text group has `translate(-3 -13)` so the block sits optically centred — keep it)
  — implemented as inline SVG (nav on shop/contact, footer). The old "mdh" avatar is retired.
- **Brand spelling:** visible text always writes it **"behind the curtain."** — lowercase, with the
  period (section title, menu labels). Title-case is fine only in alt/aria/meta text.
- **Behind the Curtain** is its own brand: separate IG (@behindthecurtainseries) and
  YouTube channel. MDH main IG is @musicdiscoveryhour.
- Colours come from the Behind the Curtain set. Below the hero the surface is **deep rose**
  (`--rose-surface #8f5459`, the curtains' mid-tone; footer `--rose-deep #7d4a4f`) with WHITE
  text (5.9:1) and `--rose-soft #ecdfe0` secondary text (4.5:1) — the owner asked for a darker
  rose so the text could be white (it was light blush + green ink before). Green from the floor
  shows up as a soft glow. The owner found the black section and the gold text ugly —
  no black backgrounds below the hero, no gold. Cream `--bg #f5f0e8` on shop/contact.
- Type: **Poppins** display
  (the same family as the IG cover cards — Fraunces serif was retired Oct 2026 because it
  clashed with the emblem/covers) + Inter UI. Masthead mirrors the emblem: lowercase
  "music discovery hour.". Keep it.

## Nav structure

Links float over the hero on `index` (`.topnav-overlay`), sit on cream on other pages
(`.topnav-page`). Three dropdowns (`.nav-drop`, shared JS at the bottom of each page):

- **Instagram ⌄** → Music Discovery Hour, Behind the Curtain
- **YouTube ⌄** → Behind the Curtain (channel UCTLJP53Q57KqO9Y9fJAC_dw)
- **TikTok ⌄** → Behind the Curtain (@btc.series)
- **Spotify ⌄** → Official Playlist, Top Songs of 2025, Top Songs of 2024,
  Region Spotlight — Brazil

Look: every nav item is a translucent **soap bubble** (owner's ask — no solid fills):
clear middle, glossy top-left highlight, 1.5px iridescent conic-gradient rim (masked ring),
soft white glow that brightens on hover/open. White single-colour icons over the video
(brand colours overridden to `currentColor`; Apple Music drawn as an outlined tile + note),
green ink on the cream pages. No backdrop-filter (it lags over video). Pills with labels on
desktop, 38px icon-only bubbles on phones. Menus are light cream. This lives in the "Nav
buttons" block at the END of style.css (it overrides the older nav rules) — keep it last.
The bubble is the SITE-WIDE button language (Oct 2026, owner asked to standardise): channel
icons, carousel arrows, the thumbnails' play button and the dropdown menus (a tinted bubble
"gel" pane: dark-rose tint over video so text stays readable, white gel on cream pages) all
share it. Ink rule: white over video/photos and on the rose surface, green ink (`--green-ink`) on the
cream shop/contact pages. These
overrides are in the "One look for the whole site" block at the very end of style.css.
Footer is a slim one-line strip (26px emblem + copyright) — the owner found the big footer
wasted space; don't grow it back.

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
  bottom-left corner, **1920×1080** CRF 23 → ~1.8MB (720p looked blurry stretched across a
  2000px-wide hero — always cut from the 4K source, format 313). Poster = frame 0 of the loop. Pick a passage
  that is one continuous wide shot with slow movement and no lip-sync close-up (the hero is
  muted). Don't replace with a hard-cut loop — the camera moves, so a plain loop jump reads as
  a glitch.
- **Session thumbs** (`assets/session-<artist>.jpg`, 1280×720): the same framing for every
  performance — a clean WIDE shot, performer(s) centred, no title card, badge left in
  (`ffmpeg -ss T -i master -frames:v 1 -vf scale=1280:720:flags=lanczos -q:v 2`). Frames used:
  Raveena 2:32, GTA 3:05, FOR MIA 1:48, Lee Lewis 1:05. Use the GRADED source: the
  "Finals For Color Grading_*" files in ~/Documents are ungraded log masters, so grab from the
  YouTube upload (1080p is enough) or the Lee Lewis 4K master. The YouTube title cards animate artist and song
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
