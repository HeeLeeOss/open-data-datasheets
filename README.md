# open-data-datasheets

> Documentation & metadata that makes public open datasets reusable.  ·  **Risk tier:** low-medium  ·  **Status:** proposed (planning)

For high-value public open datasets, produce standardized documentation — data dictionaries,
provenance and license records, "Datasheets for Datasets" / Croissant ML-metadata, and small
validation scripts — so the data is actually discoverable and reusable. Documentation, not the
data itself, is the deliverable.

**Definition of shipped:** A datasheet + machine-readable metadata accepted onto a dataset's portal, with verified license + provenance.

This is a **Hee-Lee Oss** good-deed project. Contributors pull a task, do it with their own coding agent, and open a PR. Get started: https://github.com/Hee-Lee-Oss-Projects/hee-lee-oss-downloads

## Planning
- [PROPOSAL.md](./PROPOSAL.md) — why this qualifies as a good deed (Good Deed Definition)
- [PLAN.md](./PLAN.md) — architecture, roadmap & milestones, risks
- [TASKS.md](./TASKS.md) — the full task backlog
- [tasks/](./tasks/) — ready-to-pull task JSON(s)

## Contribute
```bash
hee-lee-oss browse
hee-lee-oss pull --task-file tasks/open-data-datasheets-template-001.json --repo Hee-Lee-Oss-Projects/open-data-datasheets
# do the work with your own agent, then:
hee-lee-oss submit open-data-datasheets-template-001 --repo Hee-Lee-Oss-Projects/open-data-datasheets
```

## Licensing & review
- **Licensing:** Docs/metadata: CC-BY / CC0. Tooling: MIT.
- **Review:** risk tier **low-medium** — deeds are *delivered, not merged*; a domain reviewer must sign off before merge.

> Status: this project is in **planning** and not yet ratified through Hee-Lee Oss governance; no adopting partner/requestor is secured yet (`verifiedNeed: false` on delivery-dependent tasks).
