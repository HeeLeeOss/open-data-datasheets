# Datasheet — HUD Open Data (Housing Programs)

One-line summary: A documentation datasheet for the U.S. Department of Housing and Urban
Development (HUD) GIS open-data portal, covering housing-program and geography layers used
for civic analysis (illustrated here with the **Public Housing Developments** layer).

*This datasheet (the documentation in this file) is licensed under **CC-BY-4.0**. It is
documentation only: it does not host, mirror, transform, or republish any HUD dataset.*

---

## Provenance & attribution

- **Publisher / source authority:** U.S. Department of Housing and Urban Development (HUD),
  Office of the Chief Information Officer — Enterprise GIS (eGIS).
- **Portal (source URL):** https://hudgis-hud.opendata.arcgis.com/
- **Representative layer used to verify structure:** HUD eGIS *Public Housing Developments*
  feature service (ArcGIS REST `FeatureServer/0` service metadata).
- **Retrieval date:** 2026-06-28
- **Catalog id (Hee-Lee Oss):** cat-housing-002
- **Required attribution string:**
  > Source: U.S. Department of Housing and Urban Development (HUD), Enterprise GIS Open Data
  > (https://hudgis-hud.opendata.arcgis.com/), retrieved 2026-06-28. A work of the U.S.
  > Government, not subject to domestic copyright (17 U.S.C. § 105).

---

## Source licence

- **Licence status:** **U.S. Government Work — public domain** (no domestic copyright).
- **Verified, permits derivatives:** **YES.**
- **Citation / clause:** 17 U.S.C. § 105(a):
  > "Copyright protection under this title is not available for any work of the United States
  > Government, but the United States Government is not precluded from receiving and holding
  > copyrights transferred to it by assignment, bequest, or otherwise."
  - Statute text: https://www.law.cornell.edu/uscode/text/17/105
  - Plain-language reference (U.S. government works are free to use, copy, distribute, and
    adapt): https://www.usa.gov/government-copyright
- **What this means for reuse:** Because the work is not under copyright, reuse,
  redistribution, and creation of **derivative works** are permitted without a licence grant.
  Attribution is not legally required but is recorded above as good practice. Note that HUD's
  feature-service metadata lists `copyrightText` as "U.S. Department of Housing and Urban
  Development" (an attribution/courtesy string, not a copyright assertion).
- **Caveats to honour:** Do not imply HUD endorsement of derived products; geographic point
  locations are HUD geocoder approximations, not survey-grade.

> Licence snapshot note: the operative licence is statutory (17 U.S.C. § 105), reproduced
> verbatim above; the portal itself asserts no copyright. A hash/archived copy of a single
> licence text file is therefore not meaningful here — the controlling text is the statute,
> cited and quoted above.

---

## Data dictionary

Verified from the ArcGIS REST service metadata of the **Public Housing Developments** layer
(`f=pjson`) retrieved 2026-06-28. Geometry type: point (`esriGeometryPoint`). The layer is
aggregate, **development-level** data (counts and percentages per development); it contains
**no resident-level records**. Esri field types map as: `String`→text, `Integer`/`OID`→int,
`Single`/`Double`→float, `Date`→datetime.

The layer exposes ~150 fields. The table below documents the analytically important fields;
the large block of USPS/Census geocoding fields is summarised in grouped rows and marked.
Allowed values / null handling are **inferred** unless stated, since the service metadata does
not publish per-field domains or null rules — such rows are marked *(unverified)*.

| Field | Type | Units | Description | Allowed values / nulls |
|---|---|---|---|---|
| OBJECTID | int (OID) | — | Esri internal unique row id | Not null; system-generated |
| PARTICIPANT_CODE | text(5) | — | Housing authority (PHA) code | *(unverified)* nullable |
| FORMAL_PARTICIPANT_NAME | text(60) | — | PHA formal name | *(unverified)* nullable |
| DEVELOPMENT_CODE | text(11) | — | HUD development identifier | *(unverified)* nullable |
| PROJECT_NAME | text(40) | — | Development / project name | *(unverified)* nullable |
| SCATTERED_SITE_IND | text(1) | flag | Scattered-site indicator | likely Y/N *(unverified)* |
| PD_STATUS_TYPE_CODE | text(6) | code | Development status code | coded domain *(unverified)* |
| TOTAL_UNITS | int | units | Total units | ≥0; nulls/sentinel possible *(unverified)* |
| TOTAL_DWELLING_UNITS | int | units | Total dwelling units | ≥0 *(unverified)* |
| ACC_UNITS | int | units | Units under Annual Contributions Contract | ≥0 *(unverified)* |
| TOTAL_OCCUPIED | int | units | Occupied units | ≥0 *(unverified)* |
| REGULAR_VACANT | int | units | Vacant units | ≥0 *(unverified)* |
| PHA_TOTAL_UNITS | int | units | PHA total units | ≥0 *(unverified)* |
| PCT_OCCUPIED | float | percent | Percent occupied | 0–100 *(unverified)* |
| NUMBER_REPORTED | int | count | Households reporting | ≥0 *(unverified)* |
| PCT_REPORTED | float | percent | Percent reporting | 0–100 *(unverified)* |
| MONTHS_SINCE_REPORT | int | months | Months since last report | ≥0 *(unverified)* |
| PCT_MOVEIN | float | percent | Percent recent move-ins | 0–100 *(unverified)* |
| PEOPLE_PER_UNIT | float | persons/unit | Average household size | ≥0 *(unverified)* |
| PEOPLE_TOTAL | int | persons | Total residents (aggregate) | ≥0 *(unverified)* |
| RENT_PER_MONTH | int | USD/month | Average tenant rent | ≥0 *(unverified)* |
| SPENDING_PER_MONTH | int | USD/month | Average spending per month | ≥0 *(unverified)* |
| SPENDING_PER_MONTH_PREV_YR | int | USD/month | Prior-year spending/month | ≥0 *(unverified)* |
| HH_INCOME | int | USD/year | Average household income | ≥0 *(unverified)* |
| PERSON_INCOME | int | USD/year | Average per-person income | ≥0 *(unverified)* |
| MEDIAN_INC_AMNT | int | USD/year | Median income amount | ≥0 *(unverified)* |
| PCT_LT5K … PCT_GE20K | float | percent | Income-bracket distribution (households by income band) | each 0–100 *(unverified)* |
| PCT_WAGE_MAJOR / PCT_WELFARE_MAJOR / PCT_OTHER_MAJOR | float | percent | Major income-source distribution | 0–100 *(unverified)* |
| PCT_MEDIAN / PCT_LT80_MEDIAN / PCT_LT50_MEDIAN / PCT_LT30_MEDIAN | float | percent | Households relative to area median income | 0–100 *(unverified)* |
| PCT_2ADULTS / PCT_1ADULT | float | percent | Household-composition distribution | 0–100 *(unverified)* |
| PCT_FEMALE_HEAD / PCT_FEMALE_HEAD_CHILD | float | percent | Female-headed household share | 0–100 *(unverified)* |
| PCT_DISABLED_LT62 / PCT_DISABLED_GE62 / PCT_DISABLED_ALL / PCT_DISABLED_LT62_ALL | float | percent | Disability-status distribution | 0–100 *(unverified)* |
| PCT_LT24_HEAD / PCT_AGE25_50 / PCT_AGE51_61 / PCT_AGE62PLUS / PCT_AGE85PLUS | float | percent | Age-of-head distribution | 0–100 *(unverified)* |
| ELDLY_PRCNT | float | percent | Elderly share | 0–100 *(unverified)* |
| PCT_MINORITY / PCT_BLACK / PCT_NATIVE_AMERICAN / PCT_ASIAN / PCT_HISPANIC | float | percent | Race/ethnicity distribution (aggregate) | 0–100 *(unverified)* |
| MONTHS_WAITING | int | months | Avg months on waiting list | ≥0 *(unverified)* |
| MONTHS_FROM_MOVEIN | int | months | Avg months since move-in | ≥0 *(unverified)* |
| PCT_UTILITY_ALLOW / AVE_UTIL_ALLOW | float / int | percent / USD | Utility-allowance metrics | *(unverified)* |
| PCT_BED1 / PCT_BED2 / PCT_BED3 / PCT_OVERHOUSED | float | percent | Bedroom-size distribution / overhousing | 0–100 *(unverified)* |
| CHLDRN_MBR_CNT | int | persons | Children member count (aggregate) | ≥0 *(unverified)* |
| TMINORITY / TPOVERTY / TPCT_OWNSFD | float | percent | Tract-level minority / poverty / owner-occ SFD context | 0–100 *(unverified)* |
| ANNL_EXPNS_AMNT / ANNL_EXPNS_AMNT_PREV_YR | float | USD/year | Annual expense amount (current / prior yr) | ≥0 *(unverified)* |
| LAST_UPDT_DTTM | datetime | — | Last update timestamp | epoch-ms (Esri date) *(unverified)* |
| STATE2KX / CNTY2KX / CNTY_NM2KX / TRACT2KX / BG2KX / BLOCK2KX | text | FIPS / name | Census geography (state, county, tract, block group, block) | FIPS codes *(unverified)* |
| PLACE2KX / PLACE_NM2KX / CURCNTY / CURCNTY_NM / CURCOSUB / CURCOSUB_NM | text | FIPS / name | Place & county-subdivision geography | *(unverified)* |
| MSA / MSA_NM / CBSA / CBSA_NM / NECTA / NECTA_NM / METRO / MICRO | text | code / name | Metropolitan/micropolitan statistical-area geography | *(unverified)* |
| LAT / LON | float | degrees | Point latitude / longitude (HUD geocoder) | WGS84 approx *(unverified)* |
| STD_ADDR / STD_CITY / STD_ST / STD_ZIP5 / STD_ZIP9 / ZCTA2KX | text | — | Standardized **development** mailing address (facility, not resident) | *(unverified)* |
| USPS/geocoder block: DPV*, DPBC*, C1P*, MSGUSPS, RC2KX, STM2KX, LVL2KX, UR, MSG2KX, *_LEVEL, HLC, FCD_FIPS91, ZIP_CLASS, ADDR_TYPE, APT_NO, APT_TYPE, URB_OUT | text | codes | USPS address-validation / geocoding diagnostic codes for the development address | technical codes *(unverified)* |
| HA_PHN_NUM / HA_FAX_NUM / HA_EMAIL_ADDR_TEXT | text | — | Housing-authority **office** contact (organizational, public) | *(unverified)* nullable |
| EXEC_DIR_PHONE / EXEC_DIR_FAX / EXEC_DIR_EMAIL | text | — | PHA executive-director **official-role** contact | *(unverified)* — see PII note |

See **Limitations & gaps** and **Datasheet → Composition** for the PII assessment of the
contact and address fields.

---

## Datasheet for Datasets

**Motivation.** Created by HUD to publish where assisted/public housing exists and the
aggregate characteristics of the households served, supporting transparency, fair-housing
analysis, program oversight, and civic/academic research. Funded and maintained by HUD.

**Composition.** Each record is one **public housing development** (point geometry at the
building with the most units), with aggregate counts and percentage statistics about units,
occupancy, income, demographics, and Census/metro geography. Instances are facilities, not
people. Demographic fields are **percentages aggregated to the development**, not individual
records — no resident is identifiable from these statistics. Address/geocoding fields describe
the **development facility's** address. Contact fields (`HA_*`, `EXEC_DIR_*`) reference the
housing authority office and the executive director in their **official capacity**. See PII
note in Limitations.

**Collection process.** Operational/administrative data from HUD systems (e.g., PHA reporting
and HUD's enterprise geocoding service for locations), aggregated to the development level and
published as a GIS feature service. Exact cadence and source systems not fully specified in the
service metadata *(unverified)*.

**Preprocessing / cleaning / labeling.** Addresses are standardized and geocoded via HUD's
enterprise geocoder (hence the USPS/DPV diagnostic columns); household-level data are
aggregated into counts and percentages. Specific suppression/rounding rules are not documented
in the service metadata *(unverified)*.

**Uses.** Civic analysis, fair-housing and equity research, siting and accessibility studies,
joining to Census geographies. Should **not** be used to infer attributes of any individual
resident, nor as survey-grade location data. Avoid implying HUD endorsement.

**Distribution.** Publicly distributed via the HUD eGIS ArcGIS Hub open-data portal
(downloadable as CSV/GeoJSON/Shapefile and queryable via ArcGIS REST). As a U.S. Government
work it is in the public domain (17 U.S.C. § 105). **This deed does not redistribute it.**

**Maintenance.** Maintained by HUD eGIS; a `LAST_UPDT_DTTM` field carries an update timestamp.
Update frequency and versioning policy are not stated in the service metadata *(unverified)*.

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
  "name": "HUD Public Housing Developments",
  "description": "Point locations and aggregate development-level statistics for U.S. public housing developments (units, occupancy, income, demographics, Census/metro geography), published by HUD Enterprise GIS. Documentation-only datasheet; no data redistributed.",
  "url": "https://hudgis-hud.opendata.arcgis.com/",
  "sameAs": "https://catalog.data.gov/dataset?q=HUD+public+housing+developments",
  "license": "https://www.law.cornell.edu/uscode/text/17/105",
  "creditText": "U.S. Department of Housing and Urban Development (HUD), Enterprise GIS",
  "isAccessibleForFree": true,
  "creator": {
    "@type": "GovernmentOrganization",
    "name": "U.S. Department of Housing and Urban Development",
    "url": "https://www.hud.gov/"
  },
  "datePublished": "2026-06-28",
  "keywords": ["housing", "public housing", "HUD", "public-data", "geography"],
  "distribution": [
    {
      "@type": "cr:FileObject",
      "@id": "public-housing-developments-featureservice",
      "name": "Public Housing Developments (ArcGIS Feature Service)",
      "contentUrl": "https://hudgis-hud.opendata.arcgis.com/",
      "encodingFormat": "application/json",
      "description": "Source feature service; not mirrored by this datasheet."
    }
  ],
  "recordSet": [
    {
      "@type": "cr:RecordSet",
      "@id": "development",
      "name": "development",
      "description": "One record per public housing development (point).",
      "field": [
        {"@type": "cr:Field", "@id": "development/DEVELOPMENT_CODE", "name": "DEVELOPMENT_CODE", "description": "HUD development identifier", "dataType": "sc:Text"},
        {"@type": "cr:Field", "@id": "development/PROJECT_NAME", "name": "PROJECT_NAME", "description": "Development name", "dataType": "sc:Text"},
        {"@type": "cr:Field", "@id": "development/TOTAL_UNITS", "name": "TOTAL_UNITS", "description": "Total units", "dataType": "sc:Integer"},
        {"@type": "cr:Field", "@id": "development/PCT_OCCUPIED", "name": "PCT_OCCUPIED", "description": "Percent occupied", "dataType": "sc:Float"},
        {"@type": "cr:Field", "@id": "development/LAT", "name": "LAT", "description": "Latitude (WGS84, HUD geocoder)", "dataType": "sc:Float"},
        {"@type": "cr:Field", "@id": "development/LON", "name": "LON", "description": "Longitude (WGS84, HUD geocoder)", "dataType": "sc:Float"}
      ]
    }
  ]
}
```

---

## Validation script

Documentation-only. The script below validates the **structure** of a locally-obtained export
against the documented schema. It does **not** download, host, or mirror the dataset — the
operator must supply their own local file path. It reads only the header (and an optional
bounded row sample) and stays local/ephemeral.

```python
#!/usr/bin/env python3
"""Validate the structure of a local HUD Public Housing Developments export (CSV/GeoJSON).

Documentation-only: this script does NOT fetch, host, or mirror the dataset. Point it at a
file you have already obtained yourself from the official HUD portal. It reads the header and,
optionally, a bounded sample of rows (default <= 1000) purely to check column structure.
"""
import csv
import sys

EXPECTED = {
    "DEVELOPMENT_CODE", "PROJECT_NAME", "FORMAL_PARTICIPANT_NAME",
    "TOTAL_UNITS", "TOTAL_DWELLING_UNITS", "TOTAL_OCCUPIED",
    "PCT_OCCUPIED", "HH_INCOME", "LAT", "LON",
    "STATE2KX", "CNTY2KX", "TRACT2KX",
}
# Fields a derived/public product should usually drop (official-role contacts).
DISCOURAGED = {"EXEC_DIR_EMAIL", "EXEC_DIR_PHONE", "EXEC_DIR_FAX"}
MAX_SAMPLE = 1000

def main(path: str) -> int:
    with open(path, newline="", encoding="utf-8-sig") as fh:
        reader = csv.reader(fh)
        try:
            header = set(next(reader))
        except StopIteration:
            print("FAIL: file is empty"); return 1
        missing = EXPECTED - header
        present_contacts = DISCOURAGED & header
        rows = sum(1 for _, _ in zip(reader, range(MAX_SAMPLE)))  # bounded sample only

    print(f"columns found: {len(header)}; sampled rows (<= {MAX_SAMPLE}): {rows}")
    if missing:
        print(f"FAIL: missing expected columns: {sorted(missing)}"); return 1
    if present_contacts:
        print(f"WARN: official-role contact columns present, drop before sharing: "
              f"{sorted(present_contacts)}")
    print("OK: expected core columns present.")
    return 0

if __name__ == "__main__":
    if len(sys.argv) != 2:
        print("usage: validate.py <local_export.csv>"); sys.exit(2)
    sys.exit(main(sys.argv[1]))
```

Expected structure summary (for the data-quality check):
- One row per development; ~150 columns in the full feature service.
- Core keys present: `DEVELOPMENT_CODE`, `PROJECT_NAME`; geometry via `LAT`/`LON`.
- Percentage fields expected within 0–100; unit/income/count fields expected ≥ 0.
- Row count is dataset-version dependent (nationwide; thousands of developments) — not asserted
  here to avoid pinning to a snapshot.

---

## Limitations & gaps

- **Schema source:** Fields were verified from the live ArcGIS REST **service metadata** of the
  *Public Housing Developments* layer (retrieved 2026-06-28), not from a published HUD data
  dictionary. The portal landing page is JavaScript-rendered and could not be scraped for a
  human-readable field catalogue; per-field **domains, units, and null rules are inferred**
  and marked *(unverified)* in the data dictionary.
- **Portal breadth:** The HUD eGIS portal hosts many layers (e.g., insured multifamily
  properties, LIHTC, qualified census tracts, CDBG/entitlement geographies). This datasheet
  documents one representative housing-program layer; other layers have different schemas.
- **PII assessment:** Demographic and income fields are **aggregated to the development** and
  do not identify individuals — no resident-level PII is documented or sampled. The dataset
  does include **official-role** contact fields (`EXEC_DIR_EMAIL/PHONE/FAX`,
  `HA_EMAIL_ADDR_TEXT/HA_PHN_NUM/HA_FAX_NUM`) for housing-authority offices/directors. These
  are organizational/public-role contacts published by HUD, not private personal data; even so,
  the validation script flags the director-contact columns as fields to drop from any derived
  public product. No personal data was sampled or reproduced here.
- **Location precision:** `LAT`/`LON` are HUD geocoder approximations ("building with the most
  units"), not survey-grade; do not use for precise siting.
- **License:** Verified as a U.S. Government work (public domain, 17 U.S.C. § 105(a)),
  permitting derivatives. The portal asserts an attribution courtesy string in `copyrightText`
  but no copyright; no separate click-through terms were located/confirmed for this layer.
- **No data handled:** Consistent with Hee-Lee Oss guardrails, no dataset rows were downloaded,
  transformed, hosted, or committed; only public service metadata and the statute were read.
