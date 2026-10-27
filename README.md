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

## Updating the data dictionary (`data-dictionary/`)

`data-dictionary/index.html` renders `data-dictionary/schema.csv`, which sits
next to it in this repo. The CSV is exported from the "Cubby-partner-data -
Schema" Google Sheet, which the `syncSchema()` Apps Script in the schema helper
spreadsheet keeps current.

The page used to fetch that sheet straight from Google. That stopped working
when a Workspace sharing policy change blocked anonymous access to the sheet,
so the CSV is now committed here instead. To publish a schema change:

1. Run `syncSchema()` in the helper spreadsheet.
2. Run the `publish-data-dictionary` script. It downloads the sheet with your
   own Google login (one-time setup: `gcloud auth login --enable-gdrive-access`),
   refuses to publish anything missing the `Table_Name`, `Column_Name`,
   `Data_Type` and `Description` columns, and commits `schema.csv` to `main`
   with your `gh` login. It does nothing if the CSV hasn't changed;
   `--dry-run` shows what it would do.

The page's CSV parser reads one line per row, so cells must not contain line
breaks.
