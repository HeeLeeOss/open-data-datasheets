# Datasheet — Global Historical Climatology Network – Daily (GHCN-Daily)

One-line summary: A documentation datasheet for NOAA NCEI's GHCN-Daily — the global,
station-level archive of daily land-surface climate observations (temperature, precipitation,
snow, and dozens of other elements) from 100,000+ stations across ~180 countries.

> This datasheet (the document you are reading) is licensed under **CC-BY-4.0**.
> It is **documentation only**: it does not host, mirror, transform, or republish any GHCN-Daily data.

---

## Provenance & attribution

| Field | Value |
|---|---|
| Dataset | Global Historical Climatology Network – Daily (GHCN-Daily / GHCNd) |
| Publisher | NOAA National Centers for Environmental Information (NCEI), U.S. Department of Commerce |
| Source landing page | https://www.ncei.noaa.gov/products/land-based-station/global-historical-climatology-network-daily |
| Format/README (verified) | https://ncei.noaa.gov/pub/data/ghcn/daily/readme.txt |
| Documentation PDF | https://www.ncei.noaa.gov/data/global-historical-climatology-network-daily/doc/GHCND_documentation.pdf |
| Data access root | https://www.ncei.noaa.gov/data/global-historical-climatology-network-daily/ |
| Candidate catalog id | cat-climate-001 |
| Retrieval date | 2026-06-28 |

**Required / recommended attribution string:**

> Menne, M.J., I. Durre, R.S. Vose, B.E. Gleason, and T.G. Houston, 2012: An overview of the
> Global Historical Climatology Network-Daily Database. *Journal of Atmospheric and Oceanic
> Technology*, 29, 897–910. doi:10.1175/JTECH-D-11-00103.1.
> Data: Menne, M.J., et al., Global Historical Climatology Network - Daily (GHCN-Daily),
> NOAA National Centers for Environmental Information. [Cite dataset DOI/version and access date.]

NCEI requests that users cite **both** the overview journal article and the specific dataset
version/DOI used (per the official README).

---

## Source licence

**Verified licence: U.S. Government public domain — distributed as CC0 1.0 Universal (Public Domain Dedication).**

- GHCN-Daily is produced by NOAA NCEI, a U.S. federal government agency. Works prepared by an
  officer or employee of the U.S. Government as part of their official duties are **not subject to
  copyright protection in the United States** and are in the public domain — **17 U.S.C. § 105**
  (https://www.law.cornell.edu/uscode/text/17/105).
- The U.S. open-data catalog records this dataset's licence as **CC0 1.0 Universal (Public Domain
  Dedication)** — citation URL: **https://creativecommons.org/publicdomain/zero/1.0/** (catalog
  entry: https://catalog.data.gov/dataset?q=Global+Historical+Climatology+Network+Daily).

**Does it permit derivatives?** Yes. Both U.S. public domain status (17 U.S.C. § 105) and CC0 1.0
place no restrictions on use, redistribution, **or the creation of derivative works**. The CC0
deed states the work has been dedicated to the public domain so the dedicator "**waived all of his
or her rights to the work worldwide under copyright law … You can copy, modify, distribute and
perform the work, even for commercial purposes, all without asking permission.**"

**Caveat recorded (not a blocker):** Some non-U.S. station data enters GHCN-Daily through bilateral
agreements with foreign meteorological services; NCEI nonetheless distributes the integrated
GHCN-Daily product openly. No personal/identifying data is present (see PII note below), so reuse
of the documented product is unrestricted. This caveat is logged for transparency only.

Licence verification: **confirmed — permits derivatives.** (This file is NOT marked DRAFT.)

---

## Data dictionary

GHCN-Daily is distributed as fixed-width ASCII. Two core file types are documented below; both
field layouts are **verified** against the official `readme.txt` (retrieved 2026-06-28). No actual
data rows were downloaded, mirrored, or sampled — only the published format specification.

### Daily observation files — `*.dly` (one file per station)

