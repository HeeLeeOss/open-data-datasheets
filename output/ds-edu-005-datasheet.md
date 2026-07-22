# DRAFT — flagged for review: dataset contains personal/identifying fields (PII)

> **Status: DRAFT.** The source licence (OGL v3.0) is verified and *does* permit derivatives, so this
> is **not** a licence-blocking DRAFT. It is flagged because the GIAS "all establishment data"
> download includes **personal data about named individuals** — head teacher name fields
> (`HeadTitle`, `HeadFirstName`, `HeadLastName`, `HeadPreferredJobTitle`) and direct contact details
> (`TelephoneNum`, `SchoolWebsite`). Under the Hee-Lee Oss refusal/PII guardrails this must be reviewed
> before the datasheet is treated as final. **No personal values are reproduced anywhere in this
> file.** See *Limitations & gaps* for what could not be verified.

# Datasheet: Get Information about Schools (GIAS)

**One-line summary:** The authoritative public register of schools and other education providers
in England, published by the UK Department for Education, covering identity, status, type, phase,
location and governance of establishments.

*This datasheet (the document you are reading) is licensed under **CC-BY-4.0**.* It is documentation
only: it does **not** host, mirror, transform, or republish any GIAS data.

---

## Provenance & attribution

| Item | Value |
|---|---|
| Publisher | UK Department for Education (DfE) |
| Service / source | Get Information about Schools (GIAS) |
| Source URL | https://www.get-information-schools.service.gov.uk/ |
| Download portal | https://www.get-information-schools.service.gov.uk/Downloads |
| Catalog id (Hee-Lee Oss) | cat-edu-005 |
| Retrieval date | 2026-06-28 |
| Crown copyright | © Crown copyright |

**Required attribution string** (per the GIAS source statement and the OGL default form):

> "Contains public sector information licensed under the Open Government Licence v3.0."
> Source: Get Information about Schools, © Crown copyright, UK Department for Education.

---

## Source licence

- **Licence:** Open Government Licence for public sector information, **version 3.0 (OGL v3.0)**.
- **Citation URL:** https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/
- **Stated on source:** the GIAS / GOV.UK guidance states "All content is available under the Open
  Government Licence v3.0, except where otherwise stated."
- **Permits derivatives?** **YES.** Verified clause (OGL v3.0, "You are free to" section):
  > "copy, publish, distribute and transmit the Information; adapt the Information; exploit the
  > Information commercially and non-commercially for example, by combining it with other Information,
  > or by including it in your own product or application."
  This expressly permits adaptation/derivatives and commercial reuse, subject to attribution.
- **Conditions:** acknowledge the source with the attribution statement above; do not use in a way
  that suggests official status or endorsement; the licence does not cover personal data, third-party
  rights, or departmental logos.

> **Note on the "personal data" exclusion:** OGL v3.0 explicitly does **not** license personal data.
> Because GIAS publishes named head-teacher fields and direct contact details, those specific fields
> may fall outside the OGL grant and engage UK GDPR / Data Protection Act considerations. This is the
> reason this datasheet is flagged as DRAFT.

---

## Data dictionary

**Verification status:** the live GIAS download/field-documentation pages returned HTTP 403/500 to
automated retrieval on 2026-06-28, and no `<=1000`-row bounded sample could be taken. The fields
below reflect the **published GIAS "all establishment data" (edubasealldata) schema** as it is
publicly documented; **every row is marked UNCONFIRMED** because it could not be validated against a
live header/sample in this session. Types/units/allowed-values must be confirmed against the official
GIAS CSV layout + code-list spreadsheet before use. **Personal-data fields are marked PII and are NOT
reproduced with any values.**

