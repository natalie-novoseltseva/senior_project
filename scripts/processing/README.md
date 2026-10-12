---
editor_options: 
  markdown: 
    wrap: 72
---

# Processing

This folder has the qmds that build the datasets for the project on
prison exposure and overdose deaths and divorce. The unit is the 2020
PUMA (2,462 PUMAs in the 50 states and DC), with 2022–2024 data pooled.

The processing qmds download and clean the raw data and save the final
files to `data/final/`.

## Folder contents

```         
processing/
├── README.md
├── processing_national_pumas_2022_2024.qmd   # outcomes + controls for every PUMA
├── processing_prison_exposure.qmd            # prison exposure measures
└── archive/                                  # older notebooks, not up to date
    ├── processing_national_pumas_2024.qmd
    ├── processing_national_ctract_2022.qmd
    ├── processing_national_ctract.qmd
    ├── processing_national.qmd
    ├── processing_nys_clean.qmd
    └── processing_nys.qmd
```

## Current qmds

### `processing_national_pumas_2022_2024.qmd`

Builds the main PUMA dataset: one row per PUMA with the outcomes and
controls.

-   **IPUMS ACS microdata**, 2022, 2023 and 2024 1-year samples pooled.
    People living in institutions (prisons, nursing homes, etc.) are
    dropped before anything is calculated, so prisoners don't change the
    PUMA's demographics. Percentages are ratios of weighted counts;
    income is in 2024 dollars.
-   **Divorce**: % of ever-married people who divorced in the past year,
    pooled 2022–2024.
-   **Overdose deaths**: CDC tract-level data. Where the count is
    suppressed, it is estimated from the rate and kept inside the
    reported range. Deaths are split among tracts by 2020 population and
    summed to the PUMA. The pooled rate is total deaths / total
    population over 2022–2024, per 100,000. Annual rates are kept to
    check that no single year drives the result.
-   **Rurality**: 2020 tract RUCA codes. `pct_pop_nonmetro` is the % of
    the PUMA's population in non-metro tracts (RUCA 4–10). The RUCA file
    also translates Connecticut's new (planning-region) tract IDs back
    to the 2020 IDs.
-   **Religion**: 2020 US Religion Census, % adherents by county. It's
    assigned to tracts by county and averaged to the PUMA, weighted by
    population (`pct_religious`). Used as a control in the divorce
    models only.
-   **PUMA boundaries**: 2020 cartographic boundaries from `tigris`.

### `processing_prison_exposure.qmd`

Builds the prison exposure measures, first for each tract and then
averaged to the PUMA.

-   **Prisons**: open state and federal facilities (HIFLD Prison
    Boundaries), as points.
-   **Tract locations**: 2020 Census tract centers of population, i.e.
    where people in the tract actually live, not the middle of the
    tract's area.
-   **Exposure**: for each tract, E = Σ exp(−d/h) over all prisons,
    where d is the distance in km to the prison. h = 1, 5, 10 and 25 km;
    h = 10 km is the main measure. Small h measures whether a prison is
    close to where people live; large h counts prisons in the wider
    area.
-   **PUMA value**: average of the PUMA's tracts, weighted by
    non-institutional population (2020 total minus people in
    institutional group quarters), so incarcerated people don't give
    prison tracts extra weight.
-   **Also built**: number of prisons inside the PUMA and any prison
    inside the PUMA (0/1).

## Archive

Earlier versions of the pipeline, kept for reference. They are not
updated to the current data and are not needed to run anything.

-   `processing_national_pumas_2024.qmd`: the first PUMA version, 2024
    overdose deaths and the 2020–2024 ACS 5-year sample
-   `processing_national_ctract_2022.qmd`,
    `processing_national_ctract.qmd`: census tract-level datasets
-   `processing_national.qmd`: national county-level dataset
-   `processing_nys.qmd`, `processing_nys_clean.qmd`: New York State
    county-level dataset (`master_nys.csv`)
