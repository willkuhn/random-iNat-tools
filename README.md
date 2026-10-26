# random-iNat-tools

Small, single-purpose tools for handling, summarizing, and visualizing [iNaturalist](https://www.inaturalist.org) data.

Each tool is one HTML page that runs entirely in your browser and talks directly to the public [iNaturalist API](https://api.inaturalist.org/v1/docs/). There's nothing to install, no account to create, and no server in the middle: open a tool and go.

Inspired by [@jumear](https://github.com/jumear)'s excellent [stirfry](https://github.com/jumear/stirfry) collection.

## Tools

| Tool | What it does | Docs |
|:-----|:-------------|:-----|
| [Smokies ATBI new records](https://willkuhn.github.io/random-iNat-tools/tools/smokies-atbi-new-records/) | Dashboard of iNaturalist observations flagged as new records for Great Smoky Mountains National Park's All Taxa Biodiversity Inventory: counts by stage and rank, top observers, and the confirmation queue. | [docs](https://github.com/willkuhn/random-iNat-tools/tree/main/tools/smokies-atbi-new-records) |

## Using the tools

**In your browser.** Click a tool's name in the table above. Tools are hosted on GitHub Pages at `https://willkuhn.github.io/random-iNat-tools/`.

**Share or bookmark a result.** Tools that take settings keep them in the URL query string (for example `?taxon_id=47157&place_id=1`), so the address bar always describes exactly what you're looking at. Copy it to share a result or come back to it later.

**Run it locally.** Clone the repo and serve it with any static file server:

```bash
git clone https://github.com/willkuhn/random-iNat-tools.git
cd random-iNat-tools
python3 -m http.server 8000
```

Then open `http://localhost:8000/tools/<tool-name>/`. Each tool's `index.html` is self-contained, so you can also save just that one file and open it directly.

## Playing nicely with iNaturalist

iNaturalist is run by a nonprofit and shared by a large community. These tools are built to follow iNat's [API Recommended Practices](https://www.inaturalist.org/pages/api+recommended+practices), [Community Guidelines](https://www.inaturalist.org/pages/community+guidelines), and [policy on machine-generated content](https://www.inaturalist.org/pages/machine_generated_content):

- **Read-only.** No tool creates, edits, or deletes anything on iNaturalist. If a future tool ever helps you post, you'll sign in as yourself and approve each individual observation, identification, or comment. Nothing gets posted automatically.
- **Gentle on the API.** Requests are throttled to about one per second, use the largest page size the API allows, combine lookups by ID into single requests, and back off when iNat says "too many requests."
- **No bulk downloading.** Tools stop at the API's 10,000-result ceiling and ask you to narrow your search. For bigger jobs, use iNat's [observation export](https://www.inaturalist.org/observations/export) or the [research-grade dataset on GBIF](https://doi.org/10.15468/ab3s5x) (which also gives you a citable DOI). Some tools can read an export file instead of calling the API.
- **No sign-in unless it's needed.** Most tools use only public data. A tool that needs your API token says so up front, keeps it in your browser, and sends it only to `api.inaturalist.org`.
- **Credit where it's due.** Observations, photos, and sounds belong to the people who made them. Tools show media with its license and attribution and link back to the original records.

## Disclaimer

This project is not affiliated with or endorsed by iNaturalist. Data are retrieved live from the iNaturalist API and are only as accurate as the underlying records. Tools are provided as-is (see [LICENSE](LICENSE)).

## Feedback and contributing

Found a bug or have an idea for a tool? [Open an issue](https://github.com/willkuhn/random-iNat-tools/issues). Pull requests are welcome; new tools should start from [`tools/_template/`](tools/_template/) and follow the guidelines above.

## License

Code is released under the [MIT License](LICENSE). iNaturalist data and media accessed through these tools remain subject to their own licenses and to iNaturalist's [Terms of Use](https://www.inaturalist.org/pages/terms).
