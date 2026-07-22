# Datasheet — National accounts & macro indicators (Eurostat)

One-line summary: Comparable European national accounts and macroeconomic indicators
(GDP and components, value added, capital formation, employment, household consumption)
for EU Member States, EFTA, and candidate countries, compiled under the ESA 2010 framework.

> This datasheet (the documentation in this file) is licensed under **CC-BY-4.0**.
> It is documentation only: no dataset is hosted, mirrored, transformed, or republished here.

---

## Provenance & attribution

- **Publisher / source:** Eurostat (Statistical Office of the European Union).
- **Dataset:** "National accounts & macro indicators".
- **Source URL:** https://ec.europa.eu/eurostat/web/national-accounts
- **Catalog id:** cat-econ-004 (Hee-Lee Oss task ds-econ-004).
- **Retrieval date:** 2026-06-28
- **Methodology framework:** European System of Accounts (ESA 2010).
- **Required attribution string:**
  > Source: Eurostat — National accounts (https://ec.europa.eu/eurostat/web/national-accounts),
  > © European Union, licensed under CC BY 4.0. Adapted/derived; changes indicated.

---

## Source licence

- **Licence:** Creative Commons Attribution 4.0 International (**CC BY 4.0**).
- **Confirmation it permits derivatives:** Yes — verified on the Eurostat copyright /
  free re-use page.
- **Citation (verified 2026-06-28):**
  - Eurostat copyright notice: https://ec.europa.eu/eurostat/help/copyright-notice
  - Licence text: https://creativecommons.org/licenses/by/4.0/
- **Verified wording (paraphrased from the copyright page):** reuse and redistribution are
  permitted for both commercial and non-commercial purposes provided the source is
  acknowledged; **derivative works/adaptations are allowed**, with a requirement to clearly
  indicate any changes made. CC BY 4.0 itself grants the right to "remix, transform, and build
  upon the material for any purpose" (creativecommons.org/licenses/by/4.0/).
- **Caveats captured at the gate (do not ignore in derivative work):**
  - Third-party copyrighted material embedded in some products requires separate authorisation.
  - Logos/trademarks are excluded unless redistributed as integral parts of unchanged products.
  - Certain **non-EU country data is restricted to non-commercial reuse only**, and some trade
    data (Switzerland, Austria) has commodity-classification restrictions. These restrictions
    are dataset-specific; downstream reuse of non-EU members' figures must re-check terms.

**Conclusion:** Licence **permits derivatives** for the core EU national-accounts content;
this datasheet is therefore not marked DRAFT. Reusers of non-EU partner data must verify the
narrower terms above.

---

## Data dictionary

National accounts are published as multiple linked tables/datasets rather than a single flat
file. The fields below are the **dimensions and measures common to Eurostat national-accounts
tables** as described on the source documentation page and the ESA 2010 framework. Field codes
follow Eurostat's standard SDMX-style dimension naming; exact code lists vary per table and are
marked unconfirmed where not verified against a specific table's metadata.

| Field | Type | Units | Description | Allowed values / null handling |
|-------|------|-------|-------------|--------------------------------|
| `geo` | string (code) | — | Reference geographic area | EU/euro-area aggregates, Member States, EFTA, candidate countries (NUTS/ISO-style codes). *Code list unconfirmed per-table.* |
| `time` (`TIME_PERIOD`) | string/date | year or year-quarter | Reference period | Annual (`YYYY`) or quarterly (`YYYY-Qn`). |
| `na_item` | string (code) | — | National accounts indicator (e.g. GDP, gross value added, gross fixed capital formation, household final consumption) | Per-table ESA 2010 code list. *Exact codes unconfirmed.* |
| `unit` | string (code) | varies | Unit of measure | e.g. current prices million EUR, chain-linked volumes, % of GDP, index. *Exact code list unconfirmed.* |
| `s_adj` | string (code) | — | Seasonal adjustment status (quarterly data) | e.g. seasonally adjusted / unadjusted. *Unconfirmed per-table.* |
| `nace_r2` | string (code) | — | Industry breakdown (NACE Rev. 2) where value added is by activity | NACE Rev. 2 sections/divisions. *Applies only to industry tables; unconfirmed.* |
| `OBS_VALUE` | numeric (float) | per `unit` | The observed statistical value | May be null/missing; Eurostat uses observation flags. |
| `OBS_FLAG` | string (code) | — | Observation status flag | e.g. `p` provisional, `e` estimated, `b` break, `c` confidential, `:` not available. *Standard Eurostat flags; per-table list unconfirmed.* |

Notes:
- **No personal or identifying fields**: national accounts are macro aggregates by country and
  period; no individual-level or personal data is present.
- Rows marked *unconfirmed* were not validated against a specific downloaded table (bounded-access
  / documentation-only constraint). Confirm against the target table's metadata before relying on
  exact code lists.

---

## Datasheet for Datasets

- **Motivation:** Provide comparable, harmonised macroeconomic statistics across EU Member
  States, EFTA, and candidate countries to support economic policy, monitoring, research, and
  cross-country comparison. Compiled by Eurostat as the EU's official statistical authority.
- **Composition:** Macroeconomic aggregates and components — GDP and expenditure/income/output
  components, gross value added by industry, gross fixed capital formation, employment and labour
  productivity, household consumption expenditure by purpose, and non-financial balance sheets.
  Instances are country-by-period observations; no individuals are represented.
- **Collection process:** Member States compile national accounts under the legally binding
  ESA 2010 methodology and transmit them to Eurostat via standardised transmission programmes
  (questionnaires/SDMX). Eurostat validates and aggregates to EU/euro-area totals.
- **Preprocessing / cleaning:** Standardisation to ESA 2010 concepts; validation; seasonal
  adjustment for quarterly series; chain-linking for volume measures; aggregation to EU/euro-area
  levels. Observation flags mark provisional, estimated, break-in-series, or confidential values.
- **Uses:** Economic policy and surveillance (e.g. macroeconomic imbalance procedure), academic
  research, journalism, business analysis, and EU budget/own-resources calculations. Caution:
  series breaks, methodological revisions, and provisional values affect comparability over time.
- **Distribution:** Published openly by Eurostat via its online database, bulk download facility,
  and API under CC BY 4.0 (with the caveats noted above). This datasheet does not redistribute it.
- **Maintenance:** Maintained and regularly updated by Eurostat per a published release calendar;
  data subject to scheduled revisions. Contact via Eurostat user support channels.

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
  "name": "National accounts & macro indicators",
  "description": "Comparable European national accounts and macroeconomic indicators (GDP and components, gross value added, gross fixed capital formation, employment, household consumption) for EU Member States, EFTA, and candidate countries, compiled under ESA 2010. Documentation datasheet only; data not hosted here.",
  "url": "https://ec.europa.eu/eurostat/web/national-accounts",
  "license": "https://creativecommons.org/licenses/by/4.0/",
  "creator": {
    "@type": "Organization",
    "name": "Eurostat (Statistical Office of the European Union)",
    "url": "https://ec.europa.eu/eurostat"
  },
  "datePublished": "2026-06-28",
  "keywords": ["national accounts", "GDP", "macroeconomics", "ESA 2010", "Eurostat", "EU"],
  "recordSet": [
    {
      "@type": "cr:RecordSet",
      "name": "observations",
      "field": [
        { "@type": "cr:Field", "name": "geo", "description": "Reference geographic area code", "dataType": "Text" },
        { "@type": "cr:Field", "name": "time", "description": "Reference period (year or year-quarter)", "dataType": "Text" },
        { "@type": "cr:Field", "name": "na_item", "description": "National accounts indicator code (ESA 2010)", "dataType": "Text" },
        { "@type": "cr:Field", "name": "unit", "description": "Unit of measure code", "dataType": "Text" },
        { "@type": "cr:Field", "name": "OBS_VALUE", "description": "Observed value", "dataType": "Float" },
        { "@type": "cr:Field", "name": "OBS_FLAG", "description": "Observation status flag", "dataType": "Text" }
      ]
    }
  ]
}
```

---

## Validation script

The script below documents how to validate the **structure** of a national-accounts table the
user has already obtained and exported to CSV. It does **not** download, host, or fetch the
dataset — the user supplies a local file path. It checks for expected columns and a non-empty,
plausibly-sized row count.

```python
#!/usr/bin/env python3
"""Validate the structure of a Eurostat national-accounts CSV export.

Documentation-only: does NOT download or host data. Point it at a file you
already obtained yourself, locally.

Usage: python validate.py path/to/your_eurostat_export.csv
"""
import csv
import sys

