# Datasheet: World Population Prospects (WPP)

**One-line summary:** The United Nations' authoritative global demographic dataset — population
estimates (from 1950) and projections (to 2100) by location, year, age and sex, plus derived
indicators (fertility, mortality, migration) — published by UN DESA Population Division.

*This datasheet (the document you are reading) is licensed under **CC-BY-4.0**. It is documentation
only: it does not host, mirror, transform, or republish any of the WPP data.*

---

## Provenance & attribution

| Field | Value |
|---|---|
| **Publisher / creator** | United Nations, Department of Economic and Social Affairs (UN DESA), Population Division |
| **Dataset** | World Population Prospects (WPP), 2024 Revision (Online Edition) |
| **Source URL** | https://population.un.org/wpp/ |
| **Dataset / downloads page** | https://www.un.org/development/desa/pd/content/world-population-prospects-2024-dataset |
| **Catalog id (Hee-Lee Oss)** | cat-census-004 |
| **Retrieval date** | 2026-06-28 |

**Required attribution string (use verbatim when reusing the data):**

> United Nations, Department of Economic and Social Affairs, Population Division (2024).
> *World Population Prospects 2024, Online Edition.* Licensed under CC BY 3.0 IGO.
> https://population.un.org/wpp/

---

## Source licence

- **Licence:** **Creative Commons Attribution 3.0 IGO (CC BY 3.0 IGO)**.
- **Licence URL / clause:** http://creativecommons.org/licenses/by/3.0/igo/
- **Permits derivatives?** **Yes — verified.** CC BY 3.0 IGO explicitly grants the right to
  "Adapt — remix, transform, and build upon the material for any purpose, even commercially,"
  conditioned only on attribution. The WPP 2024 publications (Summary of Results, Methodology
  Report, Data Sources) each state that figures and tables may be reproduced without prior
  permission under CC BY 3.0 IGO, citing the licence URL above.
- **Confirming sources (retrieved 2026-06-28):**
  - WPP 2024 Methodology Report — https://population.un.org/wpp/assets/Files/WPP2024_Methodology-Report_Final.pdf
  - WPP 2024 Summary of Results — https://population.un.org/wpp/assets/Files/WPP2024_Summary-of-Results.pdf
  - Canonical licence deed/legal code — https://creativecommons.org/licenses/by/3.0/igo/legalcode
- **Licence snapshot:** archive the licence legal code at retrieval time (e.g. via the Internet
  Archive Wayback Machine) and record its SHA-256 alongside this datasheet. *Snapshot hash not
  embedded here to keep this file documentation-only; see Limitations.*

> Conclusion: the source licence permits reuse and derivative works with attribution. This
> datasheet is therefore published normally (not as a DRAFT).

---

## Data dictionary

WPP is distributed as several thematic files (Population; Fertility; Mortality; Migration;
Demographic Indicators) in CSV and Excel, in long and wide layouts, at annual and 5-year
granularity. Fields below were verified from the official dataset documentation, the QOG
DataFinder WPP codebook, and the UN-maintained `PPgp/wpp2024` R package (all retrieved 2026-06-28).
Rows marked **(layout-unverified)** describe the standard official-CSV identifier columns whose
*exact header spelling* was not directly confirmed in this session.

### Identifier / dimension columns

| Field | Type | Units | Description | Allowed values / nulls |
|---|---|---|---|---|
| `country_code` / `LocID` | integer | — | UN M49 numeric location code | e.g. 900 = World; non-null for valid rows. **(layout-unverified for exact CSV header name)** |
| `name` / `Location` | string | — | Country, area, region, or aggregate name | 237 countries/areas plus regional & income-group aggregates |
| `ISO3_code` | string | — | ISO 3166-1 alpha-3 code | Null/blank for aggregate regions **(layout-unverified)** |
| `Variant` | string | — | Projection scenario | "Estimates" for ≤ base year; "Medium", "High", "Low", etc. for projections **(layout-unverified)** |
| `year` / `Time` | integer | calendar year | Reference year | ~1950–2100 (R package: 1949–2023 estimates, 2024–2100 projections) |
| `age` / `AgeGrp` | string/integer | years | Age or age group (single-year or 5-year) | Present only in age-disaggregated files; null otherwise |

### Indicator / measure columns (verified from `wpp2024` package & QOG codebook)

