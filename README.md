# ISCar Creations Website

Static site for [iscarcreations.com](https://iscarcreations.com) — served by a
Cloudflare **Worker** (static assets). See `DEPLOY.md` for the full workflow.

## Stack

- Plain HTML/CSS — no build step required
- Font: Jost via Google Fonts
- Contact form: Formspree (replace `PLACEHOLDER` in form action with real form ID)

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

- [ ] Replace Formspree placeholder ID in `index.html` form action
- [ ] Add real contact email (`hello@iscarcreations.com` or update to preferred address)
- [ ] Replace gallery placeholder cards with real product photos
- [ ] Add logo/brand asset once finalized
- [ ] Connect `iscarcreations.com` domain in Cloudflare Pages
