---
editor_options: 
  markdown: 
    wrap: 72
---

# Data

All data for the project on prison exposure and overdose deaths and
divorce.

```         
data/
├── raw/     # downloaded from the source, not changed
├── final/   # made by the processing notebooks, read by the analysis notebooks
└── 000/     # not used by the current pipeline (old or undecided)
```

The processing notebooks (`scripts/processing/`) read `raw/` and write
to `final/`. The analysis notebooks (`scripts/analysis/`) read only
`final/`. Nothing in `raw/` is edited by hand.

## raw/

### Used by the current pipeline

| File | What it is | Source | Read by |
|------------------|------------------|------------------|------------------|
| `data_ipums_2022_2024.csv` | ACS 1-year microdata, 2022–2024 | IPUMS USA (downloaded with `ipumsr`) | `processing_national_pumas_2022_2024` |
| `ddi_2022_2024.rds` | Codebook for that extract | IPUMS USA | `processing_national_pumas_2022_2024` |
| `injury_overdose_violence_census_tract.csv` | Overdose deaths by tract and year | CDC | `processing_national_pumas_2022_2024` |
| `tract_pop_2020.csv` | 2020 tract populations, total and institutional group quarters | 2020 Census, via `tidycensus` | both processing notebooks |
| `tract_popcenters_2020.gpkg` | 2020 tract centers of population | Census Bureau (`CenPop2020_Mean_TR.txt`) | `processing_prison_exposure` |
| `rurality/` | RUCA codes for 2020 tracts | USDA ERS | `processing_national_pumas_2022_2024` |
| `religion.xlsx` | Religious adherents by county, 2020 | 2020 US Religion Census | `processing_national_pumas_2022_2024` |
| `prison-boundaries-1-geojson.geojson` | Prison locations, type and status | HIFLD Prison Boundaries | `processing_prison_exposure`, maps in the analysis notebooks |

Downloaded in the code and not saved: the tract → PUMA relationship file
(Census Bureau) and PUMA boundaries (`tigris`).

### Older

Used by the archive notebooks, or no longer used.

| File | What it is |
|--------------------------|----------------------------------------------|
| `data_ipums_2024.csv` | ACS 2020–2024 5-year microdata (first, 2024-only PUMA version) |
| `injury_overdose_violence.csv` | CDC overdose deaths, county level |
| `Prison_Boundaries.csv` | Older HIFLD prison file, table version |
| `unemployment19.xlsx` | County unemployment, 2019 |
| `life_expectancy_2010-2015.csv` | Life expectancy by tract, 2010–2015 |
| `correctional_employmet_2019-24/` | Correctional employment, 2019–2024 |
| `naloxone_dispensing_rates_2022_2024/` | CDC naloxone dispensing rates by state (dropped from the models) |
| `nys_data/` | New York State data for the NYS county analysis |

## final/

### Datasets

| File | What it is | Made in |
|--------------------|-------------------------------|--------------------|
| `national_puma_dataset_2022_2024.gpkg` | Outcomes and controls for all PUMAs, with boundaries. **Main dataset**, read by the analysis notebooks | `processing_national_pumas_2022_2024` |
| `national_puma_dataset_2022_2024.csv` | Same, without boundaries | `processing_national_pumas_2022_2024` |
| `rural_puma_dataset_2022_2024.csv` | Rural PUMAs only | `processing_national_pumas_2022_2024` |
| `tract_overdose_by_year_2022_2024.csv`, `tract_overdose_2022_2024.csv` | Tract-level overdose deaths, by year and pooled | `processing_national_pumas_2022_2024` |
| `puma_prison_exposure.csv` | Prison exposure for every PUMA, read by the analysis notebooks | `processing_prison_exposure` |
| `tract_prison_exposure.csv` | Prison exposure for every tract | `processing_prison_exposure` |

When reading the CSVs, read the IDs as text so the leading zeros
survive, e.g.
`read_csv(file, col_types = cols(GEOID20 = col_character()))`.

### Saved models

The spatial models are slow, so they are fitted once (chunks marked
`eval: false`) and saved here.

| File | What it is | Made in |
|--------------------|-------------------------------|--------------------|
| `p6_error_models.rds` | SEM and SDEM, both outcomes, all exposures, queen and kNN weights | `analysis_puma_national_2022_2024` |
| `sem_staged_h10.rds` | Staged SEM for h = 10 km | `analysis_puma_national_2022_2024` |
| `region_sem_models.rds` | SEM M3 by Census region | `analysis_puma_south_2022_2024` |
| `south_error_models.rds` | SEM and SDEM for the South | `analysis_puma_south_2022_2024` |

### Older outputs

From earlier versions of the pipeline (2024-only PUMAs, tracts,
counties, NYS). Not read by the current notebooks.

-   PUMA, older versions: `national_puma_dataset.csv`,
    `rural_puma_dataset.csv`, `national_puma_analysis_2022_2024.csv`,
    `national_puma_analysis_2022_2024.gpkg`,
    `rural_puma_analysis_2022_2024.csv`, `puma_pct_area_nonmetro.csv`
-   Old models: `p6_lag_models.rds`, `p7_rural_error_models.rds`,
    `sem_quintiles.rds`, `cache_05/`
-   Tract, facility and county level: `tract_level_dataset_2022.csv`,
    `facility_level_dataset.csv`, `master_national.csv`,
    `master_clean.csv`, `master_clean.geojson`
-   New York State: `master_nys.csv`, `nys_dv_2019_clean.xlsx`

## 000/

Files that aren't used by the current pipeline but that I'm keeping for
now.

| File | What it is |
|--------------------------|----------------------------------------------|
| `tract_points_2020.gpkg` | Old tract points (`st_point_on_surface`), replaced by the population centers |
| `ddi.rds` | Codebook for the old 2024 5-year IPUMS extract |
| `pumas_2020.rds` | Saved 2020 PUMA boundaries |
| `38325-0001-Data.rda` | ICPSR study 38325 (correctional facilities census) |
| `ccf_geocoded_v1.rds` | Geocoded correctional facilities, from the ICPSR file |
| `closed_rural_prisons_to_research.xlsx` | List of closed rural prisons to look into |
| `deaths_facility_names.csv` | Facility names list |
| `CL-Mar-Div-2011-2019.xlsx` | State marriage and divorce rates, 2011–2019 |

## Not on GitHub

These are in `.gitignore`:

-   `raw/data_ipums_*.csv` and `raw/ddi*.rds`: IPUMS terms don't allow
    sharing the microdata, and the files are 1–2 GB. To rebuild them,
    get a free IPUMS API key and run the download chunk in
    `processing_national_pumas_2022_2024.qmd`.
-   `final/tract_level_dataset_2022.csv`: 223 MB, over GitHub's 100 MB
    limit.
