# Datasheet — Budget de l'État (PLF / execution data)

**One-line summary:** Documentation datasheet for France's State budget open data published on data.gouv.fr / data.economie.gouv.fr by the Ministères économiques et financiers (MEF/DGFiP), using the *Projet de loi de finances pour 2019 (PLF 2019), annexe budget des opérateurs de l'État pour 2018* dataset as the concrete, field-verified exemplar.

> **Datasheet licence:** This datasheet (the document you are reading) is released under **Creative Commons Attribution 4.0 International (CC-BY-4.0)**. It is documentation only; it does **not** contain, host, mirror, or republish any of the underlying dataset.

---

## Provenance & attribution

| Field | Value |
|---|---|
| Dataset title | Projet de loi de finances pour 2019 (PLF 2019), annexe budget des opérateurs de l'État pour 2018 |
| Publisher / producer | Ministères économiques et financiers (MEF) — France (DGFiP / direction du budget) |
| Catalogue / portal | data.gouv.fr (national open-data portal); resources hosted on data.economie.gouv.fr (Opendatasoft) |
| Source URL (dataset page) | https://www.data.gouv.fr/datasets/projet-de-loi-de-finances-pour-2019-plf-2019-annexe-budget-des-operateurs-de-letat-pour-2018 |
| Task catalogue id | cat-budget-004 / ds-budget-004 |
| Source last update | 2018-12-13 |
| Retrieval date | 2026-06-28 |
| Required attribution string | "Source : Ministères économiques et financiers (data.gouv.fr), PLF 2019 — annexe budget des opérateurs de l'État pour 2018, mise à jour 2018-12-13, sous Licence Ouverte / Open Licence v2.0 (Etalab)." |