| Field (name) | Type | Units | Description | Allowed values / nulls | Status |
|---|---|---|---|---|---|
| URN | integer | — | Unique Reference Number; primary key for an establishment | unique, not null | UNCONFIRMED |
| LA (code) | integer | — | Local authority code | code-list value | UNCONFIRMED |
| LA (name) | string | — | Local authority name | free text | UNCONFIRMED |
| EstablishmentNumber | integer | — | DfE establishment number within the LA | nullable | UNCONFIRMED |
| EstablishmentName | string | — | Name of the school / provider | not null | UNCONFIRMED |
| TypeOfEstablishment (code/name) | coded | — | Establishment type (e.g. community, academy) | code-list | UNCONFIRMED |
| EstablishmentTypeGroup (code/name) | coded | — | Higher-level grouping of type | code-list | UNCONFIRMED |
| EstablishmentStatus (code/name) | coded | — | Open / closed / proposed etc. | code-list | UNCONFIRMED |
| OpenDate | date | DD-MM-YYYY | Date establishment opened | nullable | UNCONFIRMED |
| CloseDate | date | DD-MM-YYYY | Date establishment closed | nullable | UNCONFIRMED |
| PhaseOfEducation (code/name) | coded | — | Primary / secondary / all-through etc. | code-list | UNCONFIRMED |
| StatutoryLowAge | integer | years | Lowest statutory pupil age | nullable | UNCONFIRMED |
| StatutoryHighAge | integer | years | Highest statutory pupil age | nullable | UNCONFIRMED |
| Gender (code/name) | coded | — | Gender of intake | code-list | UNCONFIRMED |
| ReligiousCharacter (code/name) | coded | — | Religious character of school | code-list / null | UNCONFIRMED |
| AdmissionsPolicy (code/name) | coded | — | Admissions policy | code-list | UNCONFIRMED |
| SchoolCapacity | integer | pupils | Stated capacity | nullable | UNCONFIRMED |
| NumberOfPupils | integer | pupils | Census headcount | nullable | UNCONFIRMED |
| CensusDate | date | DD-MM-YYYY | Date of pupil census | nullable | UNCONFIRMED |
| TrustSchoolFlag / Trusts | coded/string | — | Trust membership | code-list / null | UNCONFIRMED |
| UKPRN | integer | — | UK Provider Reference Number | nullable | UNCONFIRMED |
| OfstedLastInsp | date | DD-MM-YYYY | Date of last Ofsted inspection | nullable | UNCONFIRMED |
| Street / Locality / Town / County | string | — | Postal address components | nullable | UNCONFIRMED |
| Postcode | string | — | Establishment postcode | nullable | UNCONFIRMED |
| GSSLACode | string | — | ONS GSS local authority code | nullable | UNCONFIRMED |
| Easting / Northing | integer | metres (OSGB) | Grid coordinates | nullable | UNCONFIRMED |
| LSOA / MSOA (code/name) | coded | — | Census geography | nullable | UNCONFIRMED |
| LastChangedDate | date | DD-MM-YYYY | Record last updated | nullable | UNCONFIRMED |
| **HeadTitle** | string | — | **PII — title of head teacher (named individual)** | — | **UNCONFIRMED / PII — value not reproduced** |
| **HeadFirstName** | string | — | **PII — head teacher first name** | — | **UNCONFIRMED / PII — value not reproduced** |
| **HeadLastName** | string | — | **PII — head teacher surname** | — | **UNCONFIRMED / PII — value not reproduced** |
| **HeadPreferredJobTitle** | string | — | **PII-adjacent — head teacher job title** | — | **UNCONFIRMED / PII — value not reproduced** |
| **TelephoneNum** | string | — | **PII-adjacent — establishment phone number** | — | **UNCONFIRMED / PII — value not reproduced** |
| SchoolWebsite | string (URL) | — | Establishment website | nullable | UNCONFIRMED |

---

## Datasheet for Datasets

**Motivation.** Created by the DfE to provide a single authoritative, openly available register of
education establishments in England (state-funded and many independent providers), used for linking,
reference, performance reporting and public lookup. It exists to be the canonical source of school
identity (URN) and status.

**Composition.** Each row represents one education establishment. Instances carry an identity key
(URN), categorical status/type/phase fields, governance/trust links, location/geography, capacity and
census counts, and — for current head teachers — name and contact fields (personal data). The full
"all establishment data" export covers tens of thousands of establishments (open and closed).

**Collection process.** Maintained by the DfE and updated by local authorities, trusts and
establishments themselves through the GIAS service; records are continuously edited and re-published.
Not the result of survey sampling — it is an administrative register.

**Preprocessing / cleaning.** DfE applies coded value lists (code-list spreadsheet accompanies the
download). Consumers typically map codes to labels and parse `DD-MM-YYYY` dates. No cleaning was
performed by Hee-Lee Oss (documentation only; no sample taken — portal blocked automated access).

**Uses.** School lookup and linking via URN; education research; geographic/administrative analysis;
joining to performance, finance and census datasets. **Not suitable** for purposes that re-identify
or directly contact named head teachers beyond the published official-register purpose; personal
fields should be excluded from derivative products unless a lawful basis is confirmed.

**Distribution.** Published openly via the GIAS service as downloadable CSV (plus a code-list file).
Under OGL v3.0. Hee-Lee Oss does **not** redistribute it; users must obtain it from the official portal.

**Maintenance.** Maintained by the UK Department for Education; the dataset is refreshed frequently
(daily "all establishment data" exports are typical). `LastChangedDate` tracks per-record updates.

---

## Croissant metadata

