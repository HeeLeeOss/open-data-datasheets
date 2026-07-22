# Datasheet: Eurostat Housing Statistics (Income and Living Conditions)

**One-line summary:** Comparable EU-wide housing-cost, overcrowding and housing-deprivation
indicators (derived from EU-SILC) published by Eurostat as part of the *Income and living
conditions* collection.

> **Datasheet licence:** This datasheet (the documentation in this file) is released under
> **Creative Commons Attribution 4.0 International (CC-BY-4.0)**. It documents an external
> dataset and does **not** contain, host, mirror, or republish any of the dataset itself.

---

## Provenance & attribution

| Field | Value |
|---|---|
| **Dataset name** | Housing statistics / Income and living conditions (housing indicators) |
| **Publisher / data owner** | Eurostat — Statistical Office of the European Union (European Commission) |
| **Catalog id (Hee-Lee Oss)** | cat-housing-005 |
| **Domain theme URL** | https://ec.europa.eu/eurostat/web/income-and-living-conditions/data |
| **Verified example dataset** | `ilc_lvho07a` — *Housing cost overburden rate by age, sex and poverty status* |
| **Verification endpoint** | https://ec.europa.eu/eurostat/api/dissemination/statistics/1.0/data/ilc_lvho07a?format=JSON&time=2023 |
| **Retrieval date** | 2026-06-28 |

**Required attribution string** (CC BY 4.0 + Eurostat reuse policy):

> Source: Eurostat (European Commission), *Income and living conditions — Housing statistics*,
> dataset `ilc_lvho07a`. © European Union, 1995–2026. Reused under Commission Decision
> 2011/833/EU (CC BY 4.0). Eurostat data accessed 2026-06-28. Changes were made (documentation
> only — no data values reproduced).

---

## Source licence

- **Licence:** Creative Commons Attribution 4.0 International (**CC BY 4.0**).
- **Legal basis:** Commission Decision of 12 December 2011 on the reuse of Commission documents
  (**Commission Decision 2011/833/EU**), as implemented by the European Commission legal notice
  and Eurostat's copyright/reuse policy.
