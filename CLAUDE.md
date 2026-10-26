# random-iNat-tools

One-off, browser-based tools for handling, summarizing, and visualizing iNaturalist data, modeled on [jumear/stirfry](https://github.com/jumear/stirfry). Published at github.com/willkuhn/random-iNat-tools and meant to be shared publicly, so code and docs should be clear to strangers.

## Layout

```
README.md               project overview + the table of tools (keep it current)
tools/_template/        starting point for every new tool (Jekyll skips _-prefixed dirs on Pages)
tools/<tool-slug>/
  index.html            the tool: one self-contained page
  README.md             user-facing docs for that tool
reference/              local copies of iNat's guideline pages (gitignored; read these before building)
```

Hosting is GitHub Pages from the repo root, so a tool lives at `https://willkuhn.github.io/random-iNat-tools/tools/<tool-slug>/`.

## Building a tool

1. Copy `tools/_template/` to `tools/<tool-slug>/` (lowercase, hyphenated, descriptive: `species-accumulation-curve`, not `tool1`).
2. Replace every placeholder: `Tool Name`, `TOOL-SLUG`, the meta description, the form fields, and `run()`.
3. Fill in the tool's README from the template headings, including the endpoints used and roughly how many requests one run makes.
4. Add a row to the Tools table in the root README.
5. Test locally with `python3 -m http.server 8000` and open `http://localhost:8000/tools/<tool-slug>/`. Check light and dark mode and a narrow (phone-width) window.

### Conventions

- **One self-contained HTML file per tool.** Inline CSS and JS, no build step, no framework, no backend. Someone should be able to save `index.html` and open it. If a library is genuinely needed (charts, maps), load a pinned version from a CDN (jsDelivr/cdnjs/unpkg) and say so in the tool's README.
- **Copy the API client block from the template unchanged** (`inatFetch`, `inatFetchAll`, `inatFetchByIds`). If it needs a fix, fix the template and every tool that copied it.
- **State lives in the URL.** Form inputs are named after URL parameters, and where possible after the matching iNat API parameter (`taxon_id`, `place_id`, `user_id`, `d1`/`d2`, ...), so URLs can be copied between iNat's site, the API, and these tools.
- **Escape API text** with `escapeHtml()` before putting it in `innerHTML`. Taxon names, user names, and descriptions are user-supplied.
- **Link back to iNat** for every record shown (observation, taxon, user, place), and show media with its attribution and license.
- **Accessible and responsive.** Real `<label>`s, a live status region, semantic tables, readable contrast in both color schemes, no horizontal scrolling on phones except inside wide tables.
- Prefer API v1 (`api.inaturalist.org/v1`). Use v2 only when a tool needs something v1 lacks, and note it in the tool's README.

## iNaturalist rules every tool must follow

Sources (read the full text in `reference/` or online): [API Recommended Practices](https://www.inaturalist.org/pages/api+recommended+practices), [Community Guidelines](https://www.inaturalist.org/pages/community+guidelines), [Machine-generated content](https://www.inaturalist.org/pages/machine_generated_content), [Terms of Use](https://www.inaturalist.org/pages/terms).

- **About 1 request per second, ~10k per day.** The template's client enforces the interval globally and backs off on HTTP 429. Never parallelize around it. Design tools so a typical run makes a handful of requests, and show progress for anything over ~10.
- **Ask only for what's needed.** Use `per_page=0` when only a count is needed, filter server-side rather than client-side, and use `fields`/`only_id` style options where the endpoint supports them.
- **Largest page size** (200 for most endpoints, 500 for `species_counts`), and **batch ID lookups** (`/observations/1,2,3`). Per-endpoint ID limits are in the template.
- **No paging past 10,000 results.** Walk large-but-bounded result sets with `order_by=id&order=asc&id_above=<last id>`. Refuse searches over 10,000 records and point people to the [observation export](https://www.inaturalist.org/observations/export) or the [GBIF dataset](https://doi.org/10.15468/ab3s5x) instead. For tools that need lots of data, accept an uploaded export CSV and process it in the browser.
- **Unauthenticated by default.** Only use a JWT (from `https://www.inaturalist.org/users/api_token`, sent as the `Authorization` header) when the data truly requires it, since authenticated responses aren't cached. Never store tokens beyond the session or send them anywhere except `api.inaturalist.org`.
- **Read-only.** Tools must not post observations, identifications, comments, annotations, DQA votes, or messages without a human deciding on each individual item. Anything that writes must use OAuth (PKCE) so people act as themselves, never a shared account, and never automate messages. Treat any write feature as a design discussion with Will first.
- **User-Agent.** Browsers won't let pages set it, so skip it in HTML tools. Any non-browser script (Python, Node) should send `User-Agent: random-iNat-tools/<tool-slug> (github.com/willkuhn/random-iNat-tools)`.
- **Media limits:** stay well under 5 GB/hour and 24 GB/day. Use iNat's `square`/`small`/`medium` photo URLs, not `original`, unless the tool is specifically about full-size images.
- Don't imply iNat endorsement; keep the "not affiliated" line in each footer.