The Licence Ouverte 2.0 attribution obligation ("paternité") requires citing **the source (at minimum the grantor's name)** and **the date of the last update** of the reused information — both included in the string above.

---

## Source licence

- **Licence name:** **Licence Ouverte / Open Licence version 2.0** (Etalab Open Licence 2.0; portal code `lov2`).
- **Permits derivatives?** **YES — verified.** The licence text grants the right to *"adapt, modify, extract and transform the information, in particular to create 'Derived Information'."*
  Verbatim French clause (rights granted to the reuser):
  > *"de l'adapter, la modifier, l'extraire et la transformer, notamment pour créer des « Informations dérivées »"*
- **Commercial use?** Permitted ("à titre commercial"), including combining with other information and incorporating into products/applications.
- **Attribution condition:** mention paternity of the information — its source (at minimum the name of the "Concédant") and the date of last update:
  > *"mentionner la paternité de l'« Information » : sa source (a minima le nom du « Concédant ») et la date de la dernière mise à jour"*
- **Licence citation URL (canonical text):** https://github.com/etalab/licence-ouverte/blob/master/LO.md
  (Official reference page redirects, as of 2026-06-28, from `etalab.gouv.fr/licence-ouverte-open-licence` to `alliance.numerique.gouv.fr`; the canonical machine-readable text is the etalab/licence-ouverte repository above.)
- **Licence snapshot note:** The portal records the dataset licence as `lov2` (Licence Ouverte v2.0). A hash + archived copy of the licence text should be attached as the per-dataset gate artifact at review time; the canonical text source for that snapshot is the etalab/licence-ouverte repo file `LO.md`.

**Conclusion:** The source licence explicitly permits derivative works and reuse (incl. commercial) with attribution. This datasheet is therefore a permitted derivative documentation artifact.

---

## Data dictionary

The source dataset is a bundle of CSV attachments (no single queryable table; the portal reports `has_records: false` for the unified export). Fields below were verified by a **bounded header read** (header row + one example value, local-only, ephemeral, no data committed) of two representative resources. Other resources in the bundle are listed but their columns are marked **UNVERIFIED**.

### Resource A — `plf2019 - Autorisations Budgétaires BI 2018 en K€.csv` (verified)

Budget authorisations / expenditures and revenues per State operator, amounts in **thousands of euros (K€)**. `AE` = autorisations d'engagement (commitment authorisations); `CP` = crédits de paiement (payment credits).

| Field (column header) | Type | Units | Description | Allowed values / nulls |
|---|---|---|---|---|
| Type de Mission Chef de file | string | — | Type of lead budget mission | free text; non-null |
| Mission Chef de file | string | — | Lead budget mission name | free text |
| Code du programme chef de file | string | — | Lead budget programme code | numeric-like code |
| Programme Chef de file | string | — | Lead budget programme name | free text |
| Opérateur ou catégorie d'opérateur | string | — | State operator or operator category | free text; non-null |
| Comptabilité budgétaire ? | string | — | Whether on budgetary accounting | categorical: OUI / NON |
| EPST ? | string | — | Whether entity is an EPST (public scientific/technological est.) | categorical: OUI / NON |
| DEPENSES BI 2018 Personnel (a) (AE=CP) (hors env. Rech) | integer | K€ | Staff expenditure (excl. research envelope) | integer; may be 0/blank |
| DEPENSES BI 2018 Fonctionnement (b) (AE) (hors env. Rech) | integer | K€ | Operating expenditure, commitment auth. | integer |
| DEPENSES BI 2018 Fonctionnement (b) (CP) (hors env. Rech) | integer | K€ | Operating expenditure, payment credits | integer |
| DEPENSES BI 2018 Intervention (c) (AE) | integer | K€ | Intervention expenditure (AE) | integer |
| DEPENSES BI 2018 Intervention (c) (CP) | integer | K€ | Intervention expenditure (CP) | integer |
| DEPENSES BI 2018 Investissement (d) (AE) (hors env. Rech) | integer | K€ | Investment expenditure (AE) | integer |
| DEPENSES BI 2018 Investissement (d) (CP) (hors env. Rech) | integer | K€ | Investment expenditure (CP) | integer |
| DEPENSES BI 2018 Total Enveloppe recherche (e=f+g+h) (AE) | integer | K€ | Total research envelope (AE) | integer |
| DEPENSES BI 2018 Total Enveloppe recherche (e=f+g+h) (CP) | integer | K€ | Total research envelope (CP) | integer |
| DEPENSES BI 2018 Personnel Enveloppe recherche (f) (AE = CP) | integer | K€ | Research-envelope staff cost | integer |
| DEPENSES BI 2018 Fonctionnement Enveloppe recherche (g) (AE) | integer | K€ | Research-envelope operating (AE) | integer |
| DEPENSES BI 2018 Fonctionnement Enveloppe recherche (g) (CP) | integer | K€ | Research-envelope operating (CP) | integer |
| DEPENSES BI 2018 Investissement Enveloppe recherche (h) (AE) | integer | K€ | Research-envelope investment (AE) | integer |
| DEPENSES BI 2018 Investissement Enveloppe recherche (h) (CP) | integer | K€ | Research-envelope investment (CP) | integer |
| DEPENSES BI 2018 TOTAL DES DEPENSES (a+b+c+d+e) (AE) | integer | K€ | Total expenditure (AE) | integer |
| DEPENSES BI 2018 TOTAL DES DEPENSES (a+b+c+d+e) (CP) | integer | K€ | Total expenditure (CP) | integer |
| DEPENSES BI 2018 dont charges de pensions civiles (AE=CP) | integer | K€ | of which civil pension charges | integer |
| RECETTES BI 2018 Recettes globalisées (i=j+k+l+m+n) | integer | K€ | Global revenues | integer |
| RECETTES BI 2018 Subventions pour charges de service public (j) | integer | K€ | Public-service-charge subsidies | integer |
| RECETTES BI 2018 Autres financements de l'Etat (k) | integer | K€ | Other State funding | integer |
| RECETTES BI 2018 Fiscalité affectée (l) | integer | K€ | Earmarked taxation | integer |
| RECETTES BI 2018 Autres financements publics (m) | integer | K€ | Other public funding | integer |
| RECETTES BI 2018 Recettes propres (n) | integer | K€ | Own revenues | integer |
| RECETTES BI 2018 Recettes fléchées (o=p+q+r) | integer | K€ | Earmarked ("flagged") revenues | integer |
| RECETTES BI 2018 Financements de l'Etat fléchés (p) | integer | K€ | Earmarked State funding | integer |
| RECETTES BI 2018 Autres financements publics fléchés (q) | integer | K€ | Earmarked other public funding | integer |
| RECETTES BI 2018 Recettes propres fléchées (r) | integer | K€ | Earmarked own revenues | integer |
| RECETTES BI 2018 TOTAL DES RECETTES (i+o) | integer | K€ | Total revenues | integer |
| RECETTES BI 2018 SOLDE BUDGETAIRE (Recettes - Dépenses (CP)) | integer | K€ | Budget balance (revenues − CP expenditure) | integer; may be negative |
| Code Ministère chef de file | string | — | Lead ministry code | code |
| Ministère Chef de file | string | — | Lead ministry name | free text |

### Resource B — `plf-2019-liste-operateurs-de-etat.csv` (verified)

Reference list of State operators for 2019.

| Field (column header) | Type | Units | Description | Allowed values / nulls |
|---|---|---|---|---|
| Opérateur 2019 | string | — | Operator name (institution, not a person) | free text; non-null |
| Statut | string | — | Legal status category | categorical |
| Comptabilité budgétaire ? | string | — | On budgetary accounting | categorical: OUI / NON |
| Nom de la catégorie 2019 d'appartenance (...) | string | — | Membership category name (if any) | free text; nullable |
| Programme (chef de file) | string | — | Lead programme (numeric id + label) | code + text |
| Mission (correspondant au programme chef de file) | string | — | Mission for the lead programme | free text |

### Other resources in the bundle — columns UNVERIFIED

The following CSVs are part of the dataset but their columns were **not** header-read here; treat their field lists as **UNVERIFIED**:
- `plf2019 - operateurs compte de résultat prévisionnel des opérateurs pour 2018.csv`
- `plf2019 - operateurs - autorisations budgétaires du BI 2018 des EPST.csv`
- `plf2019 - Tableau des dépenses par destinations.csv`
- `plf2019 - Equilibre Financier en K€ - Périmètre Opérateurs en comptabilité budgétaire yc EPST.csv`

**PII pass:** No personal/identifying fields were found. All records describe public institutions (State operators), budget programmes, ministries, and monetary aggregates. No natural-person names, contact details, or identifiers are present in the verified resources.

---

## Datasheet for Datasets

**Motivation.** The dataset documents the budget of the French State's *opérateurs* (public agencies/establishments) as annexed to the 2019 Finance Bill (PLF 2019), covering the 2018 initial budget (Budget Initial, BI). It exists to deliver legally-mandated budget transparency: showing how public money is authorised and spent across missions, programmes, ministries, and operators. Produced by the MEF / direction du budget.

**Composition.** Instances are rows describing a State operator (or operator category) and its budgeted expenditures and revenues, broken down by economic nature (staff, operating, intervention, investment), by AE/CP, and by research-envelope sub-totals; plus a reference list of operators and category/mission/programme mappings. Amounts are in K€. Units are public institutions, not individuals — there is no personal data.

**Collection process.** Figures are compiled from the official budget preparation process (Initial Budget 2018 of operators) within the French public-finance information chain (DGFiP / direction du budget), published as a finance-bill annex. They are administrative/official records, not sampled or surveyed.

**Preprocessing / cleaning.** Source amounts are expressed in thousands of euros (K€). Composite columns are explicitly defined by formulas in their headers (e.g., totals such as `a+b+c+d+e`, `i+o`, balance = revenues − CP). Consumers should be aware some cells may be blank/zero for operators where a category does not apply. No further cleaning was applied by this datasheet (documentation only).

**Uses.** Suitable for public-finance analysis, budget transparency tooling, comparisons across ministries/missions/programmes, and as a non-anglophone open-government exemplar. Not suitable as execution/actuals (it is initial-budget *forecast* data for 2018 from the PLF 2019 annex), nor for cross-year time series without joining additional yearly datasets.

**Distribution.** Distributed openly via data.gouv.fr with resources hosted on data.economie.gouv.fr (Opendatasoft), as downloadable CSV/JSON, under Licence Ouverte 2.0. This datasheet does not redistribute the data.

**Maintenance.** Maintained by the Ministères économiques et financiers. The documented snapshot's last update is 2018-12-13. Budget annexes are republished per finance-bill cycle; consult the portal for newer PLF/execution datasets. For corrections, use the data.gouv.fr dataset page feedback channel.

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
  "name": "Budget de l'Etat — PLF 2019, annexe budget des opérateurs de l'Etat pour 2018",
  "description": "Initial 2018 budget (expenditures and revenues, in K€) of French State operators, annexed to the 2019 Finance Bill (PLF 2019). Published by the Ministeres economiques et financiers via data.gouv.fr. Documentation-only datasheet; data not hosted here.",
  "url": "https://www.data.gouv.fr/datasets/projet-de-loi-de-finances-pour-2019-plf-2019-annexe-budget-des-operateurs-de-letat-pour-2018",
  "license": "https://github.com/etalab/licence-ouverte/blob/master/LO.md",
  "creator": {
    "@type": "Organization",
    "name": "Ministeres economiques et financiers (MEF / DGFiP)",
    "url": "https://www.data.gouv.fr/"
  },
  "datePublished": "2018-12-13",
  "inLanguage": "fr",
  "keywords": ["budget", "finances publiques", "PLF", "operateurs de l'Etat", "open data", "France"],
  "creditText": "Source : Ministeres economiques et financiers (data.gouv.fr), PLF 2019 — annexe budget des operateurs de l'Etat pour 2018, sous Licence Ouverte 2.0.",
  "recordSet": [
    {
      "@type": "cr:RecordSet",
      "name": "autorisations_budgetaires_bi_2018",
      "description": "Per-operator budget authorisations and revenues (K€).",
      "field": [
        {"@type": "cr:Field", "name": "operateur", "description": "State operator or operator category", "dataType": "Text"},
        {"@type": "cr:Field", "name": "ministere_chef_de_file", "description": "Lead ministry", "dataType": "Text"},
        {"@type": "cr:Field", "name": "depenses_total_ae", "description": "Total expenditure, commitment authorisations (K€)", "dataType": "Integer"},
        {"@type": "cr:Field", "name": "depenses_total_cp", "description": "Total expenditure, payment credits (K€)", "dataType": "Integer"},
        {"@type": "cr:Field", "name": "recettes_total", "description": "Total revenues (K€)", "dataType": "Integer"},
        {"@type": "cr:Field", "name": "solde_budgetaire", "description": "Budget balance = revenues - CP expenditure (K€)", "dataType": "Integer"}
      ]
    }
  ]
}
```

---

## Validation script

This script validates **structure only** against a locally-obtained copy that the user downloads themselves. It does **not** download, host, or republish the dataset. Point it at a file you already have.

```python
#!/usr/bin/env python3
"""Validate structure of a PLF-2019 operators-budget CSV (documentation only).
Does NOT download or host any data. Provide your own local file path.
Usage: python validate.py path/to/local.csv
"""
import csv, sys

