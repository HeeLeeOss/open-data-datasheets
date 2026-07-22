# DRAFT — LICENCE UNVERIFIED / GUARDRAIL FLAG: NON-COMMERCIAL LICENCE DETECTED

> **STOP / REVIEW REQUIRED.** The task's first-pass assumption was that the BOOST data is
> CC-BY-4.0 (commercial use + derivatives permitted). On verification at the World Bank Data
> Catalog, the BOOST dataset entries are labelled **"Creative Commons Attribution-NonCommercial
> 4.0 (CC BY-NC 4.0)"** — a **non-commercial** licence. Under the Hee-Lee Oss refusal guardrails, a
> source that is non-commercial must be surfaced and flagged rather than treated as fully open.
> CC BY-NC *does* permit derivatives, but the **NC restriction conflicts with the assumed
> CC-BY-4.0 status** and with Hee-Lee Oss's "primarily-public, freely-reusable" intent. This datasheet
> is therefore published as a **DRAFT for human review**: do not rely on it as a clearance to
> reuse BOOST data commercially. See "Source licence" and "Limitations & gaps" below.

# Datasheet: BOOST Public Expenditure Database

**One-line summary:** A documentation datasheet (provenance, data dictionary, Datasheet-for-Datasets
write-up, Croissant metadata, and a validation script) for the World Bank **BOOST** harmonized
cross-country public-expenditure database. **Documentation only — no dataset data is hosted,
mirrored, transformed, or republished here.**

**Datasheet licence:** This datasheet (the document you are reading) is released under
**CC-BY-4.0**. The licence of the *underlying dataset* is described separately below and is the
subject of the guardrail flag above.

---

## Provenance & attribution

| Field | Value |
|---|---|
| Dataset | BOOST Public Expenditure Database (BOOST Open Budget Portal) |
| Publisher / creator | The World Bank |
| Programme home | https://www.worldbank.org/en/programs/boost-portal |
| Source URL (task resource) | https://www.worldbank.org/en/programs/boost-portal |
| Data Catalog (example country entry verified) | https://datacatalog.worldbank.org/ (BOOST collection; e.g. "Liberia BOOST") |
| Retrieval date | 2026-06-28 |
| Catalog id (candidate, from task) | cat-budget-005 |

**Required attribution string (per World Bank dataset terms):**
> "The World Bank: BOOST Public Expenditure Database. Source: World Bank BOOST Open Budget Portal,
> https://www.worldbank.org/en/programs/boost-portal (retrieved 2026-06-28)." Used under
> Creative Commons Attribution-NonCommercial 4.0 (CC BY-NC 4.0) — **see licence note below.**

---

## Source licence

**Verified licence name:** Creative Commons Attribution-NonCommercial 4.0 International
(**CC BY-NC 4.0**).

**Where verified:** The World Bank Data Catalog entries for BOOST country datasets (e.g. the
"Liberia BOOST" entry) list the licence as *"Creative Commons Attribution-Non Commercial 4.0"*
with public access (inside and outside the World Bank). Retrieved 2026-06-28.
CC BY-NC 4.0 canonical text: https://creativecommons.org/licenses/by-nc/4.0/legalcode

