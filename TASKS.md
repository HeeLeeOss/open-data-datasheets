# TASKS — open-data-datasheets

> Status: Draft · Version: 0.4.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

## How these tasks map to Hee-Lee Oss

Each task below becomes a Hee-Lee Oss **Task JSON** validated against
`packages/schema/src/schemas.ts`. Mapping of fields:

- `id` — stable slug ID from the tables (e.g. `open-data-datasheets-template-001`).
- `title` — the table's Title.
- `project` — `open-data-datasheets`.
- `type` — one of `code | research | writing | data | design-spec | maintenance` (see each table).
- `lane` — `donated` for all tasks here (no funded escrow). Funded tasks would add `fundedBudgetUsd`.
- `priority` — `high | medium | low`.
- `domain` — array, e.g. `["public-data","open-science","civic"]`.
- `riskTier` — `low | medium | high`. License/PII-judgement and sensitive-domain tasks are `medium`.
- `urgent` — boolean; `false` for all current tasks.
- `deliverable` — `pr | dataset | document | translation`. We never deliver `dataset` (data is out of
  scope); code → `pr`, docs/metadata → `document`.
- `tokenEstimate` — `small | medium | large` (Size column).
- `status` — `open | in-progress | review | delivered | done`; all start `open`.
- `context`, `objective`, `acceptanceCriteria[]`, `resources[]`, `output` — per task.
- `requestor` — TO BE SECURED until a partner is confirmed.
- `verifiedNeed` — **`false`** until a named portal/maintainer agrees to accept contributions
  (general need is real, but per-task delivery need is unproven).
- `outputLicense` — `CC-BY-4.0` for documentation/metadata; `MIT` for code (validators/adapters).

---

## Milestone M0 — Foundation & cold-start

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| open-data-datasheets-reviewer-024 | Name/secure the License+PII reviewer (blocking, non-skippable gate role) | research | small | low | document | — | Maintainer |
| open-data-datasheets-template-001 | Documentation template + canonical metadata model | writing | small | low | document | — | Technical |
| open-data-datasheets-gate-002 | License + PII triage checklist (blocking gate) | design-spec | small | medium | document | — | License+PII |
| open-data-datasheets-policy-022 | NC / share-alike acceptance policy for derivative metadata | design-spec | small | medium | document | gate-002 | License+PII |
| open-data-datasheets-croissant-003 | Croissant ML metadata generator + spec validator | code | medium | low | pr | template-001 | Technical |
| open-data-datasheets-validator-004 | Reusable dataset validation script + quality report | code | small | low | pr | template-001 | Technical |
| open-data-datasheets-outreach-006 | Partner/portal outreach + candidate dataset shortlist | research | small | low | document | — | Maintainer |
| open-data-datasheets-pilot-005 | End-to-end documentation for one pilot dataset | data | medium | medium | document | template-001, gate-002, policy-022, croissant-003, validator-004, outreach-006, reviewer-024 | License+PII, Technical |

**Acceptance criteria — key tasks**

- **template-001 (template + canonical model)**
  - [ ] Canonical metadata model documented with every field from PLAN (license, provenance, pii,
        fields[], knownIssues, examples).
  - [ ] Markdown template covers data dictionary, provenance, license record, known-issues, examples,
        and the Datasheets-for-Datasets questionnaire.
  - [ ] Explicitly states deliverable is documentation, not data; output licensed CC-BY-4.0.
  - [ ] At least one filled-in worked example skeleton included.

- **gate-002 (license + PII gate)**
  - [ ] Checklist enumerates accepted licenses (OGL, CC0, CC-BY) and case-by-case/excluded ones, and
        defers NC/share-alike cases to the `policy-022` policy.
  - [ ] Objective license criterion: PASS only if `license.permitsDerivatives: true` is set from a
        cited clause/URL; missing/unparseable evidence = FLAG/EXCLUDE (no default-allow).
  - [ ] Requires recording license id + URL + text snapshot reference (committed copy + SHA-256 +
        Wayback URL, per the decided format) and the permits-derivatives boolean.
  - [ ] PII triage section encodes the detection methodology: column-name heuristics (direct
        identifiers), quasi-identifier k-anonymity flag (k ≥ 5), geo-precision threshold, and linkage
        risk; exclude unless publisher-documented anonymization; never anonymize ourselves.
  - [ ] Inspection during triage follows the dataset-inspection access protocol (header/streamed
        reads, row cap, local-only ephemeral, no committed samples; halt on PII signal).
  - [ ] Produces a committed, reviewable PASS/FLAG/EXCLUDE artifact per dataset recording which checks
        ran and what fired.