```json
{
  "@context": {
    "@vocab": "https://schema.org/",
    "cr": "http://mlcommons.org/croissant/",
    "data": "http://mlcommons.org/croissant/data/",
    "dataType": "http://mlcommons.org/croissant/dataType"
  },
  "@type": "Dataset",
  "name": "Get Information about Schools (GIAS)",
  "description": "Authoritative public register of schools and other education providers in England, published by the UK Department for Education. Documentation-only Croissant stub; field list UNCONFIRMED against a live sample and includes personal-data fields excluded from reuse.",
  "url": "https://www.get-information-schools.service.gov.uk/",
  "license": "https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/",
  "creator": {
    "@type": "Organization",
    "name": "UK Department for Education",
    "url": "https://www.gov.uk/government/organisations/department-for-education"
  },
  "datePublished": "2026-06-28",
  "version": "retrieved-2026-06-28",
  "citation": "Contains public sector information licensed under the Open Government Licence v3.0. Source: Get Information about Schools, Crown copyright, UK Department for Education.",
  "recordSet": [
    {
      "@type": "cr:RecordSet",
      "name": "establishments",
      "description": "One record per education establishment (UNCONFIRMED field stubs).",
      "field": [
        { "@type": "cr:Field", "name": "URN", "description": "Unique Reference Number (primary key).", "dataType": "Integer" },
        { "@type": "cr:Field", "name": "EstablishmentName", "description": "Name of the establishment.", "dataType": "Text" },
        { "@type": "cr:Field", "name": "EstablishmentStatus", "description": "Open/closed/proposed (code-list).", "dataType": "Text" },
        { "@type": "cr:Field", "name": "PhaseOfEducation", "description": "Primary/secondary/all-through (code-list).", "dataType": "Text" },
        { "@type": "cr:Field", "name": "Postcode", "description": "Establishment postcode.", "dataType": "Text" }
      ]
    }
  ]
}
```

> The Croissant stub deliberately omits the personal-data (head teacher / contact) fields.

---

## Validation script

This script validates the **structure** of a GIAS CSV that the user has **already downloaded
themselves** from the official portal. It does **not** download, host, or transmit any data, and it
prints only structural metadata (column names, row count) — never field values, so no personal data
is emitted.

```python
#!/usr/bin/env python3
"""Validate the structure of a locally-held GIAS 'edubasealldata' CSV.
Does NOT download or host data. Run only against a file you obtained yourself.
Usage: python validate_gias.py /path/to/edubasealldata.csv
"""
import csv, sys

# Core columns expected in the GIAS 'all establishment data' export.
# UNCONFIRMED against a live sample in this session; adjust to the official layout.
EXPECTED_CORE = ["URN", "EstablishmentName", "EstablishmentStatus (name)",
                 "PhaseOfEducation (name)", "Postcode"]

# Personal-data columns: presence is REPORTED so they can be dropped before reuse.
PII_COLS = ["HeadTitle", "HeadFirstName", "HeadLastName",
            "HeadPreferredJobTitle", "TelephoneNum"]

def main(path):
    with open(path, newline="", encoding="latin-1") as f:
        reader = csv.reader(f)
        header = next(reader, [])
        rows = sum(1 for _ in reader)

    print(f"columns: {len(header)}")
    print(f"data rows: {rows}")

    missing = [c for c in EXPECTED_CORE if c not in header]
    print("OK: all core columns present" if not missing
          else f"WARNING: missing core columns: {missing}")

    found_pii = [c for c in PII_COLS if c in header]
    if found_pii:
        print(f"PII WARNING: personal-data columns present (drop before reuse): {found_pii}")

    # Basic data-quality checks (structure only; no values printed).
    if "URN" in header and rows == 0:
        print("WARNING: file has a header but zero data rows")

    sys.exit(1 if missing else 0)

if __name__ == "__main__":
    if len(sys.argv) != 2:
        sys.exit("Usage: python validate_gias.py /path/to/edubasealldata.csv")
    main(sys.argv[1])
```

Expected, when run against a current full export: a column count consistent with the official GIAS
layout and tens of thousands of data rows (open + closed establishments). Exact counts vary per
release and must be confirmed against the official code-list/layout spreadsheet.

---

## Limitations & gaps

- **Field list UNCONFIRMED.** The live GIAS service (`/`, `/Downloads`) returned HTTP 403, and a
  direct CSV/mirror returned HTTP 500/404 to automated retrieval on 2026-06-28. No bounded
  (`<=1000`-row) sample was taken. The data dictionary reflects the *publicly documented* GIAS
  schema, not a verified live header — every row is marked UNCONFIRMED and must be checked against
  the official CSV layout + code-list spreadsheet.
- **PII present — this file is flagged DRAFT.** GIAS publishes named head-teacher fields and direct
  contact details. OGL v3.0 does not license personal data, so these fields likely sit outside the
  open grant and engage UK GDPR/DPA. No personal values are reproduced here; the fields are documented
  only to ensure they are excluded from any derivative.
- **Licence is verified** (OGL v3.0, permits derivatives + commercial use) — the DRAFT status is due
  solely to the PII concern, not a licence problem.
- **No licence-text hash/archive captured.** The acceptance criterion to record a hash + archived
  copy of the OGL text was not completed in this session (only the canonical OGL URL and the
  permitting clause were captured).
- Allowed-value code lists, exact types, units (e.g. coordinate datum), and precise null semantics
  were not validated against the official spec.
