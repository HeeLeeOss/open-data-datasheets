# Datasheet: NASA Exoplanet Archive (Planetary Systems table)

One-line summary: An authoritative, continuously updated catalog of confirmed exoplanets and
their host stars, operated by NASA/IPAC at Caltech; this datasheet documents its primary
**Planetary Systems (PS)** table.

*This datasheet is licensed under **CC-BY-4.0**. It is documentation only — it does not host,
mirror, transform, or republish any of the underlying dataset.*

---

## Provenance & attribution

| Field | Value |
|---|---|
| Dataset | NASA Exoplanet Archive — Planetary Systems (PS) table |
| Publisher | NASA / IPAC (Infrared Processing and Analysis Center), California Institute of Technology, under contract with NASA's Exoplanet Exploration Program |
| Source URL | https://exoplanetarchive.ipac.caltech.edu/ |
| Column docs | https://exoplanetarchive.ipac.caltech.edu/docs/API_PS_columns.html |
| Acknowledgment docs | https://exoplanetarchive.ipac.caltech.edu/docs/acknowledge.html |
| Catalog id (Hee-Lee Oss) | cat-science-002 |
| Retrieval date | 2026-06-28 |

**Required attribution string** (verbatim, from the archive's *Acknowledging the Archive* page):

> "This research has made use of the NASA Exoplanet Archive, which is operated by the
> California Institute of Technology, under contract with the National Aeronautics and Space
> Administration under the Exoplanet Exploration Program."

For images/figures, the IPAC credit line is **"Courtesy NASA/JPL-Caltech"**.

---

## Source licence

**Verified: reuse and derivative works are permitted.**

The NASA Exoplanet Archive is a work of the U.S. Government produced under NASA contract and
released to the public; U.S. Government works are not subject to domestic copyright protection
under **17 U.S.C. § 105**, placing them in the public domain. In addition, the operating
institution's reuse policy explicitly permits derivatives:

- **IPAC Image Use Policy** (https://www.ipac.caltech.edu/page/image-use-policy), which governs
  sites ending in `ipac.caltech.edu` (the archive's host):
  > "images and video on the IPAC public web sites (public sites ending with a
  > ipac.caltech.edu address) may be used for any purpose without prior permission"

  Conditions: a credit line is required ("Courtesy NASA/JPL-Caltech" unless otherwise noted),
  and endorsement by Caltech/JPL/NASA "must not be claimed or implied". NASA/JPL logos require
  separate written approval. These conditions are satisfied by attribution and are compatible
  with derivative use.

**Effective licence status:** Public domain (U.S. Government work, 17 U.S.C. § 105) +
IPAC reuse-for-any-purpose-with-attribution. Derivatives permitted. Closest SPDX-style
expression for the Croissant record: `CC0-1.0` / public-domain equivalent, with the attribution
and non-endorsement conditions above honored as good practice.

### Licence snapshot (hash + archived text)

Acknowledgment text captured verbatim above on 2026-06-28.

- SHA-256 of the acknowledgment string:
  `de46f3c795d1b77166e3b06a8bb57ebf4e25867ff5b8f52be7d7051cc9697d0c`
- Reuse clause source: IPAC Image Use Policy (quoted verbatim under *Source licence*).

> Note: The archive states the acknowledgment/citation requirement but does not, on the
> documentation pages reviewed, post a single dedicated "data licence" page; the public-domain
> conclusion rests on 17 U.S.C. § 105 (U.S. Government work) reinforced by the IPAC reuse
> policy. This is recorded transparently rather than asserted as a posted dataset licence.

---

## Data dictionary

Fields below are verified from the archive's published PS column documentation
(`/docs/API_PS_columns.html`). The PS table has hundreds of columns (each measured parameter
typically also carries `*err1`, `*err2`, `*lim`, and reference/flag companions); the table
documents the core, most-used columns. Rows not directly confirmed from the docs are marked.

| Field (db column) | Type | Units | Description | Allowed values / null handling |
|---|---|---|---|---|
| `pl_name` | string | — | Planet name most commonly used in the literature | Non-null primary identifier |
| `hostname` | string | — | Host star name most commonly used in the literature | Non-null |
| `sy_snum` | integer | count | Number of gravitationally bound stars in the system | >= 1 |
| `sy_pnum` | integer | count | Number of confirmed planets in the system | >= 1 |
| `discoverymethod` | string | — | Method by which the planet was first identified | Controlled vocabulary, e.g. Transit, Radial Velocity, Imaging, Microlensing, Transit Timing Variations, Astrometry; nullable |
| `disc_year` | integer | year | Year the planet was discovered | 4-digit year; nullable |
| `pl_orbper` | float | days | Orbital period — time for one complete orbit | Nullable; has err/lim companions |
| `pl_orbsmax` | float | au | Orbit semi-major axis (longest radius of the elliptic orbit) | Nullable |
| `pl_rade` | float | Earth radii | Planet radius in Earth-radius units | Nullable |
| `pl_bmassj` | float | Jupiter masses | Best available planet mass estimate (mass or M*sin(i)) | Nullable; see `pl_bmassprov` for provenance |
| `pl_orbeccen` | float | dimensionless | Orbital eccentricity (deviation from a perfect circle) | 0 <= e < 1 typical; nullable |
| `st_teff` | float | K | Stellar effective temperature (blackbody model) | Nullable |
| `st_rad` | float | Solar radii | Stellar radius (center to surface) | Nullable |
| `st_mass` | float | Solar masses | Stellar mass | Nullable |
| `ra` | float | decimal degrees | Right ascension of the system | Non-null for catalogued systems |
| `dec` | float | decimal degrees | Declination of the system | Non-null for catalogued systems |
| `sy_dist` | float | parsecs (pc) | Distance to the planetary system | Nullable |
| `pl_bmassprov` | string | — | Provenance of the mass value in `pl_bmassj` (Mass vs. Msini) | *Unverified from quoted docs sample — companion column noted in archive schema* |
| `default_flag` | integer (0/1) | — | Marks the default parameter set for a planet (PS is multi-row per planet) | *Unverified from quoted docs sample — standard PS flag; confirm before relying* |

Notes:
- The PS table stores **one row per published parameter set**, so a single planet may have
  multiple rows; consumers typically filter to the default set. (Flag column above marked
  unverified pending direct doc confirmation.)
- Every measured numeric column in the full schema generally has `*err1` (upper),
  `*err2` (lower), and `*lim` (limit) companions and a reference string; not enumerated here.

---

## Datasheet for Datasets

**Motivation.** Created to provide the astronomical community with an authoritative,
peer-reviewed-literature-sourced, continuously curated catalog of confirmed exoplanets and host
stars, supporting research, mission planning, and public outreach. Operated by NASA/IPAC under
NASA's Exoplanet Exploration Program.

**Composition.** Each instance is a confirmed planet (or a published parameter set for a
planet) with associated host-star and system properties: identifiers, discovery metadata,
orbital parameters, planet physical parameters, stellar parameters, sky coordinates, and
distance. Related archive tables include Stellar Hosts, Transiting Planets, Atmospheric
Spectroscopy, and mission-specific tables (TESS, Kepler/K2). No personal or human-subject data.

**Collection process.** Values are compiled from the refereed astronomical literature and
mission pipelines; parameters are ingested with references and uncertainties. Discovery methods
include transit, radial velocity, imaging, microlensing, transit-timing variations, and
astrometry. The archive is updated regularly as new results are published.

**Preprocessing / cleaning / labeling.** Parameters are organized into a uniform schema with
standardized units (days, au, Earth/Jupiter/Solar units, K, pc, decimal degrees), error and
limit companion columns, default-parameter flags, and provenance fields. The archive maintains
multiple parameter sets per object rather than discarding alternatives.

**Uses.** Population statistics of exoplanets, target selection for follow-up and missions,
mass-radius and habitability studies, education and outreach. Caveat: it is a heterogeneous
literature compilation — values come from different methods/instruments with differing
systematics; use error columns and references.

**Distribution.** Publicly available at exoplanetarchive.ipac.caltech.edu via interactive
tables, bulk downloads, and the TAP/API service. Public domain (U.S. Government work); reuse
including derivatives is permitted with attribution and no implied endorsement.

**Maintenance.** Maintained by the NASA Exoplanet Science Institute (NExScI) / IPAC at Caltech
under NASA contract; continuously updated. Versioning is via dated releases and the parameter
reference columns. Contact and update channels are on the archive site.

---

## Croissant metadata

```json
{
  "@context": {
    "@vocab": "https://schema.org/",
    "cr": "http://mlcommons.org/croissant/",
    "dataType": "cr:dataType",
    "field": "cr:field",
    "recordSet": "cr:recordSet"
  },
  "@type": "Dataset",
  "name": "NASA Exoplanet Archive - Planetary Systems (PS) table",
  "description": "Authoritative, continuously updated catalog of confirmed exoplanets and their host stars, compiled from the refereed literature and space-mission pipelines by NASA/IPAC at Caltech.",
  "url": "https://exoplanetarchive.ipac.caltech.edu/",
  "sameAs": "https://exoplanetarchive.ipac.caltech.edu/docs/API_PS_columns.html",
  "license": "https://www.ipac.caltech.edu/page/image-use-policy",
  "creditText": "This research has made use of the NASA Exoplanet Archive, which is operated by the California Institute of Technology, under contract with the National Aeronautics and Space Administration under the Exoplanet Exploration Program.",
  "creator": {
    "@type": "Organization",
    "name": "NASA Exoplanet Science Institute (NExScI) / IPAC, California Institute of Technology",
    "url": "https://exoplanetarchive.ipac.caltech.edu/"
  },
  "datePublished": "2026-06-28",
  "version": "retrieved-2026-06-28",
  "recordSet": [
    {
      "@type": "cr:RecordSet",
      "name": "planetary_systems",
      "field": [
        { "@type": "cr:Field", "name": "pl_name", "description": "Planet name most commonly used in the literature", "dataType": "Text" },
        { "@type": "cr:Field", "name": "hostname", "description": "Host star name most commonly used in the literature", "dataType": "Text" },
        { "@type": "cr:Field", "name": "discoverymethod", "description": "Method by which the planet was first identified", "dataType": "Text" },
        { "@type": "cr:Field", "name": "disc_year", "description": "Year the planet was discovered", "dataType": "Integer" },
        { "@type": "cr:Field", "name": "pl_orbper", "description": "Orbital period in days", "dataType": "Float" },
        { "@type": "cr:Field", "name": "pl_rade", "description": "Planet radius in Earth radii", "dataType": "Float" },
        { "@type": "cr:Field", "name": "pl_bmassj", "description": "Best planet mass estimate in Jupiter masses", "dataType": "Float" },
        { "@type": "cr:Field", "name": "st_teff", "description": "Stellar effective temperature in Kelvin", "dataType": "Float" },
        { "@type": "cr:Field", "name": "sy_dist", "description": "Distance to the system in parsecs", "dataType": "Float" }
      ]
    }
  ]
}
```

---

## Validation script

Documentation only — this script does **not** download, host, or republish the dataset. Point
it at a CSV you have lawfully obtained yourself from the archive's official export/TAP service;
it only checks that the structure matches this datasheet.

```python
#!/usr/bin/env python3
"""Validate the structure of a locally-obtained NASA Exoplanet Archive PS export.

Does NOT fetch, host, or redistribute data. You supply a CSV you exported yourself.
Usage: python validate_ps.py path/to/your_export.csv
"""
import csv
import sys

# Core columns documented in this datasheet (subset of the full PS schema).
EXPECTED_COLUMNS = [
    "pl_name", "hostname", "sy_snum", "sy_pnum", "discoverymethod",
    "disc_year", "pl_orbper", "pl_orbsmax", "pl_rade", "pl_bmassj",
    "pl_orbeccen", "st_teff", "st_rad", "st_mass", "ra", "dec", "sy_dist",
]
KNOWN_METHODS = {
    "Transit", "Radial Velocity", "Imaging", "Microlensing",
    "Transit Timing Variations", "Astrometry", "Eclipse Timing Variations",
    "Orbital Brightness Modulation", "Pulsar Timing", "Disk Kinematics",
    "Pulsation Timing Variations", "Astrometry/TTV",
}

def main(path: str) -> int:
    with open(path, newline="", encoding="utf-8") as f:
        # skip archive comment lines beginning with '#'
        rows = [r for r in f if not r.lstrip().startswith("#")]
    reader = csv.DictReader(rows)
    header = reader.fieldnames or []

    missing = [c for c in EXPECTED_COLUMNS if c not in header]
    if missing:
        print(f"FAIL: missing expected columns: {missing}")
        return 1

    n = 0
    issues = 0
    for row in reader:
        n += 1
        if not row.get("pl_name"):
            issues += 1
        m = (row.get("discoverymethod") or "").strip()
        if m and m not in KNOWN_METHODS:
            print(f"WARN row {n}: unrecognized discoverymethod '{m}'")
        y = (row.get("disc_year") or "").strip()
        if y and not (y.isdigit() and 1989 <= int(y) <= 2100):
            print(f"WARN row {n}: implausible disc_year '{y}'")

    if n == 0:
        print("FAIL: zero data rows")
        return 1
    print(f"OK: {n} rows; {len(header)} columns; {issues} rows with empty pl_name")
    print("Note: PS stores multiple parameter sets per planet; row count >> planet count.")
    return 0

if __name__ == "__main__":
    if len(sys.argv) != 2:
        print(__doc__)
        sys.exit(2)
    sys.exit(main(sys.argv[1]))
```

---

## Limitations & gaps

- **Licence basis is inferred, not a posted data-licence page.** The archive posts an
  *acknowledgment* requirement and IPAC posts an image/content reuse policy, but no single
  dedicated tabular-data licence file was found on the pages reviewed. The "derivatives
  permitted" conclusion rests on (a) 17 U.S.C. § 105 (U.S. Government work, public domain) and
  (b) the IPAC reuse-for-any-purpose policy. Both are cited; a reviewer should confirm at the
  gate.
- **Schema is partial.** The PS table has hundreds of columns; only ~17 core columns are
  documented here, plus two rows (`pl_bmassprov`, `default_flag`) explicitly marked
  *unverified* from the quoted docs sample. Error (`*err1`/`*err2`), limit (`*lim`), and
  reference companion columns are described but not enumerated.
- **No data was sampled or downloaded** during this datasheet's creation (documentation-only
  guardrail); allowed-value lists (e.g., discovery methods) are from documentation/domain
  knowledge, not a fetched data sample, so the validation script's `KNOWN_METHODS` set may need
  extension.
- **No PII.** The dataset describes astronomical objects only; no personal or identifying data
  is present or documented.
- Values are a heterogeneous literature compilation; cross-method systematics apply and the
  per-row references/uncertainties should be consulted for scientific use.
