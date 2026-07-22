# Datasheet: Road Safety Data (STATS19)

**One-line summary:** Documentation datasheet for the UK Department for Transport's STATS19
Road Safety Data — police-reported personal-injury road collisions in Great Britain, published
as open data across three linked tables (collisions, vehicles, casualties).

*This datasheet (the document you are reading) is licensed under **CC-BY-4.0**. It is
documentation only: it does not host, mirror, transform, or republish any of the source data.*

---

## Provenance & attribution

| Field | Value |
|---|---|
| **Publisher / source** | UK Department for Transport (DfT) |
| **Dataset name** | Road Safety Data (STATS19) |
| **Source URL (catalogue)** | https://www.data.gov.uk/dataset/road-accidents-safety-data |
| **National Data Library entry** | https://www.data.gov.uk/dataset/cb7ae6f0-4be6-4935-9277-47e5ce24a11f/road-safety-data |
| **Catalog id (Hee-Lee Oss candidate)** | cat-transport-002 |
| **Retrieval date** | 2026-06-28 |
| **Contact (publisher)** | roadacc.stats@dft.gov.uk |

**Required attribution string** (use verbatim when reusing the source data):

> Contains public sector information licensed under the Open Government Licence v3.0.
> Source: Road Safety Data, © Crown copyright, UK Department for Transport.

---

## Source licence

| Item | Value |
|---|---|
| **Licence** | Open Government Licence (OGL) v3.0 |
| **Licence URL** | https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/ |
| **Permits derivatives?** | **YES — verified** |
| **Permits commercial use?** | Yes |
| **Verified on** | 2026-06-28 |

**Citation confirming derivatives are permitted.** The OGL v3.0 text, under "You are free to",
states the licensee may:

> "copy, publish, distribute and transmit the Information; **adapt the Information**; exploit the
> Information commercially and non-commercially for example, by combining it with other
> Information, or by including it in your own product or application."

The clause "adapt the Information" explicitly grants the right to create derivative works, and
the commercial/non-commercial exploitation clause confirms there is no non-commercial
restriction. Source: https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/

The data.gov.uk catalogue entry and the National Data Library entry both record the dataset's
licence as **Open Government Licence v3.0** with publisher **Department for Transport**
(verified 2026-06-28).

> Licence snapshot note: the acceptance criteria call for an archived copy + hash of the licence
> text. Per Hee-Lee Oss guardrails this datasheet does not commit external content; the licence text
> should be archived and hashed at the per-dataset gate (the canonical text is at the licence URL
> above). This step is **not yet completed** in this documentation pass — see Limitations.

---

## Data dictionary

The STATS19 open data is published as **three linked CSV tables**, one collision can link to
multiple vehicle rows and multiple casualty rows:

- **Collision** table — one row per reported collision.
- **Vehicle** table — one row per vehicle involved.
- **Casualty** table — one row per person injured or killed.

The tables are joined on the collision identifier (`collision_index` / historically
`accident_index`). Note: DfT renamed several variables from `accident_*` to `collision_*`; older
released files and some tools still use the `accident_*` names. Field names below follow the
current DfT / `stats19` schema.

### Collision table (fields verified from DfT STATS19 schema documentation)

