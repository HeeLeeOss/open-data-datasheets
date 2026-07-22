> # ⚠️ DRAFT — LICENCE GUARDRAIL FLAGGED (do not merge as-is)
>
> **Reason:** The task's first-pass licence assumption was `CC-BY-4.0` (permissive,
> commercial reuse allowed). Verification against ESA's own sources shows the actual
> licence is **CC BY-NC 3.0 IGO** — a **NonCommercial** licence. NonCommercial datasets
> are an explicit Hee-Lee Oss guardrail trigger (see `CLAUDE.md`: "Violates a source's license"
> / "Primarily benefits a for-profit entity"). Derivatives *are* permitted, but only for
> **non-commercial** use, and a separate authorisation is required for any commercial use.
> This datasheet is published as a DRAFT so a human reviewer can decide whether the deed
> may proceed under NC terms or must be rejected. **`flagged = true`, `licenseVerified = false`.**

# Datasheet (DRAFT): Gaia Mission Astrometry (`gaia_source`)

One-line summary: Astrometric and photometric catalogue of ~1.8 billion sources (positions,
parallaxes, proper motions, magnitudes) from ESA's Gaia mission, published via the Gaia ESA
Archive.

**This datasheet's own licence:** Creative Commons Attribution 4.0 International (CC-BY-4.0).
(Note: the datasheet is freely licensed; the *described dataset* is **not** — see Source licence.)

---

## Provenance & attribution

| Field | Value |
|---|---|
| Dataset | Gaia mission astrometry (`gaia_source` main catalogue) |
| Publisher | European Space Agency (ESA) / Gaia Data Processing and Analysis Consortium (DPAC) |
| Catalog id (Hee-Lee Oss) | cat-science-006 |
| Source / access portal | https://www.cosmos.esa.int/web/gaia/data-access |
| Primary archive | Gaia ESA Archive — https://gea.esac.esa.int/archive/ |
| Data model reference | https://gea.esac.esa.int/archive/documentation/ (GDR3 archive data model, `gaia_source`) |
| Retrieval date | 2026-06-28 |
| Releases available | DR1, DR2, (E)DR3, FPR, DR4 (per data-access page) |

**Required attribution / credit line** (per ESDC terms, verified 2026-06-28):

> Credit: ESA, Gaia DPAC

ESA additionally requires following the Gaia data-citation and publishing guidelines for any
publication that uses the data.

---

## Source licence (VERIFIED — and it is the reason this file is DRAFT)

- **Licence name:** Creative Commons Attribution-NonCommercial 3.0 IGO (**CC BY-NC 3.0 IGO**).
- **Citation / clause:**
  - ESA Gaia licence statement: "Gaia data are distributed under the CC BY-NC 3.0 IGO
    license." — https://creativecommons.org/licenses/by-nc/3.0/igo/deed.en
  - ESA Science Data Centre (ESDC) Terms & Conditions for the use of data in ESA space
    science archives — https://www.cosmos.esa.int/web/esdc/terms-and-conditions , which states:
    "Prior to any commercial use by the User of any Data or Data Product, including any use or
    application that directly or indirectly generates a financial gain, a detailed request for
    authorisation/licence shall be made."
- **Derivatives:** Permitted **for non-commercial use** under the "BY" + "NC" terms (the licence
  is NOT NoDerivatives). Modified/derived works are allowed provided they are non-commercial and
  carry attribution.
- **Commercial use:** **NOT permitted** without prior ESA authorisation.

**Why this fails the first-pass assumption:** The task context recorded the source licence as
`CC-BY-4.0` ("permits derivatives — confirm at the gate"). Verification shows it is actually
`CC BY-NC 3.0 IGO`. Because it is **NonCommercial**, it does not match the permissive CC-BY-4.0
expected by the gate and triggers the Hee-Lee Oss licence guardrail. Per the task's hard guardrail
(non-commercial discovered → STOP, mark DRAFT, flag, still open PR), this file is a DRAFT.

> **Licence snapshot note:** A hash + archived copy of the full licence text was NOT committed
> by this deed (documentation-only; we do not mirror external text without review). The
> canonical text lives at the Creative Commons and ESDC URLs above; a reviewer should archive
> and hash that text if the deed is approved to proceed.

---

## Data dictionary (`gaia_source`, GDR3 data model — verified from ESA archive docs)

Verified from the Gaia (G)DR3 archive data model documentation for the `gaia_source` table.
Units and types follow the published data model. This is a representative subset of the
~150 columns; rows marked *(unverified)* could not be individually confirmed in this pass.