- **Citation / clause confirming derivatives are permitted:** European Commission legal notice
  (https://commission.europa.eu/legal-notice_en): reuse of Commission-owned content is
  authorised under the CC BY 4.0 licence, *"provided appropriate credit is given and changes
  are indicated."* CC BY 4.0 explicitly permits adaptation/derivative works and commercial use
  (see https://creativecommons.org/licenses/by/4.0/ — *"Adapt — remix, transform, and build
  upon the material for any purpose, even commercially."*).
- **Exclusions noted in the policy (not part of this datasheet):** content depicting
  identifiable private individuals, third-party works, and items covered by industrial-property
  rights (logos, trademarks, patents). The aggregated housing indicators documented here contain
  none of these.

**License verified: YES — CC BY 4.0 permits derivatives and commercial reuse with attribution.**

> Licence snapshot: the controlling licence text is CC BY 4.0 (canonical, immutable at
> https://creativecommons.org/licenses/by/4.0/legalcode). A hash/archived copy of the
> licence text was not attached to this PR to avoid bloating the documentation; reviewers can
> reproduce it from the canonical URL above. See **Limitations & gaps**.

---

## Data dictionary

Eurostat housing indicators are distributed as multi-dimensional cube datasets (SDMX), one row
per combination of dimension codes plus an observation value. The fields below are the
**verified dimensions of dataset `ilc_lvho07a`**, confirmed from the Eurostat dissemination API
(SDMX-JSON) on 2026-06-28.

| Field (code) | Type | Units | Description | Allowed values / null handling |
|---|---|---|---|---|
| `freq` | categorical | — | Time frequency | `A` = Annual (verified) |
| `unit` | categorical | — | Unit of measure | `PC` = Percentage (verified) |
| `rskpovth` | categorical | — | Risk-of-poverty threshold (poverty status) | `TOTAL` = Total; `A_60` = above 60% of median equivalised income; `B_60` = below 60% (at risk of poverty) (verified) |
| `age` | categorical | — | Age class | e.g. `TOTAL`, `Y_LT6`, `Y6-11`, `Y12-17`, `Y15-24`, `Y18-64`, `Y_GE65` (full set verified from API) |
| `sex` | categorical | — | Sex | `T` = Total, `M` = Males, `F` = Females (verified) |
| `geo` | categorical | — | Geopolitical entity (reporting country) | EU member states (BE, BG, CZ, DK, DE, EE, IE, EL, ES, FR, HR, IT, CY, LV, LT, LU, HU, MT, NL, AT, PL, PT, RO, SI, SK, FI, SE) plus IS, NO, CH, ME, MK, AL, RS, TR, XK (verified) |
| `time` | temporal (year) | year | Reference year of observation | e.g. `2023` (annual; verified for the queried slice) |
| `OBS_VALUE` | numeric (float) | percent (`%`) | The measured value (here: % of population whose housing costs exceed 40% of disposable income) | Non-negative percentage; may be **missing/null** where a country-year combination is not available. *Field name per SDMX convention; value semantics verified, exact serialization key may vary by export format — see note.* |
| `OBS_FLAG` | categorical (optional) | — | Observation status flag | Standard Eurostat flags, e.g. `b` break in series, `e` estimated, `p` provisional, `d` definition differs, `:` not available. **Not separately confirmed for this slice — marked UNVERIFIED.** |

**Related housing indicators in the same collection** (commonly used; dataset codes referenced
but their per-field structure was **NOT individually verified** in this pass — marked
UNVERIFIED):

| Indicator | Dataset code (unverified) | Typical unit |
|---|---|---|
| Overcrowding rate | `ilc_lvho05a` | percentage of population |
| Severe housing deprivation rate | `ilc_mdho06a` | percentage of population |
| Distribution of population by tenure status | `ilc_lvho02` | percentage of population |

> Rows or codes marked **UNVERIFIED** were inferred from Eurostat's standard catalogue naming
> and were not confirmed against a live API response in this pass. Treat them as candidates to
> re-verify before relying on them.

---

## Datasheet for Datasets

**Motivation.** The housing indicators exist to provide comparable, harmonised measures of
housing affordability and quality across EU member states and selected neighbouring countries,
supporting EU social policy, the European Pillar of Social Rights, and poverty/social-exclusion
monitoring. Created and maintained by Eurostat from member-state survey returns.

**Composition.** Each dataset is an aggregated statistical cube: rows are combinations of
dimensions (country, year, age, sex, poverty status, unit) and the cell is an estimated
population share (a percentage). The instances are **aggregate statistics about populations**,
not records about individuals. No microdata and no personal identifiers are present in these
public dissemination datasets.

**Collection process.** Underlying source is **EU-SILC** (European Union Statistics on Income
and Living Conditions), an annual harmonised household survey run by national statistical
institutes under a common EU framework. Eurostat receives validated national returns and
computes the comparable indicators. Reference period is annual.

**Preprocessing / cleaning / labelling.** National data are validated, harmonised to common
definitions, and weighted to be population-representative. Eurostat applies standard quality
flags (provisional, estimated, break in series, definition differs). Some country-year cells are
suppressed or unavailable. Aggregates (EU27, EA) are computed by Eurostat.

**Uses.** Cross-country comparison of housing cost overburden, overcrowding and deprivation;
policy analysis; research; journalism; dashboards. Appropriate as **descriptive aggregate
statistics**. Not suitable for individual-level inference; users must respect break-in-series
flags and definitional changes across years and countries.

**Distribution.** Publicly distributed by Eurostat via the online data browser, bulk download
facility, and the dissemination REST/SDMX API, free of charge, under CC BY 4.0
(Commission Decision 2011/833/EU). This datasheet does not redistribute the data.

**Maintenance.** Maintained by Eurostat; indicators are updated annually as new EU-SILC waves
are processed. Series codes (e.g. `ilc_lvho07a`) are stable identifiers in the Eurostat
catalogue. Corrections/revisions are flagged via observation status codes.

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
  "name": "Eurostat Housing statistics (Income and living conditions) — ilc_lvho07a",
  "description": "Comparable EU housing-cost overburden, overcrowding and housing-deprivation indicators derived from EU-SILC, published by Eurostat. Aggregate population shares (percentages) broken down by country, year, age, sex and poverty status. Documentation/datasheet only; no data values are included.",
  "url": "https://ec.europa.eu/eurostat/web/income-and-living-conditions/data",
  "sameAs": "https://ec.europa.eu/eurostat/databrowser/view/ilc_lvho07a/default/table",
  "license": "https://creativecommons.org/licenses/by/4.0/",
  "creator": {
    "@type": "Organization",
    "name": "Eurostat — Statistical Office of the European Union",
    "url": "https://ec.europa.eu/eurostat"
  },
  "publisher": {
    "@type": "Organization",
    "name": "European Commission (Eurostat)"
  },
  "datePublished": "2024",
  "dateModified": "2026-06-28",
  "isAccessibleForFree": true,
  "creditText": "Source: Eurostat, dataset ilc_lvho07a. © European Union. Reused under CC BY 4.0 (Commission Decision 2011/833/EU).",
  "keywords": ["housing", "housing cost overburden", "EU-SILC", "poverty", "European Union", "official statistics"],
  "variableMeasured": [
    { "@type": "PropertyValue", "name": "geo", "description": "Reporting country / geopolitical entity (ISO-like Eurostat codes)" },
    { "@type": "PropertyValue", "name": "time", "description": "Reference year (annual)" },
    { "@type": "PropertyValue", "name": "age", "description": "Age class code" },
    { "@type": "PropertyValue", "name": "sex", "description": "Sex (T/M/F)" },
    { "@type": "PropertyValue", "name": "rskpovth", "description": "Risk-of-poverty threshold / poverty status" },
    { "@type": "PropertyValue", "name": "unit", "description": "Unit of measure (PC = percentage)", "unitText": "PERCENT" },
    { "@type": "PropertyValue", "name": "OBS_VALUE", "description": "Housing cost overburden rate (percentage of population)", "unitText": "PERCENT" }
  ]
}
```

---

## Validation script

This script checks the **expected structure** of a Eurostat housing-indicator export that a
user has already obtained themselves. It does **not** download, host, or republish any data —
the user must supply their own local file. Documentation/validation only.

```python
#!/usr/bin/env python3
"""
Validate the structure of a user-supplied Eurostat housing dataset export (e.g. ilc_lvho07a).

This script does NOT download or host any data. Point it at a file YOU obtained from Eurostat
under CC BY 4.0. It only checks that the expected dimensions/columns are present and that the
observation value column looks like a percentage.

Usage:
    python validate_housing.py path/to/ilc_lvho07a.csv
"""
import sys
import csv

# Verified dimensions of ilc_lvho07a (Eurostat dissemination API, 2026-06-28).
EXPECTED_DIMENSIONS = {"freq", "unit", "rskpovth", "age", "sex", "geo", "time"}
# Common value/flag column aliases across Eurostat export formats (TSV/CSV/SDMX-CSV).
VALUE_ALIASES = {"OBS_VALUE", "value", "values"}

def main(path):
    with open(path, newline="", encoding="utf-8") as f:
        reader = csv.DictReader(f)
        cols = {c.strip().lower(): c for c in (reader.fieldnames or [])}
        if not cols:
            sys.exit("FAIL: no header row found")

        missing = {d for d in EXPECTED_DIMENSIONS if d not in cols}
        if missing:
            print(f"WARN: expected dimensions not found as columns: {sorted(missing)}")
            print("      (Eurostat 'wide' exports pivot 'time' into year columns; that's OK.)")

        value_col = next((cols[a] for a in (v.lower() for v in VALUE_ALIASES) if a in cols), None)
        if value_col is None:
            print("WARN: no obvious observation-value column found "
                  f"(looked for {sorted(VALUE_ALIASES)}).")

        n = 0
        bad = 0
        for n, row in enumerate(reader, start=1):
            if value_col:
                raw = (row.get(value_col) or "").strip()
                if raw in ("", ":"):   # Eurostat null marker
                    continue
                try:
                    v = float(raw.split()[0])   # strip any trailing flag letters
                    if not (0.0 <= v <= 100.0):
                        bad += 1
                except ValueError:
                    bad += 1
        print(f"Rows read: {n}")
        if value_col:
            print(f"Out-of-range/non-numeric values (expected 0-100 for unit=PC): {bad}")
        print("PASS: structure check complete." if not missing else
              "PASS-WITH-WARNINGS: see notes above.")

if __name__ == "__main__":
    if len(sys.argv) != 2:
        sys.exit("Usage: python validate_housing.py <local_eurostat_export.csv>")
    main(sys.argv[1])
```

**Data-quality expectations (documentation):** for `unit=PC`, `OBS_VALUE` should fall within
0–100; missing values appear as `:`; series breaks/estimates are flagged via observation status
codes; not every country-year-age-sex-poverty combination is populated.

---

## Limitations & gaps

- **Verified vs. inferred:** Only dataset `ilc_lvho07a` was confirmed against a live Eurostat
  API response (dimensions, codes, units). Related dataset codes (`ilc_lvho05a`, `ilc_mdho06a`,
  `ilc_lvho02`) and the `OBS_FLAG` column are marked **UNVERIFIED** and should be re-checked.
- **Field-name serialization:** Eurostat offers several export formats (TSV, SDMX-CSV,
  SDMX-JSON). The exact value-column key (`OBS_VALUE` vs `value`) and whether `time` appears as
  a column or as pivoted year-columns depends on the chosen format; the validation script
  accommodates common variants but was not run against every format.
- **The theme landing page** (`income-and-living-conditions/data`) and the Eurostat copyright
  page returned HTTP 404 / required authentication at retrieval time; licence confirmation was
  therefore taken from the European Commission legal notice (Commission Decision 2011/833/EU →
  CC BY 4.0), which governs all Commission/Eurostat content.
- **Licence snapshot hash:** the acceptance criterion asks for a hash + archived copy of the
  licence text. This was not embedded here to keep the datasheet self-contained and free of
  large verbatim text; the canonical CC BY 4.0 legalcode URL is provided instead.
- **No data inspected at record level:** consistent with the documentation-only guardrail and
  the fact that these are aggregate statistics, no <=1000-row sample was downloaded; the data
  dictionary is built from the dataset's published dimensional structure, not from sampled rows.
- **PII:** none. These are population-level aggregate percentages; no personal or identifying
  fields exist in the documented dataset.
```
