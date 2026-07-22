# Datasheet — Eurostat Transport Statistics

**One-line summary:** Documentation datasheet for Eurostat's "Transport statistics" collection — comparable EU indicators on freight volumes, passenger numbers, vehicles, infrastructure and safety across six transport modes.

> **This datasheet's own licence:** Creative Commons Attribution 4.0 International (**CC-BY-4.0**).
> This is documentation *about* the dataset. No part of the underlying dataset is hosted, mirrored, transformed, or republished here.

---

## Provenance & attribution

| Field | Value |
|---|---|
| Publisher / creator | Eurostat — Statistical Office of the European Union |
| Source URL | https://ec.europa.eu/eurostat/web/transport |
| Catalog id (Hee-Lee Oss) | cat-transport-005 |
| Retrieval date | 2026-06-28 |
| Domain | Public data · Transport |

**Required attribution string (use when reusing Eurostat data):**

> Source: Eurostat (dataset code / DOI), accessed 2026-06-28.

For publications, Eurostat's recommended longer form:

> European Commission, Eurostat, *[Title]*, Publications Office of the European Union, [year], [DOI link].

Per the licence, any modified/derived version must clearly indicate the changes made and carry a disclaimer that Eurostat bears no responsibility for the modified version.

---

## Source licence (verified)

| Item | Value |
|---|---|
| Licence | **Creative Commons Attribution 4.0 International (CC BY 4.0)** |
| Licence URL | https://creativecommons.org/licenses/by/4.0/ |
| Legal basis | Commission Decision of 12 December 2011 on the reuse of Commission documents (**2011/833/EU**) |
| Citation page | Eurostat copyright notice and free re-use of data: https://ec.europa.eu/eurostat/web/main/help/copyright-notice |
| Permits derivatives? | **Yes** |
| Permits commercial reuse? | **Yes** (with source acknowledgement) |

**Cited clause confirming reuse/derivatives are permitted** (Eurostat copyright notice):

> "Reuse of statistical data, metadata, publications and other dissemination tools published on this website for commercial or non-commercial purposes is authorised provided the source is acknowledged."

CC BY 4.0 explicitly grants the right to "remix, transform, and build upon the material for any purpose, even commercially," subject to attribution — i.e. derivatives are permitted. **Verified: the source licence permits derivatives.**

**Known exceptions (scope caveat):** Eurostat notes that the general permission does *not* extend to certain third-party-sourced content — notably some data on non-EU countries (e.g. USA, Japan, China), certain detailed trade-data classifications, and co-publications produced with other organisations. Before reusing a *specific* sub-table, confirm it is not flagged as a restricted exception on its dataset page.

---

## Data dictionary

Eurostat disseminates these tables through a common dissemination structure (SDMX / TSV / JSON via the dissemination API). Individual dataset codes vary by mode and topic; the fields below describe the shared structure. Mode classification codes are **verified** from the Eurostat transport documentation; the standardised SDMX dimension/attribute names are Eurostat's documented dissemination convention but were **not enumerated field-by-field on the fetched overview pages** — rows are marked accordingly. No personal or identifying fields exist in this aggregate statistical collection.

| Field | Type | Units | Description | Allowed values / nulls | Verified? |
|---|---|---|---|---|---|
| `geo` | code (string) | — | Reporting geography | EU aggregates + country ISO-style codes (e.g. `EU27_2020`, `DE`, `FR`) | Standard Eurostat dimension — not enumerated on fetched pages |
| `TIME_PERIOD` | code/date | year (and sometimes quarter/month) | Reference period of the observation | e.g. `2024`, `2024-Q1` | Standard Eurostat dimension — not enumerated on fetched pages |
| `freq` | code | — | Frequency of observation | `A` annual, `Q` quarterly, `M` monthly | Standard Eurostat dimension — not enumerated on fetched pages |
| `unit` | code | varies | Measurement unit | e.g. tonne-kilometre, passenger-kilometre, number, thousand tonnes | Standard Eurostat dimension — units not confirmed per-table on fetched pages |
| transport mode (e.g. `tra_mode`) | code | — | Transport mode / sub-domain prefix | `rail`, `road`, `iww` (inland waterways), `pipe` (oil pipeline), `mar` (maritime), `avia` (air), `tran` (multimodal) | **Mode codes verified** from Eurostat transport docs; exact dimension column name not confirmed |
| `OBS_VALUE` | numeric (float/int) | per `unit` | The statistical value of the observation | Numeric; may be null/empty when not available | Standard Eurostat dissemination field — not enumerated on fetched pages |
| `OBS_FLAG` / status | code (string) | — | Observation status / quality flag | e.g. `p` provisional, `e` estimated, `b` break in series, `c` confidential, `:` not available | Standard Eurostat flagging convention — exact set not confirmed on fetched pages |

