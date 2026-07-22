# Datasheet — CDC WONDER (mortality, natality, and related public-health statistics)

**One-line summary:** A documentation datasheet for CDC WONDER, the US CDC's online query system for de-identified, aggregated, small-count-suppressed public-health statistics (e.g. Underlying Cause of Death, Multiple Cause of Death, Natality).

*This datasheet (the document you are reading) is licensed under **CC-BY-4.0**. It is documentation only: it does not host, mirror, transform, or republish any CDC WONDER data.*

---

## Provenance & attribution

| Field | Value |
|---|---|
| **Publisher / producer** | US Centers for Disease Control and Prevention (CDC), National Center for Health Statistics (NCHS), and partner data providers, via the CDC WONDER system. |
| **Source URL** | https://wonder.cdc.gov/ |
| **Catalog id (Hee-Lee Oss)** | cat-health-001 |
| **Retrieval date** | 2026-06-28 |
| **Access method** | Public web query interface and the CDC WONDER API (`https://wonder.cdc.gov/controller/datarequest/<DB>`). Results are exportable as tab-delimited text. No data was downloaded, sampled, or stored to produce this datasheet — documentation was derived from CDC's published help/terms pages only. |

**Required attribution string (example, for the Underlying Cause of Death database):**

> Centers for Disease Control and Prevention, National Center for Health Statistics. National Vital Statistics System, Mortality 1999–2020 on CDC WONDER Online Database. Accessed at https://wonder.cdc.gov/ucd-icd10.html, [access date]. Powered by CDC WONDER.

CDC asks that you (a) credit "CDC WONDER" / "powered by CDC WONDER", (b) include the official **Suggested Citation** packaged with each result set, and (c) retain the footnotes/explanatory notes that accompany the results.

---

## Source licence

**Verified licence: US Government work — Public Domain (no copyright), subject to a Data Use Agreement/Restrictions.**

