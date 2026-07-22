# Datasheet — Police recorded crime (street-level)

One-line summary: A documentation datasheet for the UK street-level police recorded crime dataset (publisher-anonymised, snapped-to-point crime locations with outcomes), published via data.police.uk.

This datasheet (the document you are reading) is licensed under **CC-BY-4.0**. It contains documentation and metadata only — it does **not** host, mirror, transform, or republish any of the underlying dataset.

---

## Provenance & attribution

- **Publisher / data controller:** UK Home Office and contributing police forces, published by the Single Online Home National Digital Team via **data.police.uk**.
- **Source URL:** https://data.police.uk/ (documentation: https://data.police.uk/about/)
- **Catalog id (candidate):** cat-justice-001
- **Retrieval date:** 2026-06-28
- **Required attribution string (OGL default):**
  > "Contains public sector information licensed under the Open Government Licence v3.0."
- **Suggested source citation:** Crime data from data.police.uk, UK Home Office / contributing police forces, retrieved 2026-06-28.

---

## Source licence

- **Licence:** Open Government Licence v3.0 (OGL v3.0)
- **Licence URL:** https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/
- **Permits derivatives — VERIFIED.** Under the "You are free to" section, the OGL v3.0 explicitly grants the rights to:
  - "copy, publish, distribute and transmit the Information";
  - **"adapt the Information"** (i.e. create derivative works);
  - "exploit the Information commercially and non-commercially for example, by combining it with other Information, or by including it in your own product or application".
- **Conclusion:** Adaptation (derivatives) and redistribution are permitted, subject to the attribution requirement above. This satisfies the Hee-Lee Oss source-licence gate for derivative documentation work.
- **Licence snapshot:** The canonical, archivable licence text is hosted by The National Archives at the URL above. Reviewers should archive a copy (e.g. via the Internet Archive) and record its content hash as the per-dataset gate artifact. (No licence text is committed to this repo to avoid mirroring third-party content beyond the cited URL.)

---

## Data dictionary

Fields below are derived from the publisher's documentation at https://data.police.uk/about/ (street-level crime CSV). Types/units are inferred from the documented format and marked where unconfirmed.

| Field | Type | Units | Description | Allowed values / nulls |
|-------|------|-------|-------------|------------------------|
| Crime ID | string (hash) | n/a | Persistent one-way hashed offence reference. Not all rows have one (older/anonymous offences). | 64-char hash *(length unconfirmed)*; may be empty |
| Month | string | YYYY-MM | Year and month the crime was reported. | e.g. `2013-07`; non-null |
| Reported by | string | n/a | Police force that provided the data. | One of 43 English/Welsh forces, British Transport Police (BTP), or PSNI |
| Falls within | string | n/a | Force within whose jurisdiction the crime falls. | Currently same as "Reported by" per docs |
| Longitude | float | WGS84 decimal degrees | Anonymised, snap-to-point longitude. | May be empty/zeroed if no nearby map point (>20km) |
| Latitude | float | WGS84 decimal degrees | Anonymised, snap-to-point latitude. | May be empty/zeroed if no nearby map point (>20km) |
| Location | string | n/a | Human-readable approximate location label (e.g. "On or near ..."). *(Field presence inferred from CSV format — unconfirmed exact name.)* | May be empty |
| LSOA code | string | n/a | ONS Lower Layer Super Output Area code for the snapped point. | ONS LSOA reference; may be empty |
| LSOA name | string | n/a | ONS LSOA name. | may be empty |
| Crime type | string (categorical) | n/a | Offence classification. | One of ~14 categories, e.g. "Violence and sexual offences", "Burglary", "Anti-social behaviour" |
| Last outcome category | string (categorical) | n/a | Most recent recorded outcome for the crime. | e.g. "Formal action is not in the public interest", "Under investigation"; may be empty |
| Context | string | n/a | Additional narrative field. | Currently empty in published CSVs |

> Notes: "Crime ID" is a one-way hash, not a direct identifier; locations are deliberately coarsened (snapped to map points covering >=8 postal addresses) by the publisher. No victim/suspect names, addresses, or free-text context are present in the published street-level CSV. Rows flagged *(unconfirmed)* should be validated against a live header sample before relying on exact field names.

---

## Datasheet for Datasets

**Motivation.** Created to increase transparency of policing and give the public, researchers, and local authorities access to where and what crime occurs and how cases are resolved. Funded/maintained by the UK Home Office.

**Composition.** Each record is one recorded crime (or anti-social behaviour incident) with an approximate location, month, crime type, and latest outcome. Coverage spans 43 territorial forces in England & Wales plus BTP and PSNI. The data is publisher-anonymised; locations are snapped to representative map points and exact addresses are removed.

**Collection process.** Crimes flow monthly from force Incident/Crime/Custody/Case Management systems to the national team. Outcomes are matched against Ministry of Justice court results, then the data is QA-validated and anonymised before publication.

