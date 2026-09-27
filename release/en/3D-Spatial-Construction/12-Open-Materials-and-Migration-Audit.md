## Open Materials and Migration Audit

Open materials preserve the records of retrieval, sources, screening, quantitative analysis, reproduction, failures, and migration. They are used to reconstruct the claims in the main text. They do not upgrade static audits or planned work into executed results.

The structured source master table is `data/papers.csv`. It holds 253 unique source ID/URL entry records and 247 normalized title keys. The retrieval and screening traces are located in `sources/search_log.csv` and `sources/screening_log.csv`. Per-paper research questions, inputs and outputs, key numbers, and evidence levels are stored separately in 〈paper/附录_逐篇证据卡.md〉. They no longer interrupt the method text.

The attack-defense contracts are stored in `data/attacks_defenses.csv`, and the benchmark long table is stored in `data/benchmarks.csv`. The statistical entry point is `analysis/run_meta_correlation.py`, with outputs located in `analysis/results_part_quant/`. The original lightweight mechanism reproduction entry points are `reproduction/run_reproduction.sh`, `reproduction/STAGE_SUMMARY.md`, and `reproduction/index.html`. The programmatic dual-mesh reproduction and the host-not-executed boundary are located in `reproduction/programmatic_modeling/`. Evidence merging and the quality gate are given by `analysis/build_evidence_db.py` and `analysis/evidence_qa.json`.

Throughout the text, citations point preferentially to papers, official technical reports, project pages, standards, or official engine documentation. Product materials only attest to public interfaces and official statements. Where the model structure, training data, or independent benchmark is not public, those parts are not written up as verified performance.
---

# Appendix — Post-cutoff update (2026-08-07 → 2026-09-26)

The body's material closes on 7 August 2026. This appendix registers later material; **the body text
is unchanged.**

---

[← Back to contents](index.md)
