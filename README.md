# cubbystorage/docs

Published static documentation site, served via GitHub Pages.

This repository contains **rendered output only** — there is no build tooling
here. It is a deploy target, not a builder.

## Contents

- `api/index.html` — the external API documentation.
- `stylesheets/`, `javascripts/`, `images/`, `fonts/` — shared, fingerprinted
  assets referenced by the rendered pages.
- `data-dictionary/`, `make-webhooks/`, `storefront-css/`, `index.html` — other
  published pages/assets.

## Updating the API docs (`api/index.html`)

The API docs are authored and built in the main
[`cubbystorage/cubby`](https://github.com/cubbystorage/cubby) repo under
`doc/external-api/` (Slate sources + `build_doc`). To update this site:

1. In `cubby/doc/external-api`, edit the `*.md` sources and run `./build_doc`
   to regenerate `cubby/doc/external-api/html/` (requires Docker with Docker Hub
   access — see that folder's `README.md` for details and caveats).
2. Copy the rendered output into this repo:
   - `html/index.html` → `api/index.html`
   - `html/stylesheets/`, `html/javascripts/`, `html/images/`, `html/fonts/`
     → the matching top-level folders here.
3. Keep asset hashes in sync: `api/index.html` references specific fingerprinted
   filenames (e.g. `stylesheets/screen-9c19b03a.css`). If a Slate version bump
   rehashes assets, copy all of them together so the references still resolve.
4. Commit and push; GitHub Pages serves the update.

See `cubby/doc/external-api/README.md` for the full rebuild/deploy process, the
Slate-toolchain risk, and migration options.