**Preprocessing/cleaning/labelling.** Locations are snapped to a master list of anonymised map points (each covering >=8 addresses or a public/commercial premise); points further than ~20km are zeroed. Free-text context is stripped for privacy. Crime types and outcome categories are mapped to controlled vocabularies.

**Uses.** Crime mapping, geographic/temporal trend analysis, local-authority planning, academic research, journalism. Not suitable for identifying individuals, precise address-level location, or operational/real-time policing decisions due to anonymisation and monthly lag.

**Distribution.** Free public download (CSV and API) from data.police.uk under OGL v3.0. This datasheet does not redistribute the data; consumers must obtain it directly from the publisher.

**Maintenance.** Updated monthly by the Single Online Home National Digital Team; historical archives are retained. Field definitions occasionally change — re-validate against the live schema.

---

## Croissant metadata

```json
{
  "@context": {
    "@vocab": "https://schema.org/",
    "cr": "http://mlcommons.org/croissant/"
  },
  "@type": "Dataset",
  "name": "Police recorded crime (street-level)",
  "description": "Publisher-anonymised UK street-level police recorded crime and outcomes, snapped to representative map points, published monthly via data.police.uk.",
  "url": "https://data.police.uk/",
  "license": "https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/",
  "creator": {
    "@type": "Organization",
    "name": "UK Home Office / data.police.uk",
    "url": "https://data.police.uk/"
  },
  "datePublished": "2026-06-28",
  "citation": "Contains public sector information licensed under the Open Government Licence v3.0.",
  "distribution": [
    {
      "@type": "cr:FileObject",
      "name": "street-crime-csv",
      "encodingFormat": "text/csv",
      "description": "Monthly per-force street-level crime CSV (obtained directly from data.police.uk; not hosted here)."
    }
  ],
  "recordSet": [
    {
      "@type": "cr:RecordSet",
      "name": "crimes",
      "field": [
        {"@type": "cr:Field", "name": "Month", "dataType": "sc:Text", "description": "Report month, YYYY-MM."},
        {"@type": "cr:Field", "name": "Longitude", "dataType": "sc:Float", "description": "Anonymised WGS84 longitude."},
        {"@type": "cr:Field", "name": "Latitude", "dataType": "sc:Float", "description": "Anonymised WGS84 latitude."},
        {"@type": "cr:Field", "name": "Crime type", "dataType": "sc:Text", "description": "Offence classification (controlled vocabulary)."},
        {"@type": "cr:Field", "name": "Last outcome category", "dataType": "sc:Text", "description": "Most recent recorded outcome."}
      ]
    }
  ]
}
```

---

## Validation script

The script checks the structure of a CSV the user has **already obtained themselves** from data.police.uk. It does **not** download, host, or transform the dataset — it only inspects a local file's header and basic shape.

```python
#!/usr/bin/env python3
"""Validate a locally-obtained data.police.uk street-level crime CSV.
Documentation only: does NOT download or host the dataset.
Usage: python validate.py path/to/local-street.csv
"""
import csv, sys

EXPECTED = [
    "Crime ID", "Month", "Reported by", "Falls within",
    "Longitude", "Latitude", "Location", "LSOA code", "LSOA name",
    "Crime type", "Last outcome category", "Context",
]

def main(path):
    with open(path, newline="", encoding="utf-8") as f:
        reader = csv.reader(f)
        header = next(reader, [])
        rows = sum(1 for _ in reader)

    missing = [c for c in EXPECTED if c not in header]
    extra = [c for c in header if c not in EXPECTED]

    print(f"Columns found : {len(header)}")
    print(f"Data rows     : {rows}")
    if missing:
        print(f"WARN missing expected columns: {missing}")
    if extra:
        print(f"NOTE unexpected columns       : {extra}")
    ok = (not missing) and rows > 0
    print("RESULT: PASS" if ok else "RESULT: REVIEW")
    return 0 if ok else 1

if __name__ == "__main__":
    if len(sys.argv) != 2:
        print(__doc__); sys.exit(2)
    sys.exit(main(sys.argv[1]))
```

---

## Limitations & gaps

- **Not validated against a live sample.** Field names/types were taken from publisher documentation (https://data.police.uk/about/), not from a downloaded file. Rows marked *(unconfirmed)* in the data dictionary — notably the exact "Location" field name and the "Crime ID" hash length — must be checked against a live header before relying on them.
- **Licence snapshot hash not computed here** (no third-party text is committed to the repo). Reviewers should archive the OGL v3.0 text and record its hash as the gate artifact.
- **Residual privacy note.** The published data is publisher-anonymised (snapped locations, no names/addresses, stripped free text), so residual PII risk is low but non-zero at small-area granularity. No personal or identifying fields were documented or sampled in producing this datasheet.
- **Croissant record is minimal** (a subset of fields as stubs) and not run through a full Croissant validator; it is intended as a starting point.
- **Outcome lag.** Outcomes are updated over time as cases progress; a snapshot may understate final resolutions.