- The CDC WONDER site and the public-use data it serves are in the **public domain**: users "may access the information freely and use, copy, distribute or publish this information without additional or explicit permission." This permits **derivatives and redistribution**, with attribution requested (not legally required for the public-domain content, but expected as good practice). See CDC's *Use of Agency Materials* (https://www.cdc.gov/other/agencymaterials.html) and the CDC WONDER FAQ (https://wonder.cdc.gov/wonder/help/faq.html).
- Use is governed by the **CDC WONDER Data Use Restrictions** (https://wonder.cdc.gov/datause.html). Key binding conditions confirmed from CDC's published terms:
  - **No re-identification:** "Make no attempt to learn the identity of any person or establishment included in these data." Do not link the data with other datasets to identify an individual.
  - **Small-count suppression must be preserved:** "Do not present or publish statistics representing nine or fewer births or deaths... in figures, graphs, maps, tables, etc." Counts/rates representing 1–9 persons are returned as **"Suppressed"** by the publisher.
  - Retain the accompanying footnotes/explanatory notes when reusing results.

**Conclusion:** The source permits derivatives (public-domain content), so this file is published as a normal (non-DRAFT) datasheet. The above re-identification and suppression conditions are reproduction/use restrictions on *behavior*, not copyright restrictions on derivatives; downstream users MUST honour them.

*Licence snapshot note:* per acceptance criteria a hash + archived copy of the live `datause.html` / `agencymaterials.html` text should be attached at the gate. CDC WONDER help pages returned HTTP 403 to the automated fetcher used here; the licence facts above were verified via CDC's indexed FAQ/terms content and the *Use of Agency Materials* page. The archived-text hash artifact is therefore flagged as **to be captured at the human gate** (see Limitations).

---

## Data dictionary

CDC WONDER is not a single flat file — it is a family of queryable databases (Underlying Cause of Death, Multiple Cause of Death, Natality, Cancer, etc.). The **exported table is shaped by the user's query**: a `Notes` column, one column per selected **group-by dimension** (often paired with a `<Dimension> Code` column), followed by measure columns. The table below documents the canonical **Underlying Cause of Death (UCD), 1999–2020** export — the most-used database — as a representative example.

| Field | Type | Units | Description | Allowed values / null handling |
|---|---|---|---|---|
| Notes | string | — | Per-row annotations (e.g. "Total", suppression/unreliable flags). | Free text; usually empty for data rows. |
| State *(or County / Census Region / etc.)* | string | — | Geographic group-by dimension (one of several selectable). | US state names; varies by query. |
| State Code | string/int code | — | FIPS code for the geography. | FIPS codes (e.g. "01"). |
| Year *(or Month / Year)* | integer | calendar year | Temporal group-by dimension. | e.g. 1999–2020 for this DB. |
| Age Group | string (binned) | — | Ten-year / single-year age bands when selected. | e.g. "25-34 years"; "Not Stated" possible. |
| Gender | categorical | — | Sex group-by. | "Female", "Male". |
| Race | categorical | — | Race group-by (bridged or single-race depending on DB version). | e.g. "White", "Black or African American", etc. |
| Hispanic Origin | categorical | — | Ethnicity group-by. | "Hispanic or Latino", "Not Hispanic or Latino", "Not Stated". |
| ICD-10 Cause (e.g. "ICD Chapter" / "Cause of death") | string | — | Underlying cause grouping. | ICD-10 categories. |
| ICD-10 Code | string code | — | ICD-10 code(s) for the cause. | ICD-10 codes. |
| **Deaths** | integer (measure) | count | Number of deaths matching the row. | Integer; **"Suppressed"** when 1–9; "Missing" when unavailable. |
| **Population** | integer (measure) | persons | Denominator population. | Integer; "Not Applicable"/"Suppressed" in some cuts. |
| **Crude Rate** | decimal (measure) | deaths per 100,000 | Deaths ÷ population × 100,000. | Decimal; "Suppressed", "Unreliable" (<20 deaths), or "Not Applicable". |
| **Age Adjusted Rate** | decimal (measure) | deaths per 100,000 | Rate standardized to the 2000 US standard population. | Decimal; "Suppressed"/"Unreliable"/"Not Applicable". |
| Crude Rate Standard Error / Lower/Upper 95% CI | decimal | per 100,000 | Uncertainty for crude rate (when requested). | Decimal; suppressed with the rate. |
| Age Adjusted Rate Standard Error / CI | decimal | per 100,000 | Uncertainty for age-adjusted rate (when requested). | Decimal; suppressed with the rate. |

**Verification status:** The measure columns (Deaths, Population, Crude Rate, Age Adjusted Rate) and the `Notes` + `<Dimension>` / `<Dimension> Code` export pattern, plus the suppression sentinels ("Suppressed", "Unreliable", "Not Applicable"), are **verified** from CDC WONDER documentation and the published terms. The specific group-by dimension *rows* above (State, Year, Age Group, Gender, Race, Hispanic Origin, ICD-10) are **representative/configurable** — the exact set and column order depend on the user's query selection and on which database is used. Treat dimension rows as *example-verified, query-dependent* rather than a fixed schema. The Natality and other databases share this shape but expose different dimensions and measures (e.g. Births, birth weight, gestational age) — **not individually verified here** and marked as such.

---

## Datasheet for Datasets

**Motivation.** Created so researchers, journalists, and public-health practitioners can query core US vital-statistics and surveillance data (mortality, natality, cancer, etc.) at the population level without handling restricted record-level files. Maintained by CDC/NCHS for public-health monitoring and research.

**Composition.** De-identified, **aggregated** counts and rates derived from administrative/vital records (e.g. death certificates, birth certificates) and surveillance systems. Instances are aggregate cells defined by the chosen dimensions, not individual people. Small cells (1–9) are suppressed by the publisher, so the released data is intended to contain **no personally identifying information**.

**Collection process.** Underlying record-level data are collected by states/jurisdictions and compiled by NCHS through systems such as the National Vital Statistics System; WONDER exposes aggregated views with provider-defined suppression and footnotes.

**Preprocessing / cleaning / labeling.** ICD-10 coding of causes, age-group binning, bridged/single-race classification, population denominators from Census/NCHS estimates, age-adjustment to the 2000 US standard population, and small-count suppression are applied by the publisher.

**Uses.** Epidemiologic research, surveillance, rate calculation, geographic/temporal trend analysis. **Not** suitable for re-identification, for inferring individual-level outcomes, or for publishing cells representing ≤9 events.

**Distribution.** Distributed only via the CDC WONDER web UI and API as on-demand query exports (tab-delimited). Public domain content; reuse permitted under the Data Use Restrictions. This datasheet does not redistribute any data.

**Maintenance.** Maintained by CDC/NCHS; databases are versioned by year range and periodically refreshed (e.g. provisional vs final mortality files). Contact and updates via https://wonder.cdc.gov/.

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
  "name": "CDC WONDER",
  "description": "CDC WONDER is the US Centers for Disease Control and Prevention's online query system providing de-identified, aggregated, small-count-suppressed public-health statistics, including Underlying Cause of Death, Multiple Cause of Death, and Natality databases. Data are exported as on-demand tab-delimited query results.",
  "url": "https://wonder.cdc.gov/",
  "sameAs": "https://wonder.cdc.gov/ucd-icd10.html",
  "license": "https://www.cdc.gov/other/agencymaterials.html",
  "creditText": "Centers for Disease Control and Prevention, National Center for Health Statistics, via CDC WONDER. Powered by CDC WONDER.",
  "isAccessibleForFree": true,
  "creator": {
    "@type": "GovernmentOrganization",
    "name": "Centers for Disease Control and Prevention, National Center for Health Statistics",
    "url": "https://www.cdc.gov/nchs/"
  },
  "publisher": {
    "@type": "GovernmentOrganization",
    "name": "Centers for Disease Control and Prevention"
  },
  "datePublished": "2026-06-28",
  "keywords": ["mortality", "natality", "public health", "vital statistics", "ICD-10", "epidemiology"],
  "distribution": [
    {
      "@type": "cr:FileObject",
      "@id": "ucd-export",
      "name": "ucd-tab-delimited-export",
      "description": "Per-query tab-delimited export from the Underlying Cause of Death database (generated on demand; not redistributed here).",
      "encodingFormat": "text/tab-separated-values"
    }
  ],
  "recordSet": [
    {
      "@type": "cr:RecordSet",
      "@id": "ucd-record",
      "name": "ucd-record",
      "field": [
        { "@type": "cr:Field", "@id": "ucd-record/deaths", "name": "Deaths", "description": "Number of deaths for the cell; 'Suppressed' when 1-9.", "dataType": "Integer" },
        { "@type": "cr:Field", "@id": "ucd-record/population", "name": "Population", "description": "Denominator population for the cell.", "dataType": "Integer" },
        { "@type": "cr:Field", "@id": "ucd-record/crude_rate", "name": "Crude Rate", "description": "Deaths per 100,000 population.", "dataType": "Float" },
        { "@type": "cr:Field", "@id": "ucd-record/age_adjusted_rate", "name": "Age Adjusted Rate", "description": "Age-adjusted deaths per 100,000 (2000 US standard population).", "dataType": "Float" }
      ]
    }
  ]
}
```

---

## Validation script

Documentation only: this script validates the **structure** of a CDC WONDER export that a user has obtained themselves. It does **not** download, host, or republish any data. Point it at a local file you exported from CDC WONDER.

```python
#!/usr/bin/env python3
"""Validate the structure of a locally-exported CDC WONDER tab-delimited file.
Does NOT download or host data. Usage: python validate.py path/to/export.txt"""
import sys, csv

