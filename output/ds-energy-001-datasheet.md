# Datasheet: EIA Energy Data (Open Data API)

> One-line summary: A documentation datasheet for the U.S. Energy Information Administration (EIA) Open Data API — comprehensive U.S. energy production, consumption, price, and emissions time series exposed as JSON via a versioned REST API.

**Datasheet licence:** This datasheet (the document you are reading) is released under **CC-BY-4.0**. It does **not** host, mirror, or republish any EIA data; it documents the dataset only.

---

## Provenance & attribution

| Field | Value |
| --- | --- |
| Publisher / producer | U.S. Energy Information Administration (EIA), an independent statistics and analysis agency within the U.S. Department of Energy |
| Dataset | EIA Open Data API (v1 and v2) — energy time-series catalog |
| Source URL | https://www.eia.gov/opendata/ |
| API documentation | https://www.eia.gov/opendata/documentation.php |
| Catalog id (Hee-Lee Oss) | cat-energy-001 |
| Retrieval date | 2026-06-28 |
| Required attribution string | `Source: U.S. Energy Information Administration (Jun 2026).` (per EIA Copyrights and Reuse Policy — include the publication/retrieval date) |

How the data is produced (provenance): EIA collects data primarily through its own surveys and forms; some series are pulled from external sources on a recurring basis (e.g., nuclear plant generator outages pulled daily from the U.S. Nuclear Regulatory Commission). Data is organized into time-series datasets grouped by energy category: Electricity, Natural Gas, Petroleum, Coal, Densified Biomass, Nuclear Outages, Outlooks/Projections (STEO, AEO, IEO), Total Energy, State Energy Data System (SEDS), CO2 Emissions, and International Energy.

---

## Source licence

**Verified licence:** U.S. Government **public domain** (work of the U.S. federal government; not subject to copyright protection under 17 U.S.C. § 105), governed operationally by the **EIA Copyrights and Reuse Policy**.

**Citation / clause confirming derivatives & redistribution are permitted:**

- Policy URL: https://www.eia.gov/about/copyrights_reuse.php (retrieved 2026-06-28).
- Verbatim clause (public-domain status): "U.S. government publications are in the public domain and are not subject to copyright protection."
- Verbatim clause (reuse and redistribution): "You may use and/or distribute any of our data, files, databases, reports, graphs, charts, and other information products."
- Attribution clause: "you should use an acknowledgment, which includes the publication date, such as: 'Source: U.S. Energy Information Administration (Oct 2008).'"

**Assessment:** Public-domain status combined with the explicit "use and/or distribute" grant means **derivatives and redistribution are permitted**. Attribution is requested (not legally required for public-domain works, but expected as good practice). `licenseVerified = true`.

