# Tool Name

One sentence saying what this tool does.

**[Open the tool »](https://willkuhn.github.io/random-iNat-tools/tools/TOOL-SLUG/)**

<!-- Optional: a screenshot saved next to this file, e.g. ![Screenshot](screenshot.png) -->

## What it does

A short paragraph on the question this tool answers and who might find it useful.

## How to use it

1. Enter a taxon ID (the number at the end of a taxon page URL, like `47157` in `inaturalist.org/taxa/47157`).
2. Optionally narrow the search with a place ID.
3. Press **Go**. The address bar updates with your settings, so you can bookmark or share the result.

## URL parameters

| Parameter | Required | Meaning | Example |
|:----------|:---------|:--------|:--------|
| `taxon_id` | yes | iNaturalist taxon ID | `47157` |
| `place_id` | no | iNaturalist place ID | `1` |

## How it uses the iNaturalist API

- **Endpoints:** `GET /v2/observations` (count only, `per_page=0`)
- **Requests per run:** 1
- **Sign-in:** not needed

Requests are throttled to about one per second. Searches matching more than 10,000 records are refused; narrow the filters or use iNaturalist's [observation export](https://www.inaturalist.org/observations/export) instead.

## Limitations

Anything the tool can't do, known quirks, or caveats about interpreting the results.

## Inspiration

Link to the forum thread, conversation, or idea that prompted this tool.