- **croissant-003 (Croissant generator + validator)**
  - [ ] Emits and validates against the **pinned Croissant ML v1.0** context/SHACL (version recorded
        in `specVersions`).
  - [ ] Ships committed golden fixtures: known-valid metadata that must pass and malformed cases that
        must fail, exercised in CI (synthetic/public fixtures only — no real inspected data).
  - [ ] Code MIT-licensed; `pnpm build && pnpm test && pnpm lint` green; DCO signed-off.

- **pilot-005 (pilot dataset, end-to-end)**
  - [ ] Pilot selected on a realistic acceptance path first: an informal channel (maintainer agreed
        to review) or a self-serve fallback (GitHub PR to the dataset's repo, or a Zenodo metadata
        record) so a real *accepted* outcome is reachable.
  - [ ] Dataset passed gate-002 (license permits reuse+derivatives; no unanonymized PII) with the
        artifact committed.
  - [ ] Complete data dictionary, Datasheet, and valid Croissant metadata produced; completeness
        score recorded (before/after, target after ≥ 90/100).
  - [ ] Validation script runs clean in CI and emits a quality report.
  - [ ] Provenance recorded (source URL, publisher, retrieval date, version, attribution; license
        snapshot = committed copy + SHA-256 + Wayback URL).
  - [ ] Documentation **accepted** via the informal channel or self-serve fallback with the Steward's
        acceptance evidence artifact (`outcomes/<dataset-id>.json`) recorded — or, if no channel
        materializes, **submitted** with the blocker surfaced.

**M0 Definition of Done:** License+PII reviewer named (blocking role filled before pilot review);
template + canonical model + license/PII gate + NC/share-alike policy published (policy merged before
any triage); Croissant generator and one validation script green in CI with golden fixtures; one
pilot dataset documented end-to-end and **accepted** via an informal channel or self-serve fallback
(evidence artifact recorded) — or submitted with the blocker surfaced; ≥ 1 partner-outreach thread
opened.

---

## Milestone M1 — License/provenance gate hardened + first acceptances

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| open-data-datasheets-snapshot-007 | License-text snapshot capture in provenance flow | code | small | low | pr | template-001, gate-002 | Technical |
| open-data-datasheets-triage-008 | Triage 5 candidate datasets through the gate | research | medium | medium | document | gate-002, policy-022, outreach-006 | License+PII |
| open-data-datasheets-doc-009 | Full documentation for accepted dataset #2 | data | medium | medium | document | pilot-005, triage-008 | License+PII, Technical |
| open-data-datasheets-doc-010 | Full documentation for accepted dataset #3 | data | medium | medium | document | pilot-005, triage-008 | License+PII, Technical |
| open-data-datasheets-partner-011 | Secure first confirmed contribution partner | research | small | low | document | outreach-006 | Steward |

**Acceptance criteria — key tasks**

- **snapshot-007 (license-text snapshot capture)**
  - [ ] Implements the **decided** storage format (must precede build): committed local copy of the
        license text/page + SHA-256 hash + Wayback Machine save URL; `license.snapshotRef` records the
        committed path, hash, and Wayback timestamp. Bare license URL alone is insufficient.
  - [ ] Code MIT-licensed; tests + CI green; no credentials embedded.

- **triage-008 (triage 5 candidates)**
  - [ ] Five datasets evaluated with a committed gate artifact each (PASS/FLAG/EXCLUDE + rationale),
        applying the `policy-022` NC/share-alike rule (decided before this task runs).
  - [ ] Each PASS records license id/URL/snapshot and the objective permits-derivatives = true with
        cited evidence.
  - [ ] PII detection methodology applied (column heuristics, k-anonymity, geo precision, linkage);
        any PII or unclear-license datasets are FLAGGED/EXCLUDED with the concern surfaced.

- **partner-011 (first confirmed partner)**
  - [ ] A named portal/maintainer confirms they will review and accept contributed documentation.
  - [ ] Contribution mechanism documented (PR vs. portal API vs. email).
  - [ ] Tasks for that partner updated to `verifiedNeed: true` with `requestor` set.

