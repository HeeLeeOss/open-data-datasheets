# Datasheet: World Development Indicators (WDI)

> One-line summary: The World Bank's canonical cross-country development database — ~1,400+
> time-series indicators covering economy, poverty, health, education, environment and
> infrastructure for ~217 economies, from 1960 to the present.
>
> **This datasheet is licensed under [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/).**
> It is documentation only: no dataset rows are hosted, mirrored, transformed, or republished here.

---

## Provenance & attribution

| Field | Value |
|---|---|
| Publisher | The World Bank Group (Development Data Group) |
| Dataset | World Development Indicators (WDI) |
| Source URL | https://databank.worldbank.org/source/world-development-indicators |
| Data Catalog record | https://datacatalog.worldbank.org/search/dataset/0037712/World-Development-Indicators |
| Catalog id (Hee-Lee Oss) | cat-econ-001 |
| Retrieval date | 2026-06-28 |
| Programmatic access | World Bank Indicators API v2 (`https://api.worldbank.org/v2/`) |

**Required attribution string:**

> Source: World Bank, World Development Indicators. License: Creative Commons Attribution 4.0
> International (CC BY 4.0). https://datacatalog.worldbank.org/search/dataset/0037712/World-Development-Indicators

Per CC BY 4.0 you must give appropriate credit, link to the license, and **indicate if changes
were made**. Any derivative (such as a filtered, reshaped, or recomputed subset) must carry this
notice and a statement of modifications.

---

## Source licence

| | |
|---|---|
| Licence name | Creative Commons Attribution 4.0 International (**CC BY 4.0**) |
| Permits derivatives? | **Yes — verified** |
| Verification source | World Bank Data Catalog WDI record: "This dataset is licensed under Creative Commons Attribution 4.0" — https://datacatalog.worldbank.org/search/dataset/0037712/World-Development-Indicators |
| Cross-check | World Bank data licensing policy: datasets are licensed under CC BY 4.0, which "allows users to copy, modify and distribute data" — https://datacatalog.worldbank.org/public-licenses |
| Derivative clause | CC BY 4.0 deed: "**Adapt — remix, transform, and build upon the material for any purpose, even commercially.**" — https://creativecommons.org/licenses/by/4.0/ |

**Conclusion:** CC BY 4.0 explicitly grants the right to create and distribute adaptations
(derivatives) subject to attribution. Derivative documentation work such as this datasheet is
permitted. The only obligations are attribution, a link to the license, and indication of changes.

> Note on scope: A small number of indicators inside WDI are sourced from third parties whose
> own terms may differ; the WDI compilation and the World Bank's own series are CC BY 4.0. When
> redistributing specific third-party-sourced indicators, re-verify that series' upstream terms.

---

## Data dictionary

The WDI is distributed as **long-format time series**. The columns below reflect the canonical
structure returned by the World Bank Indicators API and the bulk CSV "Data" file. Types and
descriptions are verified against the World Bank API schema; allowed-value ranges marked
*(unverified)* were not exhaustively enumerated from a live sample for this documentation-only pass.

| Field | Type | Units | Description | Allowed values / null handling |
|---|---|---|---|---|
| `Country Name` | string | — | Human-readable economy or aggregate name | ~217 economies + regional/income aggregates (e.g. "World", "Sub-Saharan Africa") |
| `Country Code` | string | — | ISO 3166-1 alpha-3 (with World Bank extensions for aggregates) | e.g. `USA`, `KEN`; aggregate codes e.g. `WLD`, `SSF` *(unverified full set)* |
| `Indicator Name` | string | — | Human-readable indicator label | e.g. "GDP (current US$)" |
| `Indicator Code` | string | — | World Bank indicator identifier | e.g. `NY.GDP.MKTP.CD`, `SP.POP.TOTL` *(unverified full set)* |
| `Year` | integer | calendar year | Observation year | 1960 .. current year; annual frequency |
| `Value` | float / null | varies by indicator (US$, %, count, per-capita, etc.) | Observed/estimated value for the (country, indicator, year) | **Null/blank where no data** is available for that cell — nulls are common and expected |

