---
editor_options: 
  markdown: 
    wrap: 72
---

# Analysis

This folder has the analysis qmds for the project on exposure to open
state and federal prisons and two community outcomes, overdose deaths
and divorce. The unit is the 2020 PUMA (2,462 PUMAs in the 50 states and
DC), with 2022–2024 data pooled. The data are built in the processing
qmds.

## Folder contents

```         
analysis/
├── README.md
├── analysis_puma_national_2022_2024.qmd   # main analysis, all PUMAs
├── analysis_puma_south_2022_2024.qmd      # regional comparison + the South
├── analysis_puma_rural_2022_2024.qmd      # rural (non-metro) PUMAs
├── figures/                               # saved figures from the qmds
└── archive/                               # older notebooks, not up to date
    ├── EDA_new.qmd
    ├── analysis_county_nys_2019_2024.qmd
    └── analysis_county_national+rural_2019_2024.qmd
├── + products of rendering
```

## Figures

Figures go in `figures/`, one subfolder per notebook
(`figures/national/`, `figures/south/`, `figures/rural/`).

## Archive

Older notebooks, kept for reference. They use earlier data and are not
updated to the current pipeline.

-   `EDA_new.qmd`: early exploratory analysis
-   `analysis_county_nys_2019_2024.qmd`: county-level analysis for New
    York State, 2019–2024
-   `analysis_county_national+rural_2019_2024.qmd`: county-level
    analysis for the whole country and rural counties, 2019–2024
