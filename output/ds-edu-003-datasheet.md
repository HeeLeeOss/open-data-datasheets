# Datasheet: World Bank EdStats — Education Statistics (All Indicators)

One-line summary: A documentation datasheet for the World Bank's *Education Statistics (EdStats)* database — a cross-country collection of internationally comparable education indicators (access, completion, literacy, expenditure, learning assessments, and projections).

*This datasheet (the documentation in this file) is licensed under **CC-BY-4.0**. It documents the dataset only; it does not host, mirror, or transform the underlying data.*

---

## Provenance & attribution

- **Publisher / creator:** The World Bank Group.
- **Dataset:** Education Statistics — All Indicators (EdStats).
- **Source URL (documentation/access page):** https://databank.worldbank.org/source/education-statistics
- **Catalog record:** https://datacatalog.worldbank.org/search/dataset/0038480/education-statistics
- **Catalog id (Hee-Lee Oss candidate):** cat-edu-003
- **Retrieval date:** 2026-06-28
- **Required attribution string:**
  > Source: World Bank, *Education Statistics (EdStats) — All Indicators*. Retrieved 2026-06-28 from https://databank.worldbank.org/source/education-statistics. Licensed under CC BY 4.0 (https://creativecommons.org/licenses/by/4.0). Changes, if any, are indicated by the reuser.

---

## Source licence