**Units note (unverified at table level):** Freight volumes are typically expressed in tonne-kilometres (TKM) or thousand tonnes, and passenger volumes in passenger-kilometres (PKM); these standard units were *not* explicitly confirmed on the pages fetched and should be checked against the specific dataset's metadata before use.

---

## Datasheet for Datasets

**Motivation.** Created by Eurostat to provide comparable, harmonised statistics on transport across EU Member States (and selected partners), covering freight and passenger movement, vehicles, infrastructure, safety, modal split and intermodal freight. It supports EU transport, environment and mobility policy and public/academic analysis.

**Composition.** Aggregate statistical indicators (counts, volumes, ratios) broken down by geography, time, transport mode and unit. Covers six modes: air, inland waterways, maritime, oil pipelines, rail, road, plus multimodal aggregates. **No microdata and no personal/identifying data** — instances are aggregate observations, not individuals.

**Collection process.** Compiled by Eurostat from data reported by national statistical authorities of the Member States (and partner countries), under EU statistical regulations and harmonised methodologies (see Eurostat's transport methodology and the "Glossary for Transport Statistics, 5th edition, 2019"). Exact per-table collection details were not fully verified here.

**Preprocessing / cleaning.** Eurostat harmonises, validates and aggregates national submissions; observations carry status flags (provisional, estimated, break in series, confidential, not available). Specific cleaning steps per table were not verified from the fetched pages.

**Uses.** Policy monitoring, modal-split and decarbonisation analysis, infrastructure planning, research, journalism, and dashboards. Suitable for derivative analytical works under CC BY 4.0. Watch the third-party/non-EU exceptions above.

**Distribution.** Published openly via the Eurostat website and dissemination API (TSV, SDMX, JSON) under CC BY 4.0. This datasheet does not redistribute the data; users should fetch it directly from Eurostat.

**Maintenance.** Maintained by Eurostat with regular release cycles (frequency varies by dataset: annual, quarterly, monthly). Dataset codes, DOIs and "last update" timestamps are shown on each dataset's page in the Eurostat database.

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
  "name": "Eurostat Transport Statistics",
  "description": "Comparable EU statistics on transport: freight volumes, passenger numbers, vehicles, infrastructure, safety and modal split across air, inland waterways, maritime, oil pipeline, rail and road transport.",
  "url": "https://ec.europa.eu/eurostat/web/transport",
  "license": "https://creativecommons.org/licenses/by/4.0/",
  "creator": {
    "@type": "Organization",
    "name": "Eurostat — Statistical Office of the European Union",
    "url": "https://ec.europa.eu/eurostat"
  },
  "datePublished": "2026-06-28",
  "keywords": ["transport", "freight", "passengers", "European Union", "modal split", "Eurostat"],
  "creditText": "Source: Eurostat, accessed 2026-06-28.",
  "recordSet": [
    {
      "@type": "cr:RecordSet",
      "name": "transport_observations",
      "description": "Aggregate transport observations by geography, period, mode and unit.",
      "field": [
        { "@type": "cr:Field", "name": "geo", "description": "Reporting geography code", "dataType": "sc:Text" },
        { "@type": "cr:Field", "name": "TIME_PERIOD", "description": "Reference period", "dataType": "sc:Text" },
        { "@type": "cr:Field", "name": "tra_mode", "description": "Transport mode (rail/road/iww/pipe/mar/avia/tran)", "dataType": "sc:Text" },
        { "@type": "cr:Field", "name": "unit", "description": "Measurement unit (e.g. TKM, PKM, number)", "dataType": "sc:Text" },
        { "@type": "cr:Field", "name": "OBS_VALUE", "description": "Observation value", "dataType": "sc:Float" },
        { "@type": "cr:Field", "name": "OBS_FLAG", "description": "Observation status flag", "dataType": "sc:Text" }
      ]
    }
  ]
}
```

> Note: field stubs reflect Eurostat's standard dissemination structure and the verified mode codes; validate exact dimension names against the specific dataset's SDMX structure before relying on this record.

---

## Validation script

This script checks the *shape* of a Eurostat transport extract that the user has **already downloaded themselves** from Eurostat. It does **not** download, host, or transform the dataset — it only validates a local file's structure and reports basic data-quality metrics.

```python
#!/usr/bin/env python3
"""Validate the structure of a locally-held Eurostat transport TSV/CSV extract.
Documentation-only: does NOT download or republish any data.
Usage: python validate_transport.py path/to/local_extract.tsv
"""
import sys
import csv