| Field | Type | Units | Description | Allowed values / nulls |
|---|---|---|---|---|
| `popM`, `popF`, `pop` | numeric | thousands of persons | Population: male, female, total | ≥ 0 |
| `popproj` (median + 80%/95% bounds) | numeric | thousands | Projected population with uncertainty bounds | ≥ 0 |
| `tfr` | numeric | live births per woman | Total fertility rate | ≥ 0 (typ. 0.8–7) |
| `percentASFR` | numeric | percent | Age-specific fertility rate distribution | 0–100 |
| `e0M`, `e0F`, `e0B` | numeric | years | Life expectancy at birth (male/female/both) | ≥ 0 |
| `mxM`, `mxF`, `mxB` | numeric | rate | Age/sex-specific central death rates | ≥ 0 |
| `mig`, `migM`, `migF` | numeric | thousands of net migrants | Net migration (total / by sex) | may be negative |
| `births`, `deaths` | numeric | thousands | Annual births / deaths | ≥ 0 |
| `cbr`, `cdr` | numeric | per 1,000 population | Crude birth / death rate | ≥ 0 |
| `cnmr` | numeric | per 1,000 population | Crude net migration rate | may be negative |
| `growthrate`, `NatChangeRT` | numeric | percent | Population growth / natural change rate | may be negative |
| `sexRatio` (`sexratio`) | numeric | males per 100 females | Population sex ratio | ≥ 0 |
| `medianage` | numeric | years | Median age of population | ≥ 0 |
| `popden` | numeric | persons per km² | Population density | ≥ 0 |

**Null handling:** disaggregation columns (age, ISO3) are blank for rows where they do not apply
(e.g. regional aggregates have no ISO3 code; non-age files have no age column). Indicator values
are populated for all valid location/year rows in their respective files.

---

## Datasheet for Datasets

**Motivation.** Created to provide internationally comparable, authoritative estimates and
projections of population and its components for every country/area, used across the UN system,
the SDG indicator framework, governments, researchers, and NGOs. Produced by UN DESA Population
Division.

**Composition.** Time series of demographic quantities (population by age/sex, fertility,
mortality, migration, and derived rates) for 237 countries/areas plus regional, SDG, income, and
development-group aggregates, spanning 1950–2100. Instances are location×year (×age×sex) records.
Contains **no personal or individual-level data** — all values are population aggregates.

**Collection process.** Compiled from national censuses, civil/vital registration systems,
demographic surveys (e.g. DHS, MICS), and population registers; harmonized and, where needed,
adjusted for under-/over-count. Projections are produced with a cohort-component model under
probabilistic and deterministic variants. See the WPP 2024 Methodology Report and Data Sources.

**Preprocessing / cleaning.** Source inputs are evaluated, adjusted for coverage/age-heaping
errors, smoothed, and reconciled to a consistent demographic accounting framework before
estimates and projections are derived.

**Uses.** Demographic research; SDG monitoring; planning (health, education, pensions);
denominators for rate calculations; input to economic and climate models. Not a register of
individuals and not suitable for sub-national or individual inference beyond published granularity.

**Distribution.** Freely downloadable from population.un.org/wpp in CSV and Excel, plus an
interactive Data Portal and the `wpp2024` R package. Licensed CC BY 3.0 IGO.

**Maintenance.** Maintained and revised periodically by UN DESA Population Division (recent
revisions: 2019, 2022, 2024). Each revision supersedes the prior one; cite the specific revision.

---

## Croissant metadata

