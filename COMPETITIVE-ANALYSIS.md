# ANALYSIS — open-data-datasheets

> Competitive + improvement analysis for the Hee-Lee Oss good-deed project `open-data-datasheets`.
> Sources: `planning/projects/open-data-datasheets/PLAN.md` (v0.4.0) and `DATASET-CATALOG.md` (v0.1.0),
> plus web research cited inline. Date: 2026-06-28.

---

## 1. Correctness & completeness review of PLAN.md

The plan is unusually rigorous for a draft — the license/PII gate, per-channel acceptance
definitions, the "describe but never store" inspection protocol, and the honest
`verifiedNeed:false` posture are all genuine strengths and rare in this space. The gaps below are
specific and, mostly, fixable.

**Substantive gaps and errors**

1. **Croissant version is mis-pinned.** The plan pins "Croissant ML — v1.0 (MLCommons published
   spec)." MLCommons' published, adopted artifact is **Croissant 1.0** as a *vocabulary/spec*, but
   the validation surface in practice is the `mlcroissant` Python library + the JSON-LD context, and
   there is **no official SHACL shapes file** that the plan assumes ("validate against the v1.0
   JSON-LD context/SHACL"). The Croissant spec is schema.org/JSON-LD with a Python reference
   validator; SHACL is not the canonical conformance mechanism. The plan should pin to the
   `mlcroissant` validator version and the published `croissant` context URL, not an imagined SHACL
   artifact. (See https://mlcommons.org/working-groups/data/croissant/ and
   https://github.com/mlcommons/croissant.)

2. **The toolkit is TypeScript/Node, but the entire Croissant + Frictionless + data-inspection
   ecosystem is Python.** `mlcroissant`, `frictionless-py`, `tableschema`, pandas-based type
   inference, and most portal SDKs (CKAN's `ckanapi`, Socrata's `sodapy`) are Python. The plan's
   "TypeScript, ESM, pnpm" mandate (inherited from Hee-Lee Oss conventions) forces either (a)
   reimplementing Croissant validation in TS, or (b) shelling out to Python — neither is acknowledged
   as a risk. This is the single biggest unstated technical risk and deserves a row in the risk table
   and a decision in "Key decisions."

3. **The completeness-score metric is defined but not operationalized.** "≥ 90/100, fraction of
   canonical-metadata fields populated and source-verified" — but there is no field-weighting scheme,
   no definition of what "source-verified" mechanically means (human attests? validator passes?), and
   no rubric. As written, two reviewers would score the same datasheet differently. Needs a published,
   versioned scoring rubric committed alongside the template (this is also exactly where Claude can
   help — §5).

4. **"≥ 95% of triaged-in datasets have a recorded, verifiable license" is a near-tautology, not an
   outcome.** By construction, the gate excludes anything without a verifiable license, so the
   triaged-in population is ~100% licensed by definition. The interesting metric is the **triage PASS
   rate** (what fraction of *candidates* survive the gate) and the **exclusion reasons distribution** —
   those measure real funnel health. The catalog already promises to track "triage yield (PASS
   rate)"; the success-metrics table should surface it instead of the tautological version.

5. **No baseline for the headline outcome.** "8 datasets accepted" has baseline 0, which is fine, but
   there is no denominator or time-to-acceptance target. With `verifiedNeed:false` and zero secured
   partners, 8 acceptances in 6 months is aggressive. The plan should state expected
   triage→accept conversion and the acceptance latency per channel (a CKAN portal PR can sit for
   months; a Zenodo self-publish is same-day). The M0 "self-serve fallback" partly de-risks this but
   the metric table doesn't reflect the two-speed reality.

6. **PII k-anonymity threshold is applied to a 1,000-row sample — statistically unsound.** The plan
   says estimate the smallest equivalence-class size on the inspection sample and flag if `k < 5`.
   On a 1,000-row sample of a multi-million-row dataset, equivalence-class sizes are not estimable —
   a quasi-identifier combination that is unique in the sample may be common in the full data, and
   vice versa. k-anonymity is a property of the *whole* release. The methodology should be reframed:
   the sample can detect *direct identifiers* and *obvious* quasi-identifier columns (which is
   valuable), but cannot certify k-anonymity. As written it risks both false reassurance and false
   exclusion. This is a correctness bug in a load-bearing safety control.

7. **Missing competitor/prior-art section entirely.** For a standards-aligned documentation project,
   there is no "related work / why not just use X" analysis. A reviewer will immediately ask "why not
   contribute Hugging Face dataset cards, or just run the existing Croissant exporters?" The plan
   never positions against the obvious incumbents (this document's §2 fills that gap). This is a real
   completeness hole, not a nicety — it's where a funder's skepticism lands.

8. **schema.org/Dataset + Google Dataset Search discoverability is under-exploited.** The plan emits
   Croissant (which *extends* schema.org/Dataset) and DCAT-US, but never states that emitting
   schema.org/Dataset JSON-LD on a landing page is what makes a dataset appear in **Google Dataset
   Search** — arguably the single highest-leverage reuse outcome. The deliverable should explicitly
   target Dataset Search indexability as a measurable reuse signal. (See
   https://developers.google.com/search/docs/appearance/structured-data/dataset.)

9. **Spec drift in the pins.** "CKAN 2.10" — CKAN is at 2.11+ and `ckanext-dcat` 2.0 added DCAT-AP
   v3 support in 2025 (https://ckan.org/blog/enhancing-dcat-support-in-ckan-dcat-ap-v3-scheming-integration-and-more).
   "DCAT-US v3" — data.gov's deployed schema is **DCAT-US 1.1 (Project Open Data)**; DCAT-US 3.0 is a
   newer W3C-aligned draft not yet the data.gov ingest target
   (https://resources.data.gov/resources/podm-field-mapping/). The plan inverts which is current vs.
   fallback. The pins are stale on the date of the plan and should be re-verified at M0.

10. **Governance bottleneck is acknowledged but unresourced.** The License+PII reviewer is correctly
    flagged as a hard, non-skippable, blocking role — but it is also a single human reviewing every
    dataset's license *and* PII. At 80 backlog tasks this is the throughput ceiling, and "rotate among
    ≥2 reviewers" is aspirational with zero named. The plan should size reviewer-hours per dataset and
    state what happens to throughput when the reviewer is the bottleneck (it currently reads as if the
    AI session is the bottleneck, but the human gate is).

**Weaker spots worth tightening**

- The "1,000 rows or 5 MB" inspection cap is sensible but unenforceable as policy — there's no
  mechanism described that *prevents* a contributor from downloading the full file. It's a guideline,
  not a control. Either accept that framing or build a wrapper that enforces it.
- "Reuse event must be externally verifiable" is good, but the definition is left as an open
  question (it's in §8 Open Questions). For a headline metric (≥3 reuse events) the definition can't
  be open — it needs to be fixed before M3 is measurable.
- The catalog's license column for `cat-science-006` Gaia says "CC-BY-4.0" but Gaia/ESA terms are
  actually a custom ESA acknowledgement clause, not plain CC-BY — it should be `Verify`, consistent
  with how SDSS is flagged. Minor catalog-accuracy nit.

---

## 2. Competitive landscape

This project sits at the intersection of **dataset documentation standards** and **metadata
publishing/discovery infrastructure**. The competitors split into two groups: *standards/formats*
(what the docs look like) and *platforms/registries* (where docs live and how they're found). The
key insight: almost none of them do the **last-mile, source-verified, license-gated contribution
back to the original portal** that this project targets — they are mostly self-service standards or
host-it-here platforms.

### Standards & frameworks

**Datasheets for Datasets (Gebru et al., 2018/2021)** — the questionnaire the project adopts.
- *What:* A 50+ question template covering motivation, composition, collection, preprocessing, uses,
  distribution, maintenance. https://arxiv.org/abs/1803.09010
- *Strengths:* Widely cited and adopted; the de-facto vocabulary for responsible dataset
  documentation; spawned Model Cards.
- *Weaknesses/gaps:* It's a **prose template, not machine-readable**; documented criticism is the
  *workload* of answering it thoroughly (Earth-science adopters shortened it for exactly this reason
  — https://journals.ametsoc.org/view/journals/bams/106/4/BAMS-D-24-0203.1.xml); no validator, no
  license discipline, no portal integration, and "few research papers document how they constructed a
  dataset" (https://arxiv.org/html/2409.00252v1). It tells you *what questions to answer*, not *how
  to verify the answers* — which is precisely the gap this project fills.

**MLCommons Croissant** — the machine-readable format the project emits.
- *What:* A JSON-LD vocabulary extending schema.org/Dataset with ML-relevant layers (resources,
  structure, default ML semantics). Led by MLCommons + 30 orgs.
  https://mlcommons.org/working-groups/data/croissant/
- *Strengths:* Real adoption — **Hugging Face, Kaggle, and OpenML expose 400k+ datasets in Croissant**
  (https://mlcommons.org/2025/10/croissant-mcp/); backed by Google Dataset Search; 2025 work on
  Croissant + MCP makes it LLM-consumable.
- *Weaknesses/gaps:* Croissant **describes data as it already exists** — it does not generate the
  documentation, verify licenses, screen PII, or do provenance capture. It's a serialization target.
  Coverage is ML-dataset-centric and thin on civic/government tabular data outside the big repos. The
  project is *complementary* to Croissant, not competing — it produces high-quality Croissant for
  datasets the big repos never touch.

**Frictionless Data (Open Knowledge Foundation)** — Data Package + Table Schema.
- *What:* A "data definition language" — `datapackage.json` + Table Schema describe tabular data
  fields, types, constraints; FAIR-oriented; Python/R/JS implementations.
  https://specs.frictionlessdata.io/ , https://framework.frictionlessdata.io/
- *Strengths:* Mature, lightweight, strong tooling for *validation* (the Frictionless Framework
  validates data against a declared schema — exactly the "validation script" the project wants to
  build). Excellent fit for the data-dictionary layer.
- *Weaknesses/gaps:* Lower adoption than Croissant for ML; no Datasheet-style social/ethical
  documentation; no license gate; no portal-contribution workflow. **The project arguably should
  *reuse* Frictionless's validator instead of building bespoke validation scripts** (see §6).

**Data Nutrition Project / Dataset Nutrition Label** — Harvard-MIT, now independent 501c3.
- *What:* A "nutrition label" for datasets — provenance, quality, intended use, subpopulation
  representation, risks; bias-mitigation focused. https://datanutrition.org/label/ ,
  https://arxiv.org/abs/1805.03677 ; now affiliated with Consumer Reports' innovation lab.
- *Strengths:* Strong on the *bias/representation/known-issues* angle the project's "known-issues"
  field gestures at; well-regarded framing; human-readable and approachable.
- *Weaknesses/gaps:* Not machine-readable in a portal-ingestible way; manual/curated; no automated
  pipeline, no license verification, limited volume. The project is more
  infrastructure/throughput-oriented.

### Platforms & registries

**Hugging Face Dataset Cards** — the dominant practical incumbent.
- *What:* A `README.md` with YAML frontmatter (license, language, task, size tags) + standardized
  prose sections (description, structure, data fields, creation, considerations/biases, additional
  info). Renders as the dataset's front page; powers Hub search. https://huggingface.co/docs/hub/datasets-cards
- *Strengths:* Enormous adoption and network effects; the YAML is effectively a metadata standard;
  emits Croissant automatically; PR-based contribution (which is *exactly* the project's
  self-serve/GitHub fallback channel).
- *Weaknesses/gaps:* HF cards are **author-supplied and frequently empty or thin** — quality is
  wildly uneven, license tags are often wrong or missing, and there is **no verification step**. HF
  is also a *hosting* platform — the project's "never host the data" stance means it would contribute
  card improvements via PR rather than mirror datasets there. **This is the project's most direct
  channel AND its most direct competitor: it could differentiate purely on verified-quality cards.**

**CKAN** — the open-source portal powering data.gov, EU, and hundreds of government portals.
- *What:* Metadata-driven open-data portal software; `ckanext-dcat` exposes/consumes DCAT (AP v1/v2/v3,
  DCAT-US) and can emit Croissant. https://ckan.org/features/metadata/ ,
  https://github.com/ckan/ckanext-dcat
- *Strengths:* The default substrate for government open data; rich metadata schema; the project's
  primary contribution target.
- *Weaknesses/gaps:* CKAN portals are notorious for **sparse, low-quality metadata** — academic
  studies repeatedly find missing licenses, inconsistent vocabularies, and low field-population
  (https://aic.ai.wu.ac.at/~polleres/publications/neum-etal-2016JDIQ.pdf ;
  https://arxiv.org/pdf/2402.06693). That low quality *is the project's market*. CKAN is the
  delivery channel, not a rival.

**Google Dataset Search** — the discovery layer.
- *What:* Indexes schema.org/Dataset (or DCAT) markup across the web; doesn't host data, points to it.
  https://developers.google.com/search/docs/appearance/structured-data/dataset ,
  https://research.google/blog/datasets-at-your-fingertips-in-google-search/
- *Strengths:* The reuse endgame — good schema.org markup → global discoverability.
- *Weaknesses/gaps:* Only as good as the markup publishers provide; no quality control; no provenance
  verification. The project should treat Dataset Search indexability as a *target outcome*, not a
  competitor.

**data.world** — commercial data catalog / collaboration platform.
- *What:* Hosted catalog with data dictionaries, lineage, governance, a knowledge graph, and an
  active open/community tier. https://data.world/
- *Strengths:* Polished collaborative cataloging, semantic layer, integrations; good UX for data
  dictionaries.
- *Weaknesses/gaps:* Commercial and host-centric (tension with "primarily benefits a for-profit"
  guardrail and "never host" non-goal); not focused on contributing verified docs *back to source
  portals*.

**OpenML** — open ML dataset/experiment platform.
- *What:* Hosts datasets + tasks + results with rich metadata APIs; co-created Croissant; emits
  Croissant via `openml-croissant`. https://github.com/openml/openml-croissant
- *Strengths:* Strong programmatic metadata, reproducibility, Croissant-native.
- *Weaknesses/gaps:* ML-research-centric; hosts data (not the project's model); narrow domain vs. the
  project's 16 civic domains.

**Adjacent: Data Provenance Initiative, Frictionless's `repository` tooling, FAIR/RDA assessment
tools (F-UJI), DataCite (DOI + metadata).** These cover slices (provenance auditing, FAIR scoring,
DOI minting) but none combine *license gate + PII gate + multi-format emission + last-mile
contribution* in one pipeline.

**Bottom line of the landscape:** the formats (Croissant, Frictionless, Datasheets, Nutrition Label)
are *standards the project should consume/emit*, and the platforms (HF, CKAN, data.gov, OpenML) are
*channels the project delivers into*. The genuinely uncontested space is **verified, license-gated,
PII-screened documentation produced as a service and contributed back to the source** — nobody owns
the verification + last-mile-delivery niche.

---

## 3. Gaps the competition leaves open

1. **No one verifies.** HF cards, CKAN metadata, and Croissant exports are all author-asserted with
   no verification step. Licenses are frequently wrong/missing; field docs are often absent. A
   **verified** datasheet (license clause cited, schema inspected, provenance snapshotted) is a
   category nobody fills at scale.

2. **License correctness is everyone's blind spot.** Metadata-quality literature consistently flags
   missing/ambiguous license info as the top barrier to reuse
   (https://zaguan.unizar.es/record/109641/files/texto_completo.pdf). No incumbent makes a
   *defensible, evidence-cited* `permitsDerivatives` determination. This is the project's headline and
   it's genuinely open.

3. **PII screening is absent from documentation pipelines.** Datasheets ask about PII in prose;
   nobody runs a repeatable detection methodology as a *gate* before documenting. (The methodology
   needs fixing per §1.6, but the *category* is uncontested.)

4. **Cross-format projection.** Each incumbent lives in one format (HF YAML, CKAN DCAT, Croissant
   JSON-LD). A **canonical model that projects to all of them** from one verified source of truth is
   not something HF/CKAN/OpenML offer to outsiders.

5. **The "orphan" civic datasets.** HF/Kaggle/OpenML cover ML-popular data; the big government
   portals host their own. The **long tail of high-value-but-poorly-documented government/civic
   datasets** (the project's 16-domain catalog) is exactly what falls between every platform's stool.

6. **Provenance snapshots with integrity.** Nobody captures a hashed, Wayback-archived license
   snapshot as a committed artifact. This is a small but real trust differentiator for
   audit/compliance reuse.

7. **Standards translation as a service.** Reusers struggle precisely because the same dataset needs
   different metadata for HF vs. data.gov vs. Zenodo. A neutral party that does that translation,
   verified, is unserved.

---

## 4. Differentiators to win

1. **Verification is the product.** "Source-verified, license-gated, PII-screened" is the moat.
   Everyone else publishes assertions; this project publishes *checked* documentation with cited
   evidence and a committed gate artifact. Lead with this in every pitch — it's the one thing
   incumbents structurally don't do.

2. **The license gate as a first-class, auditable artifact.** A committed checklist with a cited
   clause/URL and `permitsDerivatives:true` evidence is defensible in a way a HF license tag never
   is. For compliance-sensitive reusers (journalists, public sector, regulated industries) this is
   worth real money/trust.

3. **Canonical-model-first, multi-target emission.** One verified object → Datasheet + Croissant +
   CKAN + Socrata + DCAT-US + (recommended) schema.org/Dataset + HF card. No incumbent gives
   outsiders this projection. It also future-proofs against spec drift (bump one projector).

4. **Agnostic, contribute-back, never-host posture.** Because it never hosts data, the project is a
   trusted neutral that *improves the source* rather than building a rival silo. This sidesteps the
   for-profit guardrail and earns publisher goodwill (publishers want verified reuse of data they
   already paid to collect).

5. **Outcome-defined "done."** "Delivered, not merged" — acceptance recorded as a committed evidence
   artifact — is a credibility differentiator versus the vanity metrics (cards written) every other
   effort reports.

6. **Curated, license-classified civic catalog.** The 84-entry / 16-domain catalog with first-pass
   triage is itself a reusable asset and a head start no competitor has assembled for this niche.

7. **Reproducible validation any reuser can re-run.** Shipping a dependency-light validator with each
   datasheet means the documentation is *self-checking* over time — stronger than static cards that
   silently rot.

---

## 5. Claude API leverage

An Anthropic API key + Claude turns this from a hand-cranked documentation effort into a
high-throughput, human-verified pipeline. **Default model `claude-opus-4-8`** for judgment-heavy
steps; **`claude-haiku-4-5`** for cheap high-volume extraction; the **Batch API** (50% cost) for the
80-task backlog; **prompt caching** to amortize the template/rubric across every dataset; **citations**
to ground license reading; **structured outputs (`output_config.format`)** to emit the canonical
model directly. Critically — the Hee-Lee Oss guardrails and the plan's own gate mean **Claude drafts and
proposes; humans verify and assert.** Legal/PII determinations must never be auto-accepted.

**Where Claude adds the most value (high-confidence):**

1. **Data-dictionary drafting from schema/sample (highest ROI).** Feed Claude the column
   headers + a bounded sample + any publisher field notes; it drafts `fields[]` (name, inferred type,
   units, allowed values, nullability, plain-English description, caveats). This is the single most
   tedious, most automatable step. Use **structured outputs** to emit the `fields[]` array directly
   into the canonical model, and **Haiku + Batch** for cost. Human reviews; Claude does 90% of the
   typing. (LLM-based data-dictionary generation is an active, working area —
   https://arxiv.org/html/2507.04009v1.)

2. **Datasheet-for-Datasets first draft.** The documented criticism of Datasheets is *workload*
   (§2). Claude can answer the answerable subset (composition, structure, distribution, preprocessing)
   from the source page + dictionary and explicitly mark questions it *cannot* answer ("collection
   process — NOT IN SOURCE; requires publisher input"). This directly attacks the format's biggest
   adoption barrier. Default `claude-opus-4-8`, adaptive thinking.

3. **License reading as a drafting + evidence-extraction aid — NOT an assertion.** Claude reads the
   license text/page (via PDF/document input or by being handed the fetched text) and **proposes**
   `license.id`, a candidate SPDX mapping, and the **specific clause** that bears on derivatives —
   using the **citations feature** so every claim points at exact source text. The human reviewer
   then verifies and sets `permitsDerivatives`. This is the highest-leverage *assisted* step and the
   one with the sharpest must-not boundary.

4. **Schema/format-mapping and Croissant/DCAT generation.** Claude maps the canonical model to
   Croissant JSON-LD, CKAN package shape, DCAT-US, schema.org/Dataset, and an HF card. With
   **structured outputs** the projections are schema-valid by construction; golden-file CI (already in
   the plan) catches drift. This collapses the adapter-authoring effort.

5. **PII triage assist (flag, never clear).** Claude scans column names + sample for direct
   identifiers and obvious quasi-identifiers and produces a *flag report* feeding the human gate.
   It must **propose EXCLUDE/FLAG, never PASS** — a clean Claude scan is necessary but not sufficient
   for the human reviewer (and per §1.6 cannot certify k-anonymity).

6. **Provenance + attribution string generation, completeness-scoring assist.** Claude drafts the
   required attribution string per license, and can apply the (to-be-written) completeness rubric to
   produce a *proposed* score with per-field justification for human spot-check — turning §1.3's
   subjective metric into a consistent, auditable one.

7. **QA / consistency review.** A second Claude pass (LLM-as-reviewer) checks the datasheet against
   the source for internal consistency, unsupported claims, and missing fields before it reaches a
   human — raising the floor on what humans review.

**Where Claude must NOT be the decision-maker (hard boundaries):**

- **License acceptance / `permitsDerivatives`.** Claude proposes with citations; the License+PII
  reviewer asserts. The plan's "objective permits-derivatives criterion" must be set by a human from
  cited evidence — never default-allowed on an LLM read. (Hee-Lee Oss guardrail + the plan's hard gate.)
- **PII PASS.** Claude can fire flags; only a human (with the corrected methodology) can clear a
  dataset. A non-firing scan is not a pass.
- **Reading the *full* dataset.** Respect the inspection protocol — Claude sees the bounded sample,
  never a full extract; no real rows in committed outputs/logs.
- **Anything for a high-stakes domain (health/legal/safety)** without the required expert sign-off
  and `risk-tier` escalation.
- **Final acceptance/shipping.** Claude can draft the contribution; a human submits and records the
  acceptance evidence artifact.

**Cost/throughput mechanics:** template + scoring rubric + Datasheet skeleton + the gate checklist
go in the cached prefix (stable, reused every dataset). Per-dataset content (sample, source page,
license text) goes after the cache breakpoint. Run the 80-task Priority-A backlog through the **Batch
API** with Haiku for extraction and Opus for the judgment passes. This is where the *funded lane*
could eventually run a metered, budget-capped version — though the plan correctly scopes the project
as donated-lane for now.

---

## 6. Ten concrete optimizations

1. **Reuse Frictionless Framework for validation instead of bespoke scripts.** It already validates
   data against Table Schema and emits quality reports — adopt it for the "validation script" layer
   and map Table Schema ↔ canonical model. Less code to maintain, more credibility.
   (https://framework.frictionlessdata.io/)

2. **Resolve the Python-vs-TypeScript split explicitly.** Either (a) make the validator/Croissant
   packages a small Python sidecar wrapping `mlcroissant` + `frictionless`, or (b) commit to TS and
   vendor a Croissant validator. Don't reimplement Croissant validation in TS by hand. Record the
   decision in the plan.

3. **Add schema.org/Dataset JSON-LD as a first-class projection and target Google Dataset Search
   indexability as a measured reuse signal.** It's the highest-leverage discoverability output and
   nearly free given Croissant already extends schema.org/Dataset.

4. **Use Claude structured outputs to make the canonical model the literal LLM output.** Emit
   `fields[]`, provenance, and license-candidate blocks directly as schema-validated JSON — removes a
   hand-transcription step and a whole class of bugs.

5. **Fix the PII methodology:** scope sample-based detection to *direct identifiers and
   quasi-identifier column discovery* only; explicitly state that k-anonymity cannot be certified from
   a sample and must rely on publisher documentation or full-release analysis. Update the
   methodology output recorded in the gate artifact accordingly.

6. **Publish a versioned completeness-scoring rubric** (per-field weights, what "source-verified"
   means mechanically) committed with the template, and have Claude apply it for a proposed score.
   Turns the headline reuse-improvement metric from subjective to auditable.

7. **Build the HF dataset-card adapter and make HF/Zenodo the throughput engine for M0–M1.** They're
   the self-serve, no-third-party-gatekeeper channels — exactly what de-risks cold start. Prioritize
   the adapter that yields *accepted* outcomes fastest over CKAN/Socrata (which depend on portal
   maintainers).

8. **Add a license-snapshot integrity micro-tool** that fetches the license page, computes SHA-256,
   triggers a Wayback save, and writes `license.snapshotRef` — automating the already-decided
   provenance format and removing manual steps (with the human still verifying the *content*).

9. **Track the real funnel metrics:** triage PASS rate, exclusion-reason distribution, per-channel
   acceptance latency, and reviewer-hours per dataset. Replace the tautological "95% have a license"
   metric. This also exposes the human-gate bottleneck early.

10. **Prompt-cache the template/rubric/checklist prefix and Batch-process Priority-A.** Concrete cost
    lever: 52 clearly-permissive datasets through the Batch API (50% off) with a cached stable prefix
    and Haiku extraction is the cheapest path to volume; reserve Opus + human review for the judgment
    and gate steps.

---

## 7. Parallel & perpendicular spin-offs

**Shared-infrastructure plays (reuse the canonical model + gate):**

- **"License & PII gate as a service / library."** The gate (cited license determination + PII flag
  report + provenance snapshot) is the most differentiated component and is reusable far beyond
  datasheets — e.g. a pre-flight check any data team runs before publishing or ingesting a dataset.
  This could be the project's most valuable extractable asset.
- **Croissant/DCAT/schema.org projection toolkit.** The canonical-model→N-formats projector is a
  standalone utility valuable to any portal or publisher.
- **A verified-metadata badge/registry.** A lightweight registry of "verified datasheets" (with the
  gate artifact + snapshot) that reusers can trust, and that links back to source portals — network
  effects without hosting data.

**Downstream products / multiplier effects:**

- **Dataset-quality observatory.** Aggregating triage results across the catalog yields a public,
  longitudinal "state of open-data documentation quality" report by domain/portal — itself a
  citable, reuse-generating publication (and free marketing for the project's outcomes metrics).
- **Drift/freshness monitor.** The validation scripts already detect schema drift; run them on a
  schedule and you have a "this documented dataset changed / went stale" alerting service — a
  maintenance product with recurring value.
- **Reuse-graph / provenance-linking.** Because every datasheet records source + attribution +
  acceptance evidence, the corpus is a graph of who-reuses-what — feeding the outcome metrics and a
  potential impact dashboard.
- **MCP server for verified datasheets.** Given Croissant+MCP momentum (https://mlcommons.org/2025/10/croissant-mcp/),
  expose the verified corpus as an MCP server so AI agents can discover *license-cleared,
  PII-screened* datasets — a direct line into the LLM-tooling ecosystem and a strong, defensible
  niche ("the dataset source agents can trust").

**Perpendicular (same method, different artifact):**

- **Model cards / AI-system documentation** with the same verify-then-publish discipline.
- **API/data-product documentation** for public APIs (same gate, different schema).
- **A teaching/curriculum asset:** the gate checklist + worked examples are a ready-made "how to do
  open-data licensing and PII review" course — building the reviewer pipeline the governance section
  needs.

**Network-effect flywheel:** verified datasheets → Google Dataset Search visibility → reuse events →
publisher goodwill → more portals accept contributions → more verified datasheets. The MCP server and
the verified-badge registry are the two highest-leverage flywheel accelerants.

---

## 8. Open questions for the maintainer

1. **Python vs. TypeScript:** will you accept a Python sidecar for `mlcroissant`/`frictionless`, or
   mandate TS and reimplement? This blocks the toolkit architecture and should be decided at M0.
2. **Do you adopt Frictionless for validation** rather than building bespoke validators? (Strongly
   recommended — what would stop you?)
3. **Is schema.org/Dataset + Google Dataset Search indexability an explicit deliverable and metric?**
   If yes, it changes the projection list and the reuse-signal definition.
4. **How is the completeness score's "source-verified" defined mechanically,** and who owns the
   scoring rubric? Until fixed, the headline reuse metric isn't reproducible.
5. **PII k-anonymity from a sample is unsound — do you accept scoping sample-based PII detection to
   direct identifiers + quasi-identifier discovery only,** and rely on publisher docs for k-anonymity?
6. **What is the fixed definition of a "verifiable external reuse event"?** It's currently an open
   question but it's a headline M3 metric — it can't stay open.
7. **What is the realistic reviewer-hour budget per dataset,** and who are the ≥2 named License+PII
   reviewers? The human gate, not the AI session, is the throughput ceiling.
8. **Will an eventual funded-lane (metered Claude) version run the Priority-A backlog,** or does this
   stay strictly donated-lane? The Batch/caching economics make a budget-capped funded version
   attractive at 80 tasks.
9. **HF/Zenodo-first for cold start?** Committing to the self-serve channels for M0–M1 to guarantee
   *accepted* (not merely submitted) outcomes — is that the chosen sequencing?
10. **Which incumbent do you position against first** in external comms — Hugging Face cards (quality)
    or CKAN/data.gov metadata (completeness)? The pitch differs.

---

*Prepared for the Hee-Lee Oss `open-data-datasheets` maintainer. All competitor claims are grounded in the
cited sources; license/PII determinations in any resulting deliverable must follow the project's
human-verified gate — this analysis does not assert any dataset's license.*