# Dimensions we expect to find (case-insensitive). Eurostat TSV often packs
# several dimensions into the first column header, e.g. "freq,unit,tra_mode,geo\\TIME_PERIOD".
EXPECTED_DIMS = {"geo", "time_period", "unit"}
KNOWN_MODES = {"rail", "road", "iww", "pipe", "mar", "avia", "tran"}

def main(path: str) -> int:
    with open(path, newline="", encoding="utf-8") as f:
        # Eurostat bulk download is tab-separated.
        reader = csv.reader(f, delimiter="\t")
        header = next(reader, None)
        if not header:
            print("FAIL: file is empty")
            return 1

        flat = ",".join(header).lower()
        found = {d for d in EXPECTED_DIMS if d in flat}
        missing = EXPECTED_DIMS - found
        print(f"Header: {header[0][:80]}...")
        print(f"Expected dimensions found: {sorted(found)}")
        if missing:
            print(f"WARN: dimensions not detected in header: {sorted(missing)}")

        rows = 0
        empty_cells = 0
        total_cells = 0
        for row in reader:
            rows += 1
            for cell in row[1:]:
                total_cells += 1
                c = cell.strip()
                if c == "" or c == ":":  # ':' = Eurostat 'not available'
                    empty_cells += 1
        print(f"Data rows: {rows}")
        if rows == 0:
            print("FAIL: no data rows")
            return 1
        if total_cells:
            pct = 100.0 * empty_cells / total_cells
            print(f"Data-quality: {empty_cells}/{total_cells} value cells empty or ':' ({pct:.1f}%)")
        print("PASS: structural check complete")
        return 0

if __name__ == "__main__":
    if len(sys.argv) != 2:
        print("Usage: python validate_transport.py <local_extract.tsv>")
        sys.exit(2)
    sys.exit(main(sys.argv[1]))
```

Expected, on a valid extract: a non-empty header containing `geo` / `TIME_PERIOD` / `unit`, at least one data row, and a reported share of missing (`:`) cells as a data-quality signal.

---

## Limitations & gaps

- **Licence: verified.** CC BY 4.0 (Decision 2011/833/EU), confirmed to permit derivatives and commercial reuse with attribution. Note the documented third-party / non-EU-country / detailed-trade exceptions — confirm per specific sub-table.
- **Field names partly unverified.** "Transport statistics" is a *collection* of many datasets, each with its own SDMX structure and dataset code. The overview/intro pages fetched did not enumerate exact column names. Standard dimensions (`geo`, `TIME_PERIOD`, `freq`, `unit`, `OBS_VALUE`, `OBS_FLAG`) reflect Eurostat's documented dissemination convention but should be checked against the specific dataset before use. **Verified directly:** the transport mode classification codes (`rail`, `road`, `iww`, `pipe`, `mar`, `avia`, `tran`).
- **Units unverified per table.** TKM (freight) and PKM (passengers) are typical but were not explicitly confirmed on the fetched pages.
- **No data accessed.** Per guardrails, no rows were downloaded or sampled; this datasheet is documentation only. Field/unit specifics should be finalised against the chosen dataset's SDMX metadata and "last update" stamp.
- **No PII.** This is an aggregate statistical collection; no personal or identifying data is present or documented.