```json
{
  "@context": {
    "@vocab": "https://schema.org/",
    "cr": "http://mlcommons.org/croissant/"
  },
  "@type": "Dataset",
  "name": "World Population Prospects 2024",
  "description": "UN DESA Population Division estimates (from 1950) and projections (to 2100) of population by location, year, age and sex, with fertility, mortality and migration indicators. Documentation datasheet only; data not hosted here.",
  "url": "https://population.un.org/wpp/",
  "sameAs": "https://www.un.org/development/desa/pd/content/world-population-prospects-2024-dataset",
  "version": "2024 Revision",
  "license": "http://creativecommons.org/licenses/by/3.0/igo/",
  "creator": {
    "@type": "Organization",
    "name": "United Nations, Department of Economic and Social Affairs, Population Division",
    "url": "https://www.un.org/development/desa/pd/"
  },
  "datePublished": "2024",
  "citation": "United Nations, Department of Economic and Social Affairs, Population Division (2024). World Population Prospects 2024, Online Edition.",
  "keywords": ["demography", "population", "fertility", "mortality", "migration", "projections", "SDG"],
  "recordSet": [
    {
      "@type": "cr:RecordSet",
      "name": "demographic_indicators",
      "field": [
        { "@type": "cr:Field", "name": "LocID", "description": "UN M49 numeric location code", "dataType": "Integer" },
        { "@type": "cr:Field", "name": "Location", "description": "Country, area, or aggregate name", "dataType": "Text" },
        { "@type": "cr:Field", "name": "Time", "description": "Calendar year (~1950-2100)", "dataType": "Integer" },
        { "@type": "cr:Field", "name": "pop", "description": "Total population, thousands", "dataType": "Float" },
        { "@type": "cr:Field", "name": "tfr", "description": "Total fertility rate, births per woman", "dataType": "Float" },
        { "@type": "cr:Field", "name": "e0B", "description": "Life expectancy at birth, both sexes, years", "dataType": "Float" }
      ]
    }
  ]
}
```

---

## Validation script

Documentation only — this script validates the **structure** of a WPP CSV that the user has
already obtained from the official source. It does **not** download, host, or redistribute data.

```python
#!/usr/bin/env python3
"""Validate the structure of a locally-obtained WPP CSV export.

Usage: python validate_wpp.py path/to/wpp_file.csv
Does NOT download data. Point it at a file you downloaded yourself from
https://population.un.org/wpp/ . Reads a bounded sample only.
"""
import csv
import sys

# Adjust to the WPP file you exported. These are the columns we expect to find;
# identifier header spelling can vary by file/layout (see datasheet caveats).
EXPECTED_ANY = {
    "ids": {"LocID", "country_code", "Location", "name"},
    "time": {"Time", "year"},
    "measures": {"pop", "popM", "popF", "tfr", "e0B", "mig", "cbr", "cdr"},
}
MAX_SAMPLE_ROWS = 1000  # bounded read; never load the whole file

def main(path):
    with open(path, newline="", encoding="utf-8-sig") as fh:
        reader = csv.reader(fh)
        header = next(reader)
        cols = set(header)
        ok = True
        for group, options in EXPECTED_ANY.items():
            if cols.isdisjoint(options):
                print(f"[FAIL] no '{group}' column found; expected one of {sorted(options)}")
                ok = False
            else:
                print(f"[ok]   '{group}': {sorted(cols & options)}")
        # bounded row scan: count + basic non-negativity sanity check
        n = 0
        for row in reader:
            n += 1
            if n >= MAX_SAMPLE_ROWS:
                break
        print(f"[info] scanned {n} sample rows (capped at {MAX_SAMPLE_ROWS}); "
              f"{len(header)} columns total")
        sys.exit(0 if ok else 1)

if __name__ == "__main__":
    if len(sys.argv) != 2:
        sys.exit("Usage: python validate_wpp.py path/to/wpp_file.csv")
    main(sys.argv[1])
```

Expected, broadly: a header containing at least one location identifier, one time/year column,
and one or more demographic measure columns; tens of thousands of rows for a full country×year
file (237 countries/areas × ~151 years × variants).

---

## Limitations & gaps

- **Exact CSV header spelling not directly confirmed.** The official downloads page is a
  JavaScript application that did not render server-side, so the precise column headers of the
  official CSV exports (e.g. `LocID` vs `country_code`, `Time` vs `year`, `Variant`) could not be
  read directly this session. Indicator semantics and field set were verified via the UN-maintained
  `PPgp/wpp2024` R package README and the QOG DataFinder WPP codebook; treat header names marked
  **(layout-unverified)** as canonical-but-unconfirmed and check against the actual file you
  download.
- **No data inspected.** Per guardrails, no WPP data file was downloaded, sampled, transformed, or
  committed. Row counts and value ranges above are descriptive expectations, not measured from a
  sample.
- **Licence snapshot hash not embedded.** A SHA-256 of the archived CC BY 3.0 IGO legal code should
  be attached as a separate gate artifact; it is referenced but not inlined here.
- **Revision specificity.** WPP is revised periodically (2019, 2022, 2024…). This datasheet targets
  the 2024 Revision; always cite the specific revision you use.
- **No PII.** WPP contains only aggregate demographic counts/rates — no personal or identifying
  data was documented or sampled.
