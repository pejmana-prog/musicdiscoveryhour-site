# Music Discovery Hour — musicdiscoveryhour.com

The public site for [Music Discovery Hour](https://www.instagram.com/musicdiscoveryhour/),
a music blog on Instagram, and its live-performance series
[Behind the Curtain](https://www.youtube.com/channel/UCTLJP53Q57KqO9Y9fJAC_dw).

Plain static HTML/CSS/JS. No framework, no build step, no dependencies.

## Files

| File | What it is |
|------|------------|
| `index.html` | Home: masthead, full-bleed hero loop, MDH Instagram mosaic + featured Behind the Curtain session, footer |
| `shop.html` | Merch coming-soon page with an email signup (Netlify Forms, form name `shop-signup`) |
| `contact.html` | Contact card (copy email / mailto to partnerships@musicdiscoveryhour.com) |
| `404.html` | Branded not-found page (Netlify serves it automatically) |
| `style.css` | All styles, shared by every page |
| `assets/` | Hero loop video + poster, session thumbnail, IG covers (`ig-1..9.jpg`), BTC emblem, favicons |
| `robots.txt`, `sitemap.xml` | Crawl hints; the sitemap lists the three pretty URLs |

Brand rules, nav structure and the media recipes (hero loop, IG cover frames) are documented
in [`CLAUDE.md`](CLAUDE.md) — read it before changing copy or layout.

## Local preview

Any static server works, for example:

```bash
python3 -m http.server 8000
```

Then open http://localhost:8000. Locally the pages are reached as `/shop.html` and
`/contact.html`; the pretty URLs below only exist on Netlify.

## Hosting and deploy

The site is hosted on **Netlify**, connected to the `main` branch of this repository.

- **Push to `main` = deploy.** Netlify publishes the repo root as-is (no build command) in
  about 30 seconds to https://musicdiscoveryhour.com.
- Pull the latest `main` before editing — more than one machine works on this repo.
- Check a deploy landed with `curl -s https://musicdiscoveryhour.com/ | grep "<something new>"`.
- Netlify also provides:
  - **Pretty URLs**: `/shop` → `shop.html`, `/contact` → `contact.html` (canonical and
    `og:url` tags use the pretty form).
  - **Forms**: the shop signup posts to Netlify Forms (`shop-signup`); submissions are in the
    Netlify dashboard under Forms.
  - **DNS**: `musicdiscoveryhour.com` and the Google Workspace MX records for the
    `partnerships@` mailbox live in Netlify DNS.

## Related

- The Instagram clips are made with the DiscoveryClipper app (separate repo).
- Merch storefront (Fourthwall) is in progress; `shop.html` collects emails until it is live.