| Field | Type | Units | Description | Allowed values / nulls |
|---|---|---|---|---|
| `collision_index` | string | — | Unique collision identifier (join key) | Unique; not null |
| `collision_year` | integer | year | Year of collision | e.g. 2023 |
| `collision_severity` | categorical (coded) | — | Severity of the collision | Fatal / Serious / Slight (coded 1/2/3) |
| `date` | date | — | Date of collision | dd/mm/yyyy |
| `day_of_week` | categorical (coded) | — | Day of week | Sunday…Saturday (coded 1–7) |
| `time` | time | hh:mm | Time of collision | 24-hour; may be null |
| `location_easting_osgr` | integer | metres (OSGB36) | OS grid easting | Null where location withheld |
| `location_northing_osgr` | integer | metres (OSGB36) | OS grid northing | Null where location withheld |
| `longitude` | float | degrees (WGS84) | Longitude | Null where withheld |
| `latitude` | float | degrees (WGS84) | Latitude | Null where withheld |
| `police_force` | categorical (coded) | — | Reporting police force | Coded lookup |
| `number_of_vehicles` | integer | count | Vehicles involved | ≥ 1 |
| `number_of_casualties` | integer | count | Casualties in collision | ≥ 1 |
| `first_road_class` | categorical (coded) | — | Class of first road | Motorway/A(M)/A/B/C/Unclassified |
| `first_road_number` | integer | — | Road number of first road | 0 where not applicable |
| `second_road_class` | categorical (coded) | — | Class of second road (at junction) | Coded; -1/null if none |
| `second_road_number` | integer | — | Road number of second road | 0/null if none |
| `road_type` | categorical (coded) | — | Road layout | e.g. Single carriageway, Dual carriageway, Roundabout |
| `speed_limit` | integer | mph | Speed limit at location | e.g. 20/30/40/50/60/70; null/-1 unknown |
| `urban_or_rural_area` | categorical (coded) | — | Urban vs rural classification | Urban / Rural / Unallocated |
| `junction_detail` | categorical (coded) | — | Junction type | Coded lookup |
| `junction_control` | categorical (coded) | — | Junction control | Coded lookup |
| `light_conditions` | categorical (coded) | — | Lighting at time | Daylight / Darkness variants |
| `weather_conditions` | categorical (coded) | — | Weather | Coded lookup |
| `road_surface_conditions` | categorical (coded) | — | Surface state | Dry/Wet/Snow/Frost etc. |
| `pedestrian_crossing` | categorical (coded) | — | Crossing facilities/control | Coded lookup |
| `carriageway_hazards` | categorical (coded) | — | Hazards present | Coded lookup |
| `local_authority_district` | categorical (coded) | — | Local authority district | Coded lookup |
| `local_authority_ons_district` | string | — | ONS district code | ONS GSS code |

### Vehicle table (partially verified — table semantics confirmed, individual columns NOT individually verified in this pass)

> **UNVERIFIED ROWS.** Documentation confirms the vehicle table has "one row per vehicle" and
> includes vehicle and driver attributes, but the exact column list below was NOT individually
> confirmed against the DfT Road Safety Open Data Guide 2024 in this pass (the .xlsx guide was
> not retrievable — HTTP 404). Treat the following as the commonly-documented STATS19 vehicle
> fields pending guide verification.

| Field (commonly documented) | Type | Description | Status |
|---|---|---|---|
| `collision_index` | string | Join key to collision table | unverified |
| `vehicle_reference` | integer | Vehicle number within the collision | unverified |
| `vehicle_type` | categorical (coded) | Type of vehicle (car, HGV, pedal cycle, …) | unverified |
| `vehicle_manoeuvre` | categorical (coded) | Manoeuvre at time of collision | unverified |
| `age_of_driver` | integer | Driver age (de-identified) | unverified |
| `sex_of_driver` | categorical (coded) | Driver sex | unverified |
| `age_of_vehicle` | integer | Vehicle age in years | unverified |
| `engine_capacity_cc` | integer | Engine capacity (cc) | unverified |
| `propulsion_code` | categorical (coded) | Propulsion/fuel type | unverified |

### Casualty table (partially verified — table semantics confirmed, individual columns NOT individually verified in this pass)

> **UNVERIFIED ROWS.** Documentation confirms the casualty table has "one row per person injured
> or killed" and includes age and casualty-class attributes; exact columns below were NOT
> individually confirmed in this pass. See PII note in Limitations.

| Field (commonly documented) | Type | Description | Status |
|---|---|---|---|
| `collision_index` | string | Join key to collision table | unverified |
| `vehicle_reference` | integer | Vehicle the casualty was in (0 = pedestrian) | unverified |
| `casualty_reference` | integer | Casualty number within the collision | unverified |
| `casualty_class` | categorical (coded) | Driver/rider, passenger, or pedestrian | unverified |
| `sex_of_casualty` | categorical (coded) | Casualty sex | unverified |
| `age_of_casualty` | integer | Casualty age (de-identified) | unverified |
| `age_band_of_casualty` | categorical (coded) | Banded age group | unverified |
| `casualty_severity` | categorical (coded) | Fatal / Serious / Slight | unverified |

Coded categorical fields resolve to human-readable labels via the DfT **Road Safety Open Data
Guide 2024** lookup workbook (referenced on the catalogue page; not retrieved in this pass).

