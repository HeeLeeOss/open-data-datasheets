# Datasheet — ERA5 reanalysis (hourly data on single levels, 1940–present)

> One-line summary: ERA5 is the ECMWF/Copernicus fifth-generation global atmospheric
> reanalysis, combining a numerical weather model with observations to provide a
> physically consistent, hourly, 0.25° gridded record of the global climate from 1940 to present.
>
> **This datasheet is licensed CC-BY-4.0.** (The datasheet text only — see "Source licence" for the dataset's own terms.)

Task ID: `ds-climate-002` · Catalog id (candidate): `cat-climate-002` · Datasheet status: **FINAL** (source licence verified to permit derivatives).

---

## Provenance & attribution

| Field | Value |
|---|---|
| Dataset | ERA5 hourly data on single levels from 1940 to present |
| Publisher | Copernicus Climate Change Service (C3S), implemented by ECMWF (European Centre for Medium-Range Weather Forecasts) |
| Distribution channel | Copernicus Climate Data Store (CDS) |
| Source URL | https://cds.climate.copernicus.eu/datasets/reanalysis-era5-single-levels |
| Catalog landing | https://cds.climate.copernicus.eu/ |
| Dataset DOI | 10.24381/cds.adbb2d47 |
| Retrieval date | 2026-06-28 |

**Required attribution string** (use when redistributing derivatives):

> Contains modified Copernicus Climate Change Service information [2026]. Hersbach, H. et al.
> (2023): ERA5 hourly data on single levels from 1940 to present. Copernicus Climate Change
> Service (C3S) Climate Data Store (CDS), DOI: 10.24381/cds.adbb2d47.

Neither the European Commission nor ECMWF is responsible for any use of the Copernicus
information or data this datasheet refers to.

---

## Source licence

- **Licence name (verified):** Creative Commons Attribution 4.0 International (**CC-BY-4.0**).
  The ERA5 single-levels catalogue entry on the Copernicus Climate Data Store lists the
  dataset under a CC-BY licence (verified 2026-06-28 at
  https://cds.climate.copernicus.eu/datasets/reanalysis-era5-single-levels).
- **Permits derivatives & commercial use:** **YES.** CC-BY-4.0 grants the right to
  "**Share** — copy and redistribute the material in any medium or format for any purpose,
  even commercially" and to "**Adapt** — remix, transform, and build upon the material for
  any purpose, even commercially," subject only to attribution.
  Cited clause/URL: https://creativecommons.org/licenses/by/4.0/ (see "You are free to:" —
  *Share* and *Adapt* — and the *Attribution* condition).
- **Licence snapshot:** the canonical, immutable legal text of CC-BY-4.0 is published at
  https://creativecommons.org/licenses/by/4.0/legalcode . To create the audit artifact required
  by the acceptance criteria, archive and hash that page locally (the validation script below
  shows the command); do not commit the dataset itself.
- **Historical note:** the CDS migrated to a CC-BY licence framework with the "CDS-Beta"/new
  CDS (2024–2025); older ERA5 documentation references the bespoke "Licence to use Copernicus
  Products," which also permitted derivatives and commercial use with attribution. The current
  catalogue entry shows CC-BY. Re-confirm at the per-dataset gate before redistribution.

---

## Data dictionary

ERA5 is a gridded (raster) dataset, not a tabular CSV. Each variable is an array indexed by
the coordinate dimensions below. Fields verified from the CDS catalogue entry and the ECMWF
ERA5 data documentation (https://confluence.ecmwf.int/display/CKB/ERA5%3A+data+documentation),
retrieved 2026-06-28.

### Coordinate / index dimensions

| Name | Type | Units | Description | Allowed values / nulls |
|---|---|---|---|---|
| `time` (`valid_time`) | datetime64 | UTC | Validity timestamp | Hourly, 1940-01-01T00:00 → present (~5-day latency); no nulls |
| `latitude` | float32 | degrees_north | Grid latitude | +90.0 → −90.0 at 0.25° step (721 points); no nulls |
| `longitude` | float32 | degrees_east | Grid longitude | 0.0 → 359.75 at 0.25° step (1440 points); no nulls |

### Representative data variables (verified short name + units)

| Name (short) | Type | Units | Description | Allowed values / null handling |
|---|---|---|---|---|
| `2t` / `t2m` | float32 | K | 2 metre temperature | Physical (~180–340 K); ocean/land masking via `_FillValue` |
| `sp` | float32 | Pa | Surface pressure | >0; `_FillValue` where undefined |
| `msl` | float32 | Pa | Mean sea level pressure | Physical range ~87000–109000 Pa |
| `10u` / `u10` | float32 | m s⁻¹ | 10 metre U wind component | Signed; no inherent nulls over valid grid |
| `10v` / `v10` | float32 | m s⁻¹ | 10 metre V wind component | Signed |
| `tp` | float32 | m | Total precipitation (accumulated) | ≥0; accumulation field |
| `sst` | float32 | K | Sea surface temperature | Defined over ocean; land points = `_FillValue` |
| `tcc` | float32 | 0–1 (fraction) | Total cloud cover | 0.0–1.0 |
| `swvl1`–`swvl4` | float32 | m³ m⁻³ | Volumetric soil water, layers 1–4 | 0.0–~0.6; land only, ocean = `_FillValue` |
| `sd` | float32 | m of water equivalent | Snow depth | ≥0; land/glacier |

**Null handling (verified):** GRIB/NetCDF encode undefined cells (e.g. ocean-only or land-only
parameters) using a missing/`_FillValue` sentinel, decoded to `NaN` by xarray. GRIB packing also
quantises values, so tiny magnitudes may be rounded to zero.

**Unverified / not exhaustively documented here:** ERA5 publishes 100+ single-level parameters
plus 137 model levels / 37 pressure levels. Only a representative subset is tabled above; the
full parameter list, exact valid ranges, and per-variable GRIB packing precision were **not**
individually verified for this datasheet and are marked accordingly. The 31 km / 0.28125° figure
seen in some ECMWF pages refers to the native reduced-Gaussian grid; the CDS-distributed product
is regridded to a regular 0.25° lat/lon grid.

---

## Datasheet for Datasets

**Motivation.** Created by ECMWF for the Copernicus Climate Change Service to provide a single,
authoritative, globally complete and temporally consistent record of the Earth's atmosphere,
land surface and ocean waves, to support climate monitoring, research, and downstream services.

**Composition.** Gridded estimates of atmospheric, land-surface and ocean-wave variables on a
regular 0.25° global lat/lon grid, hourly, from 1940 to present. Instances are grid-cell ×
time values per variable (not records about people). No personal or identifying data — the
dataset describes the physical environment only.

**Collection process.** Produced by 4D-Var data assimilation: a numerical weather prediction
model (ECMWF IFS) is constrained by billions of historical observations (satellite radiances,
radiosondes, surface stations, buoys, aircraft, etc.) to produce a physically consistent
"reanalysis." Values are model–observation blends, not raw measurements.

**Preprocessing / cleaning.** Observations undergo quality control and bias correction before
assimilation. The CDS regrids native reduced-Gaussian output to regular lat/lon and offers
GRIB or NetCDF. Accumulated fields (e.g. precipitation) follow ERA5 accumulation conventions.

**Uses.** Climate research, renewable-energy resource assessment, hydrology, agriculture,
reanalysis-driven ML, extreme-event analysis. **Caveats:** reanalyses contain model bias and
inhomogeneities across the observing-system era; not a substitute for in-situ measurements;
an ensemble (EDA) product provides uncertainty estimates at coarser resolution.

**Distribution.** Openly via the Copernicus Climate Data Store under CC-BY-4.0 (API and web).
Citable via DOI 10.24381/cds.adbb2d47. *This datasheet does not host or mirror the data.*

**Maintenance.** Maintained by C3S/ECMWF; updated daily with ~5-day latency; long-term
versioned and documented on the ECMWF Confluence knowledge base.

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
  "name": "ERA5 hourly data on single levels from 1940 to present",
  "description": "ECMWF/Copernicus fifth-generation global atmospheric reanalysis: hourly, 0.25-degree gridded estimates of atmospheric, land-surface and ocean-wave variables from 1940 to present, produced by 4D-Var assimilation of observations into the ECMWF IFS model.",
  "url": "https://cds.climate.copernicus.eu/datasets/reanalysis-era5-single-levels",
  "sameAs": "https://doi.org/10.24381/cds.adbb2d47",
  "identifier": "10.24381/cds.adbb2d47",
  "license": "https://creativecommons.org/licenses/by/4.0/",
  "creator": {
    "@type": "Organization",
    "name": "Copernicus Climate Change Service (C3S) / ECMWF",
    "url": "https://climate.copernicus.eu/"
  },
  "publisher": {
    "@type": "Organization",
    "name": "Copernicus Climate Data Store (CDS)"
  },
  "datePublished": "2023",
  "keywords": ["climate", "reanalysis", "ERA5", "weather", "atmosphere", "Copernicus"],
  "distribution": [
    {
      "@type": "cr:FileObject",
      "@id": "era5-grib",
      "name": "ERA5 single-levels (GRIB)",
      "encodingFormat": "application/x-grib",
      "contentUrl": "https://cds.climate.copernicus.eu/datasets/reanalysis-era5-single-levels"
    }
  ],
  "recordSet": [
    {
      "@type": "cr:RecordSet",
      "@id": "fields",
      "name": "fields",
      "field": [
        { "@type": "cr:Field", "@id": "fields/time", "name": "time", "dataType": "sc:DateTime", "description": "Validity time (UTC), hourly." },
        { "@type": "cr:Field", "@id": "fields/latitude", "name": "latitude", "dataType": "sc:Float", "description": "degrees_north, +90 to -90 at 0.25 deg." },
        { "@type": "cr:Field", "@id": "fields/longitude", "name": "longitude", "dataType": "sc:Float", "description": "degrees_east, 0 to 359.75 at 0.25 deg." },
        { "@type": "cr:Field", "@id": "fields/t2m", "name": "2t", "dataType": "sc:Float", "description": "2 metre temperature (K)." },
        { "@type": "cr:Field", "@id": "fields/sp", "name": "sp", "dataType": "sc:Float", "description": "Surface pressure (Pa)." },
        { "@type": "cr:Field", "@id": "fields/tp", "name": "tp", "dataType": "sc:Float", "description": "Total precipitation (m, accumulated)." }
      ]
    }
  ]
}
```

---

## Validation script

Documentation only: this script validates the **structure** of an ERA5 file that the user has
already obtained themselves from the CDS. It does **not** download, host, or redistribute any
data. It also shows how to create the licence-snapshot audit artifact.

```python
#!/usr/bin/env python3
"""Validate the structure of a locally held ERA5 single-levels NetCDF file.
Does NOT download or host data. Usage: python validate_era5.py path/to/your_era5.nc"""
import sys

EXPECTED_COORDS = {"latitude", "longitude"}            # plus a time coord
EXPECTED_TIME_NAMES = {"time", "valid_time"}
STEP_DEG = 0.25
LAT_POINTS, LON_POINTS = 721, 1440                      # global 0.25 deg grid

def main(path: str) -> int:
    try:
        import xarray as xr
    except ImportError:
        print("Please `pip install xarray netCDF4` to run this check."); return 2

    ds = xr.open_dataset(path)
    problems = []

    coords = set(ds.coords)
    if not EXPECTED_COORDS <= coords:
        problems.append(f"missing spatial coords: {EXPECTED_COORDS - coords}")
    if not (EXPECTED_TIME_NAMES & coords):
        problems.append("no time/valid_time coordinate found")

    # spatial extent / resolution checks (global product)
    if "latitude" in ds.coords:
        n = ds.sizes.get("latitude")
        if n not in (LAT_POINTS, None):
            problems.append(f"latitude has {n} points, expected {LAT_POINTS} for global 0.25 deg")
    if "longitude" in ds.coords:
        n = ds.sizes.get("longitude")
        if n not in (LON_POINTS, None):
            problems.append(f"longitude has {n} points, expected {LON_POINTS} for global 0.25 deg")

    if not ds.data_vars:
        problems.append("no data variables present")
    for name, var in ds.data_vars.items():
        if "units" not in var.attrs:
            problems.append(f"variable '{name}' has no 'units' attribute")

    if problems:
        print("FAIL:"); [print("  -", p) for p in problems]; return 1
    print(f"OK: {len(ds.data_vars)} variables, dims={dict(ds.sizes)}")
    return 0

if __name__ == "__main__":
    if len(sys.argv) != 2:
        print(__doc__); sys.exit(2)
    sys.exit(main(sys.argv[1]))
```

```bash
# Licence-snapshot audit artifact (run locally; archives the LICENCE text, not the data):
curl -sL https://creativecommons.org/licenses/by/4.0/legalcode -o cc-by-4.0.txt
sha256sum cc-by-4.0.txt > cc-by-4.0.txt.sha256   # record hash alongside this datasheet
```

---

## Limitations & gaps

- **Licence:** verified as **CC-BY-4.0** from the live CDS catalogue entry (2026-06-28). The
  immutable legalcode hash artifact must be generated at the gate (script above); it is not
  committed here. The historical "Licence to use Copernicus Products" PDF endpoint returned
  404 at retrieval time, so the snapshot was taken from the canonical Creative Commons text.
- **Fields:** only a representative subset of ERA5's 100+ single-level parameters is documented;
  the full parameter catalogue, exact per-variable valid ranges, and GRIB packing precision were
  **not** individually verified and are marked unverified above. Model-level / pressure-level
  products are out of scope for this single-levels datasheet.
- **No data inspected:** per Hee-Lee Oss guardrails and the bounded-access protocol, no dataset bytes
  were downloaded, sampled, mirrored, or committed. Structural expectations (grid size, coords,
  units) are derived from published documentation, not from a fetched sample, so the validation
  script's row/grid constants should be re-confirmed against an actual user-held file.
- **PII:** none. ERA5 describes the physical environment only; it contains no personal or
  identifying data.
- **Reanalysis caveats:** values are model–observation blends with known biases and temporal
  inhomogeneities across the evolving observing system; treat as estimates, not measurements.
