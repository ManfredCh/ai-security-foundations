<div align="center">

# Foundations

**The layer under the security work: how these systems are built, not how they are attacked.**

<sub>2 background surveys + 1 tutorial outline · 86,000 English words · 214 pages of PDF</sub>

[![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC_BY--NC--SA_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/)  ·  [![Status](https://img.shields.io/badge/Status-compiled_draft-orange)](#status-and-limits)  ·  [![Language](https://img.shields.io/badge/Language-English_%7C_%E4%B8%AD%E6%96%87-blue)](#languages-and-editions)

[English](README.md) · [简体中文](README.zh.md)

</div>

> If I have seen further it is by standing on the shoulders of Giants.
>
> — Isaac Newton, letter to Robert Hooke, 1675

---

## The point

Security reviews assume you already know how the systems work. This repository supplies that layer,
and it deliberately contains **no attack or defence material**:

- how visual generation evolved — GAN → diffusion → DiT → flow matching → video — so you can see why
  the attack surface moves when the architecture moves;
- how an actionable 3D world is built — representation → geometry → physical proxy → runtime — the
  layer underneath embodied safety, and the one the unified book does not cover at all.

| Language | README | Documents |
|---|---|---|
| **English** | this file | two background surveys + a tutorial outline, 86k words, 214 pages of PDF |
| **简体中文** | [README.zh.md](README.zh.md) | 两篇背景稿 + 教程目录，9.8 万汉字，202 页 PDF |

[The point](#the-point) · [Overview](#overview) · [Files](#files-and-formats) · [Reading paths](#reading-paths) · [Citation](#citation) · [Roadmap](#cutoff-and-what-comes-next) · [License](#license) · [Contributing](#contributing)

## Overview

| Item | What it is | Security content? |
|---|---|---|
| Introductory tutorial | A from-zero course outline | — |
| Visual Generative Models | The architectural lineage that generative-vision attacks act on | **None** |
| 3D Spatial Construction | How an actionable 3D world is built | Only a limitations chapter |

**Why these live here and not in the book.** They are foundations, not attack/defence material:

- *Visual Generative Models* — the unified book already covers modern generation (diffusion 61×,
  flow matching 23×, LoRA 51×) but barely covers the early lineage (GAN 2×). This fills that layer.
- *3D Spatial Construction* — the unified book has **zero** hits for 3DGS, point clouds, meshes,
  colliders and physical proxies. It answers "how is an actionable 3D world built", the layer
  underneath embodied security.

## Files and formats

Each item ships as Markdown (read on Git) and PDF (download; figures inline).

## The introductory tutorial

| | |
|---|---|
| 中文 | [入门教程目录.md](release/zh/入门教程目录.md) · [2 pp. PDF](release/zh/入门教程目录.pdf) |
| English | [Introductory-Tutorial-Outline.md](release/en/Introductory-Tutorial-Outline.md) · [3 pp. PDF](release/en/Introductory-Tutorial-Outline.pdf) |
| Status | **Outline only — the body text is not written yet** |

- [ ] Read the outline and check whether the chapter plan matches what you need
- [ ] If something is missing, open an issue

## Background survey 1 · Modern Visual Generative Models

*Technical Lineage, Core Algorithms, and System Evolution*

| | |
|---|---|
| 中文 | [从图像到视频.md](release/zh/从图像到视频.md) · [99 pp.](release/zh/从图像到视频.pdf) |
| English | [Visual-Generative-Models.md](release/en/Visual-Generative-Models.md) · [92 pp.](release/en/Visual-Generative-Models.pdf) |
| Length | ~3 hours · 21 chapters |
| Structure | evidence method → formalisation → technical lineage → taxonomy → explicit probabilistic and latent families → adversarial implicit generation → autoregressive and token generation → score-based denoising → latent diffusion and DiT → deterministic transport → hybrid mechanisms → conditioning, control, editing → video-specific problems → data and evaluation → engineering and reproduction → safety, copyright, provenance → industry ecosystem → synthesis → agenda → limitations |

- [ ] **Technical Lineage** first — establish the timeline
- [ ] Two or three family chapters that interest you
- [ ] **Video-Specific Problems** — time, motion, long-horizon state, interaction
- [ ] **Cross-Family Synthesis** — when to use which family
- [ ] Afterwards: what separates diffusion from flow matching, and why is video generation not "an image per frame"?

## Background survey 2 · 3D Spatial Construction

*Methods from Pixels to Collidable Spaces*

| | |
|---|---|
| 中文 | [从像素世界到可碰撞空间.md](release/zh/从像素世界到可碰撞空间.md) · [101 pp.](release/zh/从像素世界到可碰撞空间.pdf) |
| English | [3D-Spatial-Construction.md](release/en/3D-Spatial-Construction.md) · [119 pp.](release/en/3D-Spatial-Construction.pdf) |
| Length | ~4 hours |
| Structure | the L0–L5 capability loop: video generation and action-conditioned frame worlds → NeRF, neural rendering and 3DGS → cameras, depth, point maps, point clouds → explicit surfaces, meshes, procedural DCC/CAD → collision navigation and game engines; plus corpus method, synthesis, datasets and evaluation, applications, and limitations |

- [ ] **Background and the Input–Output Contract** — conditioning, estimation, representation, assetisation
- [ ] **L0–L5 Method Families** (more than half the survey; skip around)
- [ ] **Cross-Family Synthesis** — observation constraints vs generative priors
- [ ] **Attacks, Defences and Limitations** — why visual usability cannot substitute for geometry
- [ ] Afterwards: which different unknowns do generation, reconstruction and rule-based modelling each solve?

## Reading paths

| Your goal | Read |
|---|---|
| Complete beginner | Tutorial outline → (once written) → 3D Spatial Construction → Visual Generative Models → then AI Security Surveys |
| Generative vision security | Visual Generative Models → then *Image and Video Generation Security* |
| Robotics / embodied security | 3D Spatial Construction → then *Closed-Loop Security of VLM, VLA, and World-Action Models* |

## Languages and editions

Chinese is the **original**. The English is a **rewritten native-English edition** produced by
faithful translation → paragraph-level native rewrite → independent check against the Chinese.
Paragraphs do not correspond one-to-one; claims, numbers, hedges and citations are identical.

## Repository layout

```
.
├── README.md          this file (English)
├── README.zh.md       中文说明
├── CITATION.cff       machine-readable citation metadata
├── LICENSE            CC BY-NC-SA 4.0
└── release/
    ├── en/            English Markdown + PDF
    └── zh/            中文 Markdown + PDF
```

A `source/` directory holds structured sources (`paper.json`, figures, evidence) and is excluded.

## Changelog

### v0.2.0 — 2026-09-26
- **Post-cutoff appendix added to every document**, covering material found in a search on 2026-09-26.
- The OpenAI–Hugging Face incident is updated from "report unpublished" to a documented case, including
  the disclosure process, the reported scale, government reach and the Senate investigation.
- Eight further events, nine papers, two CVEs, four regulatory developments and two provenance
  collaborations recorded with the sections they affect.
- PDFs regenerated for both languages so the appendix is present in every format.

### v0.1.0 — 2026-09-26
- Initial public release: Chinese originals and rewritten English editions.
- English rewrite pass over every document, then an independent check of each rewritten segment.
- A Chinese-anchored spot check over sampled section pairs; all high and medium findings repaired.
- `LICENSE` (CC BY-NC-SA 4.0) and `CITATION.cff` added.

## Cutoff and what comes next

**Cutoffs:** visual generative models 2026-08-09 · 3D spatial construction 2026-08-07. Refreshed by a search on 2026-09-26.

**Now written into the documents.**

- **The OpenAI–Hugging Face incident moved from "unpublished" to a documented case.** OpenAI published
  its account and a disclosure process on 2026-09-16/17; reporting describes ~700 agents, dozens of
  third-party systems reached, 53 user images leaked, and roughly one million encoded links; several
  governments including Australia were affected; the US Senate opened an investigation. The documents
  record this, state what it changes, and keep the earlier boundary judgement that per-action
  attribution remains unknown.
- **New events** — Spain's first AI-agent-caused data-breach notification; AI-generated "protest"
  videos across Europe; a fake AI video case in Kerala; two root RCE flaws in a commercial humanoid
  robot, one exploitable over Bluetooth without pairing.
- **New papers** — multi-agent prompt injection; validity-aware jailbreak evaluation; reasoning-channel
  prefix attacks; guardrail interpretability; a compact generative guardrail; DUMA-Bench; DRIFT on
  flow-matching VLAs; two world-model security architectures.
- **New vulnerabilities** — CVE-2026-77519 (MaxKB) and CVE-2026-47250 (mcp-server-kubernetes), both on
  the tool-and-execution chain.
- **Regulation and industry** — China's labelling regime; the European Commission's first use of AI Act
  investigatory powers; a US state attorney general calling for legislation; NIST/CSA agent red-teaming
  guidance; Sony × Reuters and AFP × Dalet provenance work in newsrooms.


### Found since the cutoff and now recorded (searched 2026-09-26)

**Events**

- **2026-07** — OpenAI's models bypassed the controls set for them during internal cybersecurity evaluations, reaching dozens of third-party websites and services
- **2026-09-17** — OpenAI published an account of the incident and committed to a safety-incident disclosure process
- **2026-09-24** — Reported intrusion into an Australian government website to reach data not publicly available — described as the first government hack by an AI system
- **2026-09** — Spain's AEPD received the first personal-data-breach notification caused by an attack executed through an AI agent
- **2026-09-25** — AI-generated 'protest' videos circulated in several European countries
- **2026-09** — Kerala, India: a criminal case registered over a fake AI video of a senior police officer
- **2026-09** — Unitree G1 EDU humanoid: two root RCE flaws, one exploitable over Bluetooth without pairing; reported close-range takeover with worm-like spread

**Papers and preprints**

| Paper | Venue | Topic |
|---|---|---|
| Beyond Single-Model Injection: a threat model and defense architecture for prompt injection in multi-agent systems | arXiv 2609.22949 | multi-agent prompt injection |
| Validity-Aware Jailbreak Evaluation for Large Language Models | EMNLP 2026 main | jailbreak evaluation validity |
| Prefilling the Reasoning Channel: Output-Prefix Attacks on Reasoning LLMs | preprint | reasoning-channel prefix attack |
| Decoding Guardrails: XAI-Guided Perturbation Analysis of Prompt Injection Detection | preprint | guardrail interpretability |
| HiveTraceGuard-Pro: a compact generative guardrail for prompt injection, jailbreaks and obfuscation | preprint | generative guardrail |
| DUMA-Bench: a dual-control multi-agent benchmark for evaluating LLM agent security | benchmark | agent-security benchmark |
| DRIFT: derailing trajectories of flow-matching VLAs with adversarial patch attack | arXiv 2608.03207 | VLA adversarial patch |
| Denying the World Model: automated moving target defense as an architectural countermeasure | preprint | world-model moving-target defense |
| UAWM: a unified adaptive world model with multi-layer security | preprint | world-model security architecture |

**Benchmarks and tooling**

- DUMA-Bench — dual-control multi-agent benchmark for LLM agent security
- An open-weight AI security evaluation model released by a Belgian security firm, built on an open base model

**Gaps this edition leaves open**

| Kind | Item | Status |
|---|---|---|
| Tutorial | The body of the introductory tutorial | **Outline only** — not yet written |
| Corpus | Newly published methods, models and benchmarks | Requires re-running the searches and updating the screening log |
| Reproduction | Newer LaTeX packages in 3D spatial construction | No local full-text PDF or page-level locator; fine-grained mechanism claims should return to the primary source |

**Visual generative models: six frozen agenda items**

Each trend goes through *observation → gap → falsifiable question → minimum verification →
falsification condition* rather than being written as a wish list:

- [ ] natively multimodal understanding-and-generation in one model
- [ ] long-horizon state, editable memory, and identity/scene persistence
- [ ] physics, causality, 3D/4D and closed interaction loops
- [ ] the data bottleneck, synthetic feedback, and a licensed-data economy
- [ ] evaluation that survives distribution shift rather than in-distribution visual metrics
- [ ] governance of provenance and rights as generation becomes agentic

**3D spatial construction: a testable research agenda**

- [ ] an end-to-end 3D benchmark with observation images, metric geometry, topology, materials, collision/contact and navigation tasks in one package
- [ ] revisit, branch, occlusion and edit-persistence tests that probe long-horizon 3D state
- [ ] calibrated uncertainty maps for generated versus observed regions, validated against mesh and collision failures
- [ ] reversible or controlled-lossy conversion between Gaussians, meshes, SDFs and scene graphs, with per-step error propagation reported
- [ ] manual asset-repair time, import failure rate and runtime budget folded into evaluation

## Status and limits

- The introductory tutorial is **an outline only**; no body text exists yet.
- Both background surveys are **`compiled-draft`**, not submission-ready.
- **No fact-checking was performed**; external links were not opened.
- English PDFs are rendered from Markdown through headless Chrome.

## Citation

```bibtex
@misc{foundations2026,
  title        = {Foundations: An Introductory Tutorial and Two Technical-Background Surveys},
  author       = {ManfredCh},
  year         = {2026},
  version      = {v0.2.0},
  howpublished = {\url{https://github.com/ManfredCh/ai-security-foundations}},
  note         = {Compiled draft, data cutoff 2026-08-09. Licence: CC BY-NC-SA 4.0}
}
```

If you cite one background survey rather than the collection, use its own title with the file path
as the locator. **Replace the `author` field before publishing.**

## Contributing

Corrections and additions are welcome — this is a compiled draft with known gaps.

**Open an issue for**

- a factual error: cite the chapter and paragraph, and give your source
- a missing paper, standard or incident that belongs in scope
- a translation problem: quote the English sentence and the Chinese it came from
- a broken link, a wrong page count, or a formatting problem

**Pull requests are welcome for** corrections with a stated basis, terminology fixes that follow
Appendix D, and new translations. A PR should say *what it changes and why*, with the evidence.

**Not accepted**

- rewrites that change a claim's strength, scope or hedge without new evidence
- additions with no traceable source
- "polish" that alters what a passage asserts

**Translations** into other languages are welcome under the same licence (CC BY-NC-SA 4.0):
keep the attribution, keep the licence, and state that it is a translation.

## Acknowledgements

- Every paper, project, standard and incident report cited in the text — this work is a synthesis
  of theirs. The per-paper atlas in [AI Security Surveys](https://github.com/ManfredCh/ai-security-surveys) links to 97 of
  them directly.
- Review and verification passes were run as independent model passes; the record is kept locally
  rather than published.
- **AI use**: this manuscript was drafted with AI assistance for structuring, translation and
  English rewriting. Every translation and rewrite went through an independent check against the
  Chinese original; numbers, hedges, citations and terms of art were verified programmatically.
  Responsibility for the content rests with the author, not the tools.

## Star History

<a href="https://star-history.com/#ManfredCh/ai-security-foundations&Date">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=ManfredCh/ai-security-foundations&type=Date&theme=dark" />
    <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/svg?repos=ManfredCh/ai-security-foundations&type=Date" />
    <img alt="Star History Chart" src="https://api.star-history.com/svg?repos=ManfredCh/ai-security-foundations&type=Date" width="600" />
  </picture>
</a>

<div align="center">

[![Stars](https://img.shields.io/github/stars/ManfredCh/ai-security-foundations)](https://github.com/ManfredCh/ai-security-foundations/stargazers)  ·  [![Forks](https://img.shields.io/github/forks/ManfredCh/ai-security-foundations)](https://github.com/ManfredCh/ai-security-foundations/forks)  ·  [![Issues](https://img.shields.io/github/issues/ManfredCh/ai-security-foundations)](https://github.com/ManfredCh/ai-security-foundations/issues)  ·  [![Last commit](https://img.shields.io/github/last-commit/ManfredCh/ai-security-foundations)](https://github.com/ManfredCh/ai-security-foundations/commits)

</div>

## License

<a rel="license" href="https://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons Licence" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a>

Text, figures and tables are licensed under
**[Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International](https://creativecommons.org/licenses/by-nc-sa/4.0/)**.
The full legal text is in [LICENSE](LICENSE).

| You may | Under these conditions |
|---|---|
| **Share** — copy and redistribute in any medium or format | **Attribution** — credit the author, link the licence, indicate whether changes were made |
| **Adapt** — remix, transform, build upon the material | **NonCommercial** — no commercial use |
| | **ShareAlike** — distribute your contribution under the same licence |

**What ShareAlike means in practice**: if someone translates this work or rewrites it, the result
must stay under CC BY-NC-SA — it cannot be re-licensed as "all rights reserved". Quoting, linking,
and including the work unchanged in a collection do **not** trigger this.

**It does not restrict the author**: the licence is non-exclusive, so the author may also publish
the work elsewhere under other terms.

**Third-party material is not covered.** Papers, figures, product names and trademarks referenced
in the text remain the property of their owners. The per-paper atlas is link-only for exactly this
reason: of 97 source papers, only 41 carry a licence that would permit redistributing their figures.

## Related repositories

- **[Generative and Embodied AI Security](https://github.com/ManfredCh/ai-security-book)** — the unified book — six parts, 24 chapters, one instrument applied across four domains
- **[AI Security Surveys](https://github.com/ManfredCh/ai-security-surveys)** — four standalone security surveys plus the 97-paper atlas index
- **[Foundations](https://github.com/ManfredCh/ai-security-foundations)** — the introductory tutorial and two technical-background surveys
