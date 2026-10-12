---
editor_options: 
  markdown: 
    wrap: 72
---

# Prisons Project

Natalie Novoseltseva

Does living near prisons go together with more overdose deaths and more
divorce? This project looks at exposure to open state and federal
prisons across 2020 PUMAs (2,462 PUMAs in the 50 states and DC), with
2022–2024 data pooled, for the whole country, the South and rural PUMAs.

## Structure

```         
sproj/
├── README.md
├── sproj.Rproj
├── data/
│   ├── raw/          # downloaded data, not changed
│   ├── final/        # cleaned datasets and saved models
│   └── 000/          # not used now (old or undecided)
├── scripts/
│   ├── processing/   # build the datasets: raw/ → final/
│   └── analysis/     # models, tables and figures: read final/
├── docs/             # writing and notes
└── archive/          # old material from earlier versions of the project
```

`data/`, `scripts/processing/` and `scripts/analysis/` each have their
own README with the details.

## 
