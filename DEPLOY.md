# Deploying iscarcreations.com

## How this site works (Workers, standardized 2026-07-17)

- This repo (`ISolo-AV-Design/iscar-creations-website`) is a static site:
  `index.html` plus `_headers` / `_redirects`. No build step, no framework.
- The live site is served by a Cloudflare **Worker** named `iscar-creations-website`,
  using Workers' static-assets feature (`assets.directory: "."` in `wrangler.jsonc`)
  -- **NOT Cloudflare Pages.** (This site was migrated from Pages to Workers on
  2026-07-17 so it matches the ISolo AV Design site's deploy model. See the shared
  standard: `ISolo Enterprises/WEBSITE-DEPLOY-STANDARD.md`.)
- Push to `main` auto-deploys via **Cloudflare Workers Builds** (deploy command
  `npx wrangler deploy`, no build command, root directory `/`, watch paths `*`),
  once that connection is set up in the Cloudflare dashboard.
- Sibling repo: `ISolo-AV-Design/Isolo-av-design-website` (isoloavdesign.com) uses
  the identical model. This is the intended, matching setup.

## Deploying a change (push and it's live)

1. Pull first: `git pull --ff-only`.
2. Edit `index.html` (and/or `_headers`, `_redirects`). Review the diff.
3. Commit and push to `main` -- this deploys automatically via Workers Builds.
   Prefix commits with the agent (e.g. `Cowork: ...`, `Hermes: ...`).
4. Wait ~1 minute, then verify the live site actually reflects the change (see below).

### Manual fallback
`npx wrangler deploy` from the repo root still works (requires `CLOUDFLARE_API_TOKEN`
with Workers Scripts:Edit and `wrangler` installed). Use only if Workers Builds is
down -- routine use lets the deployed version and `git log` drift apart.

## CRITICAL: `.assetsignore` must stay in this repo

`assets.directory` is `"."` (the whole repo root), and the Workers static-assets
uploader does **not** auto-skip `.git/` the way Pages did. Without `.assetsignore`,
a deploy will upload `.git/config` etc. as publicly servable files (this really
happened on the AV Design site before its `.assetsignore` existed -- `.git/config`
was live at a public URL). **Do not delete or weaken `.assetsignore`.** If any deploy
log mentions uploading anything under `.git/`, stop and fix it immediately.

## Edge-cache gotcha (inherited lesson from AV Design)

Cloudflare can edge-cache the plain `/` response independently of deploys, so a
successful push+deploy may still show stale content at `https://iscarcreations.com/`.
When verifying, append a throwaway query string (`?cb=1`) to bypass cache. If the
cache-busted version is correct but the plain URL isn't, it's a caching issue, not a
deploy failure -- purge cache for `/` in the Cloudflare dashboard.

## Auth (never commit secrets)

The GitHub remote URL must NOT contain a token. Auth goes through Windows Credential
Manager (prompted on first push). A token was once leaked here via `.git/config`
syncing to Google Drive -- which is exactly why this repo now lives in
`C:\Users\ivanc\Documents\Hermes Projects\`, not in Drive.

## Pre-launch to-do (still open)
- [ ] Replace Formspree placeholder id in `index.html`.
- [ ] Confirm contact email (`hello@iscarcreations.com`).
- [ ] Add finalized logo asset + favicon.
- [ ] Real product photos for the gallery.
- [ ] Cloudflare dashboard: create the Worker + Workers Builds connection, remove the
      old Pages project, connect `iscarcreations.com`. (See parent project handoff.)

## www -> apex redirect (set up 2026-07-17)
Handled by a zone-level Cloudflare Redirect Rule ("Redirect from WWW to root"
template: match `https://www.*` -> `https://${1}` 301), plus a proxied CNAME
`www -> iscarcreations.com`. NOT via `_redirects` -- Workers static assets only
allows relative paths there. Confirmed working: https://www.iscarcreations.com/
301s to https://iscarcreations.com/.