**Conflict / why this is a DRAFT:**
- The World Bank's *general* "Terms of use for Datasets"
  (https://www.worldbank.org/en/about/legal/terms-of-use-for-datasets) state that most datasets
  default to **CC BY 4.0** (https://creativecommons.org/licenses/by/4.0/), which permits
  derivatives *and* commercial use: "You may extract, download, and make copies of the data
  contained in the Datasets, and you may share that data with third parties."
- However, the **dataset-specific metadata in the Data Catalog overrides the default**, and BOOST
  country datasets are marked **CC BY-NC 4.0 (Non-Commercial)**. Per the terms, third-party /
  dataset-specific licences in the metadata take precedence over the default.

**Derivatives:** CC BY-NC 4.0 *does* permit adaptations/derivatives (clause 2(a)(1): licensee may
"reproduce and Share the Licensed Material" and "produce, reproduce, and Share Adapted Material")
**but only for NonCommercial purposes** (clause 1(k) "NonCommercial" + 2(a)(1)). Therefore the
licence **does not** confirm the unrestricted reuse assumed by the task.

**Conclusion for the gate:** `licenseVerified = false` for the purposes of "permits derivatives
*and* unrestricted (incl. commercial) reuse." The licence is verified to be **CC BY-NC 4.0**, a
non-commercial licence. A human must decide whether Hee-Lee Oss's use is compatible with NC before any
downstream use. Licence snapshot/hash for an archived copy of the licence text was **not** taken
in this pass (see Limitations).

---

## Data dictionary

BOOST is a *harmonized* framework: each country's raw budget execution data is mapped into a
common set of classification dimensions and budget-stage measures. The dimensions below are
verified from the World Bank programme/catalog descriptions; **exact machine column headers vary
by country file and were not confirmed against a sampled file in this pass** and are marked
*Unverified*.

| Field | Type | Units | Description | Allowed values / nulls |
|---|---|---|---|---|
| country | string | — | Country (or sub-national entity) the record belongs to | ISO country name; required (verified) |
| year | integer | calendar year | Fiscal year of the budget line | e.g. 2012–2021 for Liberia; required (verified) |
| budget_type / budget stage | categorical | — | Budget stage indicator | e.g. approved/initial vs. executed/actual (verified concept; exact labels *Unverified*) |
| admin_class (administrative classification) | string | — | Spending unit / ministry / agency hierarchy | Country-specific code lists (verified dimension; codes *Unverified*) |
| func_class (functional classification) | string | — | Function of government (COFOG-aligned in harmonized view) | Country-specific; COFOG mapping (verified dimension; exact values *Unverified*) |
| econ_class (economic classification) | string | — | Economic nature of spend (wages, goods/services, capital, transfers, etc.) | Country-specific code lists (verified dimension; codes *Unverified*) |
| geo_class (geographic/sub-national) | string | — | Geographic location of spend where available | May be null when not tracked (verified dimension; nulls *Unverified*) |
| amount / budget parameter | numeric | national currency (per-country) | Monetary value for the line/stage | Non-negative typical; currency per country (verified concept; field name + units *Unverified*) |
| indicator | categorical | — | BOOST data-lab indicator selector (e.g. expenditure measure) | From data-lab UI (verified that "indicator" is a selectable variable) |

*Verified variable selectors observed in the BOOST data lab UI: **year, budget type, indicator,
budget parameter, country**.* Granular per-country column names, exact code lists, units, and null
conventions require a bounded-access sample read of an actual country file, which was **not**
performed in this documentation-only pass.

---

## Datasheet for Datasets

**Motivation.** BOOST was created by the World Bank to make government budget data transparent,
granular, and comparable across countries, supporting public participation in the budget process
and evidence-based fiscal policy. It harmonizes heterogeneous national budget-execution systems
into a common analytical structure.

**Composition.** Records are line-item budget and expenditure entries, disaggregated by
administrative, economic, functional, and (where available) geographic classifications, across
budget stages (e.g. approved vs. executed) and fiscal years. Instances are fiscal aggregates, not
individuals. **No personal/identifying data is expected** (it is aggregate public-finance data);
this should be re-confirmed against any sampled file.

**Collection process.** Data originates from national treasury / financial-management information
systems and official budget execution reports, supplied or validated by country authorities, then
mapped by the World Bank into the BOOST harmonized schema. Publication occurs only for countries
that have endorsed it.

**Preprocessing / cleaning.** Country raw data is reclassified into the common BOOST taxonomy
(administrative/economic/functional/geographic) and standardized across budget stages. Exact
mapping rules are country-specific and documented in per-country BOOST methodology notes (not
reproduced here).

**Uses.** Cross-country and within-country fiscal analysis, transparency dashboards, budget
credibility analysis (approved vs. executed), and research. **Use is constrained by the
NonCommercial licence on the catalog entries** — see Source licence.

**Distribution.** Distributed by the World Bank via the BOOST Open Budget Portal, the BOOST Data
Lab (interactive tables/charts), and per-country Data Catalog entries (Excel downloads + API).
This datasheet redistributes **none** of that data.

**Maintenance.** Maintained by the World Bank BOOST team; country datasets are updated
periodically (e.g. the Liberia entry was updated in 2023). Contact for the example country dataset
was listed in the catalog metadata.

---

## Croissant metadata

```json
{
  "@context": {
    "@vocab": "https://schema.org/",
    "cr": "http://mlcommons.org/croissant/",
    "data": "http://mlcommons.org/croissant/data/",
    "dct": "http://purl.org/dc/terms/"
  },
  "@type": "Dataset",
  "name": "BOOST Public Expenditure Database",
  "description": "World Bank BOOST: harmonized cross-country public-expenditure (budget execution) data, disaggregated by administrative, economic, functional, and geographic classifications across budget stages and fiscal years. Documentation-only datasheet; data not hosted here.",
  "url": "https://www.worldbank.org/en/programs/boost-portal",
  "sameAs": "https://datacatalog.worldbank.org/",
  "license": "https://creativecommons.org/licenses/by-nc/4.0/",
  "creator": {
    "@type": "Organization",
    "name": "The World Bank",
    "url": "https://www.worldbank.org"
  },
  "datePublished": "2026-06-28",
  "version": "draft-2026-06-28",
  "keywords": ["public finance", "government spending", "budget execution", "fiscal transparency", "BOOST"],
  "creditText": "The World Bank: BOOST Public Expenditure Database (CC BY-NC 4.0).",
  "recordSet": [
    {
      "@type": "cr:RecordSet",
      "name": "expenditure_lines",
      "description": "Harmonized budget/expenditure line items (field stubs; exact per-country columns unverified).",
      "field": [
        {"@type": "cr:Field", "name": "country", "dataType": "sc:Text", "description": "Country/entity"},
        {"@type": "cr:Field", "name": "year", "dataType": "sc:Integer", "description": "Fiscal year"},
        {"@type": "cr:Field", "name": "budget_type", "dataType": "sc:Text", "description": "Budget stage (e.g. approved vs executed)"},
        {"@type": "cr:Field", "name": "func_class", "dataType": "sc:Text", "description": "Functional classification (COFOG-aligned)"},
        {"@type": "cr:Field", "name": "econ_class", "dataType": "sc:Text", "description": "Economic classification"},
        {"@type": "cr:Field", "name": "amount", "dataType": "sc:Float", "description": "Monetary value in national currency"}
      ]
    }
  ]
}
```

> Note: field names in `recordSet` are **stubs** reflecting the harmonized BOOST dimensions, not
> verified column headers from a specific country file.

---

## Validation script

The script below documents *how a reviewer would validate a locally obtained BOOST country file*.
It performs **no download and hosts nothing** — the operator must already have a file locally,
under the source licence, and supply its path. It checks expected structure (presence of core
dimension columns and a non-empty row count), reading only a bounded sample.

```python
#!/usr/bin/env python3
"""Validate the STRUCTURE of a locally-held BOOST country expenditure file.

Documentation-only: this script does NOT download, mirror, or republish data.
The reviewer must already hold the file locally under its source licence (CC BY-NC 4.0).

Usage:
    python validate_boost.py /path/to/local_boost_country.csv
"""
import sys
import csv

# Core harmonized dimensions BOOST is expected to expose (concept-verified).
# Exact headers vary by country; adjust the alias map after inspecting the real header row.
EXPECTED_DIMENSIONS = {
    "country":    ["country", "country_name"],
    "year":       ["year", "fiscal_year", "fy"],
    "budget_stage": ["budget_type", "budget_stage", "stage"],
    "admin":      ["admin", "admin_class", "administrative"],
    "func":       ["func", "func_class", "functional", "cofog"],
    "econ":       ["econ", "econ_class", "economic"],
    "amount":     ["amount", "value", "budget_parameter"],
}
SAMPLE_ROW_CAP = 1000  # bounded read, local-only, ephemeral

def find(header_lower, aliases):
    return next((a for a in aliases if a in header_lower), None)

def main(path):
    with open(path, newline="", encoding="utf-8-sig") as f:
        reader = csv.reader(f)
        try:
            header = next(reader)
        except StopIteration:
            print("FAIL: file is empty"); return 1
        header_lower = [h.strip().lower() for h in header]
        missing = [dim for dim, al in EXPECTED_DIMENSIONS.items()
                   if find(header_lower, al) is None]
        rows = sum(1 for _ in zip(range(SAMPLE_ROW_CAP), reader))

    print(f"Columns found: {len(header)}")
    print(f"Sampled rows (capped at {SAMPLE_ROW_CAP}): {rows}")
    if missing:
        print(f"WARN: expected dimensions not matched by alias: {missing}")
        print("      -> inspect header and extend the alias map; country files differ.")
    if rows == 0:
        print("FAIL: no data rows in sample"); return 1
    print("PASS: file has a header and at least one data row (structure smoke-test).")
    return 0

if __name__ == "__main__":
    if len(sys.argv) != 2:
        print(__doc__); sys.exit(2)
    sys.exit(main(sys.argv[1]))
```

Data-quality checks a reviewer should add once column headers are confirmed: numeric `amount`
parses as a number; `year` within plausible coverage (e.g. 2006–present); no negative budget
values except where the classification legitimately allows them; executed <= revised where both
exist (budget-credibility sanity); and a PII scan to re-confirm the file contains only aggregate
public-finance data.

---

## Limitations & gaps

- **Licence is non-commercial (NC).** This is the central flag: BOOST catalog entries are
  CC BY-NC 4.0, not the CC-BY-4.0 the task assumed. Reuse rights are therefore narrower than
  expected. `licenseVerified=false` for "unrestricted derivatives + commercial reuse."
- **No licence snapshot/hash taken.** Acceptance criteria call for an archived copy of the licence
  text + hash; that artifact was not produced in this documentation-only pass and should be added
  at the gate.
- **No data sample was read.** Per the documentation-only guardrail, no BOOST file was downloaded.
  Consequently, **exact column names, units, code lists, and null handling are unverified** and are
  marked *Unverified* in the data dictionary. They vary per country.
- **Single-entry verification.** Licence was confirmed on a representative BOOST catalog entry
  (Liberia) plus the general World Bank dataset terms; not every country entry was individually
  checked. Some entries could carry different terms.
- **PII.** BOOST is aggregate public-finance data and is not expected to contain personal data;
  this was not file-verified and must be re-confirmed against any sampled file before use.
- **Croissant field stubs** are illustrative harmonized dimensions, not verified headers.
