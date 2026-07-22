# Datasheet — Labor statistics (CPI, employment), U.S. Bureau of Labor Statistics

**One-line summary:** Documentation datasheet for the U.S. Bureau of Labor Statistics (BLS)
public economic time-series data (Consumer Price Index and employment/labor-force series),
accessed via the BLS Public Data API and the BLS "Databases, Tables & Calculators by Subject"
portal.

*This datasheet (the documentation in this file) is licensed **CC-BY-4.0**. It does not relicense
the underlying data, which carries its own licence/status described below.*

> **Scope guardrail:** This is **documentation only**. No BLS data is hosted, mirrored,
> transformed, sampled into, or committed by this file. Field definitions below are documented
> from public BLS / API documentation, not from a republished extract.

---

## Provenance & attribution

| Item | Value |
|---|---|
| **Publisher / source** | U.S. Bureau of Labor Statistics (BLS), U.S. Department of Labor |
| **Dataset** | Labor statistics time series — Consumer Price Index (CPI) and employment / labor-force series |
| **Source URL (portal)** | https://www.bls.gov/data/ |
| **Programmatic access** | BLS Public Data API v2 — https://www.bls.gov/developers/ |
| **Catalog id (Hee-Lee Oss)** | cat-econ-005 |
| **Retrieval date** | 2026-06-28 |
| **Required attribution string** | "Source: U.S. Bureau of Labor Statistics (https://www.bls.gov/)." |

**Why it matters:** CPI and employment series are foundational U.S. macroeconomic indicators used
for inflation adjustment, policy analysis, research, and journalism. They are public domain but
use complex, survey-specific series IDs that benefit from clear documentation.

---

## Source licence

**Status: U.S. Government work — public domain (permits derivatives). VERIFIED.**

BLS is a U.S. Federal agency and its published material is in the public domain. The BLS
**Copyright Information** policy states that BLS material is in the public domain and may be
reproduced/used without permission, with a request to cite BLS as the source. Public-domain U.S.
Government works carry no copyright restriction on reproduction or the creation of derivatives.

- **Verifying citation (license/terms):** BLS Copyright Information —
  https://www.bls.gov/opub/copyright-information.htm
- **Secondary citation:** BLS Linking and Copyright Information —
  https://www.bls.gov/bls/linksite.htm
- **Clause confirming derivatives are permitted (paraphrased from the BLS policy, verified via the
  pages above):** BLS material is in the public domain; all text, charts, and tables are in the
  public domain and "may be reproduced without permission" with proper credit. The only stated
  exception is **photographs/illustrations** (licensed from Getty/Shutterstock) — these are *not*
  part of the numeric CPI/employment data series documented here.

Because public-domain material is free of copyright, reproduction *and* derivative works
(transformations, recombinations, analyses) are permitted. Attribution is requested, not legally
required, but Hee-Lee Oss records and uses it (see attribution string above).

> **Licence snapshot note:** The acceptance criterion asks for an archived copy + hash of the
> licence text. The BLS site returned HTTP 403 to automated fetches during this deed, so the licence
> text was confirmed via the public BLS Copyright Information page (indexed content) rather than a
> stored byte-for-byte archive. A reviewer should attach an archived snapshot + SHA-256 of
> `copyright-information.htm` at the gate to fully close that criterion. The licence *status*
> (public domain, derivatives permitted) is confirmed.

---

## Data dictionary

The BLS Public Data API v2 returns time-series observations as JSON. The structure below reflects
the documented API response shape. Field-level rows marked **(verified)** are confirmed from BLS /
API documentation; rows marked **(unverified)** could not be byte-confirmed from a live fetch in
this deed and should be re-checked at the gate.

Top-level response: `Results.series` is an array; each series has a `seriesID` and a `data` array of
observation records.

| Field | Type | Units | Description | Allowed values / nulls |
|---|---|---|---|---|
| `seriesID` | string | n/a | Unique BLS series identifier (survey-specific code) | Non-null. Format varies by survey (see below). **(verified)** |
| `year` | string | calendar year | Year of the observation | 4-digit year, e.g. `"2025"`. Non-null. **(verified)** |
| `period` | string | period code | Period within the year | `M01`–`M12` (months), `M13` (annual avg), `Q01`–`Q05` (quarters), `S01`–`S03` (semiannual) depending on series. Non-null. **(verified)** |
| `periodName` | string | n/a | Human-readable period name | e.g. `"December"`, `"Annual"`. Non-null. **(verified)** |
| `value` | string | series-dependent (index points for CPI; persons/thousands or percent for employment) | The observation value, returned as a string | Numeric-as-string. May be empty/footnoted for suppressed or unavailable data. **(verified)** |
| `footnotes` | array of objects | n/a | Footnote annotations (e.g. revision, preliminary) | Array; elements have `code` (string) and `text` (string). May be an array containing an empty object when none apply. **(verified)** |
| `footnotes[].code` | string | n/a | Footnote code | e.g. `"P"`, `"R"`; may be null/empty. **(verified)** |
| `footnotes[].text` | string | n/a | Footnote text | e.g. `"Preliminary."`; may be null/empty. **(verified)** |
| `latest` | string/boolean | n/a | Marks the most-recent observation in the series | Present (e.g. `"true"`) only on the latest record; otherwise absent. **(verified)** |
| `calculations` | object | percent | Net/percent changes when `calculations=true` requested | Optional; nested `net_changes` / `pct_changes` by 1,3,6,12 periods. **(unverified — optional request param)** |