| Field | Type | Units | Description | Allowed values / null handling |
|---|---|---|---|---|
| `source_id` | long | — | Unique numerical source identifier; encodes approximate position (HEALPix) and processing provenance | Not null; not guaranteed unique across releases |
| `ra` | double | deg | Barycentric right ascension (ICRS) at reference epoch | Not null for valid solutions |
| `ra_error` | float | mas | Standard error of RA (σα* ≡ σα·cos δ) | — |
| `dec` | double | deg | Barycentric declination (ICRS) at reference epoch | Not null for valid solutions |
| `dec_error` | float | mas | Standard error of declination | — |
| `parallax` | double | mas | Absolute stellar parallax at reference epoch | Null for 2-parameter solutions |
| `parallax_error` | float | mas | Standard error of parallax | Null where parallax null |
| `pmra` | double | mas yr⁻¹ | Proper motion in RA direction (μα*) | Null for 2-parameter solutions |
| `pmra_error` | float | mas yr⁻¹ | Standard error of pmra | Null where pmra null |
| `pmdec` | double | mas yr⁻¹ | Proper motion in declination (μδ) | Null for 2-parameter solutions |
| `pmdec_error` | float | mas yr⁻¹ | Standard error of pmdec | Null where pmdec null |
| `ref_epoch` | double | Julian Years (TCB) | Reference epoch for the astrometric parameters | Not null |
| `astrometric_params_solved` | byte | — | Bit flags for which parameters were estimated | DR3 values typically 3, 31, or 95 |
| `ruwe` | float | — | Renormalised Unit Weight Error (astrometric quality) | Null for 2-parameter solutions |
| `astrometric_excess_noise` | float | mas | Excess noise (residual disagreement with model) | — |
| `phot_g_mean_mag` | float | mag (Vega) | Mean magnitude, G band | May be null if photometry absent |
| `phot_bp_mean_mag` | float | mag (Vega) | Mean magnitude, integrated BP band | May be null |
| `phot_rp_mean_mag` | float | mag (Vega) | Mean magnitude, integrated RP band | May be null |
| `bp_rp` | float | mag | BP − RP colour | Null if BP or RP missing |
| `designation` | string | — | Source designation, prefix "Gaia DRx "; unique across releases | Not null |
| `astrometric_n_obs_al` | int | — | Number of along-scan observations | *(unverified detail)* |
| `astrometric_n_good_obs_al` | int | — | Number of good along-scan observations | *(unverified detail)* |
| (10 correlation columns, e.g. `ra_dec_corr`) | float | — | Correlation coefficients between astrometric parameter pairs | Range [−1, +1] |

PII note: `gaia_source` contains **no personal or identifying data** — it describes
astronomical objects (stars), not people.

---

## Datasheet for Datasets

**Motivation.** Gaia is an ESA astrometric survey mission building the most precise 3D map of
the Milky Way: positions, distances (via parallax), and motions for ~1.8 billion sources. The
catalogue is foundational for galactic dynamics, stellar astrophysics, exoplanets, and the
celestial reference frame. It is "famously hard to use without a clear datasheet," motivating
this deed.

**Composition.** Each row is one astronomical source. Core content: equatorial coordinates,
parallax, proper motions with associated uncertainties and correlations, broad-band
photometry (G, BP, RP), and quality indicators (RUWE, excess noise, observation counts).
No human-subject data.

**Collection process.** Data are collected by the Gaia spacecraft scanning the sky repeatedly
from L2; raw measurements are reduced by DPAC into the published astrometric/photometric
solution. Reference frame: ICRS; epoch given by `ref_epoch`.

**Preprocessing / cleaning.** Extensive DPAC pipeline calibration and astrometric global
iterative solution (AGIS). Quality flags (`astrometric_params_solved`, `ruwe`,
`astrometric_excess_noise`) let users filter solutions. Many fields are conditionally null
(e.g., 2-parameter solutions lack parallax/proper motion).

**Uses.** Galactic structure and kinematics, distance ladders, variability/cluster studies,
reference frame work. **Constraint:** NonCommercial only without ESA authorisation.

**Distribution.** Via the Gaia ESA Archive (TAP/ADQL, bulk download) and partner data centres
(CDS, ASI SSDC, ARI, AIP, Flatiron). This datasheet does **not** redistribute any data.

**Maintenance.** Maintained by ESA/DPAC across successive releases (DR1→DR4). Each release is a
fixed, versioned snapshot; consult release-specific documentation.

---