---

## Datasheet for Datasets

**Motivation.** Created by the UK Department for Transport to provide official statistics on
the circumstances of personal-injury road collisions reported to the police in Great Britain
(the STATS19 collection). It underpins national road-safety policy, casualty-reduction targets,
and public/academic research. It is open data published in the public interest.

**Composition.** Three linked tables: collisions (circumstances, location, conditions), vehicles
(one row per vehicle), and casualties (one row per injured/killed person). Records are
police-reported personal-injury collisions; damage-only collisions are excluded. Locations are
provided as OS grid references / lat-long, **anonymised by the publisher to street level**. Each
record describes a real event; casualty/driver rows carry de-identified demographic attributes
(age, sex) but no names, exact addresses, or direct identifiers.

**Collection process.** Data is recorded by police forces at the scene or from reports, using
the STATS19 reporting form, and collated nationally by DfT. Coverage is collisions on public
roads involving personal injury; reporting practices and injury-severity recording systems
(CRASH/COPA) vary over time, which DfT addresses via severity-adjustment guidance.

**Preprocessing / cleaning.** DfT codes fields to standard lookups, applies severity
adjustments, and anonymises location to street level prior to release. Some location/value
fields are withheld or set to "unknown"/-1 where data is missing or could risk disclosure.

**Uses.** Road-safety analysis, casualty-reduction monitoring, infrastructure and speed-limit
assessment, academic transport research, civic data journalism. **Should not** be used to
attempt re-identification of individuals, nor as a complete record of all road collisions
(damage-only and unreported injury collisions are out of scope).

**Distribution.** Published openly by DfT via data.gov.uk and gov.uk under OGL v3.0; freely
downloadable, no registration. Updated periodically (annual releases plus revisions; catalogue
last updated 2025-09-25).

**Maintenance.** Maintained by DfT road safety statistics team (roadacc.stats@dft.gov.uk).
Annual updates with periodic methodological guidance (e.g. severity adjustment, historical-data
notes). Variable naming migrated from `accident_*` to `collision_*`.

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
  "name": "Road Safety Data (STATS19)",
  "description": "Police-reported personal-injury road collisions in Great Britain (STATS19), published by the UK Department for Transport as three linked tables: collisions, vehicles, and casualties.",
  "url": "https://www.data.gov.uk/dataset/road-accidents-safety-data",
  "license": "https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/",
  "creator": {
    "@type": "GovernmentOrganization",
    "name": "Department for Transport",
    "url": "https://www.gov.uk/government/organisations/department-for-transport"
  },
  "publisher": {
    "@type": "GovernmentOrganization",
    "name": "Department for Transport"
  },
  "datePublished": "2025-09-25",
  "creditText": "Contains public sector information licensed under the Open Government Licence v3.0.",
  "keywords": ["road safety", "collisions", "STATS19", "transport", "casualties", "Great Britain"],
  "distribution": [
    {
      "@type": "cr:FileObject",
      "@id": "collision-table",
      "name": "collision",
      "description": "One row per reported personal-injury collision.",
      "encodingFormat": "text/csv"
    },
    {
      "@type": "cr:FileObject",
      "@id": "vehicle-table",
      "name": "vehicle",
      "description": "One row per vehicle involved in a collision.",
      "encodingFormat": "text/csv"
    },
    {
      "@type": "cr:FileObject",
      "@id": "casualty-table",
      "name": "casualty",
      "description": "One row per person injured or killed.",
      "encodingFormat": "text/csv"
    }
  ],
  "recordSet": [
    {
      "@type": "cr:RecordSet",
      "@id": "collision",
      "name": "collision",
      "field": [
        {"@type": "cr:Field", "@id": "collision/collision_index", "name": "collision_index", "dataType": "sc:Text", "description": "Unique collision identifier (join key)."},
        {"@type": "cr:Field", "@id": "collision/collision_severity", "name": "collision_severity", "dataType": "sc:Text", "description": "Fatal / Serious / Slight."},
        {"@type": "cr:Field", "@id": "collision/date", "name": "date", "dataType": "sc:Date", "description": "Date of collision."},
        {"@type": "cr:Field", "@id": "collision/longitude", "name": "longitude", "dataType": "sc:Float", "description": "Longitude (WGS84), anonymised to street level."},
        {"@type": "cr:Field", "@id": "collision/latitude", "name": "latitude", "dataType": "sc:Float", "description": "Latitude (WGS84), anonymised to street level."},
        {"@type": "cr:Field", "@id": "collision/number_of_casualties", "name": "number_of_casualties", "dataType": "sc:Integer", "description": "Number of casualties in the collision."}
      ]
    }
  ]
}
```

---

## Validation script

The script below documents how to validate the **structure** of locally-held STATS19 CSV files.
It does **not** download, host, or republish any data — the user supplies their own local file
path. It checks expected columns and basic row sanity only.

```python
#!/usr/bin/env python3
"""Validate the structure of a locally-held STATS19 collision CSV.

Documentation/QA only: does NOT download, host, mirror, or transform the dataset.
Point it at a file you have already obtained from the official DfT source under OGL v3.0.

Usage:
    python validate_stats19.py /path/to/dft-road-safety-collision.csv
"""
import csv
import sys