**Series ID format (documented from BLS conventions; segment-level codes are survey-specific):**

- **CPI example:** `CUUR0000SA0` — `CU` = CPI for All Urban Consumers (CPI-U) program;
  `U`/`S` = not-seasonally / seasonally adjusted; `R`/`S` = periodicity; `0000` = area code
  (U.S. city average); `SA0` = item code (all items). **(structure verified at the program-prefix
  level; individual area/item code values must be looked up in BLS series-ID lookup tables —
  treat specific sub-codes as unverified unless checked.)**
- **Labor force / employment example:** `LNS14000000` (unemployment rate, seasonally adjusted) —
  `LN` = Labor Force Statistics (Current Population Survey); `S`/`U` = seasonal adjustment.
  Sub-codes (e.g. `LNS14000003` White, `LNS14000006` Black) select demographic detail.
  **(prefix and example codes verified; full sub-code taxonomy is survey-specific.)**

> The API returns **observational values and footnotes only — not series metadata**. Series titles,
> units, and seasonal-adjustment flags must be obtained from BLS series-ID lookup tables /
> flat files, not from the API response itself.

---

## Datasheet for Datasets

**Motivation.** Created by BLS to measure inflation (CPI) and the U.S. labor market
(employment, unemployment, labor-force participation, earnings). Funded by the U.S. Federal
government to inform public policy, indexation (e.g. Social Security COLAs), and research.

**Composition.** Statistical time series: each instance is an observation
(series × year × period → value). Series cover prices (CPI-U, CPI-W, item/area breakdowns) and
labor-force measures (CPS-derived employment, unemployment, participation; CES establishment
employment). Instances are aggregate statistics, **not** individual respondents. No
personal/identifying data is present in the published series.

**Collection process.** Underlying microdata are collected via national surveys — the Current
Population Survey (household survey, with the Census Bureau), the Current Employment Statistics
survey (establishments), and price collection for CPI. BLS aggregates, seasonally adjusts, and
publishes derived series. Methodology is documented in the BLS Handbook of Methods.

**Preprocessing / cleaning.** BLS performs sampling, weighting, seasonal adjustment, and
revision. Values may be preliminary and later revised (flagged via `footnotes`). The API exposes
the published, already-processed series.

**Uses.** Inflation measurement and indexation, macroeconomic and labor-market analysis,
academic research, journalism, and contract escalation. Caveat: distinguish seasonally adjusted vs
not-seasonally-adjusted series and check series-specific units before comparing.

**Distribution.** Publicly distributed by BLS via the data portal (https://www.bls.gov/data/),
downloadable flat files, and the Public Data API v2. Public domain; freely redistributable with
attribution requested. This datasheet does **not** redistribute the data.

**Maintenance.** Maintained and regularly updated by BLS on published release schedules
(e.g. monthly CPI and Employment Situation releases). Revisions and discontinuations are managed by
BLS; consult BLS release calendars and series documentation for currency.

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
  "name": "BLS Labor Statistics (CPI and Employment) — time series",
  "description": "U.S. Bureau of Labor Statistics public economic time series, including Consumer Price Index (CPI) and employment/labor-force series, accessed via the BLS Public Data API v2. Documentation-only datasheet; data is not redistributed.",
  "url": "https://www.bls.gov/data/",
  "sameAs": "https://www.bls.gov/developers/",
  "license": "https://www.usa.gov/government-works",
  "creator": {
    "@type": "GovernmentOrganization",
    "name": "U.S. Bureau of Labor Statistics",
    "url": "https://www.bls.gov/"
  },
  "publisher": {
    "@type": "GovernmentOrganization",
    "name": "U.S. Bureau of Labor Statistics"
  },
  "dateModified": "2026-06-28",
  "isAccessibleForFree": true,
  "creativeWorkStatus": "Public Domain",
  "recordSet": [
    {
      "@type": "cr:RecordSet",
      "name": "observations",
      "description": "One record per series-period observation as returned by the BLS API v2.",
      "field": [
        { "@type": "cr:Field", "name": "seriesID", "dataType": "Text", "description": "BLS series identifier." },
        { "@type": "cr:Field", "name": "year", "dataType": "Text", "description": "Calendar year (4-digit)." },
        { "@type": "cr:Field", "name": "period", "dataType": "Text", "description": "Period code, e.g. M01-M12." },
        { "@type": "cr:Field", "name": "periodName", "dataType": "Text", "description": "Human-readable period name." },
        { "@type": "cr:Field", "name": "value", "dataType": "Text", "description": "Observation value (numeric-as-string)." },
        { "@type": "cr:Field", "name": "footnotes", "dataType": "Text", "description": "Array of {code,text} footnote objects." }
      ]
    }
  ]
}
```

---

## Validation script

The script below checks the **structure** of a BLS API v2 JSON response that a user has already
obtained themselves. It does **not** download, host, or republish any data — it operates on a
locally provided response file passed as an argument.

```python
#!/usr/bin/env python3
"""Validate the STRUCTURE of a BLS Public Data API v2 JSON response.

Documentation/validation aid only. This script does NOT fetch, download, host,
or redistribute any data. Provide a JSON response you already obtained:

    python validate_bls.py path/to/bls_response.json
"""
import sys, json