- **Licence name:** Creative Commons Attribution 4.0 International (CC-BY-4.0).
- **Licence URL:** https://creativecommons.org/licenses/by/4.0
- **Verified at:** World Bank Data Catalog record for this dataset (https://datacatalog.worldbank.org/search/dataset/0038480/education-statistics), which lists the licence as "Creative Commons Attribution 4.0", and the World Bank public-licensing policy (https://datacatalog.worldbank.org/public-licenses), which states that for datasets the Bank produces, users may "copy, modify and distribute data in any format for any purpose, including commercial use," subject to attribution.
- **Permits derivatives?** **Yes.** CC-BY-4.0 §2(a)(1)(B) grants the licensee the right "to produce, reproduce, and Share Adapted Material." The only condition is appropriate attribution and indicating whether changes were made (CC-BY-4.0 §3(a)).
- **Verification status:** **VERIFIED — licence permits derivatives.**

> Licence snapshot note: A full hash + archived copy of the licence text (CC-BY-4.0 deed and World Bank ToU) should be attached as the per-dataset gate artifact at review time. The canonical licence text is at https://creativecommons.org/licenses/by/4.0/legalcode. This documentation file records the licence identity and the citing clause; it does not embed the dataset.

---

## Data dictionary

The EdStats "All Indicators" extract is distributed in the standard World Bank DataBank wide/long CSV layout. The columns below reflect that documented standard structure (country × series × time). Because the live extract was **not downloaded** (documentation-only guardrail; bounded-access not exercised), individual indicator value ranges are described generically and rows that could not be confirmed against a sample are marked **[UNVERIFIED — from DataBank standard schema]**.

| Field | Type | Units | Description | Allowed values / null handling |
|---|---|---|---|---|
| `Country Name` | string | n/a | Human-readable name of the country or aggregate region. | ~272 entities including regional/income aggregates. Non-null. |
| `Country Code` | string | n/a | ISO-3166 alpha-3 style World Bank country/region code. | e.g. `USA`, `KEN`, `WLD` (World). Non-null. |
| `Series Name` | string | n/a | Human-readable name of the education indicator (series). | ~4,000+ indicators (catalog states 8,450 series available in DataBank). Non-null. |
| `Series Code` | string | n/a | World Bank indicator code for the series. | e.g. pattern like `SE.PRM.ENRR`. Non-null. **[UNVERIFIED — exact codes not sampled]** |
| `YYYY [YRYYYY]` (one column per year) | numeric (float) | depends on indicator (%, count, ratio, currency, score) | Annual observation value for that country×series×year. | Wide format: one column per year. Missing observations encoded as `..` (DataBank convention) or empty. **[UNVERIFIED — null token not sampled]** |

Notes on units: EdStats indicators are heterogeneous — e.g. enrolment ratios (%), pupil-teacher ratios (ratio), expenditure (% of GDP or current US$), learning-assessment scores (scale points, e.g. PISA). The unit is defined per `Series Code`; consult the per-indicator metadata in DataBank. **No single global unit applies.**

The dataset documentation page does **not** expose any personal/identifying fields. All entities are countries/aggregates; observations are statistical aggregates. **No PII fields documented or expected.**

---

## Datasheet for Datasets

**Motivation.** Created by the World Bank to provide internationally comparable education statistics supporting policy analysis, monitoring of education access/quality/equity, and progress tracking (e.g. SDG 4). Funded and maintained by the World Bank Group.

**Composition.** Instances are (country/region, indicator, year) observations of education metrics: access, progression, completion, literacy, teachers, expenditure, population, learning assessments (PISA, TIMSS, PIRLS), household-survey equity measures, and projections. Catalog states 200+ countries/aggregates, 4,000+ indicators, annual cadence, with temporal coverage spanning historical years through long-range projections. Instances are aggregate statistics, not individuals — no personal data.

**Collection process.** Indicators are compiled by the World Bank from member-country reporting, UNESCO Institute for Statistics (UIS), international learning assessments, and household surveys, then harmonized into comparable series. (Specific per-indicator source and methodology are in DataBank's per-series metadata — **not individually re-verified here**.)

**Preprocessing / cleaning.** The World Bank harmonizes definitions across sources, applies standard codes, and computes derived aggregates (weighted means, medians, growth rates, sums) for regional/income groupings. Some series include modeled estimates and projections.

**Uses.** Education-policy research, cross-country comparison, SDG 4 monitoring, dashboards, and academic analysis. Reusers should note that projection/modeled series differ in reliability from observed series.

**Distribution.** Publicly distributed by the World Bank via the DataBank interactive interface, an API, and Excel/CSV downloads. Public classification. Licensed CC-BY-4.0.

**Maintenance.** Maintained by the World Bank Group; updated annually. Authoritative version is the live DataBank source; this datasheet is a point-in-time (2026-06-28) description and may drift from later updates.

---

## Croissant metadata

```json
{
  "@context": {
    "@vocab": "https://schema.org/",
    "cr": "http://mlcommons.org/croissant/",
    "dct": "http://purl.org/dc/terms/"
  },
  "@type": "Dataset",
  "name": "World Bank EdStats - Education Statistics (All Indicators)",
  "description": "Internationally comparable education indicators compiled by the World Bank covering access, progression, completion, literacy, teachers, expenditure, learning assessments, and projections across 200+ countries and aggregates.",
  "url": "https://databank.worldbank.org/source/education-statistics",
  "sameAs": "https://datacatalog.worldbank.org/search/dataset/0038480/education-statistics",
  "license": "https://creativecommons.org/licenses/by/4.0",
  "creator": {
    "@type": "Organization",
    "name": "The World Bank Group",
    "url": "https://www.worldbank.org"
  },
  "datePublished": "2026-06-28",
  "keywords": ["education", "indicators", "cross-country", "SDG4", "World Bank", "EdStats"],
  "recordSet": [
    {
      "@type": "cr:RecordSet",
      "name": "observations",
      "field": [
        { "@type": "cr:Field", "name": "Country Name", "dataType": "sc:Text" },
        { "@type": "cr:Field", "name": "Country Code", "dataType": "sc:Text" },
        { "@type": "cr:Field", "name": "Series Name", "dataType": "sc:Text" },
        { "@type": "cr:Field", "name": "Series Code", "dataType": "sc:Text" },
        { "@type": "cr:Field", "name": "value", "dataType": "sc:Float" },
        { "@type": "cr:Field", "name": "year", "dataType": "sc:Integer" }
      ]
    }
  ]
}
```

---

## Validation script

The script below validates the **structure** of a locally obtained EdStats CSV extract. It does **not** download, host, or republish the data — the user supplies their own local file path. It checks for the expected DataBank columns and reports basic shape.

```python
#!/usr/bin/env python3
"""Validate the structure of a locally-held World Bank EdStats CSV extract.

Documentation-only: this script does NOT download or host any data.
Provide your own local CSV path. It reads the header and a bounded sample.
"""
import csv
import sys

REQUIRED_DIM_COLUMNS = {"Country Name", "Country Code", "Series Name", "Series Code"}
SAMPLE_LIMIT = 1000  # bounded read, never load the whole file

def validate(path: str) -> int:
    with open(path, newline="", encoding="utf-8-sig") as f:
        reader = csv.reader(f)
        try:
            header = next(reader)
        except StopIteration:
            print("FAIL: file is empty")
            return 1

        cols = set(h.strip() for h in header)
        missing = REQUIRED_DIM_COLUMNS - cols
        if missing:
            print(f"FAIL: missing expected dimension columns: {sorted(missing)}")
            return 1

        # Year columns in DataBank wide format look like "1990 [YR1990]".
        year_cols = [h for h in header if "[YR" in h]
        print(f"OK: dimension columns present. Year columns detected: {len(year_cols)}")

        rows = 0
        for _ in reader:
            rows += 1
            if rows >= SAMPLE_LIMIT:
                print(f"OK: read bounded sample of {SAMPLE_LIMIT} rows (more may exist)")
                break
        else:
            print(f"OK: total data rows = {rows}")

    print("PASS: structure looks consistent with EdStats DataBank export")
    return 0

if __name__ == "__main__":
    if len(sys.argv) != 2:
        print("usage: validate_edstats.py <path-to-local-edstats.csv>")
        sys.exit(2)
    sys.exit(validate(sys.argv[1]))
```

Expected on a valid extract: the four dimension columns present, one numeric column per year (header pattern `YYYY [YRYYYY]`), and missing observations encoded as `..` or empty.

---

## Limitations & gaps

- **Live data not sampled.** Per the documentation-only guardrail, the actual CSV/API extract was not downloaded. Field names, the year-column header pattern, the null token (`..`), and series codes are taken from the World Bank DataBank **standard schema** and the catalog description, and are marked **[UNVERIFIED]** in the data dictionary where a sample would be needed to confirm.
- **Indicator counts vary by view.** The catalog page cites 4,000+ indicators while the DataBank source view reports ~8,450 series; exact counts depend on the selected database/version and update date.
- **Units are per-indicator.** No global unit applies; per-series metadata in DataBank is authoritative and was not enumerated indicator-by-indicator.
- **Modeled/projected series.** Some series are estimates or projections (coverage extends into future years); reliability differs from observed values. Not individually flagged here.
- **Licence snapshot artifact.** The hash + archived copy of the licence text required by the acceptance criteria should be generated/attached at the review gate; this file records licence identity and the permitting clause but does not embed the archived copy.
- **No PII.** No personal or identifying fields were found or documented; all instances are country/region-level aggregates.