# Minimal set of headers expected in 'Autorisations Budgetaires BI 2018 en KEUR'.
EXPECTED_MIN = {
    "Opérateur ou catégorie d'opérateur",
    "Ministère Chef de file",
    "DEPENSES BI 2018 TOTAL DES DEPENSES (a+b+c+d+e) (AE)",
    "DEPENSES BI 2018 TOTAL DES DEPENSES (a+b+c+d+e) (CP)",
    "RECETTES BI 2018 TOTAL DES RECETTES (i+o)",
}

def main(path):
    with open(path, newline="", encoding="utf-8-sig") as f:
        # Try common French CSV delimiters.
        sample = f.read(4096); f.seek(0)
        delim = ";" if sample.count(";") >= sample.count(",") else ","
        reader = csv.DictReader(f, delimiter=delim)
        headers = set(reader.fieldnames or [])
        missing = EXPECTED_MIN - headers
        rows = sum(1 for _ in reader)
    print(f"delimiter={delim!r} columns={len(headers)} data_rows={rows}")
    if missing:
        print("MISSING expected columns:", sorted(missing)); sys.exit(1)
    if rows < 1:
        print("FAIL: no data rows"); sys.exit(1)
    print("OK: expected structure present.")

if __name__ == "__main__":
    if len(sys.argv) != 2:
        print(__doc__); sys.exit(2)
    main(sys.argv[1])
```

Expected when run against the genuine resource: a delimiter of `;` or `,`, ~38 columns for the authorisations file, and at least one data row; the five sentinel columns above must be present.

---

## Limitations & gaps

- **Forecast, not execution:** Despite the task title mentioning "execution data", the field-verified exemplar is the **initial budget (BI 2018) forecast** annexed to PLF 2019, not realised/executed spending. A true execution dataset (e.g., comptes/exécution budgétaire) would be a separate data.gouv.fr resource and is **not** verified here.
- **Partial field verification:** Only two of the bundle's resources were header-read; columns of the other four CSVs are marked UNVERIFIED. Row counts, exact delimiters, encodings, and null conventions were not exhaustively confirmed (bounded-access, header-only reads).
- **Types inferred:** Data types come from single example values; integers may include blanks/zeros and the balance column may be negative.
- **Licence reference URL:** The historical Etalab licence page now redirects to `alliance.numerique.gouv.fr`; the canonical text cited is the `etalab/licence-ouverte` repository. A formal hash/archive snapshot of the licence text should be attached at the review gate.
- **No data hosted:** Per Hee-Lee Oss guardrails, no dataset content is mirrored, transformed, or committed; the validation script operates only on a file the user supplies locally.
