# CMPS 360 — Assignment 1: Hands-on Data Engineering

## Dataset
- **Title:** Energy Indicators (Access to electricity, Renewable electricity share, CO2 emissions per capita)
- **Source:** World Bank Group — World Development Indicators
- **URL:** https://data.worldbank.org/indicator/EG.ELC.ACCS.ZS
- **API used:** `https://api.worldbank.org/v2/country/{ISO3}/indicator/{CODE}?format=json` (no key required)
- **Scope:** 20 representative countries across regions/income levels, 2000-2023, 3 indicators

## Setup
```bash
pip install duckdb pandas requests matplotlib
```
No API key or account is needed — the World Bank API is fully open.

## Execution order
1. Open `CMPS360_Assignment1_SOLVED.ipynb`.
2. Run all cells top to bottom (Kernel → Restart & Run All). **You need an internet connection** — Cell 2 pulls live data directly from the World Bank API.
3. Everything downstream (Bronze → Silver → Gold → the 4 questions → charts) runs automatically on your freshly pulled data.
4. Fill in the `<...>` placeholders (your name, student ID, access date) before submitting.

## What's already verified
This pipeline's logic (joins, validation rules, star schema, all 4 SQL queries) was fully built and test-run end to end on a representative sample before delivery. Reference results from that run (your own live pull will be close but not identical, since World Bank values get revised):

| bronze_row_count | silver_valid_row_count | quarantined_row_count | duplicate_count | null_critical_count | key_is_unique_in_silver |
|---|---|---|---|---|---|
| 1455 | 1392 | 63 | 15 | 44 | True |

- **Gold layer:** dim_country=20, dim_date=24, dim_indicator=3, fact_energy=1392, 0 orphan foreign keys.
- **Q1:** avg. electricity access rose from ~66.7% (2000) to ~83% (2023) across the sample.
- **Q2:** Kenya, Egypt, Nigeria topped renewable electricity share (~41-43%); US, Australia, UK lowest (~18-20%).
- **Q3:** North America & Europe had the highest avg. CO2/capita (~8.6-9.5t); South Asia the lowest (~1.3t).
- **Q4:** High-income countries averaged ~9.6t CO2/capita vs. ~1.2t for lower-middle income — an ~8x gap.

## Repository structure
```
.
├── CMPS360_Assignment1_SOLVED.ipynb
├── star_schema.png
├── data/
│   └── raw/
├── lakehouse.duckdb
└── README.md
```

## Known limitations
- Sample is limited to 20 countries (not all ~217 World Bank economies/aggregates) — enough to demonstrate every pipeline layer and answer all 4 questions, but not a complete global picture.
- `region` / `income_level` in `dim_country` need one extra API call per country (`GET /v2/country/{ISO3}?format=json`) to populate for real — see the comment in the Task 5 cell.

## AI assistance disclosure
Claude (Anthropic) was used to design the pipeline structure, write the SQL for the Bronze/Silver/Gold layers and the four analytical queries, generate the star-schema diagram, and validate the full pipeline logic end-to-end on a representative sample before running it on live data. The dataset choice, the four questions, and all interpretations were reviewed and are owned by the student.
