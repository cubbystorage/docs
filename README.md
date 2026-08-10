# cubbystorage/docs

Published static documentation site, served via GitHub Pages.

This repository contains **rendered output only** — there is no build tooling
here. It is a deploy target, not a builder.

## Contents

- `api/index.html` — the external API documentation. This page is **self-contained**
  (CSS and JS are inlined), so it does not depend on the shared asset folders.
- `stylesheets/`, `javascripts/`, `images/`, `fonts/` — shared, fingerprinted
  assets referenced by the other rendered pages.
- `data-dictionary/`, `make-webhooks/`, `storefront-css/`, `index.html` — other
  published pages/assets.

## Updating the API docs (`api/index.html`)

The API docs are authored in the main
[`cubbystorage/cubby`](https://github.com/cubbystorage/cubby) repo under
`doc/external-api/`. The current page is built with the **Node/Shins** pipeline
(`doc/external-api/build_doc_shins`), which renders the Slate-flavored markdown
without Docker and emits a single self-contained HTML file. To update this site:

1. In `cubby/doc/external-api`, edit the `*.md` sources and run `./build_doc_shins`
   to regenerate `cubby/doc/external-api/html-shins/index.html`.
2. Copy that file to `api/index.html` here. Because the output is self-contained,
   that single file is the whole update — no asset folders need to be copied.
3. Commit and push; GitHub Pages serves the update.

See `cubby/doc/external-api/README.md` for the full build/deploy process and
details on the Shins toolchain.

### History: this page used to be built with Slate

Previously `api/index.html` was produced by the Ruby/Docker **Slate** toolchain
(`cubby/doc/external-api/build_doc`, the `slatedocs/slate` image) and referenced
external fingerprinted assets (`../stylesheets/screen-<hash>.css`,
`../javascripts/all-<hash>.js`, etc.) from the folders above. The project moved
to Shins after the upstream Slate repository was removed from GitHub and its
Docker image became unreliable to pull; Shins renders the same sources and its
output was verified to match the Slate build (layout, sidebar nav, TOC, and
Monokai code highlighting).

If you ever rebuild the page with the canonical Slate `build_doc` instead, the
output will again reference the external asset files — in that case copy
`html/index.html` **and** the `html/stylesheets/`, `html/javascripts/`,
`html/images/`, and `html/fonts/` folders together, keeping the fingerprinted
filenames in sync so the references resolve.