Indicator-level metadata (separate "Series" / metadata tables) additionally provides, per
`Indicator Code`: long definition, source organization, unit of measure, periodicity, aggregation
method, and topic. Country-level metadata provides region, income group, and lending category.
These metadata tables are *(structure verified from API; individual values unverified in this pass)*.

> Units are **indicator-specific** — there is no single global unit. The unit for any value must be
> read from that indicator's metadata (`unit` / definition field), not assumed.

---

## Datasheet for Datasets

**Motivation.** Created by the World Bank to provide a consistent, comparable, openly accessible
compilation of national and international development statistics, so that researchers, policymakers,
journalists, and the public can measure and compare development outcomes across countries and time.

**Composition.** Each record is an observation: a (country/economy, indicator, year, value) tuple.
Instances are aggregate statistics about economies and regions — **not** about individual people.
No personal or identifying microdata is present. Coverage spans ~217 economies plus regional and
income-group aggregates, ~1,400+ indicators, annual from 1960 to the present. Missingness is
substantial and varies by indicator, country, and era.

**Collection process.** WDI is a **compilation**, not a primary collection. The World Bank
assembles series from officially recognized international sources and national statistical
systems (e.g. national accounts, censuses, household surveys, administrative records, and partner
agencies such as the IMF, UN agencies, OECD, WHO, ILO, and UNESCO). Values may be reported,
estimated, modeled, or imputed depending on the source and indicator.

**Preprocessing / cleaning / labelling.** The Bank harmonizes definitions, standardizes units,
converts currencies where applicable, applies aggregation methods to compute regional/income-group
aggregates, and assigns standardized indicator codes and metadata. Some values are model-based
estimates. Methodological details are documented per indicator in the series metadata.

**Uses.** Cross-country and longitudinal development analysis, benchmarking, SDG-adjacent
monitoring, econometric research, teaching, and data journalism. Inappropriate uses: treating
modeled/estimated values as ground truth, comparing values across indicators without checking
units, or ignoring missingness and methodology breaks across years.

**Distribution.** Freely distributed by the World Bank via the DataBank web UI, bulk CSV/Excel
downloads, and the Indicators REST API, under CC BY 4.0. This datasheet does **not** redistribute
any of it.

**Maintenance.** Maintained and updated on a recurring schedule by the World Bank Development Data
Group (multiple releases per year, with revisions). Authoritative point of contact: data@worldbank.org.
Versioning is by release date; historical values may be revised between releases.

---

## Croissant metadata

```json
{
  "@context": {
    "@vocab": "https://schema.org/",
    "cr": "http://mlcommons.org/croissant/",
    "dataType": "cr:dataType",
    "field": "cr:field",
    "recordSet": "cr:recordSet"
  },
  "@type": "Dataset",
  "name": "World Development Indicators (WDI)",
  "description": "World Bank cross-country development database: ~1,400+ time-series indicators across economy, poverty, health, education, environment, and infrastructure for ~217 economies, 1960-present. Long-format (country, indicator, year, value).",
  "url": "https://datacatalog.worldbank.org/search/dataset/0037712/World-Development-Indicators",
  "sameAs": "https://databank.worldbank.org/source/world-development-indicators",
  "license": "https://creativecommons.org/licenses/by/4.0/",
  "creator": {
    "@type": "Organization",
    "name": "The World Bank Group",
    "url": "https://www.worldbank.org"
  },
  "datePublished": "1960",
  "version": "retrieved-2026-06-28",
  "keywords": ["development", "economics", "indicators", "cross-country", "time-series", "open-data"],
  "recordSet": [
    {
      "@type": "cr:RecordSet",
      "name": "observations",
      "description": "One row per (country, indicator, year) observation.",
      "field": [
        { "@type": "cr:Field", "name": "country_name", "dataType": "sc:Text", "description": "Economy or aggregate name." },
        { "@type": "cr:Field", "name": "country_code", "dataType": "sc:Text", "description": "ISO3 / World Bank economy code." },
        { "@type": "cr:Field", "name": "indicator_name", "dataType": "sc:Text", "description": "Human-readable indicator label." },
        { "@type": "cr:Field", "name": "indicator_code", "dataType": "sc:Text", "description": "World Bank indicator code, e.g. NY.GDP.MKTP.CD." },
        { "@type": "cr:Field", "name": "year", "dataType": "sc:Integer", "description": "Observation calendar year (annual)." },
        { "@type": "cr:Field", "name": "value", "dataType": "sc:Float", "description": "Observed/estimated value; null where unavailable. Units are indicator-specific." }
      ]
    }
  ]
}
```