**Caveat (recorded, not blocking):** The policy notes that "Materials contributed by third parties may be protected by copyright and require separate permission." A small number of series ingested from external providers may carry third-party terms; users redistributing specific series should confirm the individual series source. Also note that **API access** requires a free API key and agreement to the API Terms of Service (https://www.eia.gov/opendata/terms-of-service.php) — this governs API usage/rate-limiting, not the copyright status of the data itself.

License snapshot note: For the per-dataset gate artifact, archive the rendered text of `copyrights_reuse.php` and record its content hash at retrieval time. (This datasheet records the clauses above verbatim; the archived-copy + hash step is to be attached by the gate reviewer and is not committed here, per the documentation-only guardrail.)

---

## Data dictionary

The EIA API v2 returns two layers: **dataset/route metadata** and **data rows**. Field names below are taken from the EIA API v2 documentation (retrieved 2026-06-28). Because the API is a catalog of many heterogeneous datasets, the *measure* columns differ per dataset; the example shown is the electricity retail-sales dataset.

### Response envelope (top-level)

| Field | Type | Units | Description | Allowed values / nulls |
| --- | --- | --- | --- | --- |
| `apiVersion` | string | — | API version identifier | e.g. "2.x.x" |
| `request` | object | — | Echo of the submitted query parameters | non-null |
| `response` | object | — | Container holding `total`, `dateFormat`, and `data` | non-null |
| `total` | integer/string | rows | Count of rows matching the query | >= 0 |
| `dateFormat` | string | — | Actual date format of returned periods | e.g. "YYYY-MM" |
| `data` | array | — | Array of data row objects | may be empty |
| `warning` / `description` | string | — | Diagnostic messages (e.g., row-limit warnings) | nullable |

### Dataset/route metadata (returned when querying a route)

| Field | Type | Units | Description | Allowed values / nulls |
| --- | --- | --- | --- | --- |
| `id` | string | — | Dataset identifier | non-null |
| `name` | string | — | Human-readable dataset title | non-null |
| `description` | string | — | Detailed dataset description | nullable |
| `frequency` | array of objects | — | Available periodicities (`id`, `description`, `query`, `format`) | non-null |
| `facets` | array of objects | — | Filterable dimensions (`id`, `description`) | may be empty |
| `data` | object | varies | Available measure columns, each with `alias` and `units` | non-null |
| `startPeriod` / `endPeriod` | string | — | Range of available data | non-null |
| `defaultDateFormat` | string | — | Assumed date pattern | non-null |
| `routes` | array | — | Child datasets in the catalog hierarchy | may be empty |

### Data row fields (example: electricity retail sales `electricity/retail-sales`)

| Field | Type | Units | Description | Allowed values / nulls |
| --- | --- | --- | --- | --- |
| `period` | string | date | Time period of observation | format per `dateFormat`, e.g. "2001-01" |
| `stateid` | string | — | Geographic identifier | 2-letter code, e.g. "CO"; also region codes |
| `stateDescription` | string | — | Human-readable location name | e.g. "Colorado" |
| `sectorid` | string | — | Sector classification code | e.g. "RES", "COM", "IND", "ALL" |
| `sectorName` | string | — | Sector description | e.g. "residential" |
| `price` | string (numeric) | cents per kilowatthour | Measure value (varies by dataset) | nullable when not reported |
| `price-units` | string | — | Units string for the `price` column | e.g. "cents per kilowatthour" |

> Note on measure columns: each dataset exposes its own measures (e.g. `price`, `revenue`, `sales`, `customers` for electricity; other datasets expose different aliases). The accompanying `<column>-units` field always carries the unit string for that measure. **Unverified per-dataset specifics** beyond the electricity example above should be confirmed against each route's metadata response before relying on them.

**Marked unverified:** Exact null-encoding semantics per series (empty string vs. omitted key vs. `null`) were not exhaustively verified across all datasets; the value columns are returned as JSON strings in the documented examples rather than native numbers.

---

## Datasheet for Datasets

**Motivation.** The data is collected by EIA to provide free, authoritative, and timely statistics on U.S. (and international) energy production, consumption, prices, stocks, and emissions, supporting public policy, markets, research, and transparency. The Open Data API exists to make these series programmatically accessible.

**Composition.** The catalog comprises many time-series datasets across energy categories (electricity, natural gas, petroleum, coal, biomass, nuclear outages, total energy, SEDS, CO2 emissions, international, and outlook/projection products such as STEO/AEO/IEO). Each "instance" is an observation keyed by period plus dimension facets (geography, sector, fuel, etc.) with one or more measure values. No personal or identifying data about individuals is present; records describe aggregate energy quantities, prices, and facility/region-level statistics.

**Collection process.** Data is gathered primarily via EIA's mandatory and voluntary surveys/forms submitted by energy producers, utilities, and other respondents; some series are ingested from external agencies (e.g., NRC for nuclear outages) on recurring schedules. Frequencies range from daily to annual depending on the series.

**Preprocessing / cleaning.** EIA performs its own validation, estimation, and aggregation as part of its statistical programs (methodologies are published per survey on eia.gov). The API serves the published series; projection products (STEO/AEO/IEO) are modeled estimates rather than observations.

**Uses.** Suitable for energy market analysis, research, policy work, dashboards, and education. Users should distinguish historical/observed series from projections, and check per-series methodology and revision notes. Not a source of personal data; not suitable for inferring individual behavior.

**Distribution.** Distributed by EIA at no charge via the Open Data API (v1/v2), bulk ZIP downloads, an Excel add-in, and embeddable widgets. API access requires a free API key and acceptance of the API Terms of Service.

**Maintenance.** Maintained by EIA; series are updated on their published release schedules and may be revised. The API is versioned (v1 legacy and v2 current); v1 series IDs map to v2 routes/facets. Contact and support are provided through eia.gov/opendata.

---

## Croissant metadata

```json
{
  "@context": {
    "@vocab": "https://schema.org/",
    "cr": "http://mlcommons.org/croissant/"
  },
  "@type": "Dataset",
  "name": "EIA Open Data API",
  "description": "U.S. Energy Information Administration Open Data API: comprehensive U.S. and international energy time series (production, consumption, prices, stocks, CO2 emissions, and projections) served as versioned JSON.",
  "url": "https://www.eia.gov/opendata/",
  "sameAs": "https://www.eia.gov/opendata/documentation.php",
  "license": "https://www.eia.gov/about/copyrights_reuse.php",
  "creditText": "Source: U.S. Energy Information Administration (Jun 2026).",
  "isAccessibleForFree": true,
  "creator": {
    "@type": "GovernmentOrganization",
    "name": "U.S. Energy Information Administration",
    "url": "https://www.eia.gov/"
  },
  "datePublished": "2026-06-28",
  "distribution": [
    {
      "@type": "DataDownload",
      "encodingFormat": "application/json",
      "contentUrl": "https://api.eia.gov/v2/"
    }
  ],
  "recordSet": [
    {
      "@type": "cr:RecordSet",
      "name": "electricity_retail_sales",
      "description": "Example record set: electricity retail sales by state and sector.",
      "field": [
        { "@type": "cr:Field", "name": "period", "dataType": "sc:Date", "description": "Observation time period." },
        { "@type": "cr:Field", "name": "stateid", "dataType": "sc:Text", "description": "Geographic (state/region) identifier." },
        { "@type": "cr:Field", "name": "sectorid", "dataType": "sc:Text", "description": "Sector classification code (RES, COM, IND, ALL)." },
        { "@type": "cr:Field", "name": "price", "dataType": "sc:Float", "description": "Measure value; units carried in price-units (cents per kilowatthour)." }
      ]
    }
  ]
}
```

> Note: This is a minimal, illustrative Croissant/JSON-LD stub for the electricity retail-sales route. A full Croissant record would enumerate one RecordSet per EIA route with the route's actual measure fields and facets.

---

## Validation script

This script checks the **expected structure** of an EIA API v2 response that a user has *already* obtained themselves (e.g., a local file the user fetched with their own API key). It does **not** download, host, or republish any data — it only validates shape. Supply your own local JSON file path.

```python
#!/usr/bin/env python3
"""Validate the structure of a locally-saved EIA API v2 response.

Documentation-only: this script does NOT fetch, download, or host any data.
Point it at a JSON file you obtained yourself from the EIA API.

Usage:  python validate_eia.py path/to/eia_response.json
"""
import json
import sys

# Expected envelope + (example) data-row columns for electricity/retail-sales.
REQUIRED_ENVELOPE = {"response", "request", "apiVersion"}
REQUIRED_RESPONSE = {"total", "data"}
EXAMPLE_ROW_COLUMNS = {"period", "stateid", "sectorid"}  # measure columns vary by dataset

def main(path: str) -> int:
    with open(path, "r", encoding="utf-8") as fh:
        doc = json.load(fh)

    errors = []

    missing_env = REQUIRED_ENVELOPE - doc.keys()
    if missing_env:
        errors.append(f"missing envelope keys: {sorted(missing_env)}")

    resp = doc.get("response", {})
    missing_resp = REQUIRED_RESPONSE - resp.keys()
    if missing_resp:
        errors.append(f"missing response keys: {sorted(missing_resp)}")

    rows = resp.get("data", [])
    if not isinstance(rows, list):
        errors.append("response.data is not a list")
    else:
        # Row-count sanity vs. reported total (bounded, no fetching).
        total = resp.get("total")
        print(f"rows in file: {len(rows)}; reported total: {total}")
        if rows:
            missing_cols = EXAMPLE_ROW_COLUMNS - rows[0].keys()
            if missing_cols:
                errors.append(
                    f"first row missing expected columns "
                    f"(example dataset): {sorted(missing_cols)}"
                )
            # Every measure column should have a matching <col>-units key.
            for k in rows[0]:
                if not k.endswith("-units") and f"{k}-units" not in rows[0]:
                    if k not in {"period", "stateid", "stateDescription",
                                 "sectorid", "sectorName"}:
                        print(f"note: column '{k}' has no '{k}-units' sibling")

    if errors:
        print("VALIDATION FAILED:")
        for e in errors:
            print("  -", e)
        return 1
    print("VALIDATION OK: structure matches expected EIA API v2 shape.")
    return 0

if __name__ == "__main__":
    if len(sys.argv) != 2:
        print(__doc__)
        sys.exit(2)
    sys.exit(main(sys.argv[1]))
```

---

## Limitations & gaps

- **Per-dataset measure columns are heterogeneous.** Only the electricity retail-sales example was verified in detail; measure field names/units for other routes (natural gas, petroleum, coal, projections, etc.) must be read from each route's own metadata response and were **not exhaustively verified**.
- **Null encoding not fully verified.** The exact representation of missing values (empty string vs. omitted key vs. JSON `null`) and whether numeric values are returned as strings or numbers may vary by route; the documented examples return values as JSON strings.
- **License caveat for third-party series.** EIA's policy flags that third-party-contributed materials may carry separate copyright; a small subset of ingested series could differ from the public-domain default. Confirm per-series provenance before redistributing specific series.
- **API ToS vs. data licence.** Data copyright status (public domain) is distinct from the API Terms of Service (rate limits, key registration); this datasheet verifies the former and notes the latter.
- **License snapshot artifact.** Per the documentation-only guardrail, the archived copy + content hash of the licence text is left to the gate reviewer to attach; this file records the verbatim clauses and citation URL only.
- **No data was downloaded, sampled, hosted, or committed** in producing this datasheet; field structure was taken from EIA's public API documentation.