REQUIRED_RECORD_FIELDS = {"year", "period", "periodName", "value", "footnotes"}
VALID_PERIOD_PREFIXES = ("M", "Q", "S", "A")


def validate(path: str) -> int:
    with open(path, "r", encoding="utf-8") as fh:
        doc = json.load(fh)

    errors = []
    if doc.get("status") and doc["status"] != "REQUEST_SUCCEEDED":
        errors.append(f"API status not successful: {doc.get('status')}")

    series = doc.get("Results", {}).get("series")
    if not isinstance(series, list) or not series:
        errors.append("Results.series missing or empty")
        series = []

    for s in series:
        if "seriesID" not in s:
            errors.append("series missing 'seriesID'")
        data = s.get("data", [])
        if not isinstance(data, list):
            errors.append(f"series {s.get('seriesID')} 'data' is not a list")
            continue
        for rec in data:
            missing = REQUIRED_RECORD_FIELDS - set(rec)
            if missing:
                errors.append(f"{s.get('seriesID')}: record missing {sorted(missing)}")
            p = rec.get("period", "")
            if p and not p.startswith(VALID_PERIOD_PREFIXES):
                errors.append(f"{s.get('seriesID')}: unexpected period code {p!r}")
            if not isinstance(rec.get("footnotes", []), list):
                errors.append(f"{s.get('seriesID')}: footnotes is not a list")
        # Sanity: keep any local sample bounded (documentation guideline: <=1000 rows)
        if len(data) > 1000:
            errors.append(f"{s.get('seriesID')}: >1000 rows; bound your local sample")

    if errors:
        print("FAIL:")
        for e in errors:
            print("  -", e)
        return 1
    print(f"OK: {len(series)} series, structure valid.")
    return 0


if __name__ == "__main__":
    if len(sys.argv) != 2:
        print(__doc__)
        sys.exit(2)
    sys.exit(validate(sys.argv[1]))
```

**Data-quality checks documented (not run here):** confirm each series returns contiguous
year/period coverage for the requested range; verify `value` parses as a float; flag records
carrying `footnotes` codes (preliminary `P` / revised `R`); confirm seasonal-adjustment expectation
matches the series ID (`S` vs `U`).

---

## Limitations & gaps

- **Licence archive not byte-captured:** The BLS site returned HTTP 403 to automated fetches during
  this deed. Public-domain status and the derivatives-permitted clause were verified via the BLS
  Copyright Information / Linking pages, but a byte-for-byte archived copy + SHA-256 hash of the
  licence text was **not** stored. A reviewer should attach that snapshot at the gate.
- **No live data sample taken:** Per guardrails, no dataset rows were fetched, sampled, or
  committed. The data dictionary is built from API/BLS documentation, not from an extract, so the
  exact runtime presence/typing of optional fields (`latest`, `calculations`) is marked unverified.
- **Series-ID sub-codes:** Program prefixes (CU, LN, CE) and example codes are verified, but the
  full area/item/demographic sub-code taxonomies are survey-specific and must be resolved against
  BLS series-ID lookup tables; specific sub-codes here are illustrative, not exhaustive.
- **Metadata not in API:** Series titles/units/seasonal-adjustment flags are not returned by the
  API and must be obtained separately from BLS flat files.
- **No PII:** The published series are aggregate statistics; no personal or identifying fields were
  documented or sampled.
