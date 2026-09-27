## Chapter 1 Introduction: Why a Unified Image–Video Technical Lineage Is Needed

Input and problem: image, video, system and governance materials change quickly. No existing survey covers their mechanisms and their evidence interfaces at the same time.

Argumentative move: put model names, backbones, representations, conditioning, modalities and products into separate layers. Fix the final visual-state update operator as the sole first-level axis.

A list of model names cannot unify image and video generation. A static image mainly learns a distribution from conditions or priors to a single visual state. Video must also represent history, motion, occlusion, identity, camera, events and actions. Per-frame sharpness does not entail temporal consistency. Continuous generation does not entail a queryable world state. Action inputs do not entail closed-loop planning. This survey therefore separates four levels of proposition: visual prediction, physical consistency, interactive response and planning utility. [@vae; @gan; @ddpm; @video_2204_03458; @video_2402_15391]

The primary time window runs from 2014-01-01 to 2026-08-09. Before 2014 we trace direct precursors only. Generation objectives, representations, control and editing, video time, data and evaluation, system efficiency, safety governance and verified events are included. Purely discriminative tasks, untraceable demonstrations, architectural speculation without primary materials, and best-score tables stitched across heterogeneous protocols are excluded. Closed-source products support conclusions about public interfaces, specifications, terms and availability only.

A core work must satisfy at least two of the preset A–E criteria. At least one of those two must be an objective/representation, a shared interface, or a cross-route bridge. A branch work must connect to the core or to a shared bottleneck. It must also present a different mechanism or failure in control, editing, personalization, long-horizon, efficiency, evaluation or governance. Citation counts, company prominence and self-reported claims to primacy cannot by themselves determine role.

**Scope and auditable gaps of existing surveys; only full texts that have been checked are described.**

Survey | Scope | Search audit | Gaps observed in this survey |
|---|---|---|---|
Image Generation Models: A Technical Hist… [@src_s_arxiv_2603_07455] | Technical history of image generation, including chapters on video and safety | not_reported | No reproducible database queries, inclusion/exclusion, version or evidence ledger found. The survey is image-centric, and video, control, systems and news do not sit under one unified contract |
| Bridging Text and Video Generation: A Sur… [@src_s_arxiv_2510_04999] | Text-to-video models, data, training configurations and evaluation | not_reported | No reproducible search/screening method found. No single cross-image–video axis is formed, and coverage of AR/token, DiT/flow and governance/news is limited |
| | | | |

This survey answers RQ1–RQ12. The questions span technical turning points, algorithmic mechanisms, representation and scaling, control branches, video-specific bottlenecks and image–video unification. They also cover evaluation comparability, efficiency and deployment, trunk/branch, safety governance, news trends and conditional selection. Its contributions are an auditable taxonomy, canonical paper cards and lineage edges, an algorithm atlas, conditional rules and a falsifiable agenda. They are not conclusions of exhaustiveness or unconditional superiority.

Overall judgment: the minimal object of a unified survey is "task–representation–update operator–conditioning–decoding–evaluation–budget–failure". Brands and backbones are not that object.

Evidence boundary: the current target is a generic compiled-draft. No venue has been specified, and full screening, full double coding and end-to-end model reproduction have not been completed.

Transition: Chapter 2 gives the search denominator, the version rules and the correction status behind these judgments.

---

[← Back to contents](index.md)