MEASURE_COLS = {"Deaths", "Population", "Crude Rate", "Age Adjusted Rate"}
SUPPRESSION_TOKENS = {"Suppressed", "Unreliable", "Not Applicable", "Missing"}

def main(path: str) -> int:
    with open(path, newline="", encoding="utf-8") as f:
        reader = csv.reader(f, delimiter="\t")
        rows = [r for r in reader if r and any(c.strip() for c in r)]
    if not rows:
        print("FAIL: file is empty"); return 1
    header = [h.strip() for h in rows[0]]
    print(f"Columns ({len(header)}): {header}")

    # 1) Expect a 'Notes' column and at least one known measure column.
    assert "Notes" in header, "missing 'Notes' column"
    present_measures = MEASURE_COLS.intersection(header)
    assert present_measures, f"expected one of {MEASURE_COLS} in header"
    print(f"Measure columns present: {sorted(present_measures)}")

    # 2) Row-count / width sanity + suppression check on Deaths.
    data = rows[1:]
    print(f"Data rows: {len(data)}")
    bad_width = [i for i, r in enumerate(data, 2) if len(r) != len(header)]
    assert not bad_width, f"ragged rows at lines: {bad_width[:5]}"

    if "Deaths" in header:
        di = header.index("Deaths")
        for i, r in enumerate(data, 2):
            v = r[di].strip()
            if v and v not in SUPPRESSION_TOKENS and not v.replace(",", "").isdigit():
                print(f"WARN line {i}: unexpected Deaths value {v!r}")
            # Guardrail: small counts must remain suppressed by the publisher.
            if v.replace(",", "").isdigit() and 1 <= int(v.replace(",", "")) <= 9:
                print(f"WARN line {i}: count {v} should be 'Suppressed' (<=9)")
    print("OK: structure validation passed")
    return 0

if __name__ == "__main__":
    if len(sys.argv) != 2:
        print(__doc__); sys.exit(2)
    sys.exit(main(sys.argv[1]))
```

---

## Limitations & gaps

- **Not verified by live fetch:** CDC WONDER help pages (`datause.html`, `ucd.html`, etc.) returned HTTP 403 to the automated fetcher. Licence and terms facts were verified via CDC's indexed FAQ/terms content and the *Use of Agency Materials* page rather than a direct fetch of `datause.html`. A human reviewer should attach an **archived copy + hash** of the live licence text at the gate (acceptance criterion).
- **Schema is query-dependent:** CDC WONDER has no fixed export schema; the data dictionary documents the canonical Underlying Cause of Death export. Exact group-by columns, ordering, and available measures differ per database (Natality, Cancer, MCD, etc.) and per user query. Non-UCD databases were not individually field-verified.
- **No data inspected:** No file was downloaded or sampled (documentation-only deed), so allowed-value lists are derived from CDC documentation, not from an observed sample. Treat dimension value lists as indicative.
- **PII:** Released WONDER data is aggregated and small-count-suppressed by the publisher, so it should contain no PII; the validation script includes a defensive suppression check. Re-identification and small-cell publication are prohibited by the Data Use Restrictions and must be honoured downstream.