**M1 Definition of Done:** gate applied to ≥ 3 datasets with committed artifacts; ≥ 2 datasets
accepted onto a portal/repo (per-channel acceptance evidence artifacts recorded); ≥ 1 confirmed
partner; license-snapshot capture working in the decided format (committed copy + SHA-256 + Wayback).

---

## Milestone M2 — Portal adapters & scale

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| open-data-datasheets-ckan-012 | CKAN package metadata adapter | code | medium | low | pr | croissant-003 | Technical |
| open-data-datasheets-socrata-013 | Socrata dataset metadata adapter | code | medium | low | pr | croissant-003 | Technical |
| open-data-datasheets-dcat-014 | data.gov / DCAT-US metadata adapter | code | medium | low | pr | croissant-003 | Technical |
| open-data-datasheets-doc-015 | Documentation for datasets #4 and #5 via adapters | data | large | medium | document | ckan-012, socrata-013, partner-011 | License+PII, Technical |

**Acceptance criteria — key tasks**

- **ckan-012 (CKAN adapter)** *(pattern also applies to socrata-013 and dcat-014)*
  - [ ] Transforms canonical metadata into a valid CKAN package payload for the **pinned CKAN 2.10**
        package schema (version in `specVersions`); socrata-013 targets SODA 2.x, dcat-014 targets
        DCAT-US v3 (v1.1 fallback recorded).
  - [ ] Ships committed golden input→output fixtures diffed in CI, and output validated against the
        pinned spec/schema.
  - [ ] Verified against a **sandbox portal** round-trip (e.g. a CKAN demo/Docker instance) or, where
        no sandbox exists, against a committed captured real import schema.
  - [ ] Code MIT-licensed; unit tests + CI green; no credentials embedded.

