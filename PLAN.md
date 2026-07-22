# PLAN — open-data-datasheets

> Status: Draft · Version: 0.4.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

## Executive summary

High-value public open datasets are frequently published with little to no usable documentation:
no data dictionary, no provenance trail, ambiguous licensing, and no machine-readable metadata.
The data is technically "open" but practically unusable — reusers cannot tell what a column means,
whether the license permits derivatives, how current the data is, or whether it contains personal
information. This project produces standardized, source-verified **documentation and metadata** for
such datasets — data dictionaries, "Datasheets for Datasets", Croissant ML metadata, provenance and
license records, and small validation scripts — and contributes them back to the dataset's portal
or repository.

The **deliverable is documentation, not the data**. We never republish, mirror, or transform the
underlying data; we describe it. Each dataset is a self-contained unit of work suitable for a single
donated AI session plus human review. The hard constraint and central design principle is the
**license gate**: every dataset is verified to permit reuse and derivative metadata (OGL, CC-BY,
CC0, or equivalent) before any documentation work begins, and any dataset with unclear terms or
unanonymized personal/identifying data is flagged and excluded rather than guessed at.

Risk tier is **low–medium**. The primary risks are not technical but factual and legal: misstating
what a dataset contains, mischaracterizing a license, or documenting a dataset that should never
have been in scope (PII). The plan front-loads license/provenance/PII review as a hard gate.

## Problem & beneficiaries

**Who is helped.** Researchers, data journalists, civic-tech groups, students, and downstream data
portals — anyone who wants to reuse a public dataset but is blocked by missing documentation.
Improving documentation also helps the original publishing body (often a government or public
institution) by increasing the verifiable, attributed reuse of data they already paid to collect.

**The verified need.** Poor documentation is widely cited as the single largest barrier to reuse of
open data. The proposal asserts this need as verified. We treat the *general* need as well
established, but the **per-dataset, per-partner need is TO BE SECURED**: we have not yet confirmed a
named portal or publisher who has agreed to accept contributed documentation. Until a specific
portal/maintainer confirms they will review and accept contributions, individual tasks carry
`verifiedNeed: false`. This honesty matters because "delivered, not merged" requires the output to
actually be accepted by a beneficiary, not merely produced.

**Partner org.** TO BE SECURED. Candidate channels include CKAN-based national/municipal portals,
Socrata-based city portals, data.gov dataset maintainers, and dataset repositories on GitHub/Hugging
Face/Zenodo. M0 includes explicit partner-outreach work; no partner is assumed.

## Goals and non-goals

**Goals**
- Produce a reusable, standards-aligned documentation template and toolkit (data dictionary,
  provenance record, license record, known-issues, examples, Datasheet, Croissant metadata).
- For each in-scope dataset, deliver complete, source-verified documentation + machine-readable
  metadata that measurably improves reuse.
- Make license and provenance verification a non-skippable, auditable gate.
- Provide small, dependency-light validation scripts that let any reuser re-check the dataset
  against its documented schema.
- Provide portal-contribution adapters (CKAN/Socrata/data.gov) so metadata lands in the right
  format on the right portal.

**Non-goals**
- We do **not** host, mirror, clean, transform, or republish the underlying data.
- We do **not** create datasets, scrape data, or generate synthetic data.
- We do **not** document datasets with unclear licenses or unanonymized PII — these are flagged and
  excluded, not "best-guessed."
- We do **not** provide analytical conclusions, rankings, or interpretation of the data's meaning.
- We do **not** auto-publish to portals; a human submits/contributes after review.

## Success metrics (outcomes)

Outcome-based, beneficiary-centric. Vanity metrics (e.g., "docs written") are explicitly excluded.

| Metric | Baseline | Target (first 6 months) |
| --- | --- | --- |
| Datasets with documentation **accepted onto the source portal/repo** (last-mile delivered) | 0 | 8 accepted |
| Datasets passing the license+provenance+PII gate / total triaged | n/a | ≥ 95% of *triaged-in* datasets have a recorded, verifiable license |
| Reuse signal: documented datasets cited/forked/referenced by a third party | 0 | ≥ 3 with a verifiable external reuse event |
| Confirmed contribution partners (portals/maintainers accepting contributions) | 0 | ≥ 2 secured |
| Documentation defects found in human/expert review (factual or license errors) | n/a | < 1 license/PII error per 10 delivered (target: 0 PII/license errors) |
| Reusable template + Croissant validator adopted across tasks | n/a | 100% of delivered datasets use the standard template |

