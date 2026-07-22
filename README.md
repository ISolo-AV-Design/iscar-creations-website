# ISCar Creations Website

Static site for [iscarcreations.com](https://iscarcreations.com) — served by a
Cloudflare **Worker** (static assets). See `DEPLOY.md` for the full workflow.

## Stack

- Plain HTML/CSS — no build step required
- Font: Jost via Google Fonts
- Contact form: Formspree (endpoint `/f/mvzeapqw`, live and confirmed working)

## Deploy

Cloudflare **Workers** (static assets) via Workers Builds — push to `main`
auto-deploys. Config lives in `wrangler.jsonc`; `.assetsignore` keeps `.git/` and
internal docs from being served. Full details and gotchas: `DEPLOY.md`.

- **Worker name:** `iscar-creations-website`
- **Deploy command:** `npx wrangler deploy` (no build step)
- **Branch:** `main`

## Domain

`iscarcreations.com` — connect in Cloudflare dashboard (Workers & Pages →
`iscar-creations-website` → Custom Domains).

## To-do before launch

- [x] Wire the contact form to the real Formspree endpoint (`/f/mvzeapqw`) — done, confirmed working
- [x] Add favicon set from the gradient logo mark — done (favicon.svg, favicon.ico, apple-touch-icon.png)
- [x] Connect `iscarcreations.com` domain to the Worker (Workers & Pages → `iscar-creations-website` → Custom Domains) — done
- [ ] Confirm `hello@iscarcreations.com` receives mail (advertised on the site + used for form replies)
- [ ] Replace gallery placeholder cards with real product photos
- [ ] Add a finalized standalone logo image file once available
