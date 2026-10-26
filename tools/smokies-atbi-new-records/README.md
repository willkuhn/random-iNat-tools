# Smokies ATBI new records

A dashboard that tracks iNaturalist observations that represent new records for Great Smoky Mountains National Park as part of the [Smokies ATBI](https://dlia.org/about/atbi/) (All Taxa Biodiversity Inventory).

**[Open the dashboard »](https://willkuhn.github.io/random-iNat-tools/tools/smokies-atbi-new-records/)**

## What it does

A small team uses the observation field [New Smokies ATBI record?](https://www.inaturalist.org/observation_fields/14382) (field ID 14382) to flag observations from inside the park that may be new park records, and to track each one through confirmation. This dashboard loads every observation in the park ([iNaturalist place 72645](https://www.inaturalist.org/places/72645)) with that field and summarizes where things stand:

- **Summary tiles:** confirmed new records, records awaiting final confirmation, records awaiting ID confirmation, and the number of observers with confirmed records.
- **Records by rank:** counts for every field value, split into confirmed and awaiting final confirmation, by the rank of the new record (kingdom through subspecies). Every count links to the matching iNaturalist search.
- **Confirmed records by group and by year observed:** which kinds of organisms the new records are (insects, fungi, arachnids, ...) and when they were observed.
- **Top observers:** a leaderboard of iNaturalist users whose observations became confirmed new records, with how many of theirs are still in progress.
- **In progress:** the observations awaiting final confirmation and awaiting ID confirmation, with photos. The final-confirmation list can be downloaded as a CSV to send for review.
- **Download all records (CSV):** every flagged observation with its taxon, observer, date, quality grade, and stage.

## How field values map to stages

The field's values follow the team's workflow for adding new records from iNaturalist:

| Field value | Stage |
|:------------|:------|
| `awaiting ID confirmation` | **Awaiting ID confirmation.** Looks like a new record, but the identification still needs expert help. |
| `possibly` | Counted with awaiting ID confirmation (older value). |
| `species (confirming)`, `genus (confirming)`, ... (any rank + ` (confirming)`) | **Awaiting final confirmation.** The ID is settled and the taxon appears to be new for the park. These are awaiting a final check before being declared "new." The rank is the highest rank the observation would add, e.g. `species (confirming)` when the park has the genus but not the species. |
| `confirming` | Awaiting final confirmation, rank not given (older value). |
| `species`, `genus`, ... (any rank on its own) | **Confirmed new record.** Park staff have confirmed it and added the taxon to the park checklist. |

If a record is later retracted, for example because the observation is re-identified as something already known from the park, the field value is removed and the observation drops off the dashboard.

## How it uses the iNaturalist API

- **Endpoint:** `GET /v2/observations?field:New Smokies ATBI record?=&place_id=72645` (an empty field value matches any value), paged with `order_by=id&id_above=...`
- **Fields:** the `fields` parameter requests only the dozen attributes the dashboard uses, about 1 KB per observation instead of roughly 100 KB for a full record. That keeps the page fast and light on iNaturalist's servers.
- **Requests per load:** one per 200 flagged observations (3 as of October 2026), sent about one second apart.
- **Sign-in:** not needed. All data is public.

Links to iNaturalist searches add `place_id=72645` and `verifiable=any` so their counts match the dashboard.

## Limitations

- **Links for combined totals use observation ID lists.** iNaturalist's search can't match several field values at once, so links for totals, groups, years, and observers list the observation IDs instead. Past 400 observations, the link is left off to keep the URL a safe length. (The confirmed-records tile links to the by-rank table for this reason.)
- **"Year observed," not "year confirmed."** iNaturalist doesn't record when a field value was set, so records are dated by when the organism was observed.
- **Group** is iNaturalist's iconic taxon. "Other animals" are animals outside the named groups, such as woodlice, centipedes, and land planarians.
- The data reflect the field values as they stand on iNaturalist when the page loads; reload to refresh.

## Inspiration

Built for the new-records workflow described above, which [@willkuhn](https://www.inaturalist.org/people/willkuhn) and colleagues run with Great Smoky Mountains National Park to bring iNaturalist records into the park's ATBI checklists.