Note on attribution of outcomes: a "reuse event" must be externally verifiable (a citation, a
portal acceptance record, an issue/PR referencing the docs). Self-reported reuse does not count.

**Quantifying "improves reuse" and "effort reduced" (so DoDs are verifiable).**
- *Reuse improvement* is measured per dataset as a **documentation-completeness score** (0–100):
  the fraction of canonical-metadata fields populated and source-verified (data dictionary
  coverage of all columns, provenance complete, license recorded with `permitsDerivatives`,
  Datasheet sections answered, valid Croissant emitted). Target: every delivered dataset reaches
  **≥ 90/100**, versus a recorded **before-score** captured at triage on the dataset as-published.
  The before/after pair is stored in the dataset's gate/provenance artifact.
- *Per-dataset effort* is measured in **AI-session wall-clock minutes + number of human-review
  cycles** from gate-pass to upstream submission, logged by the Steward in the outcome ledger
  (see below). M2's "effort reduced" DoD compares the median of adapter-era datasets against the
  recorded M0/M1 baseline median.

**What "accepted onto the portal/repo" means, per channel, and the evidence the Steward records.**
The Steward records a single canonical **acceptance evidence artifact** per dataset in the outcome
ledger (a committed `outcomes/<dataset-id>.json` with channel, URL/permalink, timestamp, and the
completeness before/after scores). Acceptance is defined per channel:
- **CKAN/Socrata/data.gov portals:** the contributed metadata is live on the dataset's portal page
  (permalink) or a maintainer's written confirmation of acceptance; evidence = the permalink.
- **GitHub/Hugging Face repos:** a merged PR (or accepted dataset-card change); evidence = the
  merge commit URL.
- **Zenodo:** an updated record / new version DOI for the metadata; evidence = the DOI.
- **Informal/email channel:** explicit written acceptance from the named maintainer; evidence = the
  archived message reference. Self-reported or "submitted but unconfirmed" never counts as accepted.

## Scope

**In scope**
- Documentation artifacts: data dictionary, provenance record, license record, known-issues,
  worked examples, Datasheet-for-Datasets, Croissant ML metadata (JSON-LD).
- Small validation scripts (schema/type/row-count/null checks) emitting a data-quality report.
- Portal-contribution adapters that emit portal-native metadata (CKAN package, Socrata dataset
  metadata, data.gov / DCAT-US fields) from our canonical metadata.
- License, provenance, and PII triage and recording for each candidate dataset.

**Candidate dataset backlog.** The pool of datasets we *might* document is maintained in
`DATASET-CATALOG.md` — a comprehensive, license-classified catalog of real open datasets across ~16
domains (government spending, census, public health, climate, air quality, energy, transport,
education, justice, geospatial, economy, agriculture, housing, elections, science, humanitarian).
The catalog is a **candidate backlog only**: each entry's `License` / `Permits derivatives?` /
`PII risk` columns are first-pass triage signals, and no entry becomes a task until it passes the
per-dataset license+PII gate (`gate-002` under `policy-022`). The catalog deliberately biases toward
clearly permissive sources (OGL v3, CC0, CC-BY, US-gov public domain) and honestly flags
share-alike (ODbL), NC, and ambiguous/custom terms as `Verify` or excluded rather than guessing.

**Out of scope**
- The data itself (no hosting/mirroring/transformation/cleaning/republishing).
- Datasets with unclear/unverifiable licenses or with personal/identifying data not properly
  anonymized.
- Interpretation, analysis, modeling, or ranking of the data.
- Automated, unattended publishing to any portal.
- Any task that primarily serves a for-profit entity's private data.

## Solution approach & architecture

This is a **content/data-documentation project with light software** (templates + validators +
adapters). It is not a data pipeline that moves data.

**Pipeline (per dataset)**
1. **Triage & gate** — identify the dataset, its publisher, its license, and presence of PII.
   PASS only if license permits reuse + derivatives and no unanonymized PII. Record decision.
2. **Provenance capture** — source URL, publisher, retrieval date, version/edition, update cadence,
   license URL + license text snapshot reference, attribution string.
3. **Schema documentation** — data dictionary: field name, type, units, allowed values, nullability,
   description, caveats. Derived by *inspecting* the data under the access protocol below, never by
   republishing it.
