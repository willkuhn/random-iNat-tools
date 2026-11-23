# Who Eats Whom food web

A dashboard of iNaturalist observations tagged for the [Who Eats Whom](https://www.inaturalist.org/projects/who-eats-whom) project, at every quality grade: a food web, flow diagrams of who eats whom, and how close each pair is to reaching [whoeatswhom.org](https://whoeatswhom.org/).

**[Open the dashboard »](https://willkuhn.github.io/random-iNat-tools/tools/who-eats-whom-food-web/)**

## What it does

Who Eats Whom collects feeding interactions on iNaturalist as pairs of observations, one of the eater and one of the organism being eaten, linked to each other with three observation fields:

| Field | ID | Values |
|:------|:---|:-------|
| [ID meant for "eater" or organism being eaten?](https://www.inaturalist.org/observation_fields/12795) | 12795 | `eater`, `organism being eaten` |
| [URL for "partner" observation](https://www.inaturalist.org/observation_fields/12796) | 12796 | the partner's URL; several are joined with ` / ` |
| [Is observation one of these special types of feeding?](https://www.inaturalist.org/observation_fields/16376) | 16376 | `No, none of these`, `Nectar/pollen feeding`, `Parasitism`, `Scavenging or Decomposition` |

The Who Eats Whom website only uses pairs in which both observations are research grade, so a pair can wait a long time before it shows up there, and some never will. This dashboard reads the fields straight from iNaturalist and shows every tagged pair, whatever its quality grade:

- **Summary tiles:** tagged observations, linked pairs, taxa, pairs ready for Who Eats Whom, and observers.
- **On the way to Who Eats Whom:** every pair sorted into one of four statuses (below), with counts and a link to each group's list.
- **Food web:** every taxon as a circle and every interaction as an arrow from the food to the consumer, as on Who Eats Whom. Circles are colored by group (plants & fungi, invertebrates, vertebrates, other) and sized by how many pairs they're in; thicker lines mean more pairs, and dashed lines have no pair ready for Who Eats Whom yet. Select a taxon (or find it by name) to highlight what it eats and what eats it, see the lists with links to the observations, and jump from partner to partner. Zoom, drag, or go full screen.
- **Who eats whom, by group:** a Sankey diagram (food on the left, consumers on the right) or a radial version of it, with each side grouped at its own rank (for example plant families against insect orders) and the smallest groups combined into "Other."
- **Consumers with the most foods** and **foods with the most consumers**, at the food web's grouping.
- **Pairs by feeding type** and **by month observed** (all years combined).
- **Pairs:** a sortable table of every pair with photos, quality grades, feeding type, date, observer, status, and exactly what's holding each one back. Filter it by status and download it as a CSV.
- **Tagged observations that couldn't be paired:** observations with the role field but no usable partner link, such as an empty or mistyped URL, a link to the observation itself, or two linked observations with the same role.

The filter row above the food web applies to everything below it: show all tagged pairs or only those ready for Who Eats Whom, pick feeding types, choose how taxa are grouped (as identified, species, genus, family, or order), and flip the **Names** switch between common and scientific names. The switch changes every name on the page: the food web's labels, the flow diagrams, the lists, and the tables, where the other name appears underneath. Taxa with no common name always show their scientific name. Toggling "Only pairs ready for Who Eats Whom" shows roughly what the Who Eats Whom website would show for the same search.

## How to use it

1. Fill in any of the search boxes: an observer's login, a project ID or slug, a place ID, a taxon ID, or a date range. Anything else iNaturalist's observation search accepts goes in **Other search parameters**, written like a URL query, e.g. `month=6,7&iconic_taxa=Insecta`.
2. Press **Load**. The address bar updates with your search, so you can bookmark or share the result.

Some searches to try:

- [Everything one person has tagged](https://willkuhn.github.io/random-iNat-tools/tools/who-eats-whom-food-web/?user_id=willkuhn): `?user_id=willkuhn`
- [Tagged observations in a project](https://willkuhn.github.io/random-iNat-tools/tools/who-eats-whom-food-web/?project_id=smokies-atbi): `?project_id=smokies-atbi`
- [Butterflies and moths in Tennessee](https://willkuhn.github.io/random-iNat-tools/tools/who-eats-whom-food-web/?place_id=45&taxon_id=47157): `?place_id=45&taxon_id=47157`

The search picks the tagged observations; their partners are then loaded wherever they are. So a search for butterflies and moths also brings in the plants they were eating, and a project search includes partners outside the project.

## URL parameters

Every parameter in the page's URL except `names` is passed to iNaturalist's [observation search](https://api.inaturalist.org/v2/docs/#/Observations/get_observations) as is, plus `field:ID meant for "eater" or organism being eaten?=` (any value), which limits the search to observations tagged for Who Eats Whom. The search boxes cover the common ones:

| Parameter | Meaning | Example |
|:----------|:--------|:--------|
| `user_id` | Observer login or ID | `willkuhn` |
| `project_id` | Project ID or slug | `smokies-atbi` |
| `place_id` | iNaturalist place ID | `45` |
| `taxon_id` | Taxon ID; only tagged observations of this taxon are searched | `47157` |
| `d1`, `d2` | Observed on or after, on or before | `2025-01-01` |
| `names` | `scientific` to show scientific names (the Names switch). A display setting: it isn't sent to iNaturalist. | `scientific` |
| anything else | Any other observation search parameter, such as `month`, `year`, `quality_grade`, `iconic_taxa`, `place_id` lists, or `locale` (for common names in another language) | `month=6,7` |

At least one parameter is needed: all of Who Eats Whom is more than 25,000 tagged observations, past what the tool will fetch (see below). The tool sets `fields`, `per_page`, `order_by`, `order`, and `id_above` itself and ignores them in the URL.

## How pairs are built and judged

Two observations form a pair when either one lists the other in its partner field. The role field says which is the eater; if only one of the two has it, the other is taken to have the opposite role (and the pair is flagged). Partner URLs on any iNaturalist network site (inaturalist.ca, uk.inaturalist.org, ...) and with either the observation's number or its UUID are recognized. Older field values are folded into the current ones: `thing being eaten` counts as `organism being eaten`, `Pollination` as nectar or pollen feeding, and `Scavenging` as scavenging or decomposition. A pair's feeding type comes from the eater, or from the food if the eater doesn't have one.

Each pair gets one status, checked in this order:

| Status | Meaning |
|:-------|:--------|
| **Tagging to check** | Something to fix in the tags: a link that goes only one way, an observation with no role, or one that isn't in the Who Eats Whom project. |
| **Casual** | One or both observations are casual (often captive or cultivated, or missing a date or location), so Who Eats Whom won't use the pair. |
| **Waiting on research grade** | Tagged and linked; one or both observations still need ID agreement. |
| **Ready for Who Eats Whom** | Both research grade, both in the project, each linking to the other. |

The checks follow the Who Eats Whom project's instructions (both observations in the project, each with the role, feeding type, and partner URL fields) and the website's research-grade rule as described in [Allf et al. 2026](https://doi.org/10.1371/journal.pbio.3003988). The website may apply other filters of its own, so "ready" means the pair meets these conditions, not that it's guaranteed to be on whoeatswhom.org.

## How it uses the iNaturalist API

- **Endpoints:**
  - `GET /v2/observations` with `field:ID meant for "eater" or organism being eaten?=` and your search parameters, paged with `order_by=id&id_above=...`
  - `GET /v2/observations?id=...` for partner observations outside the search (and `GET /v2/observations/{uuids}` for the rare partner linked by UUID)
  - `GET /v2/taxa?id=...&rank=kingdom,phylum,class,order,family,genus,species` for the names and ranks of the observations' higher taxa, so taxa can be grouped by family, order, and so on. Only ranked taxa that aren't already known are requested.
- **Fields:** the `fields` parameter requests only what the dashboard uses, about 2 KB per observation instead of roughly 100 KB for a full record.
- **Requests per load:** one per 200 tagged observations, one per 200 partners outside the search, and one per 200 higher taxa, sent about one second apart. For one person's 1,436 tagged observations that's 16 requests (about 20 seconds); for a project with 450, 8.
- **Searches over 10,000 observations are refused.** Narrow the search, or use iNaturalist's [observation export](https://www.inaturalist.org/observations/export).
- **Sign-in:** not needed. All data is public.

## Outside services

- **[D3](https://d3js.org) 7.9.0**, which draws the food web and the flow diagrams, loaded from [jsDelivr](https://www.jsdelivr.com). The version is pinned and checked against jsDelivr's published integrity hash. If it can't load, the diagrams are replaced by a note and the rest of the dashboard still works. Like any website, jsDelivr sees your IP address when your browser loads from it.
- **Photos** come from iNaturalist's own photo storage, at the smallest (square) size.

## Downloads

- **Download these pairs (CSV):** the pairs in the table (after the filters and status choice), one row per pair, with each side's observation, taxon, group, quality grade, date, and observer, plus the status, the issues, the feeding type, and the eater's coordinates. Obscured coordinates are iNaturalist's public, randomized point, and `eater_coordinates_obscured` says so.
- **Download the food web (CSV):** one row per food–consumer link at the current grouping, with the number of pairs, how many are ready for Who Eats Whom, the feeding types, and the observation IDs. It loads directly into network tools such as Cytoscape, Gephi, or R's igraph.

## Limitations

- **Arrows run from food to consumer,** following Who Eats Whom, so "eats" in the lists means the taxon at the arrowhead.
- **Groups are close to Who Eats Whom's:** plants and fungi together, vertebrates (subphylum Vertebrata), and all other animals as invertebrates. Who Eats Whom also counts protists as invertebrates; here they go in "other," with bacteria and observations with no ID.
- **Grouping keeps coarse IDs coarse.** An observation identified only to order stays an order when the food web is grouped by species, so a group can mix ranks. Self-links (a taxon eating its own kind, or two taxa that merge at a coarse grouping) aren't drawn in the food web but appear in the lists and the CSV.
- **"Month observed"** uses the eater's date, all years combined.
- **Links that list observation IDs** (such as "3 pairs") are left off past 400 observations to keep the URL a safe length.
- **The diagrams are for looking.** Their circles and bands can't all be reached with the keyboard; the tables and CSVs hold the same information. The search box and the lists under a selected taxon work from the keyboard.
- **The food web is rebuilt** when filters change, keeping taxa where they were; use **Fit** to see the whole web again.
- The data reflect iNaturalist as it stands when the page loads; reload to refresh. iNaturalist's search index can lag a few minutes behind edits.

## Inspiration

Built for [@willkuhn](https://www.inaturalist.org/people/willkuhn), who tags his observations for Who Eats Whom and wanted to see all of them, not just the research-grade pairs, with a food web like the one on [whoeatswhom.org](https://whoeatswhom.org/).
