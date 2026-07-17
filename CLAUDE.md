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

- `index.html` — home: floating nav over a full-bleed video hero → Behind the Curtain
  session section (dark) → Instagram grid → footer
- `shop.html` — merch coming-soon page with email signup (Netlify Forms, name `shop-signup`)
- `contact.html` — contact card (copy email / mailto to partnerships@musicdiscoveryhour.com)
- `style.css` — all styles, shared across pages
- `assets/` — hero loop video, poster, session thumb, IG covers (`ig-1..4.jpg`), BTC emblem,
  favicons

## Brand rules (decided by the owner — do not undo)

- **No follower counts anywhere.** No "1M followers" copy, no stats strips.
- **No taglines** in the hero ("Your next favorite song…" was rejected as cheesy).
  The hero is the video, the floating nav links, nothing else.
- **No buzzy claim-y copy** ("the feed that found them first" was rejected — MDH is a music
  blog, it doesn't claim to have discovered artists). Current IG section title:
  "Artists & moments we love."
- The featured session is labeled **"Featured Session"** (not "Session 001").
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
- **Spotify ⌄** → Official Playlist, Top Songs of 2025, Top Songs of 2024

Dropdowns: hover-open on desktop with an invisible bridge + 300ms grace (don't reintroduce
the hover-gap bug); tap-toggle everywhere; fixed sheet under the nav on ≤720px because the
nav row scrolls horizontally there.

## Media recipes

- **Hero loop**: cut from the 4K master on the owner's Mac
  (`~/Documents/Most Recent Lee Lewis Files/New Folder With Items/MDH Lee Lewis Final.mov`).
  It's a seamless crossfade loop: 18s segment, last 1s xfaded into the first 1s
  (ffmpeg `xfade` offset = len−2·fade), cropped ~75% biased right/up to exclude the
  burned-in BTC badge, 1280×720 CRF 27 → ~1MB. Don't replace with a hard-cut loop — the
  camera dollies, so a plain loop jump reads as a glitch.
- **IG grid covers**: Instagram's API thumbnail is the *wrong frame* (last frame of slide 1).
  The true cover = **frame zero of the first slide**: `yt-dlp --playlist-items 1` (with the
  cookies file from the DiscoveryClipper backend) then `ffmpeg -frames:v 1`. Self-host as
  `assets/ig-N.jpg` — never use IG embed.js (blocked/throttled; already tried and removed).

## Related but separate

- The DiscoveryClipper app (repo `spyqs/MDPOUR`, at `~/Projects/DiscoveryClipper`) makes the
  IG clips. Different project — never commit/push it from a session working on this site.
- Merch storefront (Fourthwall) is in progress; `shop.html` collects emails until it's live,
  then the Shop links should point at the storefront.
- Contact email `partnerships@musicdiscoveryhour.com` is a Google Workspace mailbox; its MX
  records live in **Netlify DNS** (not the registrar).