4. **Datasheet** — Datasheets-for-Datasets questionnaire: motivation, composition, collection,
   preprocessing, uses, distribution, maintenance.
5. **Croissant metadata** — machine-readable JSON-LD (Croissant ML format) describing the dataset.
6. **Validation script** — small, dependency-light script that checks the live/source data against
   the documented schema and emits a quality report.
7. **Portal adapter** — transform canonical metadata into the target portal's format.
8. **Review & contribute** — human review (license/PII reviewer + technical reviewer), then a human
   submits the contribution to the portal/repo.

**Canonical metadata model.** A single internal metadata object per dataset is the source of truth;
all outputs (Datasheet, Croissant, portal-specific) are *projections* of it. Fields include:
`id`, `title`, `publisher`, `sourceUrl`, `license {id, url, permitsDerivatives:boolean, snapshotRef}`,
`provenance {retrievedAt, version, updateCadence, attribution}`, `pii {present:boolean,
anonymized:boolean, notes}`, `fields[] {name, type, units, allowedValues, nullable, description,
caveats}`, `knownIssues[]`, `examples[]`, `specVersions {croissant, ckan, socrata, dcatUs}`,
`completenessScore {before, after}`.

**Tech stack.** TypeScript, ESM, pnpm workspaces (per Hee-Lee Oss conventions). Validators and adapters
are small Node packages with minimal dependencies. Documentation authored in Markdown + JSON/JSON-LD.
No runtime services; everything runs locally or in CI.