- **doc-015 (datasets #4 and #5)**
  - [ ] Both datasets pass the gate with committed artifacts.
  - [ ] Metadata emitted via the relevant portal adapter and accepted upstream, with the Steward's
        acceptance evidence artifact recorded per channel.
  - [ ] Per-dataset effort recorded in the outcome ledger (AI-session minutes + human-review cycles)
        to show reduction vs. the recorded M0/M1 baseline median.

**M2 Definition of Done:** ≥ 2 portal adapters emitting valid portal-native metadata (golden fixtures
+ sandbox/captured-schema verified, pinned spec versions); ≥ 5 datasets accepted cumulatively;
measurable median per-dataset effort reduction vs. the M0/M1 baseline.

---

## Milestone M3 — Reuse outcomes & sustainability

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| open-data-datasheets-reuse-016 | Track and verify external reuse events | research | small | low | document | doc-009, doc-010, doc-015 | Steward |
| open-data-datasheets-refresh-017 | Staleness/refresh process + drift detection | maintenance | small | low | document | validator-004 | Maintainer |
| open-data-datasheets-scale-018 | Documentation for datasets #6–#8 | data | large | medium | document | doc-015, refresh-017 | License+PII, Technical |

**Acceptance criteria — key tasks**

- **reuse-016 (reuse tracking)**
  - [ ] ≥ 3 verifiable external reuse events recorded in the outcome ledger (citation/portal
        acceptance/PR reference), alongside the per-channel acceptance evidence artifacts.
  - [ ] Each event links to externally verifiable evidence (no self-reported reuse).

- **refresh-017 (staleness/refresh)**
  - [ ] Documented process to detect when a documented dataset has drifted from its recorded version.
  - [ ] Validation script flags schema drift; stale docs become `maintenance` tasks.

**M3 Definition of Done:** ≥ 3 verifiable reuse events; ≥ 8 datasets accepted cumulatively;
maintenance/refresh process documented and steward identified.

---

## Catalog-driven scaling tasks

These tasks operationalize the candidate backlog in `DATASET-CATALOG.md`. One task triages the
catalog into an approved shortlist; the rest are **concrete example per-dataset datasheet tasks**
drawn from real catalog entries (their `cat-…` IDs are cited). They share the catalog→task field
mapping in `DATASET-CATALOG.md` ("How to turn a catalog entry into a task"): all are
`type: data`, `deliverable: document` (never `dataset`), `lane: donated`, `verifiedNeed: false`
(no partner secured), `requestor: TO BE SECURED`. Each per-dataset task still requires its own
committed `gate-002` artifact before work proceeds — listing it here does not pre-approve it.

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| open-data-datasheets-catalog-025 | Triage the candidate catalog → approved shortlist | research | medium | medium | document | gate-002, policy-022 | License+PII |
| open-data-datasheets-ds-naturalearth-026 | Datasheet for Natural Earth boundaries (`cat-geo-002`) | data | small | low | document | catalog-025, template-001, croissant-003 | License+PII, Technical |
| open-data-datasheets-ds-owidco2-027 | Datasheet for OWID CO2 & GHG Emissions (`cat-climate-004`) | data | medium | low | document | catalog-025, template-001, croissant-003 | License+PII, Technical |
| open-data-datasheets-ds-faostat-028 | Datasheet for a FAOSTAT domain (`cat-agri-001`) | data | medium | low | document | catalog-025, template-001, croissant-003 | License+PII, Technical |
| open-data-datasheets-ds-collegescorecard-029 | Datasheet for College Scorecard (`cat-edu-001`) | data | medium | low | document | catalog-025, template-001, croissant-003 | License+PII, Technical |
| open-data-datasheets-ds-policeuk-030 | Datasheet for UK street-level crime (`cat-justice-001`) | data | medium | medium | document | catalog-025, template-001, croissant-003 | License+PII, Technical |

**Acceptance criteria — catalog-025 (catalog triage → approved shortlist)**

- [ ] Every catalog entry is assigned a triage disposition: `APPROVED` (license verified +
      `permitsDerivatives: true` with cited clause/URL, PII risk acceptable), `VERIFY-LATER`, or
      `EXCLUDED` (NC/share-alike rejected by `policy-022`, non-redistribution terms, or high-PII).
- [ ] `Verify`-marked rows are resolved with a cited license source URL (no default-allow); rows that
      cannot be confirmed stay `VERIFY-LATER` and are not promoted to tasks.
- [ ] Share-alike (ODbL) and NC entries are decided strictly per `policy-022`, with rationale recorded.
- [ ] PII detection methodology (column heuristics, k-anonymity k≥5, geo precision, linkage) applied
      to each `APPROVED` candidate; any flag downgrades to `EXCLUDED`/`VERIFY-LATER`.
- [ ] Output is a committed shortlist (≥ 10 `APPROVED` candidates) with one gate artifact per
      decision, ordered by realistic acceptance path (informal channel / self-serve fallback first).

**Acceptance criteria — per-dataset datasheet tasks (pattern for `ds-*-026..030`)**

- [ ] Source dataset passed `gate-002` with a committed PASS artifact (license id+URL+snapshot,
      `permitsDerivatives: true`, PII methodology result). For `ds-policeuk-030`, the artifact must
      explicitly confirm the publisher's street-level anonymization is adequate (hence `riskTier: medium`).
- [ ] Complete data dictionary + Datasheet-for-Datasets + valid Croissant v1.0 metadata produced;
      completeness score recorded (before/after, target after ≥ 90/100).
- [ ] Validation script runs clean in CI and emits a quality report; no real inspected data committed.
- [ ] Provenance recorded (source URL, publisher, retrieval date, version, attribution; license
      snapshot = committed copy + SHA-256 + Wayback URL).
- [ ] `outputLicense: CC-BY-4.0` for the documentation; deliverable is documentation, never the data.
- [ ] Accepted upstream (per-channel acceptance evidence artifact) — or submitted with the blocker
      surfaced if no channel materializes. `verifiedNeed` flips to `true` only once a named
      portal/maintainer confirms acceptance.

**Notes on the chosen examples (all real catalog entries):**
- `ds-naturalearth-026` — public domain, no PII → lowest-risk worked example / pilot candidate.
- `ds-owidco2-027`, `ds-faostat-028`, `ds-collegescorecard-029` — clearly permissive (CC-BY-4.0,
  CC-BY-4.0 IGO, US-gov public domain), no PII → `riskTier: low`.
- `ds-policeuk-030` — OGL v3 with publisher-anonymised street-level crime → `riskTier: medium`
  because residual PII/linkage judgement is involved; included to exercise the gate's PII path.

---

## Backlog / future

| ID | Title | Type | Size | Risk | Deliverable | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| open-data-datasheets-hf-019 | Hugging Face dataset-card / metadata adapter | code | medium | low | pr | If HF datasets enter scope |
| open-data-datasheets-zenodo-020 | Zenodo metadata adapter + DOI provenance | code | medium | low | pr | For archived/citable datasets |
| open-data-datasheets-i18n-021 | Translate a delivered Datasheet (domain reviewer) | translation | small | medium | translation | Widens reuse; needs language reviewer |
| open-data-datasheets-dash-023 | Outcome dashboard for accepted docs + reuse events | code | medium | low | pr | Supports success-metric tracking; reads the outcome ledger |

---

## Example task JSON

```json
{
  "id": "open-data-datasheets-template-001",
  "title": "Documentation template + canonical metadata model",
  "project": "open-data-datasheets",
  "type": "writing",
  "lane": "donated",
  "priority": "high",
  "domain": ["public-data", "open-science", "civic"],
  "riskTier": "low",
  "urgent": false,
  "deliverable": "document",
  "tokenEstimate": "small",
  "status": "open",
  "context": "Public open datasets are often unusable because they lack data dictionaries, provenance, clear licensing, and machine-readable metadata. Before documenting any specific dataset, the project needs one reusable template and a canonical metadata model that all other outputs (Datasheet, Croissant ML metadata, portal-specific metadata) are projections of. Deliverable is documentation, never the data.",
  "objective": "Create the reusable documentation template and the canonical metadata model that every per-dataset task will use.",
  "acceptanceCriteria": [
    "Canonical metadata model documents all fields: title, publisher, sourceUrl, license {id, url, permitsDerivatives, snapshotRef}, provenance {retrievedAt, version, updateCadence, attribution}, pii {present, anonymized, notes}, fields[] {name, type, units, allowedValues, nullable, description, caveats}, knownIssues[], examples[].",
    "Markdown template covers data dictionary, provenance record, license record, known-issues, worked examples, and the full Datasheets-for-Datasets questionnaire.",
    "Template explicitly states the deliverable is documentation, not data, and that documentation output is licensed CC-BY-4.0.",
    "At least one filled-in worked example skeleton is included to guide contributors.",
    "pnpm build && pnpm test && pnpm lint pass for any committed tooling; commit is DCO signed-off."
  ],
  "resources": [
    "C:\\code\\hee-lee-oss\\planning\\projects\\open-data-datasheets\\PLAN.md",
    "C:\\code\\hee-lee-oss\\governance\\proposals\\open-data-datasheets.md",
    "Datasheets for Datasets (Gebru et al.)",
    "Croissant ML metadata specification"
  ],
  "output": "A documentation template plus a canonical metadata model definition, committed to the project repo and ready for reuse by per-dataset documentation tasks.",
  "requestor": "TO BE SECURED",
  "verifiedNeed": false,
  "outputLicense": "CC-BY-4.0"
}
```

---

## Per-dataset processing backlog (all catalog entries)

Generated from [DATASET-CATALOG.md](./DATASET-CATALOG.md). Every catalog entry is turned into a
**datasheet processing task** here, EXCEPT the 4 policy-excluded entries (listed at the end).
These tasks are the project's scaling backlog: they are pulled into milestones M2→steady-state by
**priority**, and each is blocked on the catalog triage + per-dataset gate before work starts.

**Shared task template** (every row below instantiates this):

```
project: open-data-datasheets   lane: donated   type: data   deliverable: document
status: open   urgent: false   verifiedNeed: false   requestor: TO BE SECURED
outputLicense: CC-BY-4.0 (the datasheet) — NOT the source dataset's licence
output: /output/<task-id>-datasheet.md
acceptanceCriteria:
  - Source licence verified to permit derivatives (cited clause/URL) + recorded; gate-002 artifact attached
  - PII pass: no personal/identifying fields documented or sampled; bounded-access protocol followed
  - Data dictionary (every field: name, type, units, allowed values, nulls) from <=1,000-row sample
  - Provenance + attribution + licence-snapshot (hash + archived copy) recorded
  - Datasheet-for-Datasets writeup + Croissant metadata produced and schema-valid
  - A small validation script + data-quality report; documentation only — data never hosted/republished
```

**Priority:** A = clearly-permissive + low/no-PII (good-first-deed). B = licence `Verify`/share-alike
(blocked on `policy-022`). C = medium-PII / sensitive (riskTier medium, reviewer required).

### Government budgets & spending

| Task ID | Catalog ID | Dataset | Priority | Risk | Licence gate | Depends on |
| --- | --- | --- | --- | --- | --- | --- |
| ds-budget-001 | cat-budget-001 | Spending over £25,000 (departmental transparency) | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-budget-002 | cat-budget-002 | USAspending.gov federal award & spending data | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-budget-003 | cat-budget-003 | EU Financial Transparency System (FTS) | B | low | Verify → gate-002 + policy-022 | catalog-025, policy-022 |
| ds-budget-004 | cat-budget-004 | Budget de l'État (PLF / execution data) | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-budget-005 | cat-budget-005 | BOOST public expenditure database | A | low | cleared (confirm at gate-002) | catalog-025 |

### Census & demographics

| Task ID | Catalog ID | Dataset | Priority | Risk | Licence gate | Depends on |
| --- | --- | --- | --- | --- | --- | --- |
| ds-census-001 | cat-census-001 | American Community Survey (ACS) summary tables | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-census-002 | cat-census-002 | Census 2021 (England & Wales) | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-census-003 | cat-census-003 | Population & demography statistics | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-census-004 | cat-census-004 | World Population Prospects (WPP) | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-census-005 | cat-census-005 | WorldPop gridded population | A | low | cleared (confirm at gate-002) | catalog-025 |

### Public health

| Task ID | Catalog ID | Dataset | Priority | Risk | Licence gate | Depends on |
| --- | --- | --- | --- | --- | --- | --- |
| ds-health-001 | cat-health-001 | CDC WONDER (mortality, natality, etc.) | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-health-002 | cat-health-002 | Our World in Data — health datasets | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-health-003 | cat-health-003 | NHS England published statistics | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-health-005 | cat-health-005 | Global Burden of Disease (GBD) results | B | low | Verify → gate-002 + policy-022 | catalog-025, policy-022 |
| ds-health-006 | cat-health-006 | ECDC surveillance & disease data | B | low | Verify → gate-002 + policy-022 | catalog-025, policy-022 |

### Environment & climate

| Task ID | Catalog ID | Dataset | Priority | Risk | Licence gate | Depends on |
| --- | --- | --- | --- | --- | --- | --- |
| ds-climate-001 | cat-climate-001 | Global Historical Climatology Network (GHCN) | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-climate-002 | cat-climate-002 | ERA5 reanalysis | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-climate-003 | cat-climate-003 | GISTEMP surface temperature analysis | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-climate-004 | cat-climate-004 | CO2 & Greenhouse Gas Emissions | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-climate-005 | cat-climate-005 | Global Carbon Budget | A | low | cleared (confirm at gate-002) | catalog-025 |

### Air quality

| Task ID | Catalog ID | Dataset | Priority | Risk | Licence gate | Depends on |
| --- | --- | --- | --- | --- | --- | --- |
| ds-airq-001 | cat-airq-001 | OpenAQ global air-quality measurements | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-airq-002 | cat-airq-002 | Air Quality System (AQS) | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-airq-003 | cat-airq-003 | European air-quality data | B | low | Verify → gate-002 + policy-022 | catalog-025, policy-022 |
| ds-airq-005 | cat-airq-005 | CAMS atmosphere monitoring (reanalysis/forecast) | A | low | cleared (confirm at gate-002) | catalog-025 |

### Energy

| Task ID | Catalog ID | Dataset | Priority | Risk | Licence gate | Depends on |
| --- | --- | --- | --- | --- | --- | --- |
| ds-energy-001 | cat-energy-001 | EIA energy data (Open Data API) | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-energy-002 | cat-energy-002 | ENTSO-E Transparency Platform | B | low | Verify → gate-002 + policy-022 | catalog-025, policy-022 |
| ds-energy-003 | cat-energy-003 | Energy datasets | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-energy-004 | cat-energy-004 | UK energy statistics (DUKES etc.) | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-energy-005 | cat-energy-005 | Ember electricity & power-sector data | A | low | cleared (confirm at gate-002) | catalog-025 |

### Transport & mobility

| Task ID | Catalog ID | Dataset | Priority | Risk | Licence gate | Depends on |
| --- | --- | --- | --- | --- | --- | --- |
| ds-transport-001 | cat-transport-001 | GTFS transit feeds (catalog) | B | low | Verify → gate-002 + policy-022 | catalog-025, policy-022 |
| ds-transport-002 | cat-transport-002 | Road Safety Data (STATS19) | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-transport-003 | cat-transport-003 | Fatality Analysis Reporting System (FARS) | C | medium | cleared (confirm at gate-002) | catalog-025 |
| ds-transport-004 | cat-transport-004 | TLC Trip Record Data | C | medium | Verify → gate-002 + policy-022 | catalog-025, policy-022 |
| ds-transport-005 | cat-transport-005 | Transport statistics | A | low | cleared (confirm at gate-002) | catalog-025 |

### Education

| Task ID | Catalog ID | Dataset | Priority | Risk | Licence gate | Depends on |
| --- | --- | --- | --- | --- | --- | --- |
| ds-edu-001 | cat-edu-001 | College Scorecard | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-edu-002 | cat-edu-002 | UIS education statistics | B | low | Verify → gate-002 + policy-022 | catalog-025, policy-022 |
| ds-edu-003 | cat-edu-003 | EdStats education indicators | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-edu-004 | cat-edu-004 | PISA assessment data | B | low | Verify → gate-002 + policy-022 | catalog-025, policy-022 |
| ds-edu-005 | cat-edu-005 | Get Information about Schools (GIAS) | A | low | cleared (confirm at gate-002) | catalog-025 |

### Justice & crime

| Task ID | Catalog ID | Dataset | Priority | Risk | Licence gate | Depends on |
| --- | --- | --- | --- | --- | --- | --- |
| ds-justice-001 | cat-justice-001 | Police recorded crime (street-level) | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-justice-002 | cat-justice-002 | Crime Data Explorer (UCR/NIBRS) | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-justice-003 | cat-justice-003 | Bureau of Justice Statistics data | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-justice-004 | cat-justice-004 | Crime & criminal justice statistics | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-justice-005 | cat-justice-005 | Stanford Open Policing Project | C | medium | Verify → gate-002 + policy-022 | catalog-025, policy-022 |

### Geospatial & boundaries

| Task ID | Catalog ID | Dataset | Priority | Risk | Licence gate | Depends on |
| --- | --- | --- | --- | --- | --- | --- |
| ds-geo-001 | cat-geo-001 | OpenStreetMap planet data | B | low | share-alike → policy-022 | catalog-025, policy-022 |
| ds-geo-002 | cat-geo-002 | Natural Earth vector/raster | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-geo-004 | cat-geo-004 | TIGER/Line shapefiles | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-geo-005 | cat-geo-005 | geoBoundaries global admin boundaries | A | low | cleared (confirm at gate-002) | catalog-025 |

### Economy & trade

| Task ID | Catalog ID | Dataset | Priority | Risk | Licence gate | Depends on |
| --- | --- | --- | --- | --- | --- | --- |
| ds-econ-001 | cat-econ-001 | World Development Indicators (WDI) | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-econ-002 | cat-econ-002 | IMF macroeconomic databases (IFS/WEO etc.) | B | low | Verify → gate-002 + policy-022 | catalog-025, policy-022 |
| ds-econ-003 | cat-econ-003 | UN Comtrade international trade | B | low | Verify → gate-002 + policy-022 | catalog-025, policy-022 |
| ds-econ-004 | cat-econ-004 | National accounts & macro indicators | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-econ-005 | cat-econ-005 | Labor statistics (CPI, employment) | A | low | cleared (confirm at gate-002) | catalog-025 |

### Agriculture & food

| Task ID | Catalog ID | Dataset | Priority | Risk | Licence gate | Depends on |
| --- | --- | --- | --- | --- | --- | --- |
| ds-agri-001 | cat-agri-001 | FAOSTAT | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-agri-002 | cat-agri-002 | NASS Quick Stats | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-agri-003 | cat-agri-003 | FoodData Central | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-agri-004 | cat-agri-004 | Open Food Facts | B | low | share-alike → policy-022 | catalog-025, policy-022 |
| ds-agri-005 | cat-agri-005 | EU agri-food data portal | B | low | Verify → gate-002 + policy-022 | catalog-025, policy-022 |

### Housing

| Task ID | Catalog ID | Dataset | Priority | Risk | Licence gate | Depends on |
| --- | --- | --- | --- | --- | --- | --- |
| ds-housing-001 | cat-housing-001 | Price Paid Data (property transactions) | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-housing-002 | cat-housing-002 | HUD open data (housing programs) | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-housing-003 | cat-housing-003 | HMDA mortgage data | C | medium | cleared (confirm at gate-002) | catalog-025 |
| ds-housing-004 | cat-housing-004 | UK House Price Index | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-housing-005 | cat-housing-005 | Housing statistics | A | low | cleared (confirm at gate-002) | catalog-025 |

### Elections

| Task ID | Catalog ID | Dataset | Priority | Risk | Licence gate | Depends on |
| --- | --- | --- | --- | --- | --- | --- |
| ds-elections-001 | cat-elections-001 | US election returns (precinct/county/state) | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-elections-002 | cat-elections-002 | UK election results | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-elections-003 | cat-elections-003 | FEC campaign-finance data | C | medium | cleared (confirm at gate-002) | catalog-025 |
| ds-elections-004 | cat-elections-004 | OpenElections results | B | low | Verify → gate-002 + policy-022 | catalog-025, policy-022 |
| ds-elections-005 | cat-elections-005 | EU / European Parliament election data | B | low | Verify → gate-002 + policy-022 | catalog-025, policy-022 |

### Science (biodiversity, astronomy, genomics)

| Task ID | Catalog ID | Dataset | Priority | Risk | Licence gate | Depends on |
| --- | --- | --- | --- | --- | --- | --- |
| ds-science-001 | cat-science-001 | GBIF species occurrence data | B | low | Verify → gate-002 + policy-022 | catalog-025, policy-022 |
| ds-science-002 | cat-science-002 | NASA Exoplanet Archive | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-science-003 | cat-science-003 | Sloan Digital Sky Survey (SDSS) | B | low | Verify → gate-002 + policy-022 | catalog-025, policy-022 |
| ds-science-004 | cat-science-004 | Ensembl genome annotation | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-science-005 | cat-science-005 | GenBank sequence database | A | low | cleared (confirm at gate-002) | catalog-025 |
| ds-science-006 | cat-science-006 | Gaia mission astrometry | A | low | cleared (confirm at gate-002) | catalog-025 |

### Humanitarian

| Task ID | Catalog ID | Dataset | Priority | Risk | Licence gate | Depends on |
| --- | --- | --- | --- | --- | --- | --- |
| ds-humanitarian-001 | cat-humanitarian-001 | Humanitarian Data Exchange (HDX) datasets | B | low | Verify → gate-002 + policy-022 | catalog-025, policy-022 |
| ds-humanitarian-002 | cat-humanitarian-002 | ACLED conflict events | B | low | Verify → gate-002 + policy-022 | catalog-025, policy-022 |
| ds-humanitarian-003 | cat-humanitarian-003 | Refugee population statistics | B | low | Verify → gate-002 + policy-022 | catalog-025, policy-022 |
| ds-humanitarian-004 | cat-humanitarian-004 | INFORM Risk Index | B | low | Verify → gate-002 + policy-022 | catalog-025, policy-022 |
| ds-humanitarian-005 | cat-humanitarian-005 | ReliefWeb data (disasters, reports) | B | low | Verify → gate-002 + policy-022 | catalog-025, policy-022 |
| ds-humanitarian-006 | cat-humanitarian-006 | Displacement Tracking Matrix (DTM) | C | medium | Verify → gate-002 + policy-022 | catalog-025, policy-022 |

### Excluded — no datasheet task created

| Catalog ID | Dataset | Reason |
| --- | --- | --- |
| cat-census-006 | IPUMS census micro-data (international/USA) | high-PII micro-data |
| cat-health-004 | Global Health Observatory (GHO) | non-commercial (policy-022) |
| cat-airq-004 | WHO Ambient Air Quality Database | non-commercial (policy-022) |
| cat-geo-003 | GADM administrative areas | non-commercial (policy-022) |

### Backlog rollup

- **80 processing tasks** generated from 84 catalog entries (4 excluded).
- By priority: **A (do-first, clearly-permissive + low PII): 52** · **B (gate/policy-022 first): 22** · **C (medium-PII, reviewer): 6**.
- Scheduling: Priority-A feeds M2/M3; B unblocks as `policy-022` decisions land; C requires the License+PII reviewer.
  No silent caps — the full 80-task backlog is listed; milestone targets in PLAN.md pull from it by throughput.