# Core columns expected in a Eurostat SDMX-style CSV export.
# Adjust per the specific table's metadata; some are table-specific.
EXPECTED_CORE = {"geo", "TIME_PERIOD", "OBS_VALUE"}
OPTIONAL = {"na_item", "unit", "s_adj", "nace_r2", "OBS_FLAG"}
MIN_ROWS = 1
MAX_PLAUSIBLE_ROWS = 50_000_000  # sanity bound, not a hard rule

def main(path: str) -> int:
    with open(path, newline="", encoding="utf-8-sig") as f:
        reader = csv.reader(f)
        header = next(reader, [])
        cols = {c.strip() for c in header}
        missing = EXPECTED_CORE - cols
        if missing:
            print(f"FAIL: missing expected core columns: {sorted(missing)}")
            print(f"      found columns: {sorted(cols)}")
            return 1
        rows = sum(1 for _ in reader)
    if not (MIN_ROWS <= rows <= MAX_PLAUSIBLE_ROWS):
        print(f"FAIL: row count {rows} outside plausible range "
              f"[{MIN_ROWS}, {MAX_PLAUSIBLE_ROWS}]")
        return 1
    extra = cols - EXPECTED_CORE - OPTIONAL
    print(f"OK: core columns present; {rows} data rows.")
    if extra:
        print(f"NOTE: additional/table-specific columns present: {sorted(extra)}")
    return 0

if __name__ == "__main__":
    if len(sys.argv) != 2:
        print(__doc__)
        sys.exit(2)
    sys.exit(main(sys.argv[1]))
```

---

## Limitations & gaps

- **Field codes not validated against a live table.** Exact code lists for `na_item`, `unit`,
  `s_adj`, `nace_r2`, and `OBS_FLAG` are described from the documentation page and Eurostat's
  general conventions, not confirmed against a specific downloaded table (documentation-only /
  bounded-access constraint). Rows are marked *unconfirmed* accordingly.
- **Single-page verification.** Licence and dataset structure were verified from the Eurostat
  national-accounts overview page and the copyright notice page (retrieved 2026-06-28). The
  copyright page was read via a fast extraction model; the verbatim CC BY 4.0 grant is quoted
  from creativecommons.org. A licence-text snapshot/hash was not committed (documentation-only).
- **Non-EU partner data caveat.** Some non-EU country data and certain trade data carry narrower
  reuse terms (non-commercial only / classification restrictions); these were not enumerated per
  series and must be checked before reusing those specific figures.
- **No data sampled.** No rows were downloaded; the data dictionary reflects published structure,
  not an inspected sample. No personal/identifying data is present (macro aggregates only).