**Pinned target spec versions** (so the "validators pinned to versions" mitigation is real — each is
recorded in the canonical model's `specVersions` block and bumped only via a deliberate task):
- **Croissant ML** — v1.0 (MLCommons published spec); validate against the v1.0 JSON-LD context/SHACL.
- **CKAN** — package schema for CKAN **2.10** API (`package_create`/`package_show` shape).
- **Socrata** — SODA 2.x dataset metadata fields (DCAT-AP export shape as exposed by current
  Socrata portals).
- **DCAT-US** — **v3** (data.gov's current target; falls back to DCAT-US v1.1 where a portal still
  requires it, recorded per dataset).
Any spec/version drift is handled by an isolated version-bump task, never by silently changing output.

**Dataset-inspection access protocol (makes "describe but never store" enforceable).** Inspection is
the only point at which we touch the data, so it is constrained by a hard protocol the validators and
contributors must follow:
- **Header/streamed reads only.** Prefer schema/header endpoints (CKAN/Socrata field metadata, HTTP
  `Range`/`HEAD`, columnar footers); for row-level type/value inference, stream and sample rather
  than downloading the full file.
- **Row cap.** Inspect at most a bounded sample (default **1,000 rows** or the first ~5 MB,
  whichever is smaller) — enough to infer types, units, allowed values, and nullability; never a
  full extract.
- **Local-only and ephemeral.** Any bytes read live only in the contributor's local working memory
  /scratch for the session and are deleted afterward; they are never written into the repo, CI
  artifacts, receipts, or logs.
- **No committed samples.** Worked `examples[]` use synthetic or trivially public illustrative rows,
  or cite a row by reference (source URL + row id) — we never paste real data rows into committed
  documentation.
- **Stop on PII signal.** If inspection surfaces any PII signal (see Privacy/PII stance), inspection
  halts immediately, the sample is discarded, and the dataset is routed to EXCLUDE/FLAG.

**Key decisions.**
- Canonical-model-first so we never hand-maintain three parallel metadata formats.
- License/PII gate is a *blocking* step encoded as a checklist artifact committed with the work,
  not an informal judgement.
- Adapters are output-only (we generate metadata files); a human performs the actual portal upload.

## Data, licensing & compliance

**This is the critical section.** The project's deliverable is metadata *about* data; we still must
treat licensing and privacy conservatively because we describe and link to others' data.

**Source licenses we accept (must permit reuse AND derivative works):**
- **Open Government Licence (OGL)** v2.0/v3.0 — accepted; attribution required.
- **CC0 1.0** — accepted; no attribution required (we still record provenance).
- **CC-BY 4.0** (and compatible CC-BY versions) — accepted; attribution required.
- Other licenses (CC-BY-SA, CC-BY-NC, ODbL, bespoke government terms) → governed by the **written
  NC/share-alike acceptance policy** (task `policy-022`), which **must be decided and merged before
  any triage runs** so triage applies a fixed rule rather than ad-hoc judgement. NC terms require
  care: our docs are non-commercial public-commons artifacts, but the policy fixes whether/when NC
  and share-alike sources are accepted, escalated, or excluded.

**Objective "permits derivatives" acceptance criterion.** A dataset PASSes the license check only if
its license is on the accepted list (or accepted by the NC/share-alike policy) **and** an explicit
`license.permitsDerivatives: true` is recorded with a cited clause/URL evidencing that derivative
documentation/metadata is allowed. Missing evidence, an unparseable license, or `permitsDerivatives`
that cannot be set `true` from the source text = FLAG/EXCLUDE, never a default-allow.

**Excluded:**
- Datasets with **no clear license**, "all rights reserved," or terms that forbid derivatives →
  **flagged and excluded**, never guessed.
- Datasets containing **personal or identifying data** unless they are **properly anonymized** by
  the publisher and that anonymization is documented → excluded otherwise.

**Provenance model.** Every documented dataset records: source URL, publisher, retrieval timestamp,
dataset version/edition, license identifier + license URL + a captured snapshot of the license text
as it stood at retrieval (storage format decided below — committed copy + SHA-256 hash + Wayback save
URL), update cadence, and the required attribution string. Provenance is part of the committed
deliverable.

**Privacy/PII stance.** PII triage is mandatory and happens *before* documentation. If a dataset
contains direct identifiers (names, addresses, emails, precise geolocation tied to individuals,
government IDs) or plausibly re-identifiable quasi-identifiers, and the publisher has not documented
proper anonymization, the dataset is excluded and the concern is surfaced. We never anonymize data
ourselves (that would be transforming the data, which is out of scope).

**PII detection methodology** (applied during the gate, within the inspection access protocol, so
the judgement is repeatable rather than a vibe):
- **Column-name heuristics.** Flag fields whose names/types match direct-identifier patterns (name,
  first/last, email, phone, SSN/NINO/national-id, address, postcode/zip at unit level, IP, MRN,
  DOB, lat/long, device/account id). A maintained keyword+regex list lives with the gate checklist.
- **Quasi-identifier / k-anonymity flag.** For combinations of quasi-identifiers (e.g. DOB + sex +
  postcode), estimate the smallest equivalence-class size on the inspection sample; flag if the
  smallest group is below a **k ≥ 5** threshold (re-identification risk).
- **Geo precision threshold.** Flag geolocation finer than the publisher's stated aggregation —
  e.g. coordinates beyond ~3 decimal places (~100 m), full postcodes, or sub-area block codes —
  unless documented as already aggregated/anonymized.
- **Linkage risk.** Flag datasets that, while individually de-identified, are trivially linkable to a
  known external identifier set (e.g. a person-level key that joins to a public roll/registry).
Any flag = EXCLUDE/FLAG unless the publisher documents anonymization meeting these thresholds; the
methodology output (which checks ran, what fired) is recorded in the committed gate artifact.

**Attribution requirements.** All produced documentation attributes the original publisher per the
source license, links to the original source, and clearly states that the documentation — not the
data — is our contribution. Our documentation/metadata output is licensed **CC-BY-4.0**; validator
and adapter **code is MIT**.

## Quality, review & risk gates

**Risk tier: low–medium** (per proposal). Most documentation is low risk; medium applies where
factual accuracy about sensitive context, license interpretation, or PII judgement is involved.

**Required review before a deed is "done":**
- **License + PII reviewer** (mandatory, every dataset): confirms the recorded license permits
  reuse/derivatives and that no unanonymized PII is present. This is the hard gate.
- **Technical reviewer**: confirms the data dictionary, Datasheet, Croissant metadata, and
  validation script are accurate and run cleanly (CI green).

**Test fixtures & golden files (so "CI green" actually means something).** Each tool ships with
committed test assets, exercised in CI, using only synthetic/public fixtures (never real inspected
data):
- **Croissant validator** — a set of golden JSON-LD fixtures: known-valid metadata that must pass and
  deliberately malformed cases that must fail, validated against the pinned Croissant v1.0 context.
- **Portal adapters (CKAN/Socrata/DCAT-US)** — golden input (canonical metadata) → expected output
  (portal-native payload) pairs, diffed in CI; adapter output is additionally validated against the
  pinned spec/schema for that portal.
- **Sandbox portal target** — adapters are verified against a real **CKAN demo/sandbox instance**
  (e.g. a `ckan` Docker image or the public demo portal) for at least one round-trip import; where a
  live sandbox is unavailable (Socrata), a captured real import schema is committed and asserted
  against. No adapter is "done" on golden files alone without one sandbox/real-schema check.
- **Expert/domain reviewer** (for any dataset touching health/legal/safety context, or any
  ambiguous license): required sign-off, escalating the task to `riskTier: medium` (or `high` if
  the underlying domain is high-stakes — such datasets are out of scope unless an expert signs off).

**Definition of Shipped.** Documentation + machine-readable metadata **accepted onto the dataset's
portal or repository** — per the per-channel acceptance definitions in Success metrics (live portal
permalink / merged PR / Zenodo DOI / written maintainer confirmation) — with a verified license,
recorded provenance, and the completeness score reaching ≥ 90/100. Shipped requires the Steward to
have recorded the canonical **acceptance evidence artifact** (`outcomes/<dataset-id>.json`).
Producing the docs is *not* shipped; recorded acceptance by the beneficiary is.

## Roadmap & milestones

**M0 — Foundation & cold-start (thin)**
- Goal: build the reusable toolkit and prove the end-to-end flow on one dataset; begin partner
  outreach.
- **Cold-start de-risking (pilot dataset selection).** To avoid producing docs nothing accepts, the
  pilot is gated on a realistic acceptance path *before* documentation begins, in priority order:
  (a) at least an **informal channel** — a maintainer who has verbally/by-email agreed to look at a
  contribution; failing that, (b) a **self-serve fallback** on a channel where acceptance does not
  depend on a third party — a dataset we host docs for via a **GitHub PR to the dataset's own repo**
  or a **Zenodo** metadata record/DOI we can publish ourselves. The pilot must use one of these so
  M0 can yield a real *accepted* outcome, not merely a "submitted, pending" one.
- Exit criteria: (1) documentation template + canonical metadata model published; (2) Croissant
  validator + one validation script working in CI with golden fixtures; (3) the license/PII gate
  checklist (incl. the NC/share-alike policy) exists and is applied to one dataset; (4) one dataset
  fully documented end-to-end and **accepted** via an informal channel or self-serve fallback (with
  the acceptance evidence artifact recorded) — or, if no channel materializes, **submitted** with the
  blocker surfaced; (5) at least one partner-outreach thread opened.

**M1 — License/provenance gate hardened + first acceptances**
- Goal: make the gate rigorous and get real deliveries accepted.
- Exit criteria: (1) license/PII gate codified as a reviewable artifact and applied to ≥ 3 datasets;
  (2) ≥ 2 datasets **accepted** onto their portal/repo; (3) ≥ 1 confirmed contribution partner;
  (4) provenance model captures license snapshots automatically where feasible.

**M2 — Portal adapters & scale**
- Goal: reduce per-dataset effort via CKAN/Socrata/data.gov(DCAT-US) adapters.
- Exit criteria: (1) at least two portal adapters emit valid portal-native metadata, verified
  against a real portal's import format (sandbox or captured schema); (2) ≥ 5 datasets
  delivered+accepted cumulatively; (3) median per-dataset effort (AI-session minutes + human-review
  cycles, from the outcome ledger) measurably reduced vs. the recorded M0/M1 baseline median.

**M3 — Reuse outcomes & sustainability**
- Goal: demonstrate real downstream reuse and a maintenance model.
- Exit criteria: (1) ≥ 3 verifiable external reuse events; (2) ≥ 8 datasets accepted cumulatively;
  (3) documented maintenance/refresh process and a steward identified for ongoing portal liaison.

**Scaling via the candidate catalog.** From M1 onward, throughput is driven by triaging entries from
`DATASET-CATALOG.md` through the license+PII gate into per-dataset datasheet tasks. The catalog
(84 candidates / 16 domains at v0.1.0) is the funnel; the gate is the filter. Measurable
catalog-tied targets:
- **M1:** ≥ 5 catalog entries triaged (committed gate artifact each); ≥ 2 of those datasheets
  accepted upstream.
- **M2:** ≥ 15 catalog entries triaged cumulatively; ≥ 5 datasheets accepted cumulatively.
- **M3:** ≥ 25 catalog entries triaged cumulatively; ≥ 8 datasheets accepted cumulatively, drawn
  from ≥ 5 distinct catalog domains to demonstrate breadth. Triage yield (PASS rate) and exclusions
  are tracked so the catalog can be expanded/pruned as domains prove out.

Dependencies: M1 depends on M0 toolkit; M2 adapters depend on M1's canonical metadata; M3 depends on
a body of accepted deliveries from M1–M2.

## Work breakdown

The itemized, schema-mapped backlog lives in `TASKS.md`. It is organized by the milestones above,
each with a task table (`ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer`),
acceptance criteria for the most important tasks, and a milestone Definition of Done. A backlog of
sized-but-unscheduled tasks and one complete example Task JSON are included there. The **candidate
dataset backlog** that feeds the per-dataset documentation tasks lives in `DATASET-CATALOG.md`;
`TASKS.md` includes a catalog-triage task and example per-dataset tasks drawn from real catalog
entries (`cat-…` IDs), each mapped to the Task JSON schema.

**Per-dataset processing backlog (full).** `TASKS.md` now also carries a *Per-dataset processing
backlog* generated from every catalog entry: **80 datasheet tasks** (4 of the 84 catalog entries are
policy-excluded — 2 non-commercial, 1 non-redistributable, 1 high-PII micro-data). Each task
instantiates a shared datasheet template (data dictionary + provenance + licence snapshot +
Datasheet-for-Datasets + Croissant metadata + validation script; documentation only). The backlog is
prioritized so throughput is unambiguous:

- **Priority A — 52 tasks:** clearly-permissive licence (OGL v3 / CC0 / CC-BY / US-gov PD / Copernicus
  / Etalab) + none/low PII. These are the *good-first-deed* engine.
- **Priority B — 22 tasks:** licence marked `Verify` or share-alike (ODbL) — blocked on the per-dataset
  gate (`gate-002`) and the NC/share-alike policy (`policy-022`) before work starts.
- **Priority C — 6 tasks:** medium-PII / sensitive datasets (e.g. FARS, TLC trips, FEC donors, IOM DTM)
  — `riskTier: medium`, mandatory License+PII reviewer sign-off.

No silent caps: the entire 80-task backlog is enumerated; milestone targets below pull from it by
throughput rather than truncating it.

## Governance, roles & stakeholders

- **Maintainer (Owner):** TBD — owns the toolkit, triage, and backlog.
- **License + PII reviewer:** TBD (name TO BE SECURED) — mandatory, **non-skippable** gatekeeper; no
  deed ships without this sign-off. Because the role is a hard gate, it must be filled **before the M0
  pilot (`pilot-005`) is reviewed** — it is a blocking prerequisite, not a parallel hire. *How the
  role is filled:* the Maintainer recruits or designates a named reviewer who can read open-data
  licenses (OGL/CC/ODbL/SPDX) and apply the PII detection methodology; the appointment (name +
  qualification) is recorded in this section. The role may rotate among ≥ 2 qualified reviewers to
  avoid a single-person bottleneck, but at least one named, qualified reviewer must exist at all times
  or triage/documentation halts. Until named, all tasks remain `verifiedNeed:false` and no dataset
  can pass the gate.
- **Technical reviewer(s):** rotation of contributors who verify dictionaries, Croissant metadata,
  and validators (CI green).
- **Expert/domain reviewer(s):** engaged per-dataset for sensitive domains or ambiguous licenses
  (medium/high risk tier).
- **Steward (last-mile owner):** TBD — owns the relationship with portals/maintainers and confirms
  acceptance (the "delivered" signal). Critical because Definition of Shipped is acceptance, not
  production.
- **Partner / requestor:** TO BE SECURED — named portal(s) or dataset maintainer(s).

## Dependencies & integrations

- **External standards/specs (pinned — see Tech stack):** Datasheets for Datasets (Gebru et al.),
  Croissant ML v1.0, CKAN 2.10 package schema, Socrata SODA 2.x metadata, DCAT-US v3 (DCAT-US v1.1
  fallback), schema.org Dataset, SPDX license identifiers. Versions recorded in `specVersions` and
  bumped only via a deliberate task.
- **External services/portals:** CKAN, Socrata, data.gov; dataset repos (GitHub, Hugging Face,
  Zenodo). Integration is *output-only* metadata; no automated upload.
- **Datasets:** specific public datasets — TO BE SELECTED via triage; none assumed in scope yet.
- **Hee-Lee Oss pieces:** Task JSON schema (`packages/schema`), the donated-lane CLI workspace/PR flow
  (`packages/cli`), good-deed definition + refusal guardrails. No funded-lane/runner dependency
  (this project is donated lane).

## Risks & mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
| --- | --- | --- | --- | --- |
| Misclassifying a license (treating non-reusable data as reusable) | Medium | High | Mandatory license reviewer; record license URL + text snapshot; exclude on any doubt | License+PII reviewer |
| Documenting a dataset that contains unanonymized PII | Low | High | PII triage as a blocking pre-step; exclude unless publisher-documented anonymization; never anonymize ourselves | License+PII reviewer |
| Factual errors in data dictionary / Datasheet | Medium | Medium | Technical review; validation script must run; source-verified statements only | Technical reviewer |
| No portal partner secured → docs produced but never accepted (fails "delivered") | Medium | High | M0 partner outreach; steward role; mark `verifiedNeed:false` until secured | Steward |
| Source data changes/disappears, making docs stale | Medium | Medium | Record version + retrieval date; validation script detects drift; maintenance milestone | Maintainer |
| Croissant/portal format drift (spec or portal API changes) | Medium | Low | Canonical-model-first; validators pinned to spec versions; adapters isolated | Maintainer |
| Scope creep into cleaning/transforming data | Medium | Medium | Explicit non-goal; reviewers reject any data-transformation work | Maintainer |
| NC / share-alike license ambiguity blocks contribution | Medium | Low | Case-by-case human decision; default exclude; record rationale | License+PII reviewer |

## Security & privacy

- **Threat surface is small** (no runtime service, no data hosting). Main surfaces are CI and the
  metadata files we publish.
- **Secrets handling:** validators/adapters require no credentials by default. If a portal API
  token is ever needed for a contribution, it is supplied by the human submitting and must never be
  written into logs, receipts, or committed files (per Hee-Lee Oss rules).
- **PII:** the dominant privacy concern is *upstream* PII in candidate datasets. Handled by the
  mandatory exclusion gate above. We do not download, store, or process personal data; we inspect
  schema/sample only enough to document, and exclude on any PII signal.
- **Abuse/misuse prevention:** refuse and flag any task steering documentation toward surveillance,
  de-anonymization, targeting individuals, or laundering a non-open dataset as open. Documentation
  must remain descriptive and source-verified.

## Sustainability & maintenance

- **Post-delivery ownership:** the steward maintains portal relationships; the maintainer keeps the
  toolkit (validators, adapters, template) current with spec/portal changes.
- **Refresh:** validation scripts let anyone re-check a dataset against its documented schema;
  recorded version + cadence flags when a refresh is due. Stale docs are tracked as maintenance
  tasks (`type: maintenance`).
- **Outcome tracking:** the steward records acceptance events and external reuse signals against the
  success metrics; these are reviewed each milestone.

## Open questions

- Which specific portal(s)/maintainer(s) will be the first confirmed contribution partner(s)?
- How do we accept NC and share-alike licenses for *derivative metadata* — blanket policy or
  per-dataset? (Default: exclude/escalate.)
- ~~Where is the canonical license-text snapshot stored?~~ **Decided (must precede `snapshot-007`):**
  the snapshot is a **locally archived copy of the license text/page committed alongside the
  provenance record, plus a SHA-256 hash and a Wayback Machine save URL** for that page;
  `license.snapshotRef` points to the committed copy and records the hash + Wayback timestamp. WARC
  is out of scope (overkill for single license pages). Linking the bare license URL is *not*
  sufficient. This format is fixed before the capture task (`snapshot-007`) is built.
- What counts as a sufficiently "verifiable external reuse event" for the outcome metric?
- For datasets on Hugging Face/Zenodo, do we contribute via PR or via their metadata APIs?

## References

- Hee-Lee Oss work rules — `C:\code\hee-lee-oss\CLAUDE.md`
- Good Deed Definition + risk tiers — `C:\code\hee-lee-oss\docs\good-deed-definition.md`
- Task JSON schema — `C:\code\hee-lee-oss\packages\schema\src\schemas.ts`
- Proposal — `C:\code\hee-lee-oss\governance\proposals\open-data-datasheets.md`
- Candidate dataset catalog — `C:\code\hee-lee-oss\planning\projects\open-data-datasheets\DATASET-CATALOG.md`
- Datasheets for Datasets (Gebru et al., 2018/2021)
- Croissant ML metadata format specification
- DCAT / DCAT-US, schema.org/Dataset, SPDX license list
- Open Government Licence (OGL); Creative Commons CC0 / CC-BY 4.0