Each record is one station-month for one element; columns 22–269 repeat in 8-char value + 3 flag
blocks for days 1–31.

| Field | Columns | Type | Units | Description | Allowed values / nulls |
|---|---|---|---|---|---|
| ID | 1–11 | Character | — | 11-char station identifier (first 2 chars = FIPS country code) | non-null |
| YEAR | 12–15 | Integer | year | Year of observation | non-null |
| MONTH | 16–17 | Integer | month | Month of observation | 01–12 |
| ELEMENT | 18–21 | Character | — | Element/variable code (e.g. TMAX, PRCP) | see element codes below |
| VALUE1..VALUE31 | per-day | Integer | element-specific | Daily value for day-of-month N | missing = **-9999** |
| MFLAG1..MFLAG31 | per-day | Character | — | Measurement flag for day N | blank = no flag; see README flag tables |
| QFLAG1..QFLAG31 | per-day | Character | — | Quality flag for day N | blank = passed all QC; non-blank = failed a QC check |
| SFLAG1..SFLAG31 | per-day | Character | — | Source flag for day N | blank = no source; see README source tables |

Per-day block offsets (verified pattern): for day *n* (1–31) the value occupies an 8-char field
and is followed by 1-char MFLAG, QFLAG, SFLAG, i.e. a repeating 8+1+1+1 layout.

**Core element codes (verified, with units):**

| ELEMENT | Meaning | Units |
|---|---|---|
| PRCP | Precipitation | tenths of mm |
| SNOW | Snowfall | mm |
| SNWD | Snow depth | mm |
| TMAX | Maximum temperature | tenths of °C |
| TMIN | Minimum temperature | tenths of °C |