## Croissant metadata (schema.org/Dataset JSON-LD)

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
  "name": "Gaia mission astrometry (gaia_source)",
  "description": "ESA Gaia astrometric and photometric catalogue: positions, parallaxes, proper motions, and broad-band magnitudes for ~1.8 billion sources. Documentation datasheet only; no data hosted.",
  "url": "https://www.cosmos.esa.int/web/gaia/data-access",
  "sameAs": "https://gea.esac.esa.int/archive/",
  "license": "https://creativecommons.org/licenses/by-nc/3.0/igo/deed.en",
  "creditText": "Credit: ESA, Gaia DPAC",
  "creator": {
    "@type": "Organization",
    "name": "European Space Agency (ESA) / Gaia DPAC",
    "url": "https://www.cosmos.esa.int/web/gaia"
  },
  "version": "GDR3",
  "recordSet": [
    {
      "@type": "cr:RecordSet",
      "name": "gaia_source",
      "field": [
        { "@type": "cr:Field", "name": "source_id", "dataType": "cr:Integer" },
        { "@type": "cr:Field", "name": "ra", "dataType": "cr:Float" },
        { "@type": "cr:Field", "name": "dec", "dataType": "cr:Float" },
        { "@type": "cr:Field", "name": "parallax", "dataType": "cr:Float" },
        { "@type": "cr:Field", "name": "pmra", "dataType": "cr:Float" },
        { "@type": "cr:Field", "name": "pmdec", "dataType": "cr:Float" },
        { "@type": "cr:Field", "name": "phot_g_mean_mag", "dataType": "cr:Float" }
      ]
    }
  ]
}
```

---

## Validation script

Documentation-only. This script validates the **structure** of a user-obtained `gaia_source`
extract. It does **NOT** download, mirror, or host the dataset — the user must point it at a
file they already retrieved themselves from the Gaia ESA Archive under the NC licence.

```python
#!/usr/bin/env python3
"""Validate the structure of a local Gaia gaia_source extract (CSV/VOTable export to CSV).
Does NOT download or host any data. Usage: python validate.py path/to/gaia_sample.csv"""
import csv, sys

EXPECTED_COLUMNS = {
    "source_id", "ra", "ra_error", "dec", "dec_error",
    "parallax", "parallax_error", "pmra", "pmdec",
    "ref_epoch", "phot_g_mean_mag", "ruwe",
}
MAX_SAMPLE_ROWS = 1000  # bounded-access: header/streamed read, small local sample only

def main(path):
    with open(path, newline="", encoding="utf-8") as f:
        reader = csv.DictReader(f)
        cols = set(reader.fieldnames or [])
        missing = EXPECTED_COLUMNS - cols
        if missing:
            print(f"FAIL: missing expected columns: {sorted(missing)}")
            return 1
        n = 0
        for row in reader:
            n += 1
            if n > MAX_SAMPLE_ROWS:
                break
            # basic range sanity (non-null rows only)
            ra = row.get("ra", "")
            dec = row.get("dec", "")
            if ra and not (0.0 <= float(ra) <= 360.0):
                print(f"FAIL row {n}: ra out of [0,360]: {ra}")
                return 1
            if dec and not (-90.0 <= float(dec) <= 90.0):
                print(f"FAIL row {n}: dec out of [-90,90]: {dec}")
                return 1
        print(f"OK: {len(cols)} columns present, validated {n} sampled rows (cap {MAX_SAMPLE_ROWS}).")
        return 0

if __name__ == "__main__":
    if len(sys.argv) != 2:
        print(__doc__); sys.exit(2)
    sys.exit(main(sys.argv[1]))
```

---

## Limitations & gaps (honest note)

- **LICENCE MISMATCH (primary):** Source licence is **CC BY-NC 3.0 IGO**, not the assumed
  `CC-BY-4.0`. NonCommercial → guardrail flagged → this file is a DRAFT pending human review.
- **No data accessed/sampled.** This deed did not download or sample any Gaia data; the data
  dictionary is built from ESA's published data model, not from a live sample. Field
  presence/types reflect GDR3 documentation.
- **Licence snapshot not committed.** No hash + archived licence text was committed (to avoid
  mirroring external content before review). URLs are cited for a reviewer to archive/hash.
- **Subset only.** `gaia_source` has ~150 columns; only core astrometric/photometric/quality
  fields are documented. Rows marked *(unverified)* were not individually confirmed.
- **Release-specific.** Column sets and value semantics vary across DR1→DR4; this datasheet
  targets the (E)DR3/GDR3 data model. Confirm against the specific release used.
- **Row count.** ~1.8 billion sources is approximate and release-dependent; not independently
  verified in this pass.