# Verified collision-table columns (current DfT/stats19 schema).
EXPECTED_COLLISION_COLUMNS = {
    "collision_index", "collision_year", "collision_severity", "date",
    "day_of_week", "time", "location_easting_osgr", "location_northing_osgr",
    "longitude", "latitude", "police_force", "number_of_vehicles",
    "number_of_casualties", "first_road_class", "road_type", "speed_limit",
    "urban_or_rural_area", "junction_detail", "light_conditions",
    "weather_conditions", "road_surface_conditions", "local_authority_ons_district",
}

def validate(path: str) -> int:
    with open(path, newline="", encoding="utf-8-sig") as fh:
        reader = csv.reader(fh)
        try:
            header = set(next(reader))
        except StopIteration:
            print("FAIL: file is empty")
            return 1
        rows = sum(1 for _ in reader)

    # Accept legacy accident_* naming as an alias.
    norm = {c.replace("accident_", "collision_") for c in header}
    missing = EXPECTED_COLLISION_COLUMNS - norm
    ok = True
    if missing:
        print(f"WARN: missing expected columns: {sorted(missing)}")
        ok = False
    if rows < 1:
        print("FAIL: no data rows found")
        ok = False
    print(f"INFO: {len(header)} columns, {rows} data rows")
    print("PASS: structure looks valid" if ok else "CHECK: review warnings above")
    return 0 if ok else 1

if __name__ == "__main__":
    if len(sys.argv) != 2:
        print(__doc__)
        sys.exit(2)
    sys.exit(validate(sys.argv[1]))
```

---

## Limitations & gaps

- **Vehicle and casualty field lists are NOT individually verified.** The DfT "Road Safety Open
  Data Guide 2024" workbook (the authoritative variable/lookup source) was referenced on the
  catalogue page but could not be retrieved in this pass (HTTP 404 on the attempted .xlsx URL).
  Those tables' columns are marked "unverified" and reflect the commonly-documented STATS19
  schema only. Re-verify against the official guide before relying on them.
- **Collision-table fields** were verified against authoritative STATS19 schema documentation,
  but exact coded-value lookups (numeric code → label) were not enumerated here; consult the DfT
  data guide for full lookups.
- **No data was sampled or downloaded.** Per Hee-Lee Oss guardrails this was a documentation-only pass;
  no header/streamed read or ≤1000-row sample was taken, so row counts, null rates, and a
  data-quality report were not produced from real data. The acceptance criterion for a sampled
  data dictionary + data-quality report is therefore only partially met.
- **Licence snapshot/hash not committed.** The OGL v3.0 text was read and quoted (derivatives
  confirmed) but an archived copy + hash artifact was not committed (guardrail: do not commit
  external content). This should be produced at the per-dataset gate.
- **Residual PII / disclosure risk.** The data is publisher-anonymised to street level and
  carries no direct identifiers (no names/addresses); casualty and driver rows include
  de-identified demographic attributes (age, sex). DfT publishes it as open data under OGL on
  this basis, so it is treated here as low residual PII and NOT flagged. However, a small
  re-identification/disclosure risk can remain when combining rare attribute combinations with
  external data — do not attempt re-identification, and review at the gate before any reuse.
- Variable naming has migrated (`accident_*` → `collision_*`); older releases and third-party
  tools may differ.
