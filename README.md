# Music Discovery Hour — Website

Static link-in-bio site for [@musicdiscoveryhour](https://instagram.com/musicdiscoveryhour).

## Files

- `index.html` — Page markup
- `style.css` — All styles (earth tones, minimal)

## Local preview

```bash
npx serve .
```

Then open http://localhost:3000

## Before you deploy — quick edits to make

Open `index.html` and replace these placeholders with real URLs:

1. **Spotify playlist links** — search for `https://open.spotify.com` and replace with your actual playlist URLs
2. **Apple Music link** — search for `https://music.apple.com` and replace
3. **Shop link** — the "Shop MDH Merch" button currently goes to `#shop`. Replace with your store URL when ready (or remove the button)
4. **Contact form** — replace `YOUR_FORM_ID` in the `<form action="...">` with a real Formspree ID. Sign up free at https://formspree.io, create a form, copy the form ID into the action URL.

## Deploying to Netlify (recommended — free)

1. Go to https://app.netlify.com/drop
2. Drag the entire `musicdiscoveryhour-site` folder onto the page
3. Netlify gives you a temporary URL like `random-name.netlify.app`
4. In Netlify dashboard → Site settings → Domain management → Add custom domain → enter `musicdiscoveryhour.com`
5. Netlify shows you DNS records to add. In GoDaddy:
   - Log into GoDaddy → My Products → DNS for musicdiscoveryhour.com
   - Either:
     - **Option A (easiest):** Change nameservers to Netlify's nameservers (Netlify will tell you which ones)
     - **Option B:** Add an A record pointing to Netlify's load balancer IP (`75.2.60.5`) and a CNAME for `www` pointing to your Netlify subdomain
6. Wait a few minutes for DNS to propagate. Done.

Netlify handles HTTPS automatically.

## Alternative: Vercel

Same process — drag-and-drop at https://vercel.com/new, then connect the domain.

## Future ideas

- Add a real shop (Shopify Lite, Gumroad embed, or Stripe payment links)
- Newsletter signup (Buttondown / ConvertKit / Mailchimp embed)
- Individual artist spotlight pages
- RSS/blog for written reviews