---

## Validation script

This script checks the **structure** of a WDI extract you have obtained yourself from the World
Bank. It does **not** download, host, or republish any data. Point it at your own local CSV.

```python
#!/usr/bin/env python3
"""Structural validator for a locally-obtained World Development Indicators (WDI) extract.

Documentation-only: this script does NOT download or host data. Supply your own local CSV
exported from the World Bank (DataBank / API). It checks expected columns, types, and basic
sanity (year range, non-empty), reading a bounded sample only.
"""
import csv
import sys

EXPECTED_COLUMNS = {
    "Country Name", "Country Code", "Indicator Name", "Indicator Code", "Year", "Value"
}
SAMPLE_LIMIT = 1000  # bounded read; do not load the whole file

def validate(path: str) -> int:
    with open(path, newline="", encoding="utf-8-sig") as f:
        reader = csv.DictReader(f)
        cols = set(reader.fieldnames or [])
        missing = EXPECTED_COLUMNS - cols
        if missing:
            print(f"FAIL: missing expected columns: {sorted(missing)}")
            print(f"      found columns: {sorted(cols)}")
            return 1
        rows = 0
        bad_year = 0
        for row in reader:
            rows += 1
            y = (row.get("Year") or "").strip()
            if y:
                try:
                    yi = int(float(y))
                    if not (1960 <= yi <= 2100):
                        bad_year += 1
                except ValueError:
                    bad_year += 1
            # 'Value' may be empty (null) — that is valid and expected.
            if rows >= SAMPLE_LIMIT:
                break
    if rows == 0:
        print("FAIL: no data rows found.")
        return 1
    print(f"OK: columns present; checked {rows} sample rows; {bad_year} out-of-range/invalid years.")
    return 0 if bad_year == 0 else 1

if __name__ == "__main__":
    if len(sys.argv) != 2:
        print("usage: validate_wdi.py <path-to-your-local-wdi.csv>")
        sys.exit(2)
    sys.exit(validate(sys.argv[1]))
```

> Note: bulk WDI CSVs are sometimes distributed in **wide** format (one column per year:
> `1960`, `1961`, ...) rather than the long format above. If validating a wide export, adapt
> `EXPECTED_COLUMNS` to `{"Country Name", "Country Code", "Indicator Name", "Indicator Code"}`
> plus year columns. The API JSON uses the long structure documented in the Croissant block.

---

## Limitations & gaps

- **Documentation-only pass.** No live data sample was downloaded, hosted, or transformed for
  this datasheet; field structure is verified against the World Bank API/CSV schema rather than
  by enumerating a sampled file. Rows marked *(unverified)* were not exhaustively checked.
- **Allowed-value sets** (full lists of country codes, indicator codes, aggregate codes) are
  large and dynamic and were not exhaustively enumerated; representative examples are given.
- **Units are per-indicator** and must be read from each indicator's metadata; this datasheet
  does not enumerate units for all ~1,400+ indicators.
- **Missingness & methodology breaks** are significant in WDI; many cells are null, and some
  values are modeled/estimated or revised between releases. Consult per-indicator metadata.
- **Third-party-sourced indicators** within WDI may carry upstream terms; the WDI compilation is
  CC BY 4.0, but re-verify a specific series before redistributing it standalone.
- **No PII.** WDI contains aggregate national/regional statistics only; no personal or
  identifying data was documented or sampled.