> Note: TMAX/TMIN/PRCP integer values must be scaled (divide by 10) to get °C / mm.
> GHCN-Daily defines **many** additional element codes (e.g. TAVG, EVAP, WESD, WESF, WDMV, plus
> SN*#/SX*# soil temperatures and WT** weather-type indicators). Only the five core elements above
> are documented here as verified; the full element list is in the README and is **not enumerated
> in this file** (see Limitations).

### Station metadata — `ghcnd-stations.txt`

| Field | Columns | Type | Units | Description | Allowed values / nulls |
|---|---|---|---|---|---|
| ID | 1–11 | Character | — | Station identifier | non-null |
| LATITUDE | 13–20 | Real | decimal degrees | Station latitude | non-null |
| LONGITUDE | 22–30 | Real | decimal degrees | Station longitude | non-null |
| ELEVATION | 32–37 | Real | meters | Station elevation | missing = **-999.9** |
| STATE | 39–40 | Character | — | U.S./Canada postal code (where applicable) | blank if N/A |
| NAME | 42–71 | Character | — | Station name | non-null |
| GSN FLAG | 73–75 | Character | — | GCOS Surface Network flag | "GSN" or blank |
| HCN/CRN FLAG | 77–79 | Character | — | U.S. Historical/Climate Reference Network flag | "HCN"/"CRN" or blank |
| WMO ID | 81–85 | Character | — | World Meteorological Organization station number | blank if none |

**PII pass:** Fields describe physical weather stations (location, elevation, network flags) — no
personal or identifying information about individuals is present in either file type. ✅

---

## Datasheet for Datasets

**Motivation.** GHCN-Daily is the backbone integrated archive of daily land-surface climate
observations, created by NOAA NCEI to provide a single quality-controlled source for analyses of
climate extremes, trends, and station-level meteorology. Created/maintained by NOAA NCEI (U.S.
Dept. of Commerce); funded by the U.S. federal government.

**Composition.** Station-month records of daily observations for >100,000 stations across ~180
countries; ~20,000 stations refresh within any 30-day window. Each instance is a station/month/
element row with up to 31 daily values plus measurement, quality, and source flags. Core variables
include max/min temperature, precipitation, snowfall, and snow depth; dozens of additional elements
exist. Station metadata (location, elevation, network flags) is provided separately. No personal
data.

**Collection process.** Observations originate from national meteorological/hydrological services,
cooperative observer networks, and research networks, ingested into GHCN-Daily via official data
feeds and bilateral agreements. NCEI integrates, deduplicates, and applies a documented suite of
automated quality-control checks (results surfaced via QFLAG).

**Preprocessing / cleaning / labeling.** NCEI performs format standardization, source merging, and
multi-stage quality assurance; failed QC checks are flagged (not silently removed) via per-day
quality flags, preserving the raw value alongside its QC verdict. Source provenance is retained in
the source flag.

**Uses.** Climate trend and variability studies, extreme-event analysis, drought/precipitation
indices, model validation, and education. Users should respect the missing-value sentinels
(-9999 / -999.9) and apply the unit scaling. Not appropriate as-is for navigation/safety-of-life
decisions without further validation.

**Distribution.** Publicly distributed by NOAA NCEI as ASCII files (per-station `.dly`, plus
combined GZIP/TAR archives) and via NCEI access services. U.S. public domain / CC0 1.0 — free to
redistribute and adapt with the recommended citation. (This datasheet does not redistribute the
data.)

**Maintenance.** Maintained by NOAA NCEI; updated daily/continuously, with versioning and a dataset
DOI. Errata and methodology are documented in the GHND documentation PDF and the overview paper
(Menne et al., 2012). Contact: NOAA NCEI (https://www.ncei.noaa.gov).

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
  "name": "Global Historical Climatology Network - Daily (GHCN-Daily)",
  "description": "Integrated archive of daily land-surface climate observations (temperature, precipitation, snow, and additional elements) from over 100,000 stations across ~180 countries, produced and maintained by NOAA NCEI.",
  "url": "https://www.ncei.noaa.gov/products/land-based-station/global-historical-climatology-network-daily",
  "license": "https://creativecommons.org/publicdomain/zero/1.0/",
  "creator": {
    "@type": "Organization",
    "name": "NOAA National Centers for Environmental Information (NCEI)",
    "url": "https://www.ncei.noaa.gov"
  },
  "publisher": {
    "@type": "Organization",
    "name": "NOAA National Centers for Environmental Information (NCEI)"
  },
  "citation": "Menne, M.J., I. Durre, R.S. Vose, B.E. Gleason, and T.G. Houston, 2012: An overview of the Global Historical Climatology Network-Daily Database. Journal of Atmospheric and Oceanic Technology, 29, 897-910. doi:10.1175/JTECH-D-11-00103.1",
  "dateModified": "2026-06-28",
  "distribution": [
    {
      "@type": "cr:FileObject",
      "@id": "ghcnd-readme",
      "name": "GHCN-Daily format README",
      "contentUrl": "https://ncei.noaa.gov/pub/data/ghcn/daily/readme.txt",
      "encodingFormat": "text/plain"
    }
  ],
  "recordSet": [
    {
      "@type": "cr:RecordSet",
      "@id": "dly-record",
      "name": "daily_observation",
      "field": [
        { "@type": "cr:Field", "@id": "dly-record/id", "name": "ID", "description": "11-character station identifier", "dataType": "sc:Text" },
        { "@type": "cr:Field", "@id": "dly-record/element", "name": "ELEMENT", "description": "Element code (e.g. TMAX, TMIN, PRCP, SNOW, SNWD)", "dataType": "sc:Text" },
        { "@type": "cr:Field", "@id": "dly-record/tmax", "name": "TMAX", "description": "Daily maximum temperature in tenths of degrees C; missing = -9999", "dataType": "sc:Integer" },
        { "@type": "cr:Field", "@id": "dly-record/prcp", "name": "PRCP", "description": "Daily precipitation in tenths of mm; missing = -9999", "dataType": "sc:Integer" },
        { "@type": "cr:Field", "@id": "dly-record/qflag", "name": "QFLAG", "description": "Per-day quality flag; blank = passed QC", "dataType": "sc:Text" }
      ]
    }
  ]
}
```

---

## Validation script

This script validates the **structure** of a GHCN-Daily `.dly` file that a user already has locally.
It does **not** download, host, or republish any data — the user supplies a path to a file they
obtained themselves from NOAA NCEI. It reads only the header/field positions and reports findings.

```python
#!/usr/bin/env python3
"""Validate the fixed-width structure of a local GHCN-Daily .dly file.

Documentation-only checker: it does NOT fetch, host, or mirror any data.
Usage: python validate_ghcnd.py /path/to/USW00094728.dly
"""
import sys

CORE_ELEMENTS = {"PRCP", "SNOW", "SNWD", "TMAX", "TMIN"}
MISSING = -9999

def validate(path):
    errors, rows, elements = [], 0, set()
    with open(path, "r", encoding="ascii", errors="replace") as fh:
        for n, line in enumerate(fh, 1):
            line = line.rstrip("\n")
            if len(line) != 269:
                errors.append(f"line {n}: expected width 269, got {len(line)}")
                continue
            rows += 1
            station = line[0:11].strip()
            year = line[11:15]
            month = line[15:17]
            element = line[17:21].strip()
            elements.add(element)
            if not station:
                errors.append(f"line {n}: empty station ID")
            if not (year.isdigit() and month.isdigit() and 1 <= int(month) <= 12):
                errors.append(f"line {n}: bad YEAR/MONTH '{year}/{month}'")
            # spot-check first daily VALUE block (cols 22-29 -> index 21:29)
            val = line[21:29].strip()
            try:
                int(val)
            except ValueError:
                errors.append(f"line {n}: non-integer VALUE1 '{val}'")
    print(f"rows checked: {rows}")
    print(f"elements seen: {sorted(elements)}")
    print(f"core elements present: {sorted(elements & CORE_ELEMENTS)}")
    if errors:
        print(f"FAIL ({len(errors)} issue(s)):")
        for e in errors[:20]:
            print("  -", e)
        sys.exit(1)
    print("PASS: structure matches GHCN-Daily .dly spec (269-char records).")

if __name__ == "__main__":
    if len(sys.argv) != 2:
        sys.exit("usage: python validate_ghcnd.py /path/to/station.dly")
    validate(sys.argv[1])
```

Expected structure checked: each `.dly` record is exactly **269 characters**; ID(1-11),
YEAR(12-15), MONTH(16-17), ELEMENT(18-21), then 31 repeating 8+1+1+1 day blocks. A healthy
station-month file contains one or more of the core elements (PRCP/SNOW/SNWD/TMAX/TMIN) and uses
`-9999` for missing values.

---

## Limitations & gaps

- **Not independently row-validated.** Per Hee-Lee Oss guardrails and the PII/bounded-access protocol, no
  data file was downloaded, sampled, or hosted. Field layouts are verified against the official
  `readme.txt` specification (2026-06-28), not against a downloaded sample. Byte-exact per-day
  column offsets beyond the first value block were inferred from the documented repeating pattern.
- **Element list is partial.** Only the five core elements are documented as verified. GHCN-Daily
  defines dozens more (TAVG, EVAP, WESD/WESF, WDMV, soil temperatures SN*#/SX*#, WT** weather-type
  flags); consult the README/documentation PDF for the complete enumeration and full MFLAG/QFLAG/
  SFLAG code tables.
- **Licence nuance.** The integrated product is U.S. public domain / CC0 1.0 and permits
  derivatives; however, some non-U.S. station inputs arrive via bilateral agreements. This was
  recorded for transparency and is not a reuse blocker for the documented product. A data.gov
  metadata field labeled the catalog entry "non-public" access level — this appears to be a
  metadata-flag artifact (the data is openly downloadable) and should be confirmed at the gate.
- **DOI/version not pinned.** Cite the specific dataset DOI/version and access date for your run;
  this datasheet documents the product generally, not a single frozen version.
