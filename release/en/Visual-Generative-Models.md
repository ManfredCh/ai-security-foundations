

<!-- toc:start -->
## Contents

- [Modern Visual Generative Models: Technical Lineage, Core Algorithms, and System Evolution](#modern-visual-generative-models-technical-lineage-core-algorithms-and-system-evolution)
  - [Abstract](#abstract)
  - [Chapter 1 Introduction: Why a Unified Image–Video Technical Lineage Is Needed](#chapter-1-introduction-why-a-unified-imagevideo-technical-lineage-is-needed)
  - [Chapter 2 Evidence Method: Retrieval, Screening, Versions, Coding, and News Verification](#chapter-2-evidence-method-retrieval-screening-versions-coding-and-news-verification)
  - [Chapter 3 Formalization and Shared Interface](#chapter-3-formalization-and-shared-interface)
  - [Chapter 4 Technical Lineage: From Single-Frame Distributions to the Spatiotemporal World](#chapter-4-technical-lineage-from-single-frame-distributions-to-the-spatiotemporal-world)
  - [Chapter 5 Taxonomy Design and Coverage Audit](#chapter-5-taxonomy-design-and-coverage-audit)
  - [Chapter 6 Explicit Probability and Latent Variable Families](#chapter-6-explicit-probability-and-latent-variable-families)
  - [Chapter 7 The adversarial implicit generation family](#chapter-7-the-adversarial-implicit-generation-family)
  - [Chapter 8 The Autoregressive and Token-Generation Family](#chapter-8-the-autoregressive-and-token-generation-family)
  - [Chapter 9 Score and Stochastic Denoising Family](#chapter-9-score-and-stochastic-denoising-family)
  - [Chapter 10 Latent Diffusion, DiT, and Scaled Visual Generation](#chapter-10-latent-diffusion-dit-and-scaled-visual-generation)
  - [Chapter 11 Deterministic Transport: Flow Matching, Rectified Flow, and Few-Step Generation](#chapter-11-deterministic-transport-flow-matching-rectified-flow-and-few-step-generation)
  - [Chapter 12 Hybrid and Unified Generation Mechanisms](#chapter-12-hybrid-and-unified-generation-mechanisms)
  - [Chapter 13 Conditions, Control, Editing, and Personalization Side Branches](#chapter-13-conditions-control-editing-and-personalization-side-branches)
  - [Chapter 14 Video-Specific Issues: Time, Motion, Long-Range State, and Interaction](#chapter-14-video-specific-issues-time-motion-long-range-state-and-interaction)
  - [Chapter 15: Data, Evaluation, and Evidence Comparability](#chapter-15-data-evaluation-and-evidence-comparability)
  - [Chapter 16 Engineering Systems, Efficiency, and Reproduction Audit](#chapter-16-engineering-systems-efficiency-and-reproduction-audit)
  - [Chapter 17 Safety, Copyright, Provenance, and Social Impact](#chapter-17-safety-copyright-provenance-and-social-impact)
  - [Chapter 18 Industry, Open-Source Ecosystem, and News Timeline](#chapter-18-industry-open-source-ecosystem-and-news-timeline)
  - [Chapter 19 Cross-Family Synthesis and Conditional Selection Guide](#chapter-19-cross-family-synthesis-and-conditional-selection-guide)
  - [Chapter 20 Future Trends and a Falsifiable Research Agenda](#chapter-20-future-trends-and-a-falsifiable-research-agenda)
  - [Chapter 21 Limitations and Conclusion](#chapter-21-limitations-and-conclusion)
- [Appendix — Post-cutoff update (2026-08-07 ~ 2026-08-09 → 2026-09-26)](#appendix--post-cutoff-update-2026-08-07--2026-08-09-→-2026-09-26)
  - [A.1 — Movement on the visual-generative-models agenda](#a1--movement-on-the-visual-generative-models-agenda)
  - [A.2 — A note on the 3D spatial construction agenda](#a2--a-note-on-the-3d-spatial-construction-agenda)
  - [A.3 — Institutional consequence of the OpenAI–Hugging Face incident (cross-repository note)](#a3--institutional-consequence-of-the-openaihugging-face-incident-cross-repository-note)
  - [A.4 — How to use this appendix](#a4--how-to-use-this-appendix)
<!-- toc:end -->
# Modern Visual Generative Models: Technical Lineage, Core Algorithms, and System Evolution

## Abstract

Surveys of image and video generation usually slice the field by model brand, backbone, modality and task. The result is a set of lists that cannot be compared with one another. This survey takes the final visual-state update operator as its only first-level axis. It builds a retrieval-augmented taxonomic review of the verifiable materials as of 2026-08-09. Frozen broad retrieval and citation tracking together total 1332 items. They yield 132 structured cards and 231 canonical lineage edges after correction. The six family counts are 5/13/18/60/23/13 in order, and the roles include core 74 and branch 32. The survey analyses six mechanism classes isomorphically: latent-variable/invertible-flow, adversarial, AR/masked, score-denoising, deterministic-transport and hybrid. It then extends to control, video time, data and evaluation, system efficiency, safety governance, and 34 events. The second coding is a purposive sample of 24 cards and 32 edges, so it cannot be extrapolated to the population. R001 validates only three mathematical contracts. R002 validates only synthetic-feature FID/KID and protocol fingerprints. The C2PA round trip was not executed. The survey therefore rejects cross-protocol ranking, meta-analysis and any upgrade to model reproduction. It offers conditional selection together with a six-item falsifiable agenda. This draft takes generic compiled-draft as the subsequent gate and currently does not claim to be publication-ready.

\setcounter{tocdepth}{2}
\setcounter{secnumdepth}{3}

## Chapter 1 Introduction: Why a Unified Image–Video Technical Lineage Is Needed

Input and problem: image, video, system and governance materials change quickly. No existing survey covers their mechanisms and their evidence interfaces at the same time.

Argumentative move: put model names, backbones, representations, conditioning, modalities and products into separate layers. Fix the final visual-state update operator as the sole first-level axis.

A list of model names cannot unify image and video generation. A static image mainly learns a distribution from conditions or priors to a single visual state. Video must also represent history, motion, occlusion, identity, camera, events and actions. Per-frame sharpness does not entail temporal consistency. Continuous generation does not entail a queryable world state. Action inputs do not entail closed-loop planning. This survey therefore separates four levels of proposition: visual prediction, physical consistency, interactive response and planning utility. [@vae; @gan; @ddpm; @video_2204_03458; @video_2402_15391]

The primary time window runs from 2014-01-01 to 2026-08-09. Before 2014 we trace direct precursors only. Generation objectives, representations, control and editing, video time, data and evaluation, system efficiency, safety governance and verified events are included. Purely discriminative tasks, untraceable demonstrations, architectural speculation without primary materials, and best-score tables stitched across heterogeneous protocols are excluded. Closed-source products support conclusions about public interfaces, specifications, terms and availability only.

A core work must satisfy at least two of the preset A–E criteria. At least one of those two must be an objective/representation, a shared interface, or a cross-route bridge. A branch work must connect to the core or to a shared bottleneck. It must also present a different mechanism or failure in control, editing, personalization, long-horizon, efficiency, evaluation or governance. Citation counts, company prominence and self-reported claims to primacy cannot by themselves determine role.

**Scope and auditable gaps of existing surveys; only full texts that have been checked are described.}

\begin{tabular}{llll}

Survey & Scope & Search audit & Gaps observed in this survey \\

Image Generation Models: A Technical Hist… [@src_s_arxiv_2603_07455] & Technical history of image generation, including chapters on video and safety & not_reported & No reproducible database queries, inclusion/exclusion, version or evidence ledger found. The survey is image-centric, and video, control, systems and news do not sit under one unified contract \\
Bridging Text and Video Generation: A Sur… [@src_s_arxiv_2510_04999] & Text-to-video models, data, training configurations and evaluation & not_reported & No reproducible search/screening method found. No single cross-image–video axis is formed, and coverage of AR/token, DiT/flow and governance/news is limited \\

\end{tabular}

\end{table**

|
Survey | Scope | Search audit | Gaps observed in this survey |
|---|---|---|---|
|
Image Generation Models: A Technical Hist… [@src_s_arxiv_2603_07455] | Technical history of image generation, including chapters on video and safety | not_reported | No reproducible database queries, inclusion/exclusion, version or evidence ledger found. The survey is image-centric, and video, control, systems and news do not sit under one unified contract |
| Bridging Text and Video Generation: A Sur… [@src_s_arxiv_2510_04999] | Text-to-video models, data, training configurations and evaluation | not_reported | No reproducible search/screening method found. No single cross-image–video axis is formed, and coverage of AR/token, DiT/flow and governance/news is limited |
| | | | |

This survey answers RQ1–RQ12. The questions span technical turning points, algorithmic mechanisms, representation and scaling, control branches, video-specific bottlenecks and image–video unification. They also cover evaluation comparability, efficiency and deployment, trunk/branch, safety governance, news trends and conditional selection. Its contributions are an auditable taxonomy, canonical paper cards and lineage edges, an algorithm atlas, conditional rules and a falsifiable agenda. They are not conclusions of exhaustiveness or unconditional superiority.

Overall judgment: the minimal object of a unified survey is "task–representation–update operator–conditioning–decoding–evaluation–budget–failure". Brands and backbones are not that object.

Evidence boundary: the current target is a generic compiled-draft. No venue has been specified, and full screening, full double coding and end-to-end model reproduction have not been completed.

Transition: Chapter 2 gives the search denominator, the version rules and the correction status behind these judgments.

## Chapter 2 Evidence Method: Retrieval, Screening, Versions, Coding, and News Verification

Input and question: the frozen Q0—Q11 queries and the inclusion/exclusion rules. The core/branch criteria, the source registry, the structured cards and the news verification contract also define the input.

Argumentative move: broad retrieval and subsequent citation tracking stay separate. Versions are merged while identity drift is retained. A second coder spot-checks high-information cards and lineage edges.

The frozen broad retrieval holds 1288 records. Subsequent citation tracking adds 44, for a total of 1332. There are currently 132 structured cards. Of these, 129 are included with full text and 3 are candidates. The two retrieval streams stay separate. The posterior tracking does not masquerade as recall of the frozen queries.

Deduplication proceeds in the order DOI, arXiv ID, canonical title + first author + year. The formal version takes precedence. A preprint serves as version evidence for the same work_id only when it retains a unique appendix. Papers, weights, products and web pages each retain a version and an access date. Abstracts are used only to discover candidates and do not support algorithmic details. An official release supports only the facts officially announced. When independent reporting is missing, we do not write that external effects have been verified.

The second coder ran a stratified purposive spot check. It covered 24 of the 132 cards and 32 high-information edges. The strict agreement rate is 61.6% for card fields and 71.9% for edges. These rates describe only the purposive sample and cannot be extrapolated to the population. Central revisions cover 23 cards and 9 edges. The main text reads only the corrected canonical layer. The canonical edge count now stands at 231.

The post-generation consistency audit also ran a structural scan. It checked the "base—final state update—training/inference contract" across 132/132 structured cards. Of these, 124 have local full text, and 8 cannot be verified against local full text. The audit records 1 critical, 19 high and 4 medium items. The central overlay revises critical/high atomically. Before an item lands, propagation to the main manuscript and figures is not permitted. After landing, the affected content is rebuilt by reading only the corrected layer. This full-card structural scan and targeted full-text review are still not a full independent double coding of the 132 cards by the second coder. [@src_s_audit_post_generation_20260809]

Key corrections include the following. According to the full text, CogVideoX is a v-prediction stochastic diffusion DiT. ControlNet and DreamBooth are changed to core under the frozen role rules. The Latte→CogVideoX edge without direct evidence is removed. The post-generation audit adds further notes. ConsisID inherits the conditional epsilon denoising and DPM reverse diffusion of CogVideoX-5B, and identity frequency decomposition is not a flow update. DDIM and DDPM share a training objective, and eta=0 only makes sampling deterministic. The understanding encoder of Janus-Pro does not participate in the final visual state update, so the main axis is AR visual-token. The VCU/Context Adapter of VACE is a conditioning module, and the main update of the public base is velocity/flow. The real-time path of The Matrix performs independent inference through a four-step SCM student. The Swin-DPM teacher is a training or alternative path. It should not be coded as hybrid automatically merely because a teacher and a student coexist. [@src_s_audit_post_generation_20260809; @video_2408_06072; @controlnet; @dreambooth; @video_2411_17440; @ddim; @januspro; @video_2503_07598; @video_2412_03568]

![F01. Retrieval and screening denominators. Frozen broad retrieval returns 1,288 records and subsequent citation tracking adds 44, for 1,332 merged. These form 132 structured cards. Of these, 129 have entered the full-text evidence stream and 3 remain candidates. Evidence boundary: exclusion codes are not yet coded and 1,200 records remain pending. The second coder is only at the sampling stage.](../figures/F01_search_screening.png)

*F01. Retrieval and screening denominators. Frozen broad retrieval returns 1,288 records and subsequent citation tracking adds 44, for 1,332 merged. These form 132 structured cards. Of these, 129 have entered the full-text evidence stream and 3 remain candidates. Evidence boundary: exclusion codes are not yet coded and 1,200 records remain pending. The second coder is only at the sampling stage.*

The audit index that follows cites the canonical source_id and BibTeX key of the 132 structured cards, item by item. The detailed motivation, mechanism, contribution, failure and evidence sit in the separate paper card directory.

- WIMG0001｜Auto-Encoding Variational Bayes｜2013｜role: core｜family: explicit-latent-flow｜evidence: B｜locator: central-sample-audited (see cross_coder_audit)｜canonical source: S_ARXIV_1312_6114 [@vae]
- WIMG0002｜Generative Adversarial Networks｜2014｜role: core｜family: adversarial｜evidence: A｜locator: central-sample-audited (see cross_coder_audit)｜canonical source: S_ARXIV_1406_2661 [@gan]
- WIMG0003｜Unsupervised Representation Learning with Deep Convolutional Generative Adversarial Networks｜2015｜role: bridge｜family: adversarial｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_1511_06434 [@dcgan]
- WIMG0006｜Density Estimation using Real NVP｜2016｜role: core｜family: explicit-latent-flow｜evidence: B｜locator: central-targeted-fulltext-audited (exact locators in post_generation_audit)｜canonical source: S_ARXIV_1605_08803 [@realnvp]
- WIMG0014｜Pixel Recurrent Neural Networks｜2016｜role: core｜family: autoregressive-mask｜evidence: A｜locator: claim-level-partial｜canonical source: S_ARXIV_1601_06759 [@pixelrnn]
- WIMG0004｜Wasserstein GAN｜2017｜role: core｜family: adversarial｜evidence: A｜locator: claim-level-partial｜canonical source: S_ARXIV_1701_07875 [@wgan]
- WIMG0005｜Improved Training of Wasserstein GANs｜2017｜role: bridge｜family: adversarial｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_1704_00028 [@wgangp]
- WIMG0008｜Progressive Growing of GANs for Improved Quality, Stability, and Variation｜2017｜role: core｜family: adversarial｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_1710_10196 [@pggan]
- WIMG0015｜Neural Discrete Representation Learning｜2017｜role: core｜family: explicit-latent-flow｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_1711_00937 [@vqvae]
- WIMG0007｜Glow: Generative Flow with Invertible 1x1 Convolutions｜2018｜role: bridge｜family: explicit-latent-flow｜evidence: B｜locator: central-targeted-fulltext-audited (exact locators in post_generation_audit)｜canonical source: S_ARXIV_1807_03039 [@glow]
- WIMG0009｜Large Scale GAN Training for High Fidelity Natural Image Synthesis｜2018｜role: core｜family: adversarial｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_1809_11096 [@biggan]
- WIMG0010｜A Style-Based Generator Architecture for Generative Adversarial Networks｜2018｜role: core｜family: adversarial｜evidence: A｜locator: central-targeted-fulltext-audited (exact locators in post_generation_audit)｜canonical source: S_ARXIV_1812_04948 [@stylegan]
- WIMG0013｜Do Deep Generative Models Know What They Don't Know?｜2018｜role: counterexample｜family: explicit-latent-flow｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_1810_09136 [@oodlikelihood]
- WIMG0011｜Analyzing and Improving the Image Quality of StyleGAN｜2019｜role: branch｜family: adversarial｜evidence: A｜locator: central-targeted-fulltext-audited (exact locators in post_generation_audit)｜canonical source: S_ARXIV_1912_04958 [@stylegan2]
- WIMG0016｜Generating Diverse High-Fidelity Images with VQ-VAE-2｜2019｜role: bridge｜family: autoregressive-mask｜evidence: A｜locator: claim-level-partial｜canonical source: S_ARXIV_1906_00446 [@vqvae2]
- WIMG0026｜Generative Modeling by Estimating Gradients of the Data Distribution｜2019｜role: core｜family: stochastic-score-diffusion｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_1907_05600 [@ncsn]
- WIMG0017｜Taming Transformers for High-Resolution Image Synthesis｜2020｜role: core｜family: autoregressive-mask｜evidence: B｜locator: central-sample-audited (see cross_coder_audit)｜canonical source: S_ARXIV_2012_09841 [@vqgan]
- WIMG0027｜Score-Based Generative Modeling through Stochastic Differential Equations｜2020｜role: core｜family: stochastic-score-diffusion｜evidence: A｜locator: claim-level-partial｜canonical source: S_ARXIV_2011_13456 [@scoresde]
- WIMG0028｜Denoising Diffusion Probabilistic Models｜2020｜role: core｜family: stochastic-score-diffusion｜evidence: A｜locator: central-sample-audited (see cross_coder_audit)｜canonical source: S_ARXIV_2006_11239 [@ddpm]
- WIMG0029｜Denoising Diffusion Implicit Models｜2020｜role: core｜family: stochastic-score-diffusion｜evidence: A｜locator: central-targeted-fulltext-audited (exact locators in post_generation_audit)｜canonical source: S_ARXIV_2010_02502 [@ddim]
- WIMG0012｜Alias-Free Generative Adversarial Networks｜2021｜role: branch｜family: adversarial｜evidence: B｜locator: central-targeted-fulltext-audited (exact locators in post_generation_audit)｜canonical source: S_ARXIV_2106_12423 [@stylegan3]
- WIMG0018｜Zero-Shot Text-to-Image Generation｜2021｜role: core｜family: autoregressive-mask｜evidence: A｜locator: claim-level-partial｜canonical source: S_ARXIV_2102_12092 [@dalle]
- WIMG0030｜Diffusion Models Beat GANs on Image Synthesis｜2021｜role: core｜family: stochastic-score-diffusion｜evidence: A｜locator: claim-level-partial｜canonical source: S_ARXIV_2105_05233 [@adm]
- WIMG0031｜High-Resolution Image Synthesis with Latent Diffusion Models｜2021｜role: core｜family: stochastic-score-diffusion｜evidence: A｜locator: central-sample-audited (see cross_coder_audit)｜canonical source: S_ARXIV_2112_10752 [@ldm]
- WIMG0055｜SDEdit: Guided Image Synthesis and Editing with Stochastic Differential Equations｜2021｜role: branch｜family: stochastic-score-diffusion｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_2108_01073 [@sdedit]
- WIMG0019｜Hierarchical Text-Conditional Image Generation with CLIP Latents｜2022｜role: bridge｜family: stochastic-score-diffusion｜evidence: B｜locator: central-targeted-fulltext-audited (exact locators in post_generation_audit)｜canonical source: S_ARXIV_2204_06125 [@dalle2]
- WIMG0020｜MaskGIT: Masked Generative Image Transformer｜2022｜role: core｜family: autoregressive-mask｜evidence: B｜locator: central-sample-audited (see cross_coder_audit)｜canonical source: S_ARXIV_2202_04200 [@maskgit]
- WIMG0032｜Classifier-Free Diffusion Guidance｜2022｜role: core｜family: stochastic-score-diffusion｜evidence: B｜locator: central-targeted-fulltext-audited (exact locators in post_generation_audit)｜canonical source: S_ARXIV_2207_12598 [@cfg]
- WIMG0033｜Photorealistic Text-to-Image Diffusion Models with Deep Language Understanding｜2022｜role: core｜family: stochastic-score-diffusion｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_2205_11487 [@imagen]
- WIMG0034｜Scalable Diffusion Models with Transformers｜2022｜role: core｜family: stochastic-score-diffusion｜evidence: A｜locator: central-targeted-fulltext-audited (exact locators in post_generation_audit)｜canonical source: S_ARXIV_2212_09748 [@dit]
- WIMG0038｜Elucidating the Design Space of Diffusion-Based Generative Models｜2022｜role: core｜family: stochastic-score-diffusion｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_2206_00364 [@edm]
- WIMG0039｜Flow Matching for Generative Modeling｜2022｜role: core｜family: deterministic-transport｜evidence: B｜locator: central-sample-audited (see cross_coder_audit)｜canonical source: S_ARXIV_2210_02747 [@flowmatching]
- WIMG0040｜Flow Straight and Fast: Learning to Generate and Transfer Data with Rectified Flow｜2022｜role: core｜family: deterministic-transport｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_2209_03003 [@rectifiedflow]
- WIMG0056｜Prompt-to-Prompt Image Editing with Cross Attention Control｜2022｜role: branch｜family: stochastic-score-diffusion｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_2208_01626 [@prompt2prompt]
- WIMG0057｜Null-text Inversion for Editing Real Images using Guided Diffusion Models｜2022｜role: branch｜family: stochastic-score-diffusion｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_2211_09794 [@nulltext]
- WIMG0058｜InstructPix2Pix: Learning to Follow Image Editing Instructions｜2022｜role: branch｜family: stochastic-score-diffusion｜evidence: A｜locator: claim-level-partial｜canonical source: S_ARXIV_2211_09800 [@instructpix2pix]
- WIMG0059｜DiffEdit: Diffusion-based Semantic Image Editing with Mask Guidance｜2022｜role: branch｜family: stochastic-score-diffusion｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_2210_11427 [@diffedit]
- WIMG0060｜An Image is Worth One Word: Personalizing Text-to-Image Generation using Textual Inversion｜2022｜role: branch｜family: stochastic-score-diffusion｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_2208_01618 [@textualinversion]
- WIMG0061｜DreamBooth: Fine Tuning Text-to-Image Diffusion Models for Subject-Driven Generation｜2022｜role: core｜family: stochastic-score-diffusion｜evidence: A｜locator: central-sample-audited (see cross_coder_audit)｜canonical source: S_ARXIV_2208_12242 [@dreambooth]
- WIMG0062｜Multi-Concept Customization of Text-to-Image Diffusion｜2022｜role: branch｜family: stochastic-score-diffusion｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_2212_04488 [@customdiffusion]
- WIMG0021｜Muse: Text-To-Image Generation via Masked Generative Transformers｜2023｜role: bridge｜family: autoregressive-mask｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_2301_00704 [@muse]
- WIMG0035｜SDXL: Improving Latent Diffusion Models for High-Resolution Image Synthesis｜2023｜role: bridge｜family: stochastic-score-diffusion｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_2307_01952 [@sdxl]
- WIMG0036｜PixArt-α: Fast Training of Diffusion Transformer for Photorealistic Text-to-Image Synthesis｜2023｜role: bridge｜family: stochastic-score-diffusion｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_2310_00426 [@pixart]
- WIMG0041｜Consistency Models｜2023｜role: core｜family: deterministic-transport｜evidence: A｜locator: central-targeted-fulltext-audited (exact locators in post_generation_audit)｜canonical source: S_ARXIV_2303_01469 [@consistency]
- WIMG0042｜Latent Consistency Models: Synthesizing High-Resolution Images with Few-Step Inference｜2023｜role: bridge｜family: deterministic-transport｜evidence: B｜locator: central-targeted-fulltext-audited (exact locators in post_generation_audit)｜canonical source: S_ARXIV_2310_04378 [@lcm]
- WIMG0043｜InstaFlow: One Step is Enough for High-Quality Diffusion-Based Text-to-Image Generation｜2023｜role: bridge｜family: deterministic-transport｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_2309_06380 [@instaflow]
- WIMG0044｜Adversarial Diffusion Distillation｜2023｜role: bridge｜family: hybrid｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_2311_17042 [@add]
- WIMG0051｜Adding Conditional Control to Text-to-Image Diffusion Models｜2023｜role: core｜family: stochastic-score-diffusion｜evidence: A｜locator: central-sample-audited (see cross_coder_audit)｜canonical source: S_ARXIV_2302_05543 [@controlnet]
- WIMG0052｜T2I-Adapter: Learning Adapters to Dig out More Controllable Ability for Text-to-Image Diffusion Models｜2023｜role: branch｜family: stochastic-score-diffusion｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_2302_08453 [@t2iadapter]
- WIMG0053｜GLIGEN: Open-Set Grounded Text-to-Image Generation｜2023｜role: branch｜family: stochastic-score-diffusion｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_2301_07093 [@gligen]
- WIMG0054｜IP-Adapter: Text Compatible Image Prompt Adapter for Text-to-Image Diffusion Models｜2023｜role: branch｜family: stochastic-score-diffusion｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_2308_06721 [@ipadapter]
- WIMG0064｜PhotoMaker: Customizing Realistic Human Photos via Stacked ID Embedding｜2023｜role: branch｜family: stochastic-score-diffusion｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_2312_04461 [@photomaker]
- WIMG0065｜TextDiffuser: Diffusion Models as Text Painters｜2023｜role: branch｜family: stochastic-score-diffusion｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_2305_10855 [@textdiffuser]
- WIMG0066｜GlyphDraw: Seamlessly Rendering Text with Intricate Spatial Structures in Text-to-Image Generation｜2023｜role: branch｜family: stochastic-score-diffusion｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_2303_17870 [@glyphdraw]
- WIMG0067｜GlyphControl: Glyph Conditional Control for Visual Text Generation｜2023｜role: branch｜family: stochastic-score-diffusion｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_2305_18259 [@glyphcontrol]
- WIMG0068｜AnyText: Multilingual Visual Text Generation And Editing｜2023｜role: branch｜family: stochastic-score-diffusion｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_2311_03054 [@anytext]
- WIMG0069｜Improving Image Generation with Better Captions｜2023｜role: bridge｜family: stochastic-score-diffusion｜evidence: B｜locator: claim-level-partial｜canonical source: S_EXT_59F33A7592EA074B [@dalle3]
- WIMG0022｜Visual Autoregressive Modeling: Scalable Image Generation via Next-Scale Prediction｜2024｜role: core｜family: autoregressive-mask｜evidence: A｜locator: claim-level-partial｜canonical source: S_ARXIV_2404_02905 [@var]
- WIMG0023｜Autoregressive Model Beats Diffusion: Llama for Scalable Image Generation｜2024｜role: bridge｜family: autoregressive-mask｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_2406_06525 [@llamagen]
- WIMG0024｜Autoregressive Image Generation without Vector Quantization｜2024｜role: core｜family: hybrid｜evidence: B｜locator: central-targeted-fulltext-audited (exact locators in post_generation_audit)｜canonical source: S_ARXIV_2406_11838 [@mar]
- WIMG0037｜Scaling Rectified Flow Transformers for High-Resolution Image Synthesis｜2024｜role: core｜family: deterministic-transport｜evidence: B｜locator: central-sample-audited (see cross_coder_audit)｜canonical source: S_ARXIV_2403_03206 [@sd3]
- WIMG0045｜SiT: Exploring Flow and Diffusion-based Generative Models with Scalable Interpolant Transformers｜2024｜role: bridge｜family: deterministic-transport｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_2401_08740 [@sit]
- WIMG0046｜SANA: Efficient High-Resolution Image Synthesis with Linear Diffusion Transformers｜2024｜role: core｜family: deterministic-transport｜evidence: A｜locator: claim-level-partial｜canonical source: S_ARXIV_2410_10629 [@sana]
- WIMG0063｜InstantID: Zero-shot Identity-Preserving Generation in Seconds｜2024｜role: branch｜family: stochastic-score-diffusion｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_2401_07519 [@instantid]
- WIMG0025｜FlexVAR: Flexible Visual Autoregressive Modeling without Residual Prediction｜2025｜role: branch｜family: autoregressive-mask｜evidence: A｜locator: claim-level-partial｜canonical source: S_EXT_525865E4AC1310C7 [@flexvar]
- WIMG0047｜Mean Flows for One-step Generative Modeling｜2025｜role: core｜family: deterministic-transport｜evidence: B｜locator: central-targeted-fulltext-audited (exact locators in post_generation_audit)｜canonical source: S_ARXIV_2505_13447 [@meanflow]
- WIMG0048｜Align Your Flow: Scaling Continuous-Time Flow Map Distillation｜2025｜role: bridge｜family: deterministic-transport｜evidence: A｜locator: claim-level-partial｜canonical source: S_EXT_940E9BE95BC34920 [@alignflow]
- WIMG0049｜Transition Matching: Scalable and Flexible Generative Modeling｜2025｜role: bridge｜family: hybrid｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_2506_23589 [@transitionmatching]
- WIMG0050｜Unified Continuous Generative Models for Denoising-based Diffusion｜2025｜role: bridge｜family: deterministic-transport｜evidence: B｜locator: claim-level-partial｜canonical source: S_EXT_17EB3BFB20AB68C8 [@ucgm]
- WIMG0070｜TextInVision: Text and Prompt Complexity Driven Visual Text Generation Benchmark｜2025｜role: counterexample｜family: hybrid｜evidence: A｜locator: claim-level-partial｜canonical source: S_EXT_71B5C2CC3E1B0BC5 [@textinvision]
- WIMG0071｜Janus-Pro: Unified Multimodal Understanding and Generation with Data and Model Scaling｜2025｜role: bridge｜family: autoregressive-mask｜evidence: B｜locator: central-targeted-fulltext-audited (exact locators in post_generation_audit)｜canonical source: S_ARXIV_2501_17811 [@januspro]
- WIMG0072｜FlowInOne: Unifying Multimodal Generation as Image-in, Image-out Flow Matching｜2026｜role: bridge｜family: deterministic-transport｜evidence: B｜locator: claim-level-partial｜canonical source: S_ARXIV_2604_06757 [@flowinone]
- VID-W001｜Generating Videos with Scene Dynamics｜2016｜role: core｜family: adversarial｜evidence: B｜locator: central-sample-audited (see cross_coder_audit)｜canonical source: S_ARXIV_1609_02612 [@video_1609_02612]
- VID-W002｜Temporal Generative Adversarial Nets with Singular Value Clipping｜2016｜role: core｜family: adversarial｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_1611_06624 [@video_1611_06624]
- VID-W003｜MoCoGAN: Decomposing Motion and Content for Video Generation｜2017｜role: core｜family: adversarial｜evidence: B｜locator: central-sample-audited (see cross_coder_audit)｜canonical source: S_ARXIV_1707_04993 [@video_1707_04993]
- VID-W004｜Adversarial Video Generation on Complex Datasets｜2019｜role: core｜family: adversarial｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_1907_06571 [@video_1907_06571]
- VID-W005｜VideoGPT: Video Generation using VQ-VAE and Transformers｜2021｜role: core｜family: autoregressive-mask｜evidence: B｜locator: central-sample-audited (see cross_coder_audit)｜canonical source: S_ARXIV_2104_10157 [@video_2104_10157]
- VID-W006｜Long Video Generation with Time-Agnostic VQGAN and Time-Sensitive Transformer｜2022｜role: core｜family: autoregressive-mask｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2204_03638 [@video_2204_03638]
- VID-W007｜CogVideo: Large-scale Pretraining for Text-to-Video Generation via Transformers｜2022｜role: core｜family: autoregressive-mask｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2205_15868 [@video_2205_15868]
- VID-W008｜Phenaki: Variable Length Video Generation From Open Domain Textual Description｜2022｜role: core｜family: autoregressive-mask｜evidence: B｜locator: central-sample-audited (see cross_coder_audit)｜canonical source: S_ARXIV_2210_02399 [@video_2210_02399]
- VID-W010｜Video Diffusion Models｜2022｜role: core｜family: stochastic-score-diffusion｜evidence: B｜locator: central-sample-audited (see cross_coder_audit)｜canonical source: S_ARXIV_2204_03458 [@video_2204_03458]
- VID-W011｜Flexible Diffusion Modeling of Long Videos｜2022｜role: core｜family: stochastic-score-diffusion｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2205_11495 [@video_2205_11495]
- VID-W012｜Make-A-Video: Text-to-Video Generation without Text-Video Data｜2022｜role: core｜family: stochastic-score-diffusion｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2209_14792 [@video_2209_14792]
- VID-W013｜Imagen Video: High Definition Video Generation with Diffusion Models｜2022｜role: core｜family: stochastic-score-diffusion｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2210_02303 [@video_2210_02303]
- VID-W039｜Tune-A-Video: One-Shot Tuning of Image Diffusion Models for Text-to-Video Generation｜2022｜role: branch｜family: stochastic-score-diffusion｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2212_11565 [@video_2212_11565]
- VID-W049｜MM-Diffusion: Learning Multi-Modal Diffusion Models for Joint Audio and Video Generation｜2022｜role: branch｜family: stochastic-score-diffusion｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2212_09478 [@video_2212_09478]
- VID-W009｜VideoPoet: A Large Language Model for Zero-Shot Video Generation｜2023｜role: core｜family: autoregressive-mask｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2312_14125 [@video_2312_14125]
- VID-W014｜Align your Latents: High-Resolution Video Synthesis with Latent Diffusion Models｜2023｜role: core｜family: stochastic-score-diffusion｜evidence: B｜locator: central-sample-audited (see cross_coder_audit)｜canonical source: S_ARXIV_2304_08818 [@video_2304_08818]
- VID-W015｜ModelScope Text-to-Video Technical Report｜2023｜role: bridge｜family: stochastic-score-diffusion｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2308_06571 [@video_2308_06571]
- VID-W016｜Stable Video Diffusion: Scaling Latent Video Diffusion Models to Large Datasets｜2023｜role: core｜family: stochastic-score-diffusion｜evidence: B｜locator: central-targeted-fulltext-audited (exact locators in post_generation_audit)｜canonical source: S_ARXIV_2311_15127 [@video_2311_15127]
- VID-W032｜NUWA-XL: Diffusion over Diffusion for eXtremely Long Video Generation｜2023｜role: branch｜family: stochastic-score-diffusion｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2303_12346 [@video_2303_12346]
- VID-W033｜FreeNoise: Tuning-Free Longer Video Diffusion via Noise Rescheduling｜2023｜role: branch｜family: stochastic-score-diffusion｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2310_15169 [@video_2310_15169]
- VID-W040｜Dreamix: Video Diffusion Models are General Video Editors｜2023｜role: branch｜family: stochastic-score-diffusion｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2302_01329 [@video_2302_01329]
- VID-W041｜TokenFlow: Consistent Diffusion Features for Consistent Video Editing｜2023｜role: branch｜family: stochastic-score-diffusion｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2307_10373 [@video_2307_10373]
- VID-W042｜AnimateDiff: Animate Your Personalized Text-to-Image Diffusion Models without Specific Tuning｜2023｜role: branch｜family: stochastic-score-diffusion｜evidence: B｜locator: central-targeted-fulltext-audited (exact locators in post_generation_audit)｜canonical source: S_ARXIV_2307_04725 [@video_2307_04725]
- VID-W043｜VideoComposer: Compositional Video Synthesis with Motion Controllability｜2023｜role: branch｜family: stochastic-score-diffusion｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2306_02018 [@video_2306_02018]
- VID-W044｜MotionCtrl: A Unified and Flexible Motion Controller for Video Generation｜2023｜role: branch｜family: stochastic-score-diffusion｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2312_03641 [@video_2312_03641]
- VID-W050｜GAIA-1: A Generative World Model for Autonomous Driving｜2023｜role: core｜family: hybrid｜evidence: B｜locator: central-sample-audited (see cross_coder_audit)｜canonical source: S_ARXIV_2309_17080 [@video_2309_17080]
- VID-W051｜Learning Interactive Real-World Simulators｜2023｜role: core｜family: stochastic-score-diffusion｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2310_06114 [@video_2310_06114]
- VID-W017｜Lumiere: A Space-Time Diffusion Model for Video Generation｜2024｜role: core｜family: stochastic-score-diffusion｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2401_12945 [@video_2401_12945]
- VID-W018｜Latte: Latent Diffusion Transformer for Video Generation｜2024｜role: core｜family: stochastic-score-diffusion｜evidence: B｜locator: central-sample-audited (see cross_coder_audit)｜canonical source: S_ARXIV_2401_03048 [@video_2401_03048]
- VID-W019｜CogVideoX: Text-to-Video Diffusion Models with An Expert Transformer｜2024｜role: core｜family: stochastic-score-diffusion｜evidence: B｜locator: central-sample-audited (see cross_coder_audit)｜canonical source: S_ARXIV_2408_06072 [@video_2408_06072]
- VID-W020｜Movie Gen: A Cast of Media Foundation Models｜2024｜role: core｜family: deterministic-transport｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2410_13720 [@video_2410_13720]
- VID-W021｜HunyuanVideo: A Systematic Framework For Large Video Generative Models｜2024｜role: core｜family: deterministic-transport｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2412_03603 [@video_2412_03603]
- VID-W024｜Pyramidal Flow Matching for Efficient Video Generative Modeling｜2024｜role: core｜family: deterministic-transport｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2410_05954 [@video_2410_05954]
- VID-W027｜STIV: Scalable Text and Image Conditioned Video Generation｜2024｜role: core｜family: deterministic-transport｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2412_07730 [@video_2412_07730]
- VID-W030｜From Slow Bidirectional to Fast Autoregressive Video Diffusion Models｜2024｜role: core｜family: hybrid｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2412_07772 [@video_2412_07772]
- VID-W034｜FIFO-Diffusion: Generating Infinite Videos from Text without Training｜2024｜role: core｜family: stochastic-score-diffusion｜evidence: B｜locator: central-sample-audited (see cross_coder_audit)｜canonical source: S_ARXIV_2405_11473 [@video_2405_11473]
- VID-W035｜StreamingT2V: Consistent, Dynamic, and Extendable Long Video Generation from Text｜2024｜role: branch｜family: stochastic-score-diffusion｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2403_14773 [@video_2403_14773]
- VID-W036｜LinGen: Towards High-Resolution Minute-Length Text-to-Video Generation with Linear Computational Complexity｜2024｜role: core｜family: deterministic-transport｜evidence: B｜locator: central-targeted-fulltext-audited (exact locators in post_generation_audit)｜canonical source: S_ARXIV_2412_09856 [@video_2412_09856]
- VID-W045｜CameraCtrl: Enabling Camera Control for Text-to-Video Generation｜2024｜role: branch｜family: stochastic-score-diffusion｜evidence: B｜locator: central-sample-audited (see cross_coder_audit)｜canonical source: S_ARXIV_2404_02101 [@video_2404_02101]
- VID-W046｜ConsisID: Identity-Preserving Text-to-Video Generation by Frequency Decomposition｜2024｜role: branch｜family: stochastic-score-diffusion｜evidence: B｜locator: central-targeted-fulltext-audited (exact sections/equations recorded)｜canonical source: S_ARXIV_2411_17440 [@video_2411_17440]
- VID-W052｜Genie: Generative Interactive Environments｜2024｜role: core｜family: autoregressive-mask｜evidence: B｜locator: central-sample-audited (see cross_coder_audit)｜canonical source: S_ARXIV_2402_15391 [@video_2402_15391]
- VID-W053｜Diffusion Models Are Real-Time Game Engines｜2024｜role: core｜family: stochastic-score-diffusion｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2408_14837 [@video_2408_14837]
- VID-W054｜iVideoGPT: Interactive VideoGPTs are Scalable World Models｜2024｜role: core｜family: autoregressive-mask｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2405_15223 [@video_2405_15223]
- VID-W055｜The Matrix: Infinite-Horizon World Generation with Real-Time Moving Control｜2024｜role: core｜family: deterministic-transport｜evidence: B｜locator: central-targeted-fulltext-audited (exact locators in post_generation_audit)｜canonical source: S_ARXIV_2412_03568 [@video_2412_03568]
- VID-W058｜Video generation models as world simulators｜2024｜role: system｜family: stochastic-score-diffusion｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_EXT_0FE3432110402A0F [@video_system_58]
- VID-W059｜Genie 2: A large-scale foundation world model｜2024｜role: system｜family: stochastic-score-diffusion｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_EXT_9E0C24009D08375C [@video_system_59]
- VID-W022｜Wan: Open and Advanced Large-Scale Video Generative Models｜2025｜role: core｜family: deterministic-transport｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2503_20314 [@video_2503_20314]
- VID-W023｜Goku: Flow Based Video Generative Foundation Models｜2025｜role: core｜family: deterministic-transport｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2502_04896 [@video_2502_04896]
- VID-W025｜LTX-Video: Realtime Video Latent Diffusion｜2025｜role: core｜family: deterministic-transport｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2501_00103 [@video_2501_00103]
- VID-W026｜Step-Video-T2V Technical Report: The Practice, Challenges, and Future of Video Foundation Model｜2025｜role: core｜family: deterministic-transport｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2502_10248 [@video_2502_10248]
- VID-W028｜Generative Pre-trained Autoregressive Diffusion Transformer｜2025｜role: bridge｜family: hybrid｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2505_07344 [@video_2505_07344]
- VID-W029｜InfinityStar: Unified Spacetime AutoRegressive Modeling for Visual Generation｜2025｜role: core｜family: autoregressive-mask｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2511_04675 [@video_2511_04675]
- VID-W031｜Autoregressive Adversarial Post-Training for Real-Time Interactive Video Generation｜2025｜role: core｜family: hybrid｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2506_09350 [@video_2506_09350]
- VID-W037｜SkyReels-V2: Infinite-length Film Generative Model｜2025｜role: core｜family: hybrid｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2504_13074 [@video_2504_13074]
- VID-W038｜Captain Cinema: Towards Short Movie Generation｜2025｜role: core｜family: hybrid｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2507_18634 [@video_2507_18634]
- VID-W047｜VACE: All-in-One Video Creation and Editing｜2025｜role: core｜family: deterministic-transport｜evidence: B｜locator: central-targeted-fulltext-audited (exact locators in post_generation_audit)｜canonical source: S_ARXIV_2503_07598 [@video_2503_07598]
- VID-W056｜VRAG: Learning World Models for Interactive Video Generation｜2025｜role: core｜family: stochastic-score-diffusion｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2505_21996 [@video_2505_21996]
- VID-W057｜PhysCtrl: Generative Physics for Controllable and Physics-Grounded Video Generation｜2025｜role: core｜family: hybrid｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2509_20358 [@video_2509_20358]
- VID-W060｜Genie 3: A new frontier for world models｜2025｜role: system｜family: hybrid｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_EXT_01794224D4B71F7C [@video_system_60]
- VID-W048｜Learning to Generate Highly Dynamic Videos using Synthetic Motion Data｜2026｜role: branch｜family: hybrid｜evidence: B｜locator: section-category-generic-needs-precise-patch｜canonical source: S_ARXIV_2604_01666 [@video_2604_01666]

Overall judgment: the corpus is now strong enough to support retrieval-augmented taxonomic synthesis and canonical coding, provided sample-based correction is applied. It is not strong enough to claim that systematic review screening is closed.

Evidence boundary: candidates still await screening, the second coder came from a non-probability sample, video locators lack granularity, and versions drift rapidly. No kappa and no population-level confidence interval is reported.

Transition: Chapter 3 converts the included work into common mathematics and shared input/output interfaces.

## Chapter 3 Formalization and Shared Interface

Inputs and problem: the task, input, representation, state update, objective, inference, conditioning, budget, output and failure fields that a normative paper card must carry.

Argumentative move: express image and video generation in unified notation, and split likelihood, perceptual, semantic, temporal, physical, interaction and system metrics into distinct measurement objects.

Let x be the observed visual object, c the condition, E the representer, D the decoder, s the generation state and U the state update. A general system first maps pixels, frames or multimodal context to continuous latent variables, discrete tokens or spatiotemporal states. It then learns the transition from the prior, noise or history to the target state, guided by a training objective. Finally it outputs either directly or through the decoder to obtain x_hat. [@vae; @vqvae; @ldm; @video_2304_08818]

$$
s_{t+1}=U_\theta(s_t,c,t), \hat{x}=D(s_T)

$$
[@ddpm; @flowmatching; @vae]

Image output is usually a single frame of H×W×C. Video output x_{1:T} may also depend on history h_t, action a_t, camera k_t, audio y_t or memory m_t. Visual prediction can be upgraded to interaction or planning evidence only when actions, state transitions and downstream control are independently validated. [@video_2204_03458; @video_2309_17080; @video_2402_15391]

Six main classes of update exist: latent sample/invertible density, adversarial implicit mapping, token/pixel/scale conditional update, stochastic score denoising, deterministic velocity transport, and hybrids. A hybrid rests on two or more non-removable operators. Pixels or latents serve as representations. U-Net/DiT are backbones. Cross-attention/adapter are conditioning interfaces, and DDIM/solver/caching are sampling or system fields. None of these alone constitutes a first-level family.

$$
\mathcal{L}_{VAE}=\mathbb{E}_{q_\phi(z|x)}[\log p_\theta(x|z)]-\mathrm{KL}(q_\phi(z|x),p(z))

$$
[@vae]

$$
\min_G\max_D\ \mathbb{E}_{x}[\log D(x)]+\mathbb{E}_{z}[\log(1-D(G(z)))]

$$
[@gan]

$$
p_\theta(x|c)=\prod_{i=1}^{N}p_\theta(x_i|x_{1:i-1},c)

$$
[@pixelrnn]

All three independent small implementations of R001 pass the predefined contract. The VAE path derivative residual is 2.405e-11, the DDPM maximum algebraic residual is 1.110e-16, and the linear Flow/RF maximum numerical residual is 6.589e-11. The three differ in dimension and randomness, so each can be judged only against its own threshold. They cannot be ranked by residual magnitude. [@src_s_exp_r001_formula_metrics]

![F04. Unified generation interface. Different families may share a codec, a Transformer backbone or a conditioning module, yet the final generative update, the success criterion, the budget and the failure must be reported separately. Evidence boundary: this figure is a field-level unified interface. It does not mean that all systems disclose every field, and missing reports should be encoded as NR/UV.](../figures/F04_unified_interface.png)

*F04. Unified generation interface. Different families may share a codec, a Transformer backbone or a conditioning module, yet the final generative update, the success criterion, the budget and the failure must be reported separately. Evidence boundary: this figure is a field-level unified interface. It does not mean that all systems disclose every field, and missing reports should be encoded as NR/UV.*

Overall judgment: what is truly comparable is the unified interface and its failure propagation, not architecture labels. Representation, update, conditioning, decoding and evaluation must be attributed separately.

Evidence boundary: passing the formula contract proves only the measured algebraic or numerical implementation. It does not prove network training, generation quality, efficiency or reproduction of the paper's metrics.

Transition: Chapter 4 follows the time axis and examines how these interfaces are established, transferred and reorganized.

## Chapter 4 Technical Lineage: From Single-Frame Distributions to the Spatiotemporal World

Inputs and problem: the 132 cards that remain after error correction, the 231 normative lineage edges, and full-text localization of the image and video branches.

Argumentative move: periodize by technical interface rather than by year-based rankings, and distinguish direct inheritance, parallel development, combination and replacement.

Before 2014 and 2014–2017: generative contracts coexist. VAE turns an intractable posterior into the ELBO and reparameterization. GAN learns an implicit mapping through a discriminative game. PixelRNN/PixelCNN retains normalized pixel likelihoods, and RealNVP/Glow develops invertible density. VQ-VAE turns vision into discrete codes. Video GAN in turn begins to split static appearance from motion. The stage produces no single winner but four reusable contracts: latent-sample, adversarial-map, next-token and invertible-map. [@vae; @gan; @pixelrnn; @realnvp; @vqvae; @video_1609_02612; @video_1707_04993]

2018–2020: representation, scale, and score denoising. Progressive GAN turns a resolution curriculum into a component. BigGAN does the same for scale-conditional training, and StyleGAN for an editable latent space. A VQ hierarchy and a perceptual tokenizer shorten generation sequences. NCSN and DDPM rebuild multi-noise-level score/denoising regression into a stochastic generation backbone. On the video side, spatial/temporal dual discrimination still separates frame quality from dynamics. [@pggan; @biggan; @stylegan; @vqgan; @scoresde; @ddpm; @video_1907_06571]

2021–2022: text–vision, latent space, and video diffusion. DALL·E jointly models text and image tokens. Imagen strengthens text encoding and cascading. LDM moves denoising into a perceptual latent and wires in conditioning through cross-attention. DDIM, Score-SDE, EDM and CFG separate training parameterization, sampling path and conditioning strength. VideoGPT, Phenaki, Video Diffusion and Video LDM in turn bring discrete sequences, temporal layers and spatiotemporal codec into video. [@dalle; @ldm; @ddim; @cfg; @video_2104_10157; @video_2210_02399; @video_2204_03458; @video_2304_08818]

2023–2024: DiT, flow, few-step, and the control ecosystem. DiT hands latent patches to a Transformer. Flow Matching and Rectified Flow regress a vector field or a path, while Consistency, LCM and adversarial distillation compress sampling. ControlNet, IP-Adapter, DreamBooth and others share control-interface extensions. On the video side, DiT, 3D causal VAE, long windows and action conditioning diverge rapidly. A second encoding confirms CogVideoX as a v-pred diffusion DiT. It must not be changed to flow merely because it uses ODE-related terminology or a DiT backbone. [@dit; @flowmatching; @rectifiedflow; @consistency; @controlnet; @dreambooth; @video_2401_03048; @video_2408_06072]

2025–2026: few-step, long-horizon, native multimodality, and open—closed divergence. New work attempts one-step average velocity, flow maps over arbitrary intervals, unification of continuous and discrete transitions, causal few-step video, retrieval memory, interactive worlds and joint audio–video. Version drift, insufficient independent successors and closed-source evidence still limit these efforts. This survey therefore records only verifiable mechanisms and falsification conditions. It does not declare that the routes have already been unified, nor that a general world model has been reached. [@meanflow; @alignflow; @transitionmatching; @januspro; @video_2505_07344; @video_2412_03568; @video_system_60]

![F02. Visual generation technical lineage. The figure fully includes 132 work nodes and 231 edges retained by second-pass encoding. Colors denote the generation-update family of each work, and circles/diamonds denote image/video, respectively. Evidence boundary: edges denote verifiable mechanism relations, not citation counts, influence, or performance rankings. Medium- and low-confidence edges still require precise citation review.](../figures/F02_lineage_dag.png)

*F02. Visual generation technical lineage. The figure fully includes 132 work nodes and 231 edges retained by second-pass encoding. Colors denote the generation-update family of each work, and circles/diamonds denote image/video, respectively. Evidence boundary: edges denote verifiable mechanism relations, not citation counts, influence, or performance rankings. Medium- and low-confidence edges still require precise citation review.*

Overall judgment: the normative lineage contains 231 edges. Its turning points come from the reorganization of representation, state update, conditioning and temporal interfaces, not from linear replacement by model names.

Evidence boundary: lineage edges are technical relations in the current encoding, not a causal attribution network. The sampling-based error correction covers only 32 edges, and unsampled relations still require localization and re-checking.

Transition: Chapter 5 explains how historical material is stably encoded into six families, and how boundary cases are handled.

## Chapter 5 Taxonomy Design and Coverage Audit

Input and question: the unified interface, corrected role coding, decision rules and boundary cases.

Argumentative move: decide hybrid first, then assign one of the six families by the final update operator. Representation, backbone, conditioning, modality and product are all retained as secondary labels.

The decision order is as follows. If two or more non-removable generative operators jointly produce the final output, code it as hybrid and record the components. Otherwise check, in turn, latent sample/invertible density, adversarial game, sequence/masked update, score denoising, and velocity/flow map. When full-text localization cannot support a coding, label it UV. Do not guess from the model name.

After correction, the 132 cards split across the families as follows. Explicit probability and latent variable/invertible flow: 5 cards. Adversarial implicit generation: 13 cards. Autoregressive and masked generation: 18 cards. Score-based and stochastic denoising: 60 cards. Deterministic transport: 23 cards. Hybrid and unified generative mechanisms: 13 cards. [@video_2408_06072]

The counts are also subject to the constraints of the post-generation audit. Structural scan is complete for 132/132 cards, 124 have local full text, and 8 cannot be locally verified. Findings are critical 1, high 19 and medium 4. Critical/high findings are revised atomically via the central overlay and propagate only after corrections and figures are synchronized. The numbers in the table are therefore dynamic counts of the current correction layer, not a frozen old snapshot. This audit still falls short of full independent double coding. [@src_s_audit_post_generation_20260809]

Roles and families are orthogonal. The post-correction role distribution is: branch 32 cards; bridge 21 cards; core 74 cards; counterexample 2 cards; system 3 cards. Under the B/D shared-interface rule, ControlNet and DreamBooth are upgraded to core. [@controlnet; @dreambooth]

**Post-correction single-axis family coding and representative works; the counts describe only the 132 structured cards.

\begin{tabular}{lcl}

Mechanism family & Cards & Representative work_id \\

Explicit probability and latent variable/invertible flow [@vae; @glow] & 5 & WIMG0001, WIMG0007 \\
Adversarial implicit generation [@gan; @stylegan] & 13 & WIMG0002, WIMG0010 \\
Autoregressive and masked generation [@pixelrnn; @maskgit; @video_2104_10157] & 18 & WIMG0014, WIMG0020, VID-W005 \\
Score-based and stochastic denoising [@ddpm; @ldm; @video_2204_03458] & 60 & WIMG0028, WIMG0031, VID-W010 \\
Deterministic transport [@flowmatching; @rectifiedflow] & 23 & WIMG0039, WIMG0040 \\
Hybrid and unified generative mechanisms [@sana; @video_2309_17080] & 13 & WIMG0046, VID-W050 \\

\end{tabular}

\end{table**

|
Mechanism family | Cards | Representative work_id |
|---|---|---|
|
Explicit probability and latent variable/invertible flow [@vae; @glow] | 5 | WIMG0001, WIMG0007 |
| Adversarial implicit generation [@gan; @stylegan] | 13 | WIMG0002, WIMG0010 |
| Autoregressive and masked generation [@pixelrnn; @maskgit; @video_2104_10157] | 18 | WIMG0014, WIMG0020, VID-W005 |
| Score-based and stochastic denoising [@ddpm; @ldm; @video_2204_03458] | 60 | WIMG0028, WIMG0031, VID-W010 |
| Deterministic transport [@flowmatching; @rectifiedflow] | 23 | WIMG0039, WIMG0040 |
| Hybrid and unified generative mechanisms [@sana; @video_2309_17080] | 13 | WIMG0046, VID-W050 |
| | | |

**Post-correction paper role distribution; roles are orthogonal to the primary family.

\begin{tabular}{lc}

Role & Cards \\

branch & 32 \\
bridge & 21 \\
core & 74 \\
counterexample & 2 \\
system & 3 \\

\end{tabular}

\end{table**

|
Role | Cards |
|---|---|
|
branch | 32 |
| bridge | 21 |
| core | 74 |
| counterexample | 2 |
| system | 3 |
| | |

Boundary cases are handled by component responsibility. VQ/VAE codec + AR prior takes its primary class from next-token, and latent codec + DiT + rectified flow is decided by velocity-flow. Diffusion teacher + adversarial student is hybrid if both operators are non-removable. Image pretraining + temporal layers are only a cross-modal label. A closed-source brand that does not disclose its operator can only be recorded as system/UV. [@vqgan; @sd3; @instaflow; @video_2304_08818]

The post-generation review turns this boundary rule into practice. DDIM's deterministic sampling does not change the diffusion training family, and ConsisID's identity conditioning does not replace CogVideoX's reverse diffusion. Janus-Pro is coded by the final AR visual-token update. VACE's conditioning adapter does not establish a separate generative operator. The Matrix is distinguished by its real-time SCM student path and its non-real-time teacher path. [@src_s_audit_post_generation_20260809; @ddim; @video_2411_17440; @januspro; @video_2503_07598; @video_2412_03568]

![F03. Six-family single-axis taxonomy. The taxonomy is based on the construction of the generative distribution and the mechanism of the final visual state update, not on Transformer, VAE codec, conditioning control, or product name. Evidence boundary: the family counts describe only the 132 structured cards. The secondary coding of the stratified sample is not yet complete and is not used as a field prevalence rate.](../figures/F03_operator_taxonomy.png)

*F03. Six-family single-axis taxonomy. The taxonomy is based on the construction of the generative distribution and the mechanism of the final visual state update, not on Transformer, VAE codec, conditioning control, or product name. Evidence boundary: the family counts describe only the 132 structured cards. The secondary coding of the stratified sample is not yet complete and is not used as a field prevalence rate.*

Overall judgment: the single axis can code all 132 structured cards. It treats hybrid mechanisms as a category with a component contract rather than a miscellaneous bucket.

Evidence boundary: 132/132 structural scans do not equal full independent double coding. The agreement rate of the 24-card purposive sample cannot prove that the whole classification is stable, and the 8 cards without local full text still retain a verification ceiling.

Transition: Chapters 6–12 take up the six families and their bridging mechanisms, field by field.

## Chapter 6 Explicit Probability and Latent Variable Families

Input and question: how the explicit latent variable and invertible flow cards inherit from one another, together with ALG01—ALG02 and the AR, diffusion and video codecs that follow.

Argumentative move: compare the three explicit interfaces — ELBO, discrete codebooks and the invertible Jacobian — and trace how compression gains become a downstream information bottleneck.

What endures from VAEs is amortized inference and differentiable stochastic latent variables, not an overall win or loss on perceptual quality. The ELBO constrains reconstruction and the prior at the same time, so encoding, generation and conditioning share a latent. The posterior family, the likelihood choice and the KL weight can also cause posterior collapse or visual blurriness. [@vae]

> **ALG01 VAE and differentiable stochastic latent variable sampling [@vae**]
> 1. >
1. Training input: a sampled x and standard Gaussian noise ε
>
1. State and objective: the state is the continuous latent variable z, and the objective is ELBO = E_q[log pθ(x|z)] - KL(qφ(z|x)||p(z))
>
1. One parameter update: update the encoder φ and the decoder θ jointly along the reparameterization path z=μ+σ⊙ε
>
1. Inference initialization: z p(z)
>
1. Single-step state update: x_hat=Dθ(z)
>
1. Termination and complexity: one decoded sample is output, with a single encode/decode; training covers reconstruction and KL
>
1. Typical failures and limits of the claim: failures include a restricted approximate posterior, posterior collapse, and perceptual blurriness caused by pixel likelihood. The algorithm demonstrates the differentiable stochastic latent variable interface; it does not demonstrate that perceptual quality is superior to other families. Formulas and mechanisms come from first-hand full-text paper cards. Complexity and failures serve only as a qualitative contract and do not constitute a performance ranking.
>

VQ-VAE discretizes continuous states, so a strong prior does not have to handle pixels directly. Hierarchical VQ and VQGAN use hierarchical codes with perceptual and adversarial objectives to improve compressed reconstruction. The cost is that the tokenizer determines the set of reachable images first. Text, small objects, fast motion, and geometry may already be lost before the prior is applied. [@vqvae; @vqvae2; @vqgan]

> **ALG02 VQ-VAE/VQGAN discrete visual tokenizer [@vqvae; @vqgan**]
> 1. >
1. Training input: image x and codebook e_1...e_K
>
1. State and objective: the state is the codebook index k and the quantized latent z_q. The objective is reconstruction + codebook + commitment. VQGAN additionally includes perceptual and patch adversarial terms.
>
1. One parameter update: nearest-neighbor quantization, with straight-through passing of the decoding gradients.
>
1. Inference initialization: produce discrete tokens from the prior, or encode a given image.
>
1. Single-step state update: k*=argmin_j ||z_e-e_j||², then decoded by the decoder.
>
1. Termination and complexity: the image or video is output once the discrete sequence is complete. Encoding happens once. The cost of the token models that follow depends on sequence length.
>
1. Typical failures and limits of the claim: codebook collapse, a reconstruction ceiling, and loss of text/small objects/fast motion. Those failures show that representation compression becomes a shared interface for AR, masked, and latent generation. Formulas and mechanisms come from first-hand full-text paper cards. Complexity and failures serve only as a qualitative contract; they do not constitute a performance ranking.
>

RealNVP and Glow compute exact likelihood and inversion through invertible coupling and change of variables. High-dimensional vision, however, requires constrained architectures and larger GPU memory. Likelihood counterexamples further show that density may reward background or local statistics. Exact likelihood therefore does not automatically equal semantic truth. [@realnvp; @glow]

![F05. Training–inference contracts compared across VAE/invertible flows, GAN/WGAN, pixel or discrete token autoregression, DDPM/score-SDE, and Flow Matching/Rectified Flow/Consistency. Evidence boundary: formulas and mechanisms come from first-hand full-text paper cards. Complexity and failures serve only as a qualitative contract, and they do not support global rankings of quality, speed, or resources.](../figures/F05_train_inference_comparison.png)

*F05. Training–inference contracts compared across VAE/invertible flows, GAN/WGAN, pixel or discrete token autoregression, DDPM/score-SDE, and Flow Matching/Rectified Flow/Consistency. Evidence boundary: formulas and mechanisms come from first-hand full-text paper cards. Complexity and failures serve only as a qualitative contract, and they do not support global rankings of quality, speed, or resources.*

Synthesis: the explicit latent-variable route ultimately becomes shared representation infrastructure for AR, diffusion, and video systems. Choosing it requires checking inference needs, reconstruction ceilings, and structural costs at the same time.

Evidence boundary: this chapter does not compare reconstruction scores or likelihoods across protocols. It also does not infer perceptual or semantic quality from properties of the formulas.

Transition: Chapter 7 turns to the route that abandons explicit density and learns a single-step mapping through a discriminative game.

## Chapter 7 The adversarial implicit generation family

Input and problem: specification cards and failure records for GAN, WGAN, scaled GAN, StyleGAN, image translation, and video GAN.

Argumentation move: compare minimax objectives, distances and constraints, architectural inductive biases, and conditioning. Distinguish the single-step sampling legacy from the coverage/stability boundary.

A GAN provides a distribution-learning signal through a dynamic discriminator. Inference needs only a single generator forward pass. WGAN and WGAN-GP use a Wasserstein objective and a Lipschitz constraint to improve gradient interpretation. Yet they did not eliminate the optimization and cost problems. [@gan; @wgan; @wgangp]

> **ALG03 GAN/WGAN adversarial single-step mapping [@gan; @wgan; @wgangp**]
> 1. >
1. Training input: real samples x and prior noise z
>
1. State and objective: the state is the generated samples G(z) and the critic/discriminator state. The objective is the original min_G max_D adversarial objective. WGAN uses a 1-Lipschitz critic and a gradient penalty.
>
1. One parameter update: update the discriminator/critic and the generator in alternation.
>
1. Inference initialization: z p(z)
>
1. Single-step state update: x_hat=G(z)
>
1. Termination and complexity: inference is a single step, one generator forward pass. The training game and high-resolution discriminators are expensive.
>
1. Typical failures and limits of the conclusion: mode collapse, insufficient coverage, training instability, and difficulty in extending conditional text. The evidence supports the historical contributions of single-step sampling and perceptual sharpness. It does not extrapolate theoretical divergence guarantees to finite networks. Formulas and mechanisms come from first-hand full-text cards. Complexity and failures serve only as a qualitative contract; they do not constitute a performance ranking.
>

Progressive GAN changes the resolution curriculum, BigGAN changes scale and conditional residuals, and StyleGAN changes the layer-wise style interface. StyleGAN2/3 revise normalization artifacts and coordinate sticking. Those revisions show that perceptual sharpness depends on structural inductive bias. The single-step decoder, the adversarial loss, and the editable latent then migrate into hybrid routes such as VQGAN and adversarial distillation. [@pggan; @biggan; @stylegan; @stylegan2; @stylegan3; @vqgan]

Pix2Pix and CycleGAN turn paired or unpaired conditional translation into a side branch. VGAN, TGAN, MoCoGAN, and DVD-GAN separate background, content, motion, and spatial/temporal discrimination. A fixed content code within a window is not an updatable world state. Physically impossible motion and identity drift limit extrapolation. [@oodlikelihood; @video_1609_02612; @video_1611_06624; @video_1707_04993; @video_1907_06571]

Overall judgment: the structural contribution of the adversarial family is single-step implicit sampling, perceptual discrimination, and an editable representation. Its applicability depends on a fixed domain, coverage, stability, and conditional complexity.

Evidence boundary: truncation, data scale, discriminator design, and training budget are highly coupled. One cannot extrapolate from face or ImageNet protocols to general superiority or inferiority on open text.

Transition: Chapter 8 takes up explicit sequence probabilities, discrete tokens, and mask/scale updates.

## Chapter 8 The Autoregressive and Token-Generation Family

Inputs and questions: pixel-level AR, discrete/continuous tokens, masked restoration, next-scale, and video token cards.

Argument move: compare next-token, masked-refine, next-scale, and multi-stage video sequences. The comparison axes are the granularity of each state update and serial depth.

PixelRNN and PixelCNN retain exact normalized sequence probabilities, but pixel ordering brings a long serial path. Once VQ-VAE/VQGAN compress vision into discrete tokens, a Transformer prior can handle global structure. Reconstruction error and prior error also multiply. [@pixelrnn; @vqvae; @vqgan]

> **ALG04 Pixel/discrete-token autoregressive generation [@pixelrnn; @vqvae2; @video_2104_10157**]
> 1. >
1. Training input: an ordered pixel/token sequence and a condition c
>
1. State and objective: the state is the generated prefix x_<i or z_<i. The objective is -Σ_i log pθ(z_i|z_<i,c).
>
1. One parameter update: minimize the token cross-entropy under teacher forcing.
>
1. Inference initialization: a start token / text prefix / optional first frame.
>
1. Single-step state update: sample the next token from pθ(z_i|z_<i,c).
>
1. Termination and complexity: once a fixed sequence length is reached, output passes through the tokenizer decoder. Decoding is strictly serial, O(N). The KV cache grows with context.
>
1. Typical failures and limits of the claim: slow decoding, error accumulation, tokenizer distortion, and long-video context cost. The approach supports exact conditional factorization and a unified sequence interface. It does not equate token accuracy with visual/physical correctness. Formulas and mechanisms come from first-hand full-text cards. Complexity and failures serve only as a qualitative contract; they do not constitute a performance ranking.
>

DALL·E places text and image tokens in a unified sequence. MaskGIT reduces serial depth through parallel prediction and confidence-based remasking. VAR makes coarse-to-fine explicit via next-scale. MAR uses a diffusion head over continuous tokens and is therefore a composite mechanism. The Transformer itself cannot determine the family. [@dalle; @maskgit; @flexvar; @sana]

> **ALG05 Masked parallel restoration and next-scale generation [@maskgit; @video_2210_02399; @flexvar**]
> 1. >
1. Training input: randomly masked tokens or a multi-scale discrete representation
>
1. State and objective: the state is a mask set or a completed coarse-scale map. The objective is cross-entropy at masked positions. Next-scale is factorized by coarse-to-fine conditioning.
>
1. One parameter update: predict the masked tokens / next scale and update the Transformer by backpropagation.
>
1. Inference initialization: a full mask or the coarsest-scale tokens.
>
1. Single-step state update: predict in parallel, keep by confidence and reduce the mask; or generate the next scale.
>
1. Termination and complexity: the mask is emptied, or the highest resolution is reached. The number of iterations is fewer than per-token, but each round still processes the entire representation.
>
1. Typical failures and limits of the claim: confidence bias, scale error propagation, codebook reconstruction ceiling. This shows that the state update unit differs from next-token and cannot be lumped together as the same Transformer algorithm. Formulas and mechanisms come from first-hand full-text cards. Complexity and failures serve only as a qualitative contract; they do not constitute a performance ranking.
>

VideoGPT uses a 3D VQ-VAE + AR Transformer. Phenaki uses a causal video tokenizer + MaskGIT. VideoPoet supports multimodal tasks with a unified token prefix. The sequence interface naturally supports streaming conditions. Codec, context, KV cache, and error feedback, however, jointly determine the long-horizon ceiling. [@video_2104_10157; @video_2210_02399; @video_2312_14125]

> **ALG13 Discrete video tokens and long-sequence generation [@video_2104_10157; @video_2204_03638; @video_2210_02399; @video_2312_14125**]
> 1. >
1. Training input: short-video codec tokens, text/audio conditions
>
1. State and objective: the state is a spatiotemporal discrete token prefix, a mask, or hierarchical keyframes. The objective is token NLL or a random-mask restoration objective.
>
1. One parameter update: the video tokenizer and the sequence model are trained in stages or jointly.
>
1. Inference initialization: a conditional prefix, a first frame, or a full mask.
>
1. Single-step state update: next token, masked refine, or keyframe-interpolation update.
>
1. Termination and complexity: decode after reaching the target token/segment length. Complexity is determined by the T×H×W compression rate and the context length.
>
1. Typical failures and limits of the claim: codec loss of motion/contact state, sliding-window distribution shift, and long-range error feedback. Being able to extend the generation loop does not equal persistent semantic state or online interaction. Formulas and mechanisms come from first-hand full-text cards. Complexity and failures serve only as a qualitative contract; they do not constitute a performance ranking.
>

Overall judgment: the core selection variables for the AR family are the tokenizer, update granularity, context maintenance, and serial depth. Parameter count and the Transformer label alone are not.

Evidence boundary: different codecs, sequence orders, and cache implementations make authors' scores and speed not directly rankable. Successful long-sequence examples do not prove that errors will not accumulate.

Transition: Chapter 9 examines the stochastic route that learns multimodal distributions from multi-noise-level scores or denoising objectives.

## Chapter 9 Score and Stochastic Denoising Family

Inputs and problem: the full-text cards for NCSN, DDPM, Score-SDE, DDIM, conditional guidance, and cascaded diffusion, plus ALG06—ALG07.

Argumentative move: explain stochastic denoising through five fields. Those fields are forward perturbation, reverse score, parameterization, sampling path, and conditioning strength.

DDPM fixes the forward noising process and learns the reverse mean or noise. NCSN estimates the score at multiple noise levels. Score-SDE unifies the two with a continuous-time SDE and gives the reverse SDE/probability flow ODE. DDIM shows that the same training objective can be paired with different stochastic or deterministic sampling paths. It also shows that the sampler should not in turn rewrite the training family. [@scoresde; @ddpm; @ddim; @adm]

> **ALG06 DDPM, Score SDE, and Probability Flow ODE [@ddpm; @ddim; @scoresde**]
> 1. >
1. Training input: data x_0, time t, noise ε
>
1. State and objective: the state is the noise level x_t. The objective is E||ε-εθ(x_t,t,c)||² or an equivalent score/velocity objective.
>
1. One parameter update: sample x_t from the known forward perturbation and regress the noise/score/velocity.
>
1. Inference initialization: x_T N(0,I)
>
1. Single-step state update: update x_t→x_s by the reverse SDE, the DDPM posterior, or deterministic DDIM/ODE.
>
1. Termination and complexity: at t→0 output the denoised sample. Sampling requires multiple network evaluations. Cost is affected by the number of steps, the solver, resolution, and conditioning.
>
1. Typical failures and limits of the conclusion: high NFE, finite-step error, guidance artifacts, data memorization, and evaluation mismatch. A shared training perspective does not mean that finite-step sampling trajectories or quality are the same. Formulas and mechanisms come from first-hand full-text cards. Complexity and failures serve only as a qualitative contract; they do not constitute a performance ranking.
>

Classifier guidance uses external classifier gradients to strengthen conditioning. Classifier-free guidance instead mixes conditional and unconditional predictions within the same network. Turning guidance up often costs coverage, diversity, or artifacts. Text encoders, captions, and conditioning conflicts can also become the main variables. [@cfg; @imagen]

> **ALG07 Classifier-Free Guidance [@cfg**]
> 1. >
1. Training input: noisy samples with conditions randomly dropped
>
1. State and objective: the states are the conditional and unconditional predictions ε_c and ε_u. The objective is for the same network to jointly learn the conditional/null-condition denoising objective.
>
1. One parameter update: update with the standard denoising loss under conditional dropout.
>
1. Inference initialization: the current noisy state x_t and condition c
>
1. Single-step state update: after ε_g=(1+w)ε_c-wε_u, hand off to the sampler.
>
1. Termination and complexity: it terminates with the underlying diffusion/flow sampling. A naive implementation performs two network predictions per step; it can be batched or distilled.
>
1. Typical failures and limits of the conclusion: high w compresses diversity and causes saturation, overexposure, or structural artifacts. Guidance is a conditioning interface, not an independent generative family. Stronger guidance does not mean better overall. Formulas and mechanisms come from first-hand full-text cards. Complexity and failures serve only as a qualitative contract; they do not constitute a performance ranking.
>

Pixel diffusion and cascades can separate base generation, spatial super-resolution, and temporal super-resolution. Errors, however, propagate along the cascade. A metric that measures only a single perceptual distribution or text similarity cannot cover composition, text, compositionality, diversity, and memorization risk at once. [@imagen; @video_2209_14792; @video_2210_02303]

Overall judgment: the advantage of stochastic denoising is stable training and a conditional generation interface that supports multiple modes. The cost is the joint contract of sampling, guidance, and multi-stage error.

Evidence boundary: deterministic sampling is not the same as deterministic transport training. This chapter does not stitch FID, CLIP, or human ratings from heterogeneous sources into a family ranking.

Transition: Chapter 10 analyzes how perceptual compression and Transformer backbones scale diffusion up.

## Chapter 10 Latent Diffusion, DiT, and Scaled Visual Generation

Input and question: cards for LDM, DiT, scaled image models, video latent diffusion, and spatiotemporal DiT, plus ALG08—ALG09.

Argument move: attribute representation, conditioning interface, and backbone separately. Then audit whether the computational savings shift distortion into the codec, the text encoding, or the serving system.

LDM moves diffusion out of pixels and into a perceptual latent. Text, layout, and other conditions arrive through cross-attention. This reduces the volume of the denoising state. It also turns the VAE reconstruction ceiling into the generation ceiling. Errors in text, small objects, and geometry may already occur before denoising. [@ldm; @video_2304_08818]

> **ALG08 Latent Diffusion and Cross-Attention Conditioning [@ldm; @video_2304_08818**]
> 1. >
1. Training input: z_0 from a frozen/joint autoencoder, condition c, noise ε
>
1. State and objective: the state is the perceptual latent z_t. The objective is latent-domain denoising, and conditions are injected through cross-attention
>
1. One parameter update: update the U-Net/spatiotemporal network on the compressed representation
>
1. Inference initialization: latent Gaussian noise
>
1. Single-step state update: a latent sampling update, ultimately decoded by D(z_0)
>
1. Termination and complexity: complete latent denoising and one decoding pass. Spatial cost falls relative to the pixel domain, but codec and decoding costs are added
>
1. Typical failures and limits of the claim: VAE distortion, loss of text/small objects, temporal decoding flicker. LDM changes the representation and the computational domain, and it is not equivalent to a change of DiT backbone. Formulas/mechanisms come from first-hand full-text cards. Complexity and failures serve only as a qualitative contract and do not constitute a performance ranking.
>

DiT hands latent patch tokens to a Transformer and swaps the U-Net inductive bias for modulation and scaling. PixArt, SDXL, SD3, and Sana each change one of these: the training curriculum, the conditioning, the update objective, the compression, or the attention. DiT is the backbone. The rectified-flow objective determines the main SD3 family, whereas CogVideoX remains v-prediction diffusion after full-text review. [@dit; @sdxl; @sd3; @sit; @video_2408_06072]

> **ALG09 DiT and Spatiotemporal DiT [@dit; @video_2401_03048**]
> 1. >
1. Training input: latent patches, time embeddings, and conditioning tokens
>
1. State and objective: the state is a patch/tokenized noisy latent. The objective is a diffusion noise/velocity objective, and the backbone is a Transformer with modulation
>
1. One parameter update: a backward update after global/factorized attention and modulation such as adaLN
>
1. Inference initialization: image or video latent noise
>
1. Single-step state update: the Transformer predicts the ε/v/x_0 that the current sampler requires
>
1. Termination and complexity: determined by the paired diffusion or ODE solver. Attention grows with the number of tokens, and video requires spatial/temporal factorization or windowing
>
1. Typical failures and limits of the claim: high spatiotemporal token cost, absence of long-term state, codec and conditioning bottlenecks. DiT is the backbone, and it does not by itself determine whether the main class is stochastic denoising or deterministic transport. Formulas/mechanisms come from first-hand full-text cards. Complexity and failures serve only as a qualitative contract and do not constitute a performance ranking.
>

In video, the state volume and the temporal receptive field follow from the combination of 2D spatial blocks plus temporal layers, 3D U-Nets, spatiotemporal Transformers, and 3D causal VAEs. Image pretraining can transfer semantics, but video data curation, temporal decoders, and action/camera supervision still require independent evidence. [@video_2204_03458; @video_2304_08818; @video_2401_03048; @video_2408_06072]

Overall judgment: latent diffusion changes the representation, and DiT changes the backbone. Scaling gains must be reported together with codec distortion, conditioning quality, VRAM, number of steps, and throughput.

Evidence boundary: the parameters, resolutions, and best scores the authors report come from different data and system contracts. They cannot form an overall leaderboard of scaled models.

Transition: Chapter 11 discusses deterministic transport and few-step routes that learn the velocity, the path, or a consistency mapping directly.

## Chapter 11 Deterministic Transport: Flow Matching, Rectified Flow, and Few-Step Generation

Inputs and questions: Flow Matching, Rectified Flow, Consistency, LCM, InstaFlow, adversarial distillation, and few-step system cards.

Argumentative move: keep vector field regression, path/coupling, endpoint consistency, and teacher distillation separate, and decouple NFE from end-to-end latency.

Flow Matching regresses the marginal vector field through conditional probability paths, so training does not need to backpropagate inside the ODE. Rectified Flow regresses straight-line path velocities on a given coupling, and reflow can improve that coupling. Judged by their 2022 versions, the two count as parallel, contemporaneous developments, and a central correction has removed directional-inheritance statements that the timeline and the main-text citations do not support. [@flowmatching; @rectifiedflow]

> **ALG10 Conditional Flow Matching [@flowmatching**]
> 1. >
1. Training inputs: endpoint samples, time t, and conditional path samples
>
1. State and objective: the state is the continuous path x_t, and the objective is E||vθ(x,t)-u_t(x)||². Conditional paths can give an equal-gradient objective
>
1. One parameter update: regress the conditional velocity field of the path
>
1. Inference initialization: a simple prior endpoint
>
1. Single-step state update: numerically integrate dx/dt=vθ(x,t,c)
>
1. Termination and complexity: the ODE reaches the data endpoint. NFE depends on the curvature of the field, the solver, and the error tolerance
>
1. Typical failures and limits of the claim: poor path/coupling choices, field approximation error, coarse integration bias. It defines the deterministic-transport training interface, and it does not equate path theory with a fixed-step advantage. Formulas/mechanisms come from first-hand full-text cards. Complexity and failures serve only as a qualitative contract and do not constitute a performance ranking.
>

> **ALG11 Rectified Flow and reflow [@rectifiedflow; @sd3**]
> 1. >
1. Training inputs: data–prior coupling endpoints and uniform time t
>
1. State and objective: the state is the linear interpolation x_t=(1-t)x_0+t x_1, and the objective is E||vθ(x_t,t)-(x_1-x_0)||²
>
1. One parameter update: least-squares regression of the conditional expected velocity. Model-coupled reflow can be used
>
1. Inference initialization: a prior sample
>
1. Single-step state update: the ODE solver advances along vθ
>
1. Termination and complexity: the convention of reaching the data endpoint at t=0/1. When trajectories are straighter, fewer steps may suffice, but this requires joint verification by training, solver, and boundary
>
1. Typical failures and limits of the claim: residual curvature in the coupling, few-step discretization error, and condition/mode imbalance. It can explain adoption by flow-based models such as SD3, but a name containing diffusion cannot change the classification of the primary operator. Formulas/mechanisms come from first-hand full-text cards. Complexity and failures serve only as a qualitative contract and do not constitute a performance ranking.
>

Consistency Models learn a consistency mapping that sends different time points on the same trajectory to a common endpoint. LCM moves this idea into latent diffusion, InstaFlow relies on rectification, and adversarial diffusion distillation adds a discriminative objective on top. All of them reduce network evaluations, but their objectives, teachers, paths, and sources of distortion differ, so they cannot be attributed uniformly to "one step." [@consistency; @lcm; @instaflow; @add]

> **ALG12 Consistency, LCM, and few-step distillation [@consistency; @lcm**]
> 1. >
1. Training inputs: adjacent-time states, teacher trajectories, or independent consistency training samples
>
1. State and objective: the state is any point on the same probability-flow trajectory. The objective is that states on the same trajectory map to the same endpoint while satisfying boundary consistency
>
1. One parameter update: teacher distillation or consistency training, which LCM performs in the latent domain
>
1. Inference initialization: noise or an intermediate state
>
1. Single-step state update: the consistency function maps directly to the endpoint, or it applies a small number of iterative corrections
>
1. Termination and complexity: output after 1 to a few NFEs, and NFE is low. Wall-clock time is still dominated by the network, decoding, batching, and hardware
>
1. Typical failures and limits of the claim: teacher bias, coverage/diversity loss, and degradation of few-step detail and stability. Only a reduction in NFE may be claimed; when not measured uniformly, no claim is made that it is faster or better end-to-end. Formulas/mechanisms come from first-hand full-text cards. Complexity and failures serve only as a qualitative contract and do not constitute a performance ranking.
>

The minimum evaluation of the few-step route must fix the base model, codec, text encoding, hardware, batch, precision, and compilation. It must also report wall-clock time, throughput, VRAM, energy, coverage, prompt following, editability, and tail failures. The end-to-end Pareto frontier may not move when NFE drops but text encoding, the VAE, or the queue becomes the main bottleneck.

Overall judgment: deterministic transport contributes by recasting generation as velocity, path, or consistency mappings. Few-step gains must be confirmed by system evidence under the same contract.

Evidence boundary: mathematical relations in continuous time do not guarantee equivalence for finite-step solvers, training couplings, or generation quality. The authors' NFE does not equal the latency measured in this survey.

Transition: Chapter 12 addresses the hybrid and unified mechanisms in which several non-removable operators jointly generate the final output.

## Chapter 12 Hybrid and Unified Generation Mechanisms

Input and question: hybrid family cards, component interfaces, and native multimodal work.

Argumentative move: judge hybrid status with the test "whether removing a component changes the final generation contract". Record shared parameters, representations, objectives, and state separately.

A VQGAN system can consist of an adversarial tokenizer and an AR prior. Even so, if the next token independently decides the final token generation, its primary class is still AR. MAR uses an AR conditional decomposition plus a continuous diffusion head, ADD relies on a diffusion teacher and an adversarial student, and GAIA-1 uses AR world tokens plus a diffusion renderer. Only when these components are jointly non-removable does the system encode hybrid. [@vqgan; @add; @sana; @video_2309_17080]

Discrete diffusion and masked prediction sit at the boundary between sequence and denoising. Coarse-to-fine, multi-stage, and token + continuous latent approaches may each take on planning and rendering. The final update and the irreplaceable training dependencies determine the primary class. The simultaneous presence of a Transformer, a VAE, and cross-attention in a system is not a reason to record all of them as hybrid. [@maskgit; @flexvar; @transitionmatching]

Native multimodal "unification" also needs to be decomposed. Janus-Pro shares an AR backbone but decouples its understanding and generation encodings. FlowInOne attempts to visualize conditions and hand them to a flow. VideoPoet shares a token sequence interface. A shared brand or API is not enough to demonstrate shared parameters, representations, or objectives, let alone that multiple tasks incur no negative transfer. [@januspro; @flowinone; @video_2312_14125]

Image–video unification often shares a codec, a DiT, text conditioning, or image pretraining. Temporal state, actions, long-term memory, and audio-video synchronization remain video-specific constraints. The gains from unification cannot be attributed to sharing itself. That attribution requires comparisons under the same data, the same parameters, the same tokenizer, and the same budget. [@video_2304_08818; @video_2401_03048; @video_2503_20314]

Synthesis judgment: hybrid is not a catch-all bucket for what cannot be classified. It is an explicit contract with components, interfaces, dependencies, and joint failure. Unification must state exactly what is shared.

Evidence boundary: public results usually change the data, the scale, the codec, and the budget at the same time. There is currently no equal-budget experiment supporting "unification is inherently better".

Transition: Chapter 13 connects side branches such as control, editing, personalization, and text rendering back to these backbones.

## Chapter 13 Conditions, Control, Editing, and Personalization Side Branches

Input and question: in the control side-branch table, the structural conditions, editing, personalization, reference images, visual text, and video action conditions.

Argument move: compare along condition source, injection point, parameter update, inference dependency, and failure mode, rather than elevating every adapter into a generation family.

Structural control centers on "not breaking the base generator". ControlNet copies a trainable branch and injects it with zero convolutions, T2I-Adapter uses a smaller condition encoder, GLIGEN introduces grounding tokens, and IP-Adapter adds decoupled image cross-attention. Their failures differ in capacity, parameters, modality competition, and multi-condition conflict. [@controlnet; @t2iadapter; @gligen; @ipadapter]

Editing methods work on the starting point, the trajectory, attention, and supervision. SDEdit selects the noise starting point, Prompt-to-Prompt controls attention maps, Null-text inversion optimizes the unconditional embedding, DiffEdit estimates a mask, and InstructPix2Pix learns instruction triplets. Counting, object swapping, spatial relations, drift in non-edited regions, and multi-turn artifacts all require local and global metrics to be kept separate. [@sdedit; @prompt2prompt; @nulltext; @instructpix2pix; @diffedit]

Personalization started with the input embedding of Textual Inversion. It then reached DreamBooth's full-model fine-tuning, Custom Diffusion's partial weights, and the encoder route of InstantID/PhotoMaker. Subject fidelity, text adherence, and diversity form a triangle. A single identity similarity cannot cover background leakage, multi-subject attribute confusion, and group bias. [@textualinversion; @dreambooth; @customdiffusion; @instantid; @photomaker]

Visual text methods explicitly add layout, glyph, OCR representations, or more detailed captions. That shows data and codecs are algorithmic variables as well. Reliable text needs a joint contract spanning character-level data, spatial constraints, VAE reconstruction, OCR, and human evaluation. Misspellings, missing words, and overlaps cannot be masked by overall CLIP similarity. [@textdiffuser; @glyphdraw; @glyphcontrol; @anytext; @dalle3]

**Structured distribution of side-branch clusters such as control, editing, and personalization.}

\begin{tabular}{lcl}

Side-branch cluster & Record count & Examples \\

temporal & 19 & Generating Videos with Scene Dynamics, Temporal Generative Adversarial Nets with Singular Value Clipping, MoCoGAN: Decomposing Motion and Content for Video Generation \\
control & 11 & Adding Conditional Control to Text-to-Image Diffusion Models, T2I-Adapter: Learning Adapters to Dig out More Controllable Ability for Text-to-Image Diffusion Models, GLIGEN: Open-Set Grounded Text-to-Image Generation \\
editing & 11 & A Style-Based Generator Architecture for Generative Adversarial Networks, Analyzing and Improving the Image Quality of StyleGAN, Alias-Free Generative Adversarial Networks \\
personalization & 7 & An Image is Worth One Word: Personalizing Text-to-Image Generation using Textual Inversion, DreamBooth: Fine Tuning Text-to-Image Diffusion Models for Subject-Driven Generation, Multi-Concept Customization of Text-to-Image Diffusion \\
audio-video & 3 & MM-Diffusion: Learning Multi-Modal Diffusion Models for Joint Audio and Video Generation, VideoPoet: A Large Language Model for Zero-Shot Video Generation, Movie Gen: A Cast of Media Foundation Models \\
text-rendering & 1 & AnyText: Multilingual Visual Text Generation And Editing \\

\end{tabular}

\end{table**

|
Side-branch cluster | Record count | Examples |
|---|---|---|
|
temporal | 19 | Generating Videos with Scene Dynamics, Temporal Generative Adversarial Nets with Singular Value Clipping, MoCoGAN: Decomposing Motion and Content for Video Generation |
| control | 11 | Adding Conditional Control to Text-to-Image Diffusion Models, T2I-Adapter: Learning Adapters to Dig out More Controllable Ability for Text-to-Image Diffusion Models, GLIGEN: Open-Set Grounded Text-to-Image Generation |
| editing | 11 | A Style-Based Generator Architecture for Generative Adversarial Networks, Analyzing and Improving the Image Quality of StyleGAN, Alias-Free Generative Adversarial Networks |
| personalization | 7 | An Image is Worth One Word: Personalizing Text-to-Image Generation using Textual Inversion, DreamBooth: Fine Tuning Text-to-Image Diffusion Models for Subject-Driven Generation, Multi-Concept Customization of Text-to-Image Diffusion |
| audio-video | 3 | MM-Diffusion: Learning Multi-Modal Diffusion Models for Joint Audio and Video Generation, VideoPoet: A Large Language Model for Zero-Shot Video Generation, Movie Gen: A Cast of Media Foundation Models |
| text-rendering | 1 | AnyText: Multilingual Visual Text Generation And Editing |
| | | |

![F07. Control side branches are not a new generation family. They should be compared by condition carrier, injection location, scope of parameter updates, and failure mode. Evidence boundary: the injection path is an inductive label used to navigate the full text, and it does not replace the precise layer, weight, or code audit of each paper.](../figures/F07_control_module_map.png)

*F07. Control side branches do not constitute a new generation family. They should be compared by condition carrier, injection location, scope of parameter updates, and failure mode. Evidence boundary: the injection path is an inductive label used to navigate the full text. It does not replace a precise audit of each paper's layer, weight, or code.*

Overall judgment: a side branch earns its value by repairing shared bottlenecks. Whether it is upgraded to a core depends on interface reuse and independent successors. It does not depend on module size or release hype.

Evidence boundary: the current control table aggregates evidence from heterogeneous authors. No unified run has yet exercised multi-condition, editing and personalization on the same base model.

Transition: Chapter 14 carries the same conditioning question into time, motion, long-range state, audio and action.

## Chapter 14 Video-Specific Issues: Time, Motion, Long-Range State, and Interaction

Input and question: 60 video cards, the video time matrix, and ALG14.

Argumentative move: decompose temporal consistency, motion, long-range state, interaction and world capability into four questions — representation capacity, update granularity, history mechanism and supervision.

Temporal consistency is not per-frame image quality. Identity, object permanence, occlusion, camera, event order and action causality all require cross-frame state. Single-frame FID and aesthetic scores may coexist with flicker, motion errors or revisit inconsistency. [@video_1609_02612; @video_2204_03458; @video_2304_08818]

Architectures range from spatial/temporal factorization, 3D convolution and spatiotemporal DiT to causal blocks and hierarchical generation. Image pretraining can supply appearance and text semantics. Unlabeled video supplies motion. The spatiotemporal codec determines whether fast motion and small objects are visible. These factors must be attributed separately. [@video_2204_03458; @video_2209_14792; @video_2304_08818; @video_2401_03048; @video_2408_06072]

> **ALG14 Spatiotemporal Video Diffusion, DiT, and Flow [@video_2204_03458; @video_2401_03048; @video_2408_06072; @video_2412_03603; @video_2503_20314**]
> 1. >
1. Training input: mixed video/image data, condition c, time t, and noise/path endpoints
>
1. State and objective: the state is spatiotemporal noise, held in pixels or in a 3D causal VAE latent. The objective is a video denoising ε/v target, or a flow/rectified-flow velocity target.
>
1. One parameter update: the state update is regressed by 2D+time layers, a 3D U-Net, or a spatiotemporal DiT.
>
1. Inference initialization: whole-segment or sliding-window spatiotemporal noise
>
1. Single-step state update: a reverse-diffusion update or ODE velocity integration. It may carry first-frame/trajectory/action conditioning.
>
1. Termination and complexity: the run ends when a fixed window completes, or by chunked looping. Complexity varies with spatiotemporal token volume, attention scheme, number of steps and codec compression.
>
1. Typical failures and limits of the conclusion: identity/scene drift, flicker, missing long-term memory, and physics and action causality unproven. Video generation must separate four levels of evidence — visual prediction, physical consistency, online interaction and planning. Formulas and mechanisms come from first-hand full-text cards. Complexity and failures serve only as a qualitative contract and do not constitute a performance ranking.
>

Long-video methods use segmentation, sliding windows, fixed queues, first-frame anchors, KV cache, keyframes/storyboards or retrieval memory. Generatable duration is merely an operating property. Reports should instead give time to first failure, revisit consistency, event order, identity and state queries, and curves of memory and latency versus duration. [@video_2405_11473; @video_2403_14773; @video_2412_09856; @video_2412_03568; @video_2505_21996]

Action-conditioned and interactive systems must also distinguish visual prediction, physical conservation, counterfactual causality and downstream planning. GAIA-1, Genie and Genie 3 expose different state/action interfaces. Only task success, loop closure, object permanence and out-of-distribution action evidence can support actionable world claims. [@video_2309_17080; @video_2402_15391; @video_system_60]

**Distribution of video structured cards by primary family, with examples. Video capabilities still require time and action fields.}

\begin{tabular}{lcl}

Primary family & Card count & Example \\

Score-based and stochastic denoising & 28 & Video Diffusion Models, Flexible Diffusion Modeling of Long Videos, Make-A-Video: Text-to-Video Generation without Text-Video Data \\
Deterministic transport & 11 & Movie Gen: A Cast of Media Foundation Models, HunyuanVideo: A Systematic Framework For Large Video Generative Models, Pyramidal Flow Matching for Efficient Video Generative Modeling \\
Hybrid and unified generation mechanisms & 9 & GAIA-1: A Generative World Model for Autonomous Driving, From Slow Bidirectional to Fast Autoregressive Video Diffusion Models, Generative Pre-trained Autoregressive Diffusion Transformer \\
Autoregressive and masked generation & 8 & VideoGPT: Video Generation using VQ-VAE and Transformers, Long Video Generation with Time-Agnostic VQGAN and Time-Sensitive Transformer, CogVideo: Large-scale Pretraining for Text-to-Video Generation via Transformers \\
Adversarial implicit generation & 4 & Generating Videos with Scene Dynamics, Temporal Generative Adversarial Nets with Singular Value Clipping, MoCoGAN: Decomposing Motion and Content for Video Generation \\

\end{tabular}

\end{table**

|
Primary family | Card count | Example |
|---|---|---|
|
Score-based and stochastic denoising | 28 | Video Diffusion Models, Flexible Diffusion Modeling of Long Videos, Make-A-Video: Text-to-Video Generation without Text-Video Data |
| Deterministic transport | 11 | Movie Gen: A Cast of Media Foundation Models, HunyuanVideo: A Systematic Framework For Large Video Generative Models, Pyramidal Flow Matching for Efficient Video Generative Modeling |
| Hybrid and unified generation mechanisms | 9 | GAIA-1: A Generative World Model for Autonomous Driving, From Slow Bidirectional to Fast Autoregressive Video Diffusion Models, Generative Pre-trained Autoregressive Diffusion Transformer |
| Autoregressive and masked generation | 8 | VideoGPT: Video Generation using VQ-VAE and Transformers, Long Video Generation with Time-Agnostic VQGAN and Time-Sensitive Transformer, CogVideo: Large-scale Pretraining for Text-to-Video Generation via Transformers |
| Adversarial implicit generation | 4 | Generating Videos with Scene Dynamics, Temporal Generative Adversarial Nets with Singular Value Clipping, MoCoGAN: Decomposing Motion and Content for Video Generation |
| | | |

![F06. Migrating image architectures to video requires adding spatiotemporal representations, temporal interaction, frame/camera/action conditioning and out-of-window state maintenance. The final update operator still determines the primary family. Evidence boundary: the aggregate counts are non-mutually-exclusive labels. This figure describes architectural change and does not claim that an image model is necessarily the direct ancestor of every video model.](../figures/F06_image_to_video_migration.png)

*F06. Migrating image architectures to video requires adding spatiotemporal representations, temporal interaction, frame/camera/action conditioning and out-of-window state maintenance. The final update operator still determines the primary family. Evidence boundary: the aggregate counts are non-mutually-exclusive labels. This figure describes architectural change and does not claim that an image model is necessarily the direct ancestor of every video model.*

Synthesis: the four prior questions for video are whether the codec preserves motion, what each update changes, how history is maintained, and whether the conditioning includes action/camera/audio.

Evidence boundary: the video cards are still positioned mainly at the chapter level, and error correction sampled only 12 cards. Public demonstrations cannot prove long-range, physical or planning capabilities.

Transition: Chapter 15 audits image and video data, metrics and protocol comparability.

## Chapter 15: Data, Evaluation, and Evidence Comparability

Inputs and questions: datasets, metrics, evaluation implementations, human evaluation, arenas, and the R002 synthetic-feature protocol fixture.

Argument move: establish a minimal protocol for each measurement object, one that distinguishes author results, synthetic fixtures and evaluations of real generative models.

Training and evaluation data must record independent-media denominators, derived clips, caption provenance, filtering, licensing, URL drift, duplicates/near-duplicates and contamination. Image-pair counts, video clip counts and total duration are not interchangeable. A dataset-card license also does not automatically transfer rights in the underlying media.

**Data records summarized by modality. Scale, licensing and derivation relations must be traced row by row back to the data table.}

\begin{tabular}{lcl}

Modality & Record count & Example \\

video & 12 & WebVid-10M, HD-VILA-100M, InternVid-10M-FLT \\
image & 11 & MS COCO, Conceptual Captions, FFHQ \\

\end{tabular}

\end{table**

|
Modality | Record count | Example |
|---|---|---|
|
video | 12 | WebVid-10M, HD-VILA-100M, InternVid-10M-FLT |
| image | 11 | MS COCO, Conceptual Captions, FFHQ |
| | | |

FID and KID measure feature distributions. CLIP-style scores measure text-image representation similarity. FVD extends the same measurement to video features. Human evaluation and preference models measure judgments under specific questions. Each of them misses composition, text, temporal order, physics, identity, tail failures or rater bias. No single score can replace any of them.

**Metrics summarized by measurement target. Metrics with the same name are comparable only when implementation and protocol agree.}

\begin{tabular}{lcl}

Measurement target & Record count & Example \\

MMD between generated and reference Inception features & 1 & KID \\
attribute binding; object interaction; motion binding; spatial and temporal composition & 1 & T2V-CompBench vector \\
completed outputs per time & 1 & throughput \\
compliance with named physical laws & 1 & PhyGenBench physical score \\
criterion-specific perceived quality & 1 & absolute human rating \\
energy cost after quality and success gating & 1 & energy_per_accepted_output \\
fine-grained human-like quality dimensions & 1 & VideoScore \\
generated/reference feature-distribution distance & 1 & FID \\
generated/reference spatiotemporal feature-distribution distance & 1 & FVD \\
image-text semantic compatibility & 1 & CLIPScore \\
learned expert preference for a prompt-image pair & 1 & ImageReward \\
learned human preference prediction and four-style benchmark performance & 1 & HPS_v2 \\
learned prediction of real-user preference conditional on a prompt & 1 & PickScore \\
maximum accelerator memory used & 1 & peak_memory \\
object presence; co-occurrence; count; color; position; color attribution & 1 & GenEval score \\
open-world compositional prompt adherence & 1 & T2I-CompBench vector \\
physical commonsense and plausibility & 1 & VideoPhy score \\
question-answer faithfulness to prompt facts & 1 & TIFA score \\
relative preference under a named criterion & 1 & pairwise human preference \\
scientific relevance; factual accuracy; explainability & 1 & SCIEval vector \\
video quality and semantic dimensions & 1 & VBench dimension vector \\
visual quality; motion; temporal coherence; text-video alignment & 1 & EvalCrafter metric vector \\
wall-clock request completion & 1 & end_to_end_latency \\

\end{tabular}

\end{table**

|
Measurement target | Record count | Example |
|---|---|---|
|
MMD between generated and reference Inception features | 1 | KID |
| attribute binding; object interaction; motion binding; spatial and temporal composition | 1 | T2V-CompBench vector |
| completed outputs per time | 1 | throughput |
| compliance with named physical laws | 1 | PhyGenBench physical score |
| criterion-specific perceived quality | 1 | absolute human rating |
| energy cost after quality and success gating | 1 | energy_per_accepted_output |
| fine-grained human-like quality dimensions | 1 | VideoScore |
| generated/reference feature-distribution distance | 1 | FID |
| generated/reference spatiotemporal feature-distribution distance | 1 | FVD |
| image-text semantic compatibility | 1 | CLIPScore |
| learned expert preference for a prompt-image pair | 1 | ImageReward |
| learned human preference prediction and four-style benchmark performance | 1 | HPS_v2 |
| learned prediction of real-user preference conditional on a prompt | 1 | PickScore |
| maximum accelerator memory used | 1 | peak_memory |
| object presence; co-occurrence; count; color; position; color attribution | 1 | GenEval score |
| open-world compositional prompt adherence | 1 | T2I-CompBench vector |
| physical commonsense and plausibility | 1 | VideoPhy score |
| question-answer faithfulness to prompt facts | 1 | TIFA score |
| relative preference under a named criterion | 1 | pairwise human preference |
| scientific relevance; factual accuracy; explainability | 1 | SCIEval vector |
| video quality and semantic dimensions | 1 | VBench dimension vector |
| visual quality; motion; temporal coherence; text-video alignment | 1 | EvalCrafter metric vector |
| wall-clock request completion | 1 | end_to_end_latency |
| | | |

**Minimal contract for cross-study comparability. Any drift in a key field should trigger grouping or refusal of comparison.}

\begin{tabular}{lll}

Contract layer & Must be fixed & If not met \\

Task and input & task, conditions, prompt set, source media & describe as heterogeneous sources only \\
Output & resolution, frame count, frame rate, duration, format & do not compare quality or efficiency \\
Sampling & steps, solver, guidance, random seed & do not attribute to the model itself \\
Evaluation & implementation, feature extractor, reference set, sample count, human-evaluation questions & do not merge scores \\
System & hardware, precision, batch, compilation, cold start, load & do not compare latency and throughput \\
Statistics & independent units, replications, variance, missingness and failures & refuse meta-analysis or significance \\

\end{tabular}

\end{table**

|
Contract layer | Must be fixed | If not met |
|---|---|---|
|
Task and input | task, conditions, prompt set, source media | describe as heterogeneous sources only |
| Output | resolution, frame count, frame rate, duration, format | do not compare quality or efficiency |
| Sampling | steps, solver, guidance, random seed | do not attribute to the model itself |
| Evaluation | implementation, feature extractor, reference set, sample count, human-evaluation questions | do not merge scores |
| System | hardware, precision, batch, compilation, cold start, load | do not compare latency and throughput |
| Statistics | independent units, replications, variance, missingness and failures | refuse meta-analysis or significance |
| | | |

In the R002 synthetic-feature fixture, all three contracts pass for FID, KID and the protocol fingerprint. The FID same-set value is 0.0, the KID swap absolute difference is 0.0, and a preprocessing-fingerprint mismatch is blocked before any metric computation. The input is fixed synthetic features only, with no image decoding, no Inception weights, no generator and no human evaluation. It therefore does not support visual quality or model ranking. [@src_s_exp_r002_metric_fixture]

![F08. Correspondence among data, metrics and capabilities. Each metric is assigned to one capability column by its main measurement object. Reference-set dependence and known blind spots are also shown. The grouping does not expand one metric into general validity. Evidence boundary: capability grouping is for navigation only. Metrics with the same name cannot be compared directly when the extractor, preprocessing, sample count, prompt, aggregation and seed differ.](../figures/F08_data_metric_capability_matrix.png)

*F08. Correspondence among data, metrics and capabilities. Each metric is assigned to one capability column by its main measurement object. Reference-set dependence and known blind spots are also shown. The grouping does not expand one metric into general validity. Evidence boundary: capability grouping is for navigation only. Metrics with the same name cannot be compared directly when the extractor, preprocessing, sample count, prompt, aggregation and seed differ.*

Synthesis: a subset can be compared quantitatively only when task, data, output, sampling, evaluator, hardware and statistical unit are aligned at the same time.

Evidence boundary: there is currently no unified generative-model run, no independent replication and no cross-study variance. Both meta-analysis and cross-protocol aggregate leaderboards are refused.

Transition: Chapter 16 places sampling, codec, caching, quantization and serving cost inside the end-to-end reproduction ladder.

### Data Record Audit Index

- ECO-D001 | MS COCO | modality: image | scale: 328000 images | use: text-image training, COCO-FID, and caption alignment | license/terms: dataset terms, plus the licenses of the underlying Flickr images | risk: an object-centric domain, caption style, and sensitivity to duplicates and preprocessing | comparability: do not treat COCO-30K and the COCO-2014/2017 subsets as interchangeable [@src_s_ext_a2e1a9cc9bd2ee15; @src_s_ext_b8fde4a88fe06f4f]
- ECO-D002 | Conceptual Captions | modality: image | scale: 3300000 image_text_pairs | use: large-scale pretraining on image-text pairs | license/terms: dataset-page terms; underlying web-media rights do not transfer | risk: weak captions, URL decay, web bias, and ambiguous rights | comparability: record the retrieval date and the successful-download denominator [@src_s_ext_883b91a248a8f70d]
- ECO-D003 | FFHQ | modality: image | scale: 70000 images | use: face generation and inversion | license/terms: dataset under CC BY-NC-SA 4.0; individual images keep their original licenses; a face-recognition restriction applies | risk: demographic imbalance, biometric/privacy risk, and inherited licenses | comparability: do not compare it with general-scene datasets [@src_s_ext_2e0206c9bfbf134e]
- ECO-D004 | LAION-5B | modality: image | scale: 5850000000 image_text_pairs | use: foundation-model pretraining and data research | license/terms: metadata under CC BY 4.0; underlying media rights stay with their owners | risk: weak text, duplicates, bias, URL decay, and unsafe or unlawful content risk | comparability: metadata scale does not equal the valid samples retrievable today [@src_s_ext_e21d4fdc23640b7f; @src_s_ext_c32738ba82a67d01; @src_s_ext_a71e52f7d74db2c8; @src_s_ext_813aaade7ed9b752; @src_s_ext_a322cc3b396554b6]
- ECO-D005 | Re-LAION-5B | modality: image | scale: 5850000000 image_text_pairs | use: a safer index for large-scale pretraining | license/terms: metadata terms for each release; underlying media rights do not transfer | risk: known-hash filtering cannot prove the absence of all harmful content, plus source drift | comparability: report the exact variant and snapshot [@src_s_ext_a71e52f7d74db2c8]
- ECO-D006 | DataComp-1B candidate pool | modality: image | scale: 12800000000 image_text_pairs | use: a data-curation benchmark for CLIP-like training | license/terms: benchmark and data terms; underlying media rights need separate review | risk: web bias, URL decay, and optimization to a fixed downstream suite | comparability: restrict comparisons to the same DataComp scale and training budget [@src_s_ext_8931ca2bc2a2df46]
- ECO-D007 | WebVid-10M | modality: video | scale: 10000000 video_text_pairs | use: video-language pretraining and text-to-video training | license/terms: repository and data terms; source-video rights and availability remain external | risk: watermarks, weak captions, duplicates, URL decay, and stock-video bias | comparability: report the successful retrieval count and the dedup policy [@src_s_ext_9c5cfc1e3ee09475]
- ECO-D008 | HD-VILA-100M | modality: video | scale: 100000000 video_clip_text_pairs | use: high-resolution video-language pretraining | license/terms: URLs and metadata release; source media rights remain external | risk: ASR errors, spoken-content bias, source drift, and long-video correlation | comparability: 100M clips are not 100M independent source videos [@src_s_ext_27fd08d04b6030b2; @src_s_ext_be5c2e02470ae804]
- ECO-D009 | InternVid-10M-FLT | modality: video | scale: 10000000 video_text_pairs | use: video-text representation and generation | license/terms: dataset-card terms; underlying media rights require review | risk: machine-caption bias, filter-model bias, and availability drift | comparability: lock the subset version and the HF commit [@src_s_ext_3baa8d3a6825c19d; @src_s_ext_f3e09865590969f9]
- ECO-D010 | Panda-70M | modality: video | scale: 70000000 video_text_pairs | use: text-to-video training and caption research | license/terms: project terms, plus inherited source-video conditions | risk: dependent samples, teacher hallucination, and inherited ASR/source bias | comparability: the 70M pairs are derived segments, not independent videos [@src_s_ext_aad92459dff9dad3]
- ECO-D011 | OpenVid-1M | modality: video | scale: 1000000 video_text_pairs | use: high-quality text-to-video training | license/terms: repository and dataset terms; underlying source rights require review | risk: filter-model bias, duplicate segments, and source drift | comparability: name the 433K 1080p subset separately from the full set [@src_s_arxiv_2407_02371; @src_s_ext_1f5cad9fbb4d7ee1]
- ECO-D012 | OpenVidHD-0.4M | modality: video | scale: 433000 video_text_pairs | use: high-resolution video generation | license/terms: the same terms as the OpenVid-1M release | risk: resolution guarantees neither content nor caption quality | comparability: match the duration, fps, and resolution filters before comparing [@src_s_arxiv_2407_02371; @src_s_ext_1f5cad9fbb4d7ee1]
- ECO-D013 | MiraData | modality: video | scale: NA long_videos | use: long-video generation and structured evaluation | license/terms: project terms; underlying content rights require review | risk: long-range caption hallucination, generator-version dependence, and source drift | comparability: audit the scale and release composition for each version [@src_s_ext_9eae22c439bbcff9]
- ECO-D014 | MiraBench | modality: video | scale: 150 prompts | use: long-video evaluation | license/terms: benchmark terms | risk: a small prompt set, and metric aggregation that can hide dimension failures | comparability: it is not a substitute for VBench or human preference [@src_s_ext_9eae22c439bbcff9]
- ECO-D015 | FineVideo | modality: video | scale: 43000 videos | use: >3,400-hour long-form video generation and understanding | license/terms: a CC-BY source selection with attribution obligations, plus card-specific terms | risk: a snapshot can audit only CC-BY filtering accuracy, machine-label bias, and deletions | comparability: retain attribution at the video level, and record the provenance of the model-generated metadata [@src_s_ext_d8a85afa9f84a924; @src_s_ext_734a4df434589b7b]
- ECO-D016 | TIFA v1 | modality: image | scale: 4000 prompts | use: faithfulness diagnostics | license/terms: paper and repository terms | risk: LLM question-generation and VQA answer bias | comparability: state which question-generator and VQA-evaluator versions were used [@src_s_ext_1610191ec47a137a]
- ECO-D017 | T2V-CompBench | modality: video | scale: 1400 prompts | use: compositional text-to-video evaluation | license/terms: paper and repository terms | risk: detector, tracker, or MLLM failure can be scored as generator failure | comparability: use the category vector rather than single-score cross-paper ranking [@src_s_ext_886b46db89300b7c]
- ECO-D018 | PhyGenBench | modality: video | scale: 160 prompts | use: physical commonsense evaluation | license/terms: paper and repository terms | risk: sparse coverage, VLM/LLM judge bias, and prompt leakage | comparability: without shared prompts and judges, it is not directly comparable to VideoPhy [@src_s_ext_ca220ee450d111b2; @src_s_ext_28660977e9853176]
- ECO-D019 | VideoFeedback | modality: video | scale: 37600 videos | use: training and evaluating learned video reward models | license/terms: repository and data terms | risk: rater-population and model-era bias, plus distribution shift | comparability: out-of-distribution validation is required for learned scores [@src_s_ext_0ced8660dbcb0990; @src_s_ext_e279a4f042679383]
- ECO-D020 | SCIEval | modality: image | scale: NA scientific_prompts_and_images | use: scientific image generation evaluation | license/terms: paper and repository terms | risk: domain-expert dependence, and scientific subdomain coverage | comparability: do not compare it with general-aesthetic benchmarks on a single quality axis [@src_s_ext_500194e9b9e5c4e6]
- ECO-D021 | Pick-a-Pic | modality: image | scale: >500000 preference_examples | use: training and testing PickScore, and preference research | license/terms: paper and dataset terms; user consent described by the authors; the repository MIT license covers code only | risk: self-selected users, interface effects, generator-era coverage, and repeated-user dependence | comparability: fix the version and the user split; examples are not independent when users or prompts repeat [@src_s_ext_463d2c341c7ad7d0; @src_s_ext_33d29297a792c446]
- ECO-D022 | ImageRewardDB | modality: image | scale: 137000 expert_pairwise_comparisons | use: training and testing ImageReward, and reward feedback learning | license/terms: paper and dataset terms; the repository Apache-2.0 license covers code only | risk: expert-recruitment and rubric dependence, source-model coverage, and annotator clustering | comparability: do not merge its comparison count with crowdsourced or real-user datasets unless cluster metadata is available [@src_s_ext_96452f21157f1b23; @src_s_ext_0341223e1150a5f6]
- ECO-D023 | HPD_v2 | modality: image | scale: 798090 preference_choices | use: training HPS v2 and four-style text-to-image evaluation | license/terms: paper and dataset terms; the repository Apache-2.0 license covers code only | risk: model-era and style coverage, annotator dependence, and a train/test release that changed over time | comparability: lock the HPD and HPS checkpoint versions; the choice count and the unique image-pair denominator are distinct [@src_s_arxiv_2306_09341; @src_s_ext_b9537ace6ebc0114]

### Metric Record Audit Index

- ECO-M001 | FID | target: distance between generated and reference feature distributions | direction: lower | minimal protocol: same extractor implementation, same resize/preprocess, a named real split, equal and disclosed sample counts, a confidence interval or repeated seeds | blind spots: Gaussian approximation, sample bias, extractor/domain mismatch, insensitivity to prompt-level correctness | comparability: only within the same implementation and sampling protocol [@src_s_ext_f13a72e436c93975]
- ECO-M002 | KID | target: MMD between generated and reference Inception features | direction: lower | minimal protocol: same features, kernel, subset size, number of subsets, sample counts and uncertainty | blind spots: extractor/domain mismatch, subset estimator settings, no prompt-level semantics | comparability: an unbiased estimator does not remove protocol dependence [@src_s_ext_55391d95358da2e0]
- ECO-M003 | CLIPScore | target: semantic compatibility between image and text | direction: higher | minimal protocol: same CLIP model/tokenization, same prompt set, crop/resize, aggregation and seed policy | blind spots: weak on counting, negation, spatial relations, text rendering, physical correctness and aesthetics | comparability: not interchangeable across CLIP checkpoints or prompt distributions [@src_s_ext_d47e49b272b9817e]
- ECO-M004 | FVD | target: distance between generated and reference spatiotemporal feature distributions | direction: lower | minimal protocol: same I3D weights, frame count/fps/resolution, clip sampling, reference split and sample count | blind spots: I3D action-domain bias, sample inefficiency, temporal preprocessing, no prompt-specific diagnosis | comparability: never compare values taken under different frame/fps/I3D/data protocols [@src_s_ext_cdc5919a22f406ea]
- ECO-M005 | TIFA score | target: question-answer faithfulness to prompt facts | direction: higher | minimal protocol: same TIFA question set/generator, VQA checkpoint, answer normalization and prompt categories | blind spots: question omissions/hallucinations, VQA failure, weak aesthetics and global coherence | comparability: report per-category scores and the judge version [@src_s_ext_1610191ec47a137a]
- ECO-M006 | GenEval score | target: object presence, co-occurrence, count, color, position and color attribution | direction: higher | minimal protocol: official prompts, detector checkpoint/threshold, parsing, seed count | blind spots: detector bottleneck, limited attributes and relations, style/domain sensitivity | comparability: only with the same detector and benchmark release [@src_s_ext_2da47e6f77711c51]
- ECO-M007 | T2I-CompBench vector | target: open-world compositional prompt adherence | direction: higher | minimal protocol: same prompt split, evaluator versions, aggregation and seeds | blind spots: evaluator entanglement, open-world long-tail, an aggregate that masks failure modes | comparability: do not collapse across releases without preserving dimensions [@src_s_arxiv_2307_06350]
- ECO-M008 | VBench dimension vector | target: video quality and semantic dimensions | direction: higher | minimal protocol: VBench version, prompt suite, duration/fps/resolution, evaluator checkpoints and seeds | blind spots: judge-model bias, overlapping dimensions, sensitivity to video preprocessing | comparability: use within one VBench protocol; no leaderboard synthesis across papers [@src_s_ext_41ef49de30ab0c93]
- ECO-M009 | EvalCrafter metric vector | target: visual quality, motion, temporal coherence and text-video alignment | direction: mostly higher | minimal protocol: same 700 prompts, evaluator checkpoints, video preprocessing, raw metrics and coefficients | blind spots: a learned aggregate may overfit raters/models; coefficients obscure tradeoffs | comparability: preserve the raw vector; a learned aggregate is not universal utility [@src_s_ext_ada443910805e939]
- ECO-M010 | VideoScore | target: fine-grained, human-like quality dimensions | direction: higher | minimal protocol: checkpoint, input sampling, resolution/fps, prompt set, domain-shift audit | blind spots: inherits annotator and training-model bias; vulnerable to reward hacking | comparability: not valid as the sole judge for post-training models without a held-out human audit [@src_s_ext_0ced8660dbcb0990; @src_s_ext_e279a4f042679383]
- ECO-M011 | T2V-CompBench vector | target: attribute binding, object interaction, motion binding, spatial and temporal composition | direction: higher | minimal protocol: same 1,400 prompts, evaluator versions, frame sampling, seed count | blind spots: composed evaluator failure, limited prompts, long-tail relation ambiguity | comparability: same benchmark release only; no aggregation with VBench [@src_s_ext_886b46db89300b7c]
- ECO-M012 | PhyGenBench physical score | target: compliance with named physical laws | direction: higher | minimal protocol: same 160 prompts, 27 laws, four domains, judge prompt/checkpoint, human audit | blind spots: the judge may hallucinate physical violations; small coverage; ambiguity in stochastic phenomena | comparability: do not rank against VideoPhy without common clips and raters [@src_s_ext_ca220ee450d111b2; @src_s_ext_28660977e9853176]
- ECO-M013 | VideoPhy score | target: physical commonsense and plausibility | direction: higher | minimal protocol: official split, prompt and judge version, video preprocessing, seeds | blind spots: benchmark taxonomy/domain limits; evaluator error | comparability: not directly comparable to PhyGenBench scores [@src_s_ext_1bdc47e96f4aa885]
- ECO-M014 | SCIEval vector | target: scientific relevance, factual accuracy and explainability | direction: higher | minimal protocol: same scientific domains, prompts, reference material, judge version and expert audit | blind spots: domain coverage, judge factuality, scientific ambiguity | comparability: never substitute with FID or aesthetic preference [@src_s_ext_500194e9b9e5c4e6]
- ECO-M015 | pairwise human preference | target: relative preference under a named criterion | direction: higher | minimal protocol: randomized side, exact prompt/output sampling, ties, rater count/demographics, repeated items, quality checks, CI, multiple-comparison plan | blind spots: position bias, rater culture, criterion leakage, non-transitivity, version drift | comparability: win rates are transitive only under an explicit fitted model and adequate overlap [@src_s_ext_d92935b612a4c23a]
- ECO-M016 | absolute human rating | target: criterion-specific perceived quality | direction: higher | minimal protocol: anchored rubric, calibration examples, randomization, repeated items, inter-rater reliability, CI | blind spots: scale-use bias, central tendency, ceiling effects, rater fatigue | comparability: means of ordinal scales require justification; prefer distributions or ordinal models [@src_s_ext_d92935b612a4c23a]
- ECO-M017 | end_to_end_latency | target: wall-clock request completion | direction: lower | minimal protocol: model/version, prompt, frames, resolution, steps, batch/concurrency, warmup, device, dtype, compiler, cache, p50/p95, failed requests | blind spots: excludes quality unless paired; service queues and cold starts; network overhead | comparability: compare only under the same workload and service boundary [@src_s_ext_13736dc1c9b1994c; @src_s_ext_82285d355bc15510; @src_s_ext_98ddce99618b4027]
- ECO-M018 | throughput | target: completed outputs per unit of time | direction: higher | minimal protocol: same output definition, quality gate, batch/concurrency, device count, duration/fps/resolution, failure handling | blind spots: batching can raise it while latency worsens; frame/s hides video duration and quality | comparability: state numerator, denominator and the quality acceptance rule explicitly [@src_s_ext_4f224d03e281827c; @src_s_ext_57f457395c501e8a; @src_s_ext_caf93eb5901acc77]
- ECO-M019 | peak_memory | target: maximum accelerator memory used | direction: lower | minimal protocol: same model, dtype, batch, frames, resolution, offload, allocator, peak reset, device count | blind spots: host memory and transfer cost can be hidden; reserved versus allocated ambiguity | comparability: report per-device accelerator memory with host memory and offload [@src_s_ext_fe29d7e2334824e7; @src_s_ext_a04fcd8503ac7c51; @src_s_ext_8bdfea4a272027d7; @src_s_ext_27e55f929824efd0; @src_s_ext_aa8861f307dc3370]
- ECO-M020 | energy_per_accepted_output | target: energy cost after quality and success gating | direction: lower | minimal protocol: same hardware, sampling interval, idle baseline, workload, success/quality gate, retries | blind spots: sensor precision; embodied energy excluded; the quality gate is subjective | comparability: not available in most papers; mark NR rather than infer from latency [@src_s_ext_13736dc1c9b1994c; @src_s_ext_4f224d03e281827c]
- ECO-M021 | PickScore | target: learned prediction of real-user preference conditional on a prompt | direction: higher | minimal protocol: exact PickScore checkpoint, Pick-a-Pic split/version, prompt/image preprocessing, paired outputs, ties, confidence interval, held-out blind human audit | blind spots: self-selected-user bias, source-generator era, cultural and interface bias, reward hacking when optimized directly | comparability: compare only with the same checkpoint and evaluation prompts; not a universal human utility [@src_s_ext_463d2c341c7ad7d0; @src_s_ext_33d29297a792c446]
- ECO-M022 | ImageReward | target: learned expert preference for a prompt-image pair | direction: higher | minimal protocol: exact checkpoint and code commit, prompt/image preprocessing, held-out split, source-model strata, uncertainty, blind human audit | blind spots: expert-rubric dependence, model/domain drift, a scalar that collapses dimensions, reward hacking and overoptimization | comparability: do not combine raw values across checkpoint versions, or treat reward-maximized outputs as unbiased evaluation [@src_s_ext_96452f21157f1b23; @src_s_ext_0341223e1150a5f6]
- ECO-M023 | HPS_v2 | target: learned human preference prediction and performance on the four-style benchmark | direction: higher | minimal protocol: exact HPS v2 checkpoint, HPD/test-prompt release, four style labels, identical generation budget, seeds, paired blind human audit | blind spots: HPD/model-era bias, style taxonomy limits, annotator/cultural shift, optimization gaming | comparability: HPS, HPS v2 and later checkpoints are different metrics; never mix leaderboard values across releases [@src_s_ext_74fbe248ebdc6108; @src_s_arxiv_2306_09341; @src_s_ext_b9537ace6ebc0114]

## Chapter 16 Engineering Systems, Efficiency, and Reproduction Audit

Inputs and questions: the system component table, the R001/R002 run artifacts, code and weight availability, and reproduction levels.

Argumentative move: separate local algorithmic cost from end-to-end system KPIs, and report what actually ran rather than which files exist, following R1—R5.

A training system must pin the data version, the distributed topology, the mixed-precision setting, the checkpoints and the configuration identity. An inference system must record text encoding, sampler, denoiser, codec, caching, quantization, compilation, transfer, cold start, queueing and failure retries. NFE, FLOPs, parameter count and single-kernel multipliers are not end-to-end p50/p95, throughput, energy or cost.

**System components grouped by type. Author-reported efficiency figures are not rewritten into same-hardware measurements.}

\begin{tabular}{lcl}

Component type & Number of records & Examples \\

open_weight_model & 10 & Stable Diffusion XL Base 1.0, FLUX.1-schnell, Stable Diffusion 3.5 Large Turbo \\
cache & 5 & DeepCache, FasterCache, TeaCache \\
closed_service & 5 & Sora Turbo, Sora 2, Veo 3.1 \\
quantization & 5 & Q-Diffusion, Q-DiT, QVD \\
distillation & 2 & Progressive Distillation, Latent Consistency Models \\
open_weight_world_model & 2 & Cosmos Predict2, Cosmos 3 \\
attention_kernel & 1 & SageAttention \\
inference_engine & 1 & xDiT \\
model_family & 1 & Consistency Models \\
open_research_stack & 1 & Open-Sora 2.0 \\
parallel_inference & 1 & PipeFusion \\
research_only_closed_model & 1 & Movie Gen \\
sampler & 1 & DPM-Solver \\

\end{tabular}

\end{table**

|
Component type | Number of records | Examples |
|---|---|---|
|
open_weight_model | 10 | Stable Diffusion XL Base 1.0, FLUX.1-schnell, Stable Diffusion 3.5 Large Turbo |
| cache | 5 | DeepCache, FasterCache, TeaCache |
| closed_service | 5 | Sora Turbo, Sora 2, Veo 3.1 |
| quantization | 5 | Q-Diffusion, Q-DiT, QVD |
| distillation | 2 | Progressive Distillation, Latent Consistency Models |
| open_weight_world_model | 2 | Cosmos Predict2, Cosmos 3 |
| attention_kernel | 1 | SageAttention |
| inference_engine | 1 | xDiT |
| model_family | 1 | Consistency Models |
| open_research_stack | 1 | Open-Sora 2.0 |
| parallel_inference | 1 | PipeFusion |
| research_only_closed_model | 1 | Movie Gen |
| sampler | 1 | DPM-Solver |
| | | |

R001 was checked against the R1 formula: 3/3 contracts passed, and the ranking status is fixed as REFUSED_NONCOMPARABLE_CONTRACTS. It contains no author code, no neural modules, no weights, no generated outputs and no paper metrics. [@src_s_exp_r001_formula_metrics]

R002 is a smoke test of metric implementation and protocol guards. The FID/KID invariants on synthetic features and the preprocessing fingerprint both passed. It used no real images, no generative models and no visual feature extractors. The C2PA tool gate status is blocked_tool_unavailable: no tool was installed, no samples were downloaded and no round trip was executed. A measured conversion to valid, invalid or absent therefore cannot be claimed. [@src_s_exp_r002_metric_fixture; @src_s_exp_r002_c2pa_availability]

![F09. Pareto construction gate and refusal status. The 36 system entries disagree on data, hardware, quality gates, resources and openness protocols. This survey executed no model inference under one uniform protocol, so the Pareto frontier is explicitly refused rather than fabricated or assembled from heterogeneous points. Evidence boundary: this figure evaluates evidence comparability, not system performance; author-reported NFE, FID and speedup multipliers are not rewritten as measurements by this survey.](../figures/F09_pareto_evidence_gate.png)

*F09. Pareto construction gate and refusal status. The 36 system entries disagree on data, hardware, quality gates, resources and openness protocols. This survey executed no model inference under one uniform protocol, so the Pareto frontier is explicitly refused rather than fabricated or assembled from heterogeneous points. Evidence boundary: this figure evaluates evidence comparability, not system performance; author-reported NFE, FID and speedup multipliers are not rewritten as measurements by this survey.*

![F10. Reproduction maturity. This project completed the static audit, 3 formula contracts and 3 synthetic-feature metric contracts. Weight inference and end-to-end training/evaluation both stand at 0. The C2PA round trip is blocked because the tool is unavailable, and it must not be written as validation passed. Evidence boundary: the “pass” of R001/R002 refers only to frozen invariants. There is no image-feature validity, no generative-model sample and no reproduction of speed, memory or paper performance.](../figures/F10_reproduction_maturity.png)

*F10. Reproduction maturity. This project completed the static audit, 3 formula contracts and 3 synthetic-feature metric contracts. Weight inference and end-to-end training/evaluation both stand at 0. The C2PA round trip is blocked because the tool is unavailable, and it must not be written as validation passed. Evidence boundary: the “pass” of R001/R002 refers only to frozen invariants. There is no image-feature validity, no generative-model sample and no reproduction of speed, memory or paper performance.*

Overall judgment: an efficiency conclusion must establish a quality—latency—memory—throughput—energy—license vector along the complete service path. It cannot be extrapolated from local speedups.

Evidence boundary: the highest execution level at present is formula contracts and synthetic-feature metric fixtures. The project has run no public-weight inference, no training and no paper-level end-to-end reproduction.

Transition: Chapter 17 maps the risks of training data, outputs, platform and users onto control and accountability layers.

### System Component Audit Index

- ECO-Y001｜DPM-Solver｜Type sampler｜Mechanism a high-order ODE solver with a dedicated diffusion formulation｜Code official code, public｜Weights uses base-model weights｜Hardware paper-specific GPUs, not normalized｜Author-reported efficiency 10-20 NFE, and the paper reports 4-16x against selected training-free samplers｜Evidence status paper_plus_code_not_executed｜Comparability same base model, noise schedule, NFE and hardware only｜Boundary reduced NFE may not equal the wall-clock ratio [@src_s_ext_13736dc1c9b1994c; @src_s_ext_205c5653b363344f]
- ECO-Y002｜Progressive Distillation｜Type distillation｜Mechanism iteratively distills two sampling steps into one｜Code research implementations exist｜Weights distilled weights, task-specific｜Hardware specific to the paper｜Author-reported efficiency paper demonstrates reduction from many steps to 4｜Evidence status peer_reviewed_not_executed｜Comparability same teacher, student, data and training budget only｜Boundary training cost and possible diversity loss must be included [@src_s_ext_bab5c1176c129773]
- ECO-Y003｜Consistency Models｜Type model_family｜Mechanism maps noise directly to data with a consistency property｜Code official research code linked by the paper｜Weights model-specific｜Hardware specific to the paper｜Author-reported efficiency one-step or few-step generation｜Evidence status peer_reviewed_not_executed｜Comparability same model size, data and training budget only｜Boundary not a drop-in speedup for arbitrary diffusion checkpoints [@src_s_ext_041a050598868907]
- ECO-Y004｜Latent Consistency Models｜Type distillation｜Mechanism latent consistency distillation for few-step sampling｜Code official code, public｜Weights public checkpoints｜Hardware paper and model-card hardware｜Author-reported efficiency 2-4 steps in reported setups｜Evidence status paper_plus_code_weights_not_executed｜Comparability lock base model, checkpoint, guidance and scheduler｜Boundary license and quality differ by checkpoint [@lcm; @src_s_ext_fe6673cb211972b8]
- ECO-Y005｜DeepCache｜Type cache｜Mechanism reuses high-level U-Net features at selected steps｜Code official code, public｜Weights uses base-model weights｜Hardware paper-specific GPU｜Author-reported efficiency 2.3x on SD1.5, with a reported 0.05 CLIP decline, and 4.1x on LDM-4-G, with a reported +0.22 FID｜Evidence status peer_reviewed_author_report｜Comparability values stay attached to the named workload｜Boundary the cache interval can cause content and detail drift [@src_s_ext_82285d355bc15510; @src_s_ext_80c1a7d017512c6a]
- ECO-Y006｜FasterCache｜Type cache｜Mechanism reuses conditional-unconditional and temporal features｜Code availability of paper and repository varies by release｜Weights uses base-model weights｜Hardware paper-specific GPU｜Author-reported efficiency 1.67x on Vchitect-2.0 in the author report｜Evidence status peer_reviewed_author_report｜Comparability same model, frames, resolution, steps and GPU only｜Boundary longer clips and different CFG schedules may change gains [@src_s_ext_98ddce99618b4027]
- ECO-Y007｜TeaCache｜Type cache｜Mechanism uses timestep-embedding changes to estimate output changes and trigger the cache｜Code official code, public｜Weights uses base-model weights｜Hardware per-repository benchmark hardware｜Author-reported efficiency repository and paper report model-dependent speedups｜Evidence status paper_plus_code_not_executed｜Comparability compare only at a fixed threshold and model｜Boundary supported-model matrix and default thresholds drift [@src_s_ext_9034ed5341b82826; @src_s_ext_6276152978fb722c]
- ECO-Y008｜Pyramid Attention Broadcast｜Type cache｜Mechanism broadcasts attention outputs across timestep ranges at pyramid levels｜Code research code, status version-dependent｜Weights uses base-model weights｜Hardware specific to the paper｜Author-reported efficiency up to 10.5x in a selected author-reported setting｜Evidence status preprint_author_report｜Comparability maximum speedup cannot be pooled across models｜Boundary attention reuse may impair motion and detail [@src_s_ext_8b66a4dc7b04db0d]
- ECO-Y009｜PipeFusion｜Type parallel_inference｜Mechanism displaced patch pipeline parallelism with stale-feature reuse｜Code implemented in xDiT｜Weights uses base-model weights｜Hardware paper includes an 8x L40 setting｜Author-reported efficiency paper reports multi-GPU scaling under a named setup｜Evidence status preprint_plus_engine_not_executed｜Comparability same GPU count, interconnect, resolution and batch｜Boundary communication and stale patches can dominate or affect quality [@src_s_ext_4f224d03e281827c; @src_s_ext_57f457395c501e8a]
- ECO-Y010｜xDiT｜Type inference_engine｜Mechanism unifies sequence, tensor, pipe and CFG parallel methods｜Code public repository｜Weights external model weights｜Hardware GPU clusters listed by the repository｜Author-reported efficiency scaling claims specific to the repository｜Evidence status official_repo_static_audit｜Comparability requires an identical engine commit and topology｜Boundary code presence does not prove every advertised path runs [@src_s_ext_57f457395c501e8a]
- ECO-Y011｜Q-Diffusion｜Type quantization｜Mechanism applies timestep-aware calibration and shortcut splitting in PTQ｜Code research code linked by the project｜Weights quantized artifacts, model-specific｜Hardware specific to the paper｜Author-reported efficiency low-bit model quality is reported, while E2E speed is hardware-dependent｜Evidence status peer_reviewed_not_executed｜Comparability same bit-width, calibration, kernel and base checkpoint｜Boundary low bit-width without optimized kernels may not reduce latency [@src_s_ext_fe29d7e2334824e7]
- ECO-Y012｜Q-DiT｜Type quantization｜Mechanism post-training quantization tailored to diffusion transformers｜Code research implementation, status version-dependent｜Weights quantized artifacts, task-specific｜Hardware specific to the paper｜Author-reported efficiency paper reports compression and acceleration on selected hardware｜Evidence status peer_reviewed_not_executed｜Comparability match W/A bits and calibration data｜Boundary accuracy and runtime are separate claims [@src_s_ext_a04fcd8503ac7c51]
- ECO-Y013｜QVD｜Type quantization｜Mechanism video diffusion PTQ with temporal and timestep calibration｜Code research code, status version-dependent｜Weights quantized artifacts, task-specific｜Hardware specific to the paper｜Author-reported efficiency W8A8 results reported by the authors｜Evidence status preprint_not_executed｜Comparability same frames, resolution, base model and kernel｜Boundary the W8A8 label alone is not a service-speed guarantee [@src_s_ext_8bdfea4a272027d7]
- ECO-Y014｜Q-VDiT｜Type quantization｜Mechanism post-training quantization for video diffusion transformers｜Code research code, status version-dependent｜Weights quantized artifacts, task-specific｜Hardware specific to the paper｜Author-reported efficiency memory and runtime benefits reported by the authors｜Evidence status peer_reviewed_not_executed｜Comparability same hardware, runtime and bit configuration｜Boundary host offload and decoder costs may hide gains [@src_s_ext_27e55f929824efd0]
- ECO-Y015｜SVDQuant｜Type quantization｜Mechanism moves activation outliers into a low-rank branch and quantizes the main branch to W4A4｜Code project code and Nunchaku engine public｜Weights public quantized models for selected bases｜Hardware supported NVIDIA GPUs｜Author-reported efficiency authors report acceleration with Nunchaku on supported GPUs｜Evidence status peer_reviewed_project_not_executed｜Comparability must match GPU, runtime, base model and low-rank rank｜Boundary engine support is a prerequisite [@src_s_ext_aa8861f307dc3370]
- ECO-Y016｜SageAttention｜Type attention_kernel｜Mechanism quantized attention kernels｜Code public repository｜Weights uses external weights｜Hardware supported NVIDIA GPU list｜Author-reported efficiency repository reports roughly 2-5x attention-kernel speedup｜Evidence status official_repo_author_benchmark｜Comparability kernel benchmark separate from model E2E｜Boundary unsupported shapes or non-attention bottlenecks reduce benefit [@src_s_ext_caf93eb5901acc77]
- ECO-Y017｜EasyCache｜Type cache｜Mechanism architecture-agnostic, training-free cache policy｜Code preprint implementation status｜Weights uses base-model weights｜Hardware specific to the paper｜Author-reported efficiency 2.1-3.3x in the author report｜Evidence status preprint_not_executed｜Comparability same model, threshold, hardware and workload｜Boundary new preprint, independent replication absent [@src_s_ext_3d04b2a0a9b9c1fb]
- ECO-Y018｜Stable Diffusion XL Base 1.0｜Type open_weight_model｜Mechanism latent diffusion XL base plus an optional refiner｜Code official inference code public｜Weights public｜Hardware self-hosted GPU, configuration-dependent｜Author-reported efficiency NR｜Evidence status official_model_card_static_audit｜Comparability benchmark only the exact checkpoint and refiner path｜Boundary the restricted-use license is not OSI open source [@src_s_ext_3c44be280a6c9e15]
- ECO-Y019｜FLUX.1-schnell｜Type open_weight_model｜Mechanism distilled rectified-flow transformer for few-step generation｜Code public inference ecosystem｜Weights public｜Hardware self-hosted GPU, dtype and offload dependent｜Author-reported efficiency few-step generation, with no universal latency supplied here｜Evidence status official_model_card_static_audit｜Comparability do not transfer license or performance from dev or pro variants｜Boundary checkpoint-specific architecture and terms [@src_s_ext_4143cf1471f6ab87]
- ECO-Y020｜Stable Diffusion 3.5 Large Turbo｜Type open_weight_model｜Mechanism distilled MMDiT image generator｜Code official inference code public｜Weights public｜Hardware vendor reports consumer and data-center compatibility by variant｜Author-reported efficiency 4-step generation, and the 9.9 GiB claim covers Medium but excludes text encoders｜Evidence status official_release_author_report｜Comparability variant-specific, no cross-vendor ranking｜Boundary custom revenue-threshold license, plus vendor comparative claims [@src_s_ext_06d2114e2823654c]
- ECO-Y021｜Wan2.1 T2V-1.3B｜Type open_weight_model｜Mechanism diffusion transformer with Wan-VAE｜Code official code, public｜Weights public｜Hardware RTX 4090 example with offload options｜Author-reported efficiency about 4 minutes per 5-second 480p video on an RTX 4090 without quantization, per the README｜Evidence status official_repo_author_report｜Comparability exact command, checkpoint and prompt-extension setting required｜Boundary 720p is supported, but the author recommends 480p for stability [@src_s_ext_839f5acdc65fd3f7]
- ECO-Y022｜Wan2.1 T2V-14B｜Type open_weight_model｜Mechanism large diffusion transformer with Wan-VAE｜Code official code, public｜Weights public｜Hardware single or multi-GPU, with FSDP+xDiT supported｜Author-reported efficiency NR｜Evidence status official_repo_static_audit｜Comparability prompt extension and distributed path must be declared｜Boundary large memory requirement and external prompt model can alter quality and cost [@src_s_ext_839f5acdc65fd3f7]
- ECO-Y023｜Wan2.2 T2V-A14B｜Type open_weight_model｜Mechanism timestep-specialized mixture-of-experts diffusion model｜Code official code, public｜Weights public｜Hardware README single-GPU command requires at least 80 GiB VRAM｜Author-reported efficiency NR｜Evidence status official_repo_static_audit｜Comparability same checkpoint and prompt-extension setting only｜Boundary vendor says broader data, with no public sample-level training-data ledger [@src_s_ext_4460f5dc2a2e30b2]
- ECO-Y024｜Wan2.2 TI2V-5B｜Type open_weight_model｜Mechanism 5B joint text/image-to-video model with 16x16x4 VAE compression｜Code official code, public｜Weights public｜Hardware vendor states consumer GPU such as RTX 4090｜Author-reported efficiency no normalized latency in the retained evidence｜Evidence status official_repo_static_audit｜Comparability TI2V-5B must be distinguished from T2V-A14B｜Boundary consumer-GPU availability does not imply real-time [@src_s_ext_4460f5dc2a2e30b2]
- ECO-Y025｜HunyuanVideo｜Type open_weight_model｜Mechanism large video diffusion transformer｜Code official code, public｜Weights public｜Hardware multi-GPU and quantization options vary｜Author-reported efficiency NR｜Evidence status official_model_card_static_audit｜Comparability exact checkpoint, runtime and license version required｜Boundary custom license includes geographic and acceptable-use conditions [@src_s_ext_49fc67fa3d5b229b]
- ECO-Y026｜LTX-Video｜Type open_weight_model｜Mechanism video latent diffusion transformer optimized for long spatial-temporal compression｜Code official code, public｜Weights public｜Hardware vendor-listed hardware and optimized paths｜Author-reported efficiency vendor uses real-time language for selected configurations｜Evidence status official_model_card_author_report｜Comparability latency requires version, hardware, frames, fps and resolution｜Boundary open weights are not the same as OSI open source [@src_s_ext_3377b81d395d3de7]
- ECO-Y027｜Mochi 1 preview｜Type open_weight_model｜Mechanism asymmetric video diffusion transformer｜Code official code, public｜Weights public｜Hardware large GPU requirements, runtime dependent｜Author-reported efficiency NR｜Evidence status official_model_card_static_audit｜Comparability preview-version comparisons expire with updates｜Boundary training-data disclosure and safety coverage remain limited [@src_s_ext_1de1cfdb739dc858]
- ECO-Y028｜Open-Sora 2.0｜Type open_research_stack｜Mechanism open training and inference stack for video generation｜Code public｜Weights selected checkpoints public｜Hardware multi-GPU training and inference｜Author-reported efficiency NR｜Evidence status official_repo_static_audit｜Comparability code, checkpoint and data openness are three separate axes｜Boundary main branch is mutable, so a commit pin is required [@src_s_ext_bde1adbe087b0baa]
- ECO-Y029｜Cosmos Predict2｜Type open_weight_world_model｜Mechanism conditional world-generation models for physical AI｜Code public under Apache-2.0｜Weights public under NVIDIA Open Model License｜Hardware NVIDIA GPU stacks｜Author-reported efficiency NR｜Evidence status official_repo_static_audit｜Comparability not directly comparable to entertainment text-to-video｜Boundary policy, simulator and generator roles must be separated [@src_s_ext_3c4d37cf1a6a49cd]
- ECO-Y030｜Movie Gen｜Type research_only_closed_model｜Mechanism 30B transformer media foundation model with personalized, editing and audio components｜Code not publicly released as a complete system｜Weights not public｜Hardware undisclosed serving hardware｜Author-reported efficiency NR｜Evidence status official_technical_report_only｜Comparability closed technical report, no independent runtime｜Boundary do not infer production availability or hidden architecture beyond the report [@src_s_ext_629f563e4913e713]
- ECO-Y031｜Sora Turbo｜Type closed_service｜Mechanism closed video generation service｜Code closed｜Weights closed｜Hardware provider-run service｜Author-reported efficiency provider states it is faster than the February preview, with no normalized value｜Evidence status official_product_evidence｜Comparability no cross-service ranking｜Boundary the web and app were discontinued on 2026-04-26, and the page's current status supersedes launch availability [@src_s_ext_2d134659d4de90af]
- ECO-Y032｜Sora 2｜Type closed_service｜Mechanism closed synchronized video-audio generation with a likeness feature｜Code closed｜Weights closed｜Hardware provider-run service｜Author-reported efficiency NR｜Evidence status official_product_evidence｜Comparability no independent inference of architecture or runtime｜Boundary the web and app are discontinued, while the API sunset was still in the future as of cutoff [@src_s_ext_661a3476d3d51bb4; @src_s_ext_4d8dc7c1bcf322d1]
- ECO-Y033｜Veo 3.1｜Type closed_service｜Mechanism closed video generation with native audio and editing inputs｜Code closed｜Weights closed｜Hardware provider-run service｜Author-reported efficiency NR｜Evidence status official_product_evidence｜Comparability entry, version, region and upscaling must all be declared｜Boundary closed demos do not reveal model architecture [@src_s_ext_4914c72c16a5ed4b; @src_s_ext_c59cbd3dcf25d3c8; @src_s_ext_8dc07480ab081fa9]
- ECO-Y034｜Seedance 2.0｜Type closed_service｜Mechanism unified multimodal reference, edit and continuation, plus stereo audio generation｜Code closed｜Weights closed｜Hardware provider-run service｜Author-reported efficiency NR｜Evidence status official_product_evidence｜Comparability vendor comparison not merged with independent benchmarks｜Boundary the official page reports remaining issues with detail stability and audio distortion [@src_s_ext_724cb3e9025c287d]
- ECO-Y035｜Kling AI 3.0｜Type closed_service｜Mechanism product family covering Video 3.0, Omni, Image 3.0 and Omni｜Code closed｜Weights closed｜Hardware provider-run service｜Author-reported efficiency NR｜Evidence status official_product_evidence｜Comparability availability and quotas require a live product check｜Boundary corporate announcement is not independent benchmark evidence [@src_s_ext_effdb7b60b6799e6]
- ECO-Y036｜Cosmos 3｜Type open_weight_world_model｜Mechanism omnimodal backbone spanning reasoning, generation, policy and dynamics｜Code public code link｜Weights public model-card links｜Hardware NVIDIA hardware ecosystem｜Author-reported efficiency NR｜Evidence status official_research_release｜Comparability task-specific evaluation only｜Boundary rank-1 claims are provider summaries and must not be pooled with other protocols [@src_s_ext_0aa0fa2de9f38b24; @src_s_ext_129b2c193f5b258e]

## Chapter 17 Safety, Copyright, Provenance, and Social Impact

Input and question: training corpora, memorization, bias, deepfakes, watermarking, provenance credentials, and platform and legal responsibility inside the risk–control matrix.

Argument move: allocate control and verification along the data, model, generation service, content distribution, and appeal-and-correction chain. Do not mistake the existence of a specification for effective enforcement.

Training-data governance requires per-sample provenance, a rights basis, timestamps, deletion propagation, and checkpoint lineage. A license on a dataset that distributes only URLs or metadata does not automatically transfer the underlying media rights. Legal status is also bounded by jurisdiction and case facts. This survey does not provide legal advice.

At the model layer the risks are memorization and near-duplication, privacy leakage, representativeness and bias, and generation that bypasses safety policies. An evaluation should distinguish attack entry points, affected groups, base rates, false positives and false negatives, human review, and recovery. It must not estimate the incidence rate of social harm from a single refusal rate or a demonstration example.

Provenance work must separate signed content credentials, robust watermarks, detectors, service logs and platform labels. A missing credential is not proof that a human created the content. A watermark hit does not prove lawful training. Nor does publishing a standard prove the retention rate across editors, transcoding and platform transmission.

**Risk controls summarized by control layer; the existence of a control does not equal effective enforcement.}

\begin{tabular}{lcl}

Control layer & Record count & Risk examples \\

evaluation_governance & 3 & metric drift or invalid cross-paper ranking, judge bias or capability bottleneck misattributed to generator, distribution shift, population bias and reward hacking produce inflated apparent quality \\
data_governance & 2 & copyrighted material included without adequate provenance or licensing basis, privacy and biometric misuse \\
provenance & 2 & C2PA manifest stripped so provenance becomes unavailable, contradictory authenticated signals or metadata washing \\
compliance & 1 & noncompliance or misleading disclosure across generation and distribution chain \\
content_safety & 1 & non-consensual sexual imagery, impersonation, harassment \\
data_quality & 1 & hallucinated captions, demographic bias, unsafe omissions and teacher-model imprint \\
data_safety & 1 & CSAM/CSEM or other illegal harmful content enters index and training \\
human_study & 1 & position bias, rater fatigue, cultural bias, non-transitive preferences \\
legal_governance & 1 & lawsuit allegations or judgment over training, output similarity, trademark and territorial acts \\
organizational_security & 1 & deepfake identity used to authorize high-value transfer \\
platform_governance & 1 & upstream markers missing, stripped or misread; user does not disclose \\
release_governance & 1 & license misunderstood as unrestricted open source or commercial permission \\
reporting & 1 & quality dimensions and minority failure modes are hidden \\
reproducibility & 1 & current files differ from cited or tested version \\
runtime_control & 1 & motion, identity, text or detail drift from stale features \\
systems_evaluation & 1 & headline speedup fails under real workload or loses quality \\
watermark & 1 & false negative after transformation or unsupported generator; false positive near threshold \\

\end{tabular}

\end{table**

|
Control layer | Record count | Risk examples |
|---|---|---|
|
evaluation_governance | 3 | metric drift or invalid cross-paper ranking, judge bias or capability bottleneck misattributed to generator, distribution shift, population bias and reward hacking produce inflated apparent quality |
| data_governance | 2 | copyrighted material included without adequate provenance or licensing basis, privacy and biometric misuse |
| provenance | 2 | C2PA manifest stripped so provenance becomes unavailable, contradictory authenticated signals or metadata washing |
| compliance | 1 | noncompliance or misleading disclosure across generation and distribution chain |
| content_safety | 1 | non-consensual sexual imagery, impersonation, harassment |
| data_quality | 1 | hallucinated captions, demographic bias, unsafe omissions and teacher-model imprint |
| data_safety | 1 | CSAM/CSEM or other illegal harmful content enters index and training |
| human_study | 1 | position bias, rater fatigue, cultural bias, non-transitive preferences |
| legal_governance | 1 | lawsuit allegations or judgment over training, output similarity, trademark and territorial acts |
| organizational_security | 1 | deepfake identity used to authorize high-value transfer |
| platform_governance | 1 | upstream markers missing, stripped or misread; user does not disclose |
| release_governance | 1 | license misunderstood as unrestricted open source or commercial permission |
| reporting | 1 | quality dimensions and minority failure modes are hidden |
| reproducibility | 1 | current files differ from cited or tested version |
| runtime_control | 1 | motion, identity, text or detail drift from stale features |
| systems_evaluation | 1 | headline speedup fails under real workload or loses quality |
| watermark | 1 | false negative after transformation or unsupported generator; false positive near threshold |
| | | |

![F11. Safety–Control–Responsibility chain. A risk entry must record its entry point, affected parties, control layer, responsible party, verification method and known failures at the same time. A single watermark or filter must not summarize the entire chain. Evidence boundary: the risk and control entries are evidence navigation and a verification checklist, not legal advice. The phrase "the existence of a control" does not prove that enforcement is effective or that the risk has been eliminated.](../figures/F11_risk_control_responsibility.png)

*F11. Safety–Control–Responsibility chain. A risk entry must record its entry point, affected parties, control layer, responsible party, verification method and known failures at the same time. A single watermark or filter must not summarize the entire chain. Evidence boundary: the risk and control entries are evidence navigation and a verification checklist, not legal advice. The phrase "the existence of a control" does not prove that enforcement is effective or that the risk has been eliminated.*

Overall judgment: effective governance depends on combined controls across data, models, credentials, platforms and organizational correction. No single layer can prove authenticity, legality and safety at the same time.

Evidence boundary: the matrix records control design and known failures. It does not measure incidence or enforcement effect. Policy and litigation entries carry time and jurisdiction boundaries.

Transition: Chapter 18 groups the industry and news snapshot by occurrence date, announcement date, reporting date and evidence type.

### Risk–Control–Responsibility Audit Index

- ECO-R001 | Asset training_corpus | Entry web crawl and URL index | Risk copyrighted material is included without an adequate provenance or licensing basis | Affected parties rights holders, model developers and downstream deployers | Control retain the source URL, license and timestamp; record a rights-basis field; provide a takedown channel; propagate deletions; review jurisdiction | Responsibility dataset curator and model developer | Verification sample-level provenance audit; removal SLA; checkpoint lineage | Known failure a metadata license does not transfer the underlying-media rights | Status open_control_gap [@src_s_ext_e21d4fdc23640b7f; @src_s_ext_c32738ba82a67d01; @src_s_ext_8931ca2bc2a2df46; @src_s_ext_5d3ee9b7c90010d4; @src_s_ext_6584f6bb78a0e4bc]
- ECO-R002 | Asset training_corpus | Entry large-scale web ingestion | Risk CSAM/CSEM or other illegal harmful content enters the index and training | Affected parties children, victims, curators and developers | Control use trusted hash lists; run perceptual matching; escalate to humans; quarantine; rescan periodically; keep a model/version deletion ledger | Responsibility curator, developer and specialist safety organizations | Verification independent red-team sampling; known-hash coverage; false-negative audit; documented re-release diff | Known failure known hashes cannot prove the absence of unseen or transformed content | Status partially_mitigated [@src_s_ext_a71e52f7d74db2c8; @src_s_ext_813aaade7ed9b752; @src_s_ext_a322cc3b396554b6]
- ECO-R003 | Asset training_corpus | Entry face and person image collection | Risk privacy and biometric misuse | Affected parties data subjects | Control apply purpose limitation; assess consent and rights; prohibit biometric use; control access; honor deletion requests | Responsibility curator and deployer | Verification dataset-card compliance audit; downstream-use review | Known failure an open download can enable uses outside the intended purpose | Status control_present_with_enforcement_gap [@src_s_ext_2e0206c9bfbf134e]
- ECO-R004 | Asset training_corpus | Entry machine captioning and filtering | Risk hallucinated captions, demographic bias, unsafe omissions and teacher-model imprint | Affected parties model users and represented groups | Control store raw and generated fields separately; record the caption-model/version; retain confidence; run a human stratified audit; tag errors | Responsibility dataset curator | Verification audited sample by language/domain/demographic; inter-annotator agreement | Known failure teacher errors can be amplified at scale, and they are correlated | Status open_control_gap [@src_s_ext_3baa8d3a6825c19d; @src_s_ext_aad92459dff9dad3; @src_s_arxiv_2407_02371; @src_s_ext_d8a85afa9f84a924]
- ECO-R005 | Asset evaluation | Entry FID/KID/FVD computation | Risk metric drift or an invalid cross-paper ranking | Affected parties research readers and developers | Control ship an executable metric manifest; hash the extractor and the data split; fix the sample counts; report uncertainty; forbid cross-protocol ranking | Responsibility benchmark maintainer and paper author | Verification recompute fixture; schema check; known-reference regression | Known failure the same metric name can hide incompatible implementations | Status required [@src_s_ext_f13a72e436c93975; @src_s_ext_55391d95358da2e0; @src_s_ext_cdc5919a22f406ea]
- ECO-R006 | Asset evaluation | Entry CLIP/VQA/MLLM automated judge | Risk judge bias or a capability bottleneck misattributed to the generator | Affected parties model developers and users | Control use a judge ensemble only if predeclared; pin versions; audit the judge adversarially; spot-check by hand; report disagreement | Responsibility benchmark maintainer | Verification held-out human correlation by category; judge failure taxonomy | Known failure new generators may exploit the judge or fall outside its distribution | Status partially_mitigated [@src_s_ext_1610191ec47a137a; @src_s_ext_2da47e6f77711c51; @src_s_ext_886b46db89300b7c; @src_s_ext_ca220ee450d111b2; @src_s_ext_1bdc47e96f4aa885]
- ECO-R007 | Asset evaluation | Entry single aggregate score | Risk quality dimensions and minority failure modes are hidden | Affected parties users and affected groups | Control publish the raw dimension vector, the prompt strata, the worst-group results and a Pareto view; preregister the aggregation | Responsibility paper author and reviewer | Verification table/figure contract validation; aggregation sensitivity analysis | Known failure the weights encode value judgments and can reverse the ordering | Status required [@src_s_ext_41ef49de30ab0c93; @src_s_ext_ada443910805e939; @src_s_ext_0ced8660dbcb0990; @src_s_ext_886b46db89300b7c]
- ECO-R008 | Asset human_evaluation | Entry pairwise or Likert study | Risk position bias, rater fatigue, cultural bias, non-transitive preferences | Affected parties participants and research readers | Control blind/randomize; allow ties; repeat anchors; report the rater population, reliability and CI; compensate and obtain consent | Responsibility study owner and ethics reviewer | Verification attention checks; repeated-item agreement; Bradley-Terry diagnostics; subgroup analysis | Known failure a large sample does not fix an ambiguous rubric or sampling bias | Status required [@src_s_ext_d92935b612a4c23a]
- ECO-R009 | Asset inference_service | Entry few-step, cache, parallel, quantized acceleration | Risk the headline speedup fails under a real workload or loses quality | Affected parties operators and users | Control fix an end-to-end benchmark contract; report p50/p95, failures, warm/cold, concurrency and power; accept quality on a paired test | Responsibility system developer and operator | Verification replay fixed prompts/seeds; trace stages; compare accepted-output cost | Known failure kernel/NFE gains can be dominated by the decoder, transfer, queue or retries | Status required [@src_s_ext_13736dc1c9b1994c; @src_s_ext_82285d355bc15510; @src_s_ext_98ddce99618b4027; @src_s_ext_4f224d03e281827c; @src_s_ext_fe29d7e2334824e7; @src_s_ext_caf93eb5901acc77]
- ECO-R010 | Asset inference_service | Entry cache reuse | Risk motion, identity, text or detail drift from stale features | Affected parties content creators and downstream users | Control set adaptive thresholds; detect scene changes; refresh periodically; apply task-specific quality gates; keep a fallback path | Responsibility method author and operator | Verification per-frame/per-object drift metrics plus blind human review | Known failure aggregate FVD/CLIP may miss localized temporal errors | Status partially_mitigated [@src_s_ext_82285d355bc15510; @src_s_ext_98ddce99618b4027; @src_s_ext_9034ed5341b82826; @src_s_ext_8b66a4dc7b04db0d]
- ECO-R011 | Asset open_model_release | Entry weights and code publication | Risk the license is misunderstood as unrestricted open source or commercial permission | Affected parties developers and organizations | Control keep an artifact-level license inventory; record SPDX when valid; note revenue/region/use restrictions; review dependencies and derivative works | Responsibility model publisher and adopter | Verification license hash; model-card snapshot; legal review for deployment | Known failure the code license and the checkpoint license may differ | Status required [@src_s_ext_3c44be280a6c9e15; @src_s_ext_06d2114e2823654c; @src_s_ext_49fc67fa3d5b229b; @src_s_ext_3377b81d395d3de7; @src_s_ext_3c4d37cf1a6a49cd]
- ECO-R012 | Asset open_model_release | Entry mutable Git/HF repositories | Risk the current files differ from the cited or tested version | Affected parties researchers and operators | Control pin the commit/revision; hash weights, config and license; archive the README and the environment lock | Responsibility researcher and maintainer | Verification fresh clone/checksum test; provenance manifest | Known failure presence on the main branch is not proof that an old checkpoint still works | Status required [@src_s_ext_839f5acdc65fd3f7; @src_s_ext_4460f5dc2a2e30b2; @src_s_ext_bde1adbe087b0baa]
- ECO-R013 | Asset generated_media | Entry file export and platform re-encoding | Risk the C2PA manifest is stripped, so provenance becomes unavailable | Affected parties viewers, publishers and investigators | Control preserve the embedded manifest; keep an external manifest repository; use soft binding; test pass-through on each platform; show an unknown state | Responsibility generator, editor, platform and verifier | Verification round-trip test across each platform; validator logs; trust-list version | Known failure the absence of a credential cannot prove that content is human-made | Status partially_mitigated [@src_s_ext_e0ffa9f133ce7fc3; @src_s_ext_52d0dd2ef128f371; @src_s_ext_615ff3cb7e5bf360]
- ECO-R014 | Asset generated_media | Entry watermark and provenance checked independently | Risk contradictory authenticated signals or metadata washing | Affected parties viewers and investigators | Control run a joint policy engine; bind the watermark assertion into signed provenance; preserve raw detector scores; escalate conflicts | Responsibility generator and verifier vendors | Verification red-team edit pipelines; conflict matrix; human forensic review | Known failure two valid-looking layers can still describe incompatible histories | Status open_research_gap [@src_s_ext_9f2a5c76c58bb158]
- ECO-R015 | Asset generated_media | Entry invisible watermark detection | Risk a false negative after transformation or from an unsupported generator; a false positive near the threshold | Affected parties creators, platforms and viewers | Control combine the visible label, signed provenance, the watermark and account/service logs; calibrate thresholds; publish the scope | Responsibility model provider and platform | Verification transform suite; ROC by media type; unsupported-source state | Known failure a watermark is not a universal detector, and it can be damaged or absent | Status partially_mitigated [@src_s_ext_03a8a1262c665625]
- ECO-R016 | Asset generated_media | Entry real-person reference or likeness upload | Risk non-consensual sexual imagery, impersonation, harassment | Affected parties depicted persons | Control verify consent; apply adult/age safeguards; block public-figure and sexual content; match hashes rapidly; notify victims and take content down | Responsibility generator provider and platform | Verification red-team policy bypass; response-time audit; victim-centered appeal metrics | Known failure generator attribution may remain unknown, and open pipelines can omit provider controls | Status open_control_gap [@src_s_ext_661a3476d3d51bb4; @src_s_ext_724cb3e9025c287d; @src_s_ext_e10e575afc4c5f3c]
- ECO-R017 | Asset communications_and_payments | Entry video meeting approval | Risk a deepfake identity is used to authorize a high-value transfer | Affected parties employees, organizations and customers | Control require an out-of-band callback; use hardware-backed identity; set transaction thresholds; require two-person approval; detect delay and anomalies | Responsibility employer, bank and communications platform | Verification tabletop exercise; simulated deepfake call; approval-log audit | Known failure visual familiarity and voice alone are weak authentication | Status actionable [@src_s_ext_49a83cb0a82fcf79; @src_s_ext_632bd1bb7c1c71d1]
- ECO-R018 | Asset distribution_platform | Entry AI label ingestion and display | Risk upstream markers are missing, stripped or misread; a user does not disclose | Affected parties viewers and affected persons | Control ingest multiple signals; require user disclosure; enforce policy; escalate to fact-checking; preserve labels across transforms | Responsibility platform | Verification known-positive/negative fixture; cross-upload round trip; appeal/error statistics | Known failure platform labels can over- or under-apply, and they may not survive reposting | Status partially_mitigated [@src_s_ext_e57798b9027c40ed; @src_s_ext_52d0dd2ef128f371]
- ECO-R019 | Asset regulatory_compliance | Entry China/EU machine-readable and visible labeling | Risk noncompliance or misleading disclosure across the generation and distribution chain | Affected parties users, providers, deployers and platforms | Control maintain a jurisdiction matrix; apply dual-layer labels; preserve metadata; send deployer notices; keep an audit log; manage change | Responsibility provider, deployer and platform | Verification pre-release legal mapping; automated file/UI test; platform round trip; retained evidence | Known failure one technical standard may not satisfy every legal requirement or exception | Status required [@src_s_ext_c03f6cc2696d90aa; @src_s_ext_43dbe36dfbe6e288; @src_s_ext_402803ae8da90ab8]
- ECO-R020 | Asset copyright_and_training | Entry model release and output service | Risk lawsuit allegations or a judgment over training, output similarity, trademark and territorial acts | Affected parties rights holders, developers and users | Control use licensed datasets; honor opt-out/takedown; run memorization tests; set output similarity guardrails; impose a litigation hold; obtain legal review | Responsibility developer and deployer | Verification dataset ledger; canary/memorization audit; complaint tracking; jurisdiction-specific counsel | Known failure no single case resolves all training and output questions | Status open_legal_uncertainty [@src_s_ext_6584f6bb78a0e4bc; @src_s_ext_d69aa51c5b39e5ea; @src_s_ext_1bf67a1ae824bb99; @src_s_ext_365ab6d86ca889fb; @src_s_ext_48da7c84540ba04d]
- ECO-R021 | Asset evaluation | Entry learned preference scorer used for evaluation or optimization | Risk distribution shift, population bias and reward hacking inflate the apparent quality | Affected parties users, benchmark readers and model developers | Control freeze the scorer/data versions; stratify by model and prompt domain; reserve a secret, human-evaluated holdout; monitor disagreement; prohibit scorer-only release gates | Responsibility benchmark maintainer and model developer | Verification pre-registered blind pairwise study; subgroup calibration; adversarial optimization test; score-human disagreement report | Known failure a high scorer value can reflect exploitation of the learned judge rather than broader human preference | Status required [@src_s_ext_463d2c341c7ad7d0; @src_s_ext_96452f21157f1b23; @src_s_ext_74fbe248ebdc6108; @src_s_arxiv_2306_09341]

## Chapter 18 Industry, Open-Source Ecosystem, and News Timeline

Input and question: 34 event cards for models, products, open source, availability, policy and incidents, together with their official and independent sources.

Argumentative move: record occurrence, announcement and reporting dates separately. Conclusions about closed-source systems stay bounded. News serves ecosystem interpretation, not algorithmic ranking.

The unit of an event stays "event—official version—occurrence date—evidence type." A publication date may fall later than the actual occurrence. Independent reporting may confirm external impact alone. None of the three can substitute for another. Official pages can verify specifications and terms. They cannot independently prove market effects, training architecture or comparative performance.

**Events summarized by type. Occurrence counts are not adoption rates, impact rates or causal evidence.}

\begin{tabular}{lcl}

Event type & Records & Examples \\

product_release & 9 & Sora Turbo released to subscription users, Sora 2 released with a social app launch, GPT-4o native image generation launched \\
open_release & 4 & Wan2.1 code and weights released, Wan2.2 code and weights released, Stable Diffusion 3.5 series openly released \\
policy_effective & 3 & China's Measures for Labeling AI-Generated Synthetic Content take effect, the AB 2013 training-data disclosure deadline arrives, the EU AI Act Article 50 transparency obligations begin to apply \\
policy_guidance & 2 & EU publishes a code of practice on marking and labeling AI-generated content, EU publishes Article 50 transparency guidelines \\
policy_report & 2 & U.S. Copyright Office publishes the copyrightability of AI outputs report Part 2, U.S. Copyright Office publishes the generative AI training report Part 3 pre-publication version \\
technical_release & 2 & Movie Gen technical report made public, Seedance 1.0 technical report made public \\
abuse_incident & 1 & Non-consensual synthetic intimate images spread at scale on X \\
availability_change & 1 & Sora web and app services discontinued \\
copyright_dispute & 1 & Seedance 2.0 triggers a Hollywood copyright dispute \\
copyright_judgment & 1 & UK High Court Getty v Stability AI judgment issued \\
copyright_litigation & 1 & Disney and Universal sue Midjourney \\
fraud_incident & 1 & Hong Kong deepfake video-conference transfer fraud reported \\
incident_and_availability & 1 & LAION-5B temporarily taken offline for safety review \\
policy_enactment & 1 & California AB 2013 signed by the governor \\
policy_publication & 1 & China's Measures for Labeling AI-Generated Synthetic Content published \\
product_update & 1 & Veo 3.1 adds vertical and high-definition output paths \\
remediation_release & 1 & Re-LAION-5B released with known problematic hashes removed \\
standard_release & 1 & C2PA 2.4 specification released \\

\end{tabular}

\end{table**

|
Event type | Records | Examples |
|---|---|---|
|
product_release | 9 | Sora Turbo released to subscription users, Sora 2 released with a social app launch, GPT-4o native image generation launched |
| open_release | 4 | Wan2.1 code and weights released, Wan2.2 code and weights released, Stable Diffusion 3.5 series openly released |
| policy_effective | 3 | China's Measures for Labeling AI-Generated Synthetic Content take effect, the AB 2013 training-data disclosure deadline arrives, the EU AI Act Article 50 transparency obligations begin to apply |
| policy_guidance | 2 | EU publishes a code of practice on marking and labeling AI-generated content, EU publishes Article 50 transparency guidelines |
| policy_report | 2 | U.S. Copyright Office publishes the copyrightability of AI outputs report Part 2, U.S. Copyright Office publishes the generative AI training report Part 3 pre-publication version |
| technical_release | 2 | Movie Gen technical report made public, Seedance 1.0 technical report made public |
| abuse_incident | 1 | Non-consensual synthetic intimate images spread at scale on X |
| availability_change | 1 | Sora web and app services discontinued |
| copyright_dispute | 1 | Seedance 2.0 triggers a Hollywood copyright dispute |
| copyright_judgment | 1 | UK High Court Getty v Stability AI judgment issued |
| copyright_litigation | 1 | Disney and Universal sue Midjourney |
| fraud_incident | 1 | Hong Kong deepfake video-conference transfer fraud reported |
| incident_and_availability | 1 | LAION-5B temporarily taken offline for safety review |
| policy_enactment | 1 | California AB 2013 signed by the governor |
| policy_publication | 1 | China's Measures for Labeling AI-Generated Synthetic Content published |
| product_update | 1 | Veo 3.1 adds vertical and high-definition output paths |
| remediation_release | 1 | Re-LAION-5B released with known problematic hashes removed |
| standard_release | 1 | C2PA 2.4 specification released |
| | | |

![F12. Academic, product, policy, legal, and event timeline. Each of the 34 events sits at its occurrence or effective date. Where the announcement date differs, the points are hollow and the lines are dashed. This sample is not the full news stream. The density of points therefore supports no inference about occurrence rates or causation. Evidence boundary: the event list is a purposive sample of topic-relevant, source-verified items, not systematic full news data. It reports no occurrence rates, risk rates, or causal effects.](../figures/F12_event_timeline.png)

*F12. Academic, product, policy, legal, and event timeline. Each of the 34 events sits at its occurrence or effective date. Where the announcement date differs, the points are hollow and the lines are dashed. This sample is not the full news stream. The density of points therefore supports no inference about occurrence rates or causation. Evidence boundary: the event list is a purposive sample of topic-relevant, source-verified items, not systematic full news data. It reports no occurrence rates, risk rates, or causal effects.*

Overall judgment: the event table shows open weights, closed-source products, availability changes, and governance infrastructure evolving in parallel. Even so, it is only a snapshot as of the cutoff date.

Evidence boundary: the number of events is not adoption rates, incident rates, or causal evidence. An entry without an independent source supports only conclusions at the level of official self-reporting.

Transition: Chapter 19 turns mechanism, timing, system, openness, and risk into conditional selection.

### Date and Source Audit Index for the 34 Events

- ECO-E029｜LAION-5B temporarily taken offline for safety review｜Occurred: 2023-12-19｜Announced: 2023-12-19｜Reported: 2023-12-20｜Institution: LAION｜Version: LAION-5B｜Type: incident_and_availability｜Disclosed technique: a research report on suspected CSAM links led to temporary removal of the dataset and a safety review｜Disclosure level: official_response_without_full_independent_forensics｜Availability: temporarily unavailable at event time｜Official source: S_EXT_813AAADE7ED9B752｜Independent source: S_EXT_A322CC3B396554B6｜Limits of the impact claim: exposes gaps in content governance, removal and audit capability at web-scale indexes｜Confidence: high [@src_s_ext_813aaade7ed9b752; @src_s_ext_a322cc3b396554b6]
- ECO-E031｜Non-consensual synthetic intimate images spread at scale on X｜Occurred: 2024-01-25｜Announced: 2024-01-25｜Reported: 2024-01-29｜Institution: unknown creators and X｜Version: NA｜Type: abuse_incident｜Disclosed technique: synthetic explicit images involving Taylor Swift spread. X temporarily restricted related searches｜Disclosure level: harm_event_and_platform_response_without_generator_attribution｜Availability: harmful content removed or limited once it spread｜Official source: none｜Independent source: S_EXT_E10E575AFC4C5F3C｜Limits of the impact claim: generation safeguards, platform detection, rapid takedown and victim redress need to act together. Reliable attribution to a single model is not possible｜Confidence: high [@src_s_ext_e10e575afc4c5f3c]
- ECO-E032｜Deepfake video-conference transfer fraud reported in Hong Kong｜Occurred: 2024-02-02｜Announced: 2024-02-04｜Reported: 2024-02-05｜Institution: fraud actors; Hong Kong Police investigation｜Version: NA｜Type: fraud_incident｜Disclosed technique: an employee was deceived by a pre-recorded deepfake video conference. The case reported a loss of HKD 200 million｜Disclosure level: incident_summary_without_tool_attribution｜Availability: criminal investigation and prevention guidance｜Official source: S_EXT_49A83CB0A82FCF79｜Independent source: S_EXT_632BD1BB7C1C71D1｜Limits of the impact claim: live video appearance is insufficient to authorize high-value payments. Out-of-band verification and multi-person approval should be used｜Confidence: medium_high [@src_s_ext_49a83cb0a82fcf79; @src_s_ext_632bd1bb7c1c71d1]
- ECO-E030｜Re-LAION-5B released with known problematic hashes removed｜Occurred: 2024-08-30｜Announced: 2024-08-30｜Reported: 2024-08-30｜Institution: LAION｜Version: Re-LAION-5B｜Type: remediation_release｜Disclosed technique: cooperation with IWF/C3P covered the 1,008 suspected links in the Stanford report｜Disclosure level: official_filtering_process｜Availability: available variants｜Official source: S_EXT_A71E52F7D74DB2C8｜Independent source: S_EXT_A322CC3B396554B6｜Limits of the impact claim: it is an auditable iteration rather than proof of zero risk. Continuous removal and rescanning are still required｜Confidence: medium_high [@src_s_ext_a71e52f7d74db2c8; @src_s_ext_a322cc3b396554b6]
- ECO-E021｜California AB 2013 signed by the governor｜Occurred: 2024-09-28｜Announced: 2024-09-28｜Reported: 2024-09-30｜Institution: State of California｜Version: AB 2013｜Type: policy_enactment｜Disclosed technique: it requires developers offering generative AI to the California public to disclose a high-level summary of their training data｜Disclosure level: full_bill_text｜Availability: enacted; disclosure deadline 2026-01-01｜Official source: S_EXT_5D3EE9B7C90010D4｜Independent source: none｜Limits of the impact claim: it raises documentation duties for data provenance, rights types, cleaning and the use of synthetic data. It is not per-sample disclosure｜Confidence: high [@src_s_ext_5d3ee9b7c90010d4]
- ECO-E010｜Movie Gen technical report made public｜Occurred: 2024-10-16｜Announced: 2024-10-16｜Reported: 2024-10-16｜Institution: Meta｜Version: Movie Gen｜Type: technical_release｜Disclosed technique: a 30B video model with up to 16 seconds, 16 fps and 1080p, synchronized audio, personalization and editing｜Disclosure level: technical_report_without_code_or_weights｜Availability: research demos; no public complete model｜Official source: S_EXT_629F563E4913E713｜Independent source: none｜Limits of the impact claim: it provides substantial technical disclosure, but the model cannot be run independently. Human-evaluation claims cannot be ranked against systems that use different protocols｜Confidence: high [@src_s_ext_629f563e4913e713]
- ECO-E013｜Stable Diffusion 3.5 series openly released｜Occurred: 2024-10-22｜Announced: 2024-10-22｜Reported: 2024-10-22｜Institution: Stability AI｜Version: SD3.5 Large/Large Turbo｜Type: open_release｜Disclosed technique: weights and inference code were made public. Turbo uses 4 steps. Medium was released later, on October 29｜Disclosure level: weights_code_license_and_selected_hardware｜Availability: downloadable weights and hosted APIs｜Official source: S_EXT_06D2114E2823654C｜Independent source: none｜Limits of the impact claim: open weights, but not unconditional open source. Vendor cross-model comparisons do not enter this survey's ranking｜Confidence: high [@src_s_ext_06d2114e2823654c]
- ECO-E001｜Sora Turbo released to subscription users｜Occurred: 2024-12-09｜Announced: 2024-12-09｜Reported: 2024-12-09｜Institution: OpenAI｜Version: Sora Turbo｜Type: product_release｜Disclosed technique: up to 1080p and up to 20 seconds, with text, image and video entry points. All generated videos contain C2PA and a visible watermark by default｜Disclosure level: product_features_and_limits_only｜Availability: web and app discontinued as of 2026-04-26; at release it was a Plus/Pro service｜Official source: S_EXT_2D134659D4DE90AF｜Independent source: none｜Limits of the impact claim: it turns a research preview into a consumer service. It does not prove that world models or physical understanding reach a general level｜Confidence: high [@src_s_ext_2d134659d4de90af]
- ECO-E023｜U.S. Copyright Office publishes Part 2 of the AI outputs copyrightability report｜Occurred: 2025-01-29｜Announced: 2025-01-29｜Reported: 2025-01-29｜Institution: U.S. Copyright Office｜Version: Copyright and AI Part 2｜Type: policy_report｜Disclosed technique: analyzes the copyrightability of generative AI outputs and human authorship contributions｜Disclosure level: official_report｜Availability: public report｜Official source: S_EXT_6584F6BB78A0E4BC｜Independent source: none｜Limits of the impact claim: it supports judging by human creative contribution rather than tool labels. It does not replace case-by-case court judgments｜Confidence: high [@src_s_ext_6584f6bb78a0e4bc]
- ECO-E009｜Adobe Firefly Video Model released in public beta｜Occurred: 2025-02-12｜Announced: 2025-02-12｜Reported: 2025-02-12｜Institution: Adobe｜Version: Firefly Video Model｜Type: product_release｜Disclosed technique: covers video generation and Creative Cloud workflows, which Adobe calls commercially safe｜Disclosure level: product_and_data_positioning｜Availability: closed service｜Official source: S_EXT_F1084EA3E8588058｜Independent source: none｜Limits of the impact claim: rights provenance becomes a differentiating product claim. Commercially safe does not equal a legal guarantee｜Confidence: medium_high [@src_s_ext_f1084ea3e8588058]
- ECO-E011｜Wan2.1 code and weights released｜Occurred: 2025-02-25｜Announced: 2025-02-25｜Reported: 2025-02-25｜Institution: Wan Team｜Version: Wan2.1｜Type: open_release｜Disclosed technique: multiple T2V/I2V configurations. The 1.3B README reports 5 seconds of 480p in about 4 minutes on an RTX 4090｜Disclosure level: code_weights_commands_and_hardware_example｜Availability: public repository and checkpoints｜Official source: S_EXT_839F5ACDC65FD3F7｜Independent source: none｜Limits of the impact claim: provides an executable open baseline. Vendor benchmarks and hardware examples still require independent re-testing｜Confidence: high [@src_s_ext_839f5acdc65fd3f7]
- ECO-E019｜China's Measures for Labeling AI-Generated Synthetic Content published｜Occurred: 2025-03-14｜Announced: 2025-03-14｜Reported: 2025-03-14｜Institution: Cyberspace Administration of China and three other departments｜Version: NA｜Type: policy_publication｜Disclosed technique: it specifies explicit and implicit labeling and the responsibilities of distribution platforms, and prohibits malicious removal or tampering｜Disclosure level: full_official_notice_and_linked_rule｜Availability: published; not yet effective on the publication date｜Official source: S_EXT_C03F6CC2696D90AA｜Independent source: none｜Limits of the impact claim: the governance target extends from the generation side to the distribution side and to user tampering behavior｜Confidence: high [@src_s_ext_c03f6cc2696d90aa]
- ECO-E004｜GPT-4o native image generation launched｜Occurred: 2025-03-25｜Announced: 2025-03-25｜Reported: 2025-03-25｜Institution: OpenAI｜Version: GPT-4o image generation｜Type: product_release｜Disclosed technique: native image generation and editing inside ChatGPT, with C2PA metadata added｜Disclosure level: product_release_plus_system_card｜Availability: closed service｜Official source: S_EXT_389AB448B1904BC0; S_EXT_5223FE491EE43B1C｜Independent source: none｜Limits of the impact claim: it brings text rendering, conversational editing and provenance marking into one product. Quality claims remain official evaluations｜Confidence: medium_high [@src_s_ext_389ab448b1904bc0; @src_s_ext_5223fe491ee43b1c]
- ECO-E024｜U.S. Copyright Office publishes the Part 3 pre-publication version of the generative AI training report｜Occurred: 2025-05-09｜Announced: 2025-05-09｜Reported: 2025-05-09｜Institution: U.S. Copyright Office｜Version: Copyright and AI Part 3 pre-publication｜Type: policy_report｜Disclosed technique: analyzes the legal and policy issues of using copyrighted material for training｜Disclosure level: official_prepublication_report｜Availability: public pre-publication report｜Official source: S_EXT_6584F6BB78A0E4BC｜Independent source: none｜Limits of the impact claim: provides policy context for licensing, exceptions and market effects. It cannot be reduced to training being categorically lawful or unlawful｜Confidence: high [@src_s_ext_6584f6bb78a0e4bc]
- ECO-E005｜Veo 3 and Flow released｜Occurred: 2025-05-20｜Announced: 2025-05-20｜Reported: 2025-05-20｜Institution: Google｜Version: Veo 3｜Type: product_release｜Disclosed technique: video generation with native audio. Flow provides a filmmaking workflow｜Disclosure level: product_features_only｜Availability: closed service via named Google products｜Official source: S_EXT_4914C72C16A5ED4B｜Independent source: none｜Limits of the impact claim: integrated audio-video enters mainstream closed-source video products. No weights or training data are disclosed｜Confidence: medium_high [@src_s_ext_4914c72c16a5ed4b]
- ECO-E014｜Seedance 1.0 technical report made public｜Occurred: 2025-06-11｜Announced: 2025-06-11｜Reported: 2025-06-11｜Institution: ByteDance Seed｜Version: Seedance 1.0｜Type: technical_release｜Disclosed technique: a public technical report on a text-to-video system｜Disclosure level: technical_report_without_weights｜Availability: paper/report public; service availability separate｜Official source: S_EXT_4BD7BA572ABD70D0｜Independent source: none｜Limits of the impact claim: technical disclosure increases, but the complete closed-source training and inference stack still cannot be reproduced｜Confidence: high [@src_s_ext_4bd7ba572abd70d0]
- ECO-E033｜Disney and Universal sue Midjourney｜Occurred: 2025-06-11｜Announced: 2025-06-11｜Reported: 2025-06-11｜Institution: Disney; Universal; Midjourney｜Version: Midjourney｜Type: copyright_litigation｜Disclosed technique: the plaintiffs allege that training and outputs infringe copyrights in protected characters and seek relief｜Disclosure level: complaint_and_docket_not_final_judgment｜Availability: case pending in retained evidence｜Official source: S_EXT_D69AA51C5B39E5EA｜Independent source: S_EXT_1BF67A1AE824BB99｜Limits of the impact claim: the event demonstrates enforcement pressure from rights holders. It must not be written as a court having found Midjourney liable for infringement｜Confidence: high [@src_s_ext_d69aa51c5b39e5ea; @src_s_ext_1bf67a1ae824bb99]
- ECO-E012｜Wan2.2 code and weights released｜Occurred: 2025-07-28｜Announced: 2025-07-28｜Reported: 2025-07-28｜Institution: Wan Team｜Version: Wan2.2｜Type: open_release｜Disclosed technique: MoE T2V-A14B and the highly compressed TI2V-5B. Output is 480p/720p, with a 24 fps option｜Disclosure level: code_weights_commands_hardware_requirements｜Availability: public repository and checkpoints｜Official source: S_EXT_4460F5DC2A2E30B2｜Independent source: none｜Limits of the impact claim: it extends the open video ecosystem. The T2V-A14B single-GPU example needs at least 80 GiB and cannot be broadly called consumer-grade｜Confidence: high [@src_s_ext_4460f5dc2a2e30b2]
- ECO-E020｜China's Measures for Labeling AI-Generated Synthetic Content take effect｜Occurred: 2025-09-01｜Announced: 2025-03-14｜Reported: 2025-09-01｜Institution: Cyberspace Administration of China and three other departments｜Version: NA｜Type: policy_effective｜Disclosed technique: the Measures and the mandatory national standard take effect together｜Disclosure level: full_official_notice_and_linked_standard｜Availability: effective｜Official source: S_EXT_C03F6CC2696D90AA｜Independent source: none｜Limits of the impact claim: deployment must implement explicit interface labeling and implicit labeling in file metadata. It must also record platform codes｜Confidence: high [@src_s_ext_c03f6cc2696d90aa]
- ECO-E002｜Sora 2 released alongside a social app launch｜Occurred: 2025-09-30｜Announced: 2025-09-30｜Reported: 2025-09-30｜Institution: OpenAI｜Version: Sora 2｜Type: product_release｜Disclosed technique: synchronized dialogue and sound effects, character likeness capture and multi-shot controllability. The vendor claims improved physical plausibility｜Disclosure level: product_and_safety_disclosure_without_weights｜Availability: invitation-only at release; as of 2026-04-26 the web and app are discontinued｜Official source: S_EXT_661A3476D3D51BB4｜Independent source: none｜Limits of the impact claim: it drives integrated audio-video and likeness licensing to become product features. Impact judgments are limited to the official release｜Confidence: medium_high [@src_s_ext_661a3476d3d51bb4]
- ECO-E006｜Veo 3.1 released｜Occurred: 2025-10-15｜Announced: 2025-10-15｜Reported: 2025-10-15｜Institution: Google｜Version: Veo 3.1｜Type: product_release｜Disclosed technique: it updates reference inputs, editing and audio-video generation capabilities｜Disclosure level: product_features_only｜Availability: closed service; endpoint-specific｜Official source: S_EXT_C59CBD3DCF25D3C8｜Independent source: none｜Limits of the impact claim: rapid version iteration increases the risk that static leaderboards become outdated｜Confidence: medium [@src_s_ext_c59cbd3dcf25d3c8]
- ECO-E034｜UK High Court Getty v Stability AI judgment issued｜Occurred: 2025-11-04｜Announced: 2025-11-04｜Reported: 2025-11-04｜Institution: Getty Images; Stability AI｜Version: Stable Diffusion｜Type: copyright_judgment｜Disclosed technique: the judgment addresses UK trademark claims and some copyright claims. Its conclusions are limited by the specific claims, the evidence and the jurisdiction｜Disclosure level: full_judgment｜Availability: judgment public｜Official source: S_EXT_365AB6D86CA889FB｜Independent source: S_EXT_48DA7C84540BA04D｜Limits of the impact claim: it should not be generalized into generative model training being fully lawful in the UK. Its impact is to expose the complexity of current law and of evidence about training location｜Confidence: high [@src_s_ext_365ab6d86ca889fb; @src_s_ext_48da7c84540ba04d]
- ECO-E008｜Runway Gen-4.5 released｜Occurred: 2025-12-01｜Announced: 2025-12-01｜Reported: 2025-12-01｜Institution: Runway｜Version: Gen-4.5｜Type: product_release｜Disclosed technique: a capability demo and research-page disclosure for a closed-source video model｜Disclosure level: demo_and_high_level_release｜Availability: closed service｜Official source: S_EXT_F9A73D8D68254059｜Independent source: none｜Limits of the impact claim: included as a product version event. There are no weights and no end-to-end reproducible evidence｜Confidence: medium [@src_s_ext_f9a73d8d68254059]
- ECO-E022｜AB 2013 training-data disclosure deadline arrives｜Occurred: 2026-01-01｜Announced: 2024-09-28｜Reported: 2026-01-01｜Institution: State of California｜Version: AB 2013｜Type: policy_effective｜Disclosed technique: systems made public since 2022-01-01 must have a statutory high-level data document published before launch or major updates｜Disclosure level: full_bill_text｜Availability: effective for covered systems｜Official source: S_EXT_5D3EE9B7C90010D4｜Independent source: none｜Limits of the impact claim: it establishes a minimum disclosure checklist for open and closed-source systems. Specific compliance requires legal review｜Confidence: high [@src_s_ext_5d3ee9b7c90010d4]
- ECO-E007｜Veo 3.1 adds vertical and high-definition output paths｜Occurred: 2026-01-13｜Announced: 2026-01-13｜Reported: 2026-01-13｜Institution: Google｜Version: Veo 3.1｜Type: product_update｜Disclosed technique: vertical, 1080p and 4K upscaling paths. SynthID continues to be used｜Disclosure level: product_features_and_provenance｜Availability: closed service; availability varies by product｜Official source: S_EXT_8DC07480AB081FA9｜Independent source: none｜Limits of the impact claim: output specifications must distinguish native generation from upscaling. SynthID covers only supported Google outputs｜Confidence: high [@src_s_ext_8dc07480ab081fa9]
- ECO-E017｜Kling AI 3.0 released｜Occurred: 2026-02-05｜Announced: 2026-02-05｜Reported: 2026-02-05｜Institution: Kuaishou｜Version: Kling AI 3.0｜Type: product_release｜Disclosed technique: Video 3.0/Omni and Image 3.0/Omni. Up to 15 seconds and native multilingual audio remain announcement claims｜Disclosure level: corporate_product_announcement｜Availability: closed service; quotas and regions require live check｜Official source: S_EXT_EFFDB7B60B6799E6｜Independent source: none｜Limits of the impact claim: the product enters a stage of unified multimodal creation. Weights and training data are not open｜Confidence: medium_high [@src_s_ext_effdb7b60b6799e6]
- ECO-E015｜Seedance 2.0 released｜Occurred: 2026-02-12｜Announced: 2026-02-12｜Reported: 2026-02-12｜Institution: ByteDance Seed｜Version: Seedance 2.0｜Type: product_release｜Disclosed technique: multimodal reference, editing, continuation and two-channel audio. The page discloses that detail stability and occasional audio distortion still need improvement｜Disclosure level: product_features_limitations_and_demo_evaluation｜Availability: closed service｜Official source: S_EXT_724CB3E9025C287D｜Independent source: none｜Limits of the impact claim: audio-video and editing are unified. Performance comparisons can only be labeled as vendor self-reports｜Confidence: high [@src_s_ext_724cb3e9025c287d]
- ECO-E016｜Seedance 2.0 triggers a Hollywood copyright dispute｜Occurred: 2026-02-15｜Announced: 2026-02-15｜Reported: 2026-02-15｜Institution: MPA members and ByteDance｜Version: Seedance 2.0｜Type: copyright_dispute｜Disclosed technique: rights holders criticize the demo for involving protected characters. This is a dispute and a claim｜Disclosure level: no_architecture_disclosure｜Availability: service remained subject to provider controls at report time｜Official source: none｜Independent source: S_EXT_41DCE625EBEC1211｜Limits of the impact claim: it demonstrates external copyright pressure rather than an adjudicated infringement. Subsequent proceedings and product controls need to be tracked｜Confidence: medium [@src_s_ext_41dce625ebec1211]
- ECO-E028｜C2PA 2.4 specification released｜Occurred: 2026-04-01｜Announced: 2026-04-01｜Reported: 2026-04-01｜Institution: C2PA｜Version: C2PA 2.4｜Type: standard_release｜Disclosed technique: it provides documents on Content Credentials, crJSON, attestations, soft binding and security considerations. The official disclosure gives only April 2026, so the date is coded as the first day of that month｜Disclosure level: full_public_specification_month_precision｜Availability: public specification and validators ecosystem｜Official source: S_EXT_E0FFA9F133CE7FC3; S_EXT_52D0DD2EF128F371｜Independent source: S_EXT_615FF3CB7E5BF360; S_EXT_9F2A5C76C58BB158｜Limits of the impact claim: it enhances the verifiability of the media provenance chain. The standard itself explicitly notes stripping and availability threats, and independent research further points to cross-layer contradictions｜Confidence: high [@src_s_ext_e0ffa9f133ce7fc3; @src_s_ext_52d0dd2ef128f371; @src_s_ext_615ff3cb7e5bf360; @src_s_ext_9f2a5c76c58bb158]
- ECO-E003｜Sora web and app services discontinued｜Occurred: 2026-04-26｜Announced: 2026-04-26｜Reported: 2026-07-30｜Institution: OpenAI｜Version: Sora/Sora 2｜Type: availability_change｜Disclosed technique: the web and app are discontinued. The help page states that the API is scheduled to shut down on 2026-09-24｜Disclosure level: availability_and_data_export_notice｜Availability: web and app unavailable; the API has a scheduled future sunset as of the cutoff｜Official source: S_EXT_4D8DC7C1BCF322D1｜Independent source: none｜Limits of the impact claim: it indicates that product availability and paper or model capability are independent dimensions. Future API dates are not recorded as completed｜Confidence: high [@src_s_ext_4d8dc7c1bcf322d1]
- ECO-E018｜Cosmos 3 released for open research｜Occurred: 2026-06-01｜Announced: 2026-06-01｜Reported: 2026-06-01｜Institution: NVIDIA｜Version: Cosmos 3｜Type: open_release｜Disclosed technique: it covers images, audio-video, reasoning, robot policies and dynamics uniformly. A technical report, model cards and code links are provided｜Disclosure level: technical_report_code_and_model_cards｜Availability: open model artifacts through official links｜Official source: S_EXT_0AA0FA2DE9F38B24｜Independent source: S_EXT_129B2C193F5B258E｜Limits of the impact claim: it shows generative models connecting with Physical AI. Vendor rank-1 summaries are not used as cross-protocol conclusions｜Confidence: high [@src_s_ext_0aa0fa2de9f38b24; @src_s_ext_129b2c193f5b258e]
- ECO-E025｜EU publishes a code of practice on marking and labeling AI-generated content｜Occurred: 2026-06-10｜Announced: 2026-06-10｜Reported: 2026-06-10｜Institution: European Commission｜Version: Article 50 Code of Practice｜Type: policy_guidance｜Disclosed technique: it publishes final voluntary guidelines that help providers and deployers meet Article 50｜Disclosure level: official_voluntary_code｜Availability: published; voluntary｜Official source: S_EXT_402803AE8DA90AB8｜Independent source: none｜Limits of the impact claim: it provides an implementation path but does not replace legal obligations｜Confidence: high [@src_s_ext_402803ae8da90ab8]
- ECO-E026｜EU publishes Article 50 transparency guidelines｜Occurred: 2026-07-20｜Announced: 2026-07-20｜Reported: 2026-07-20｜Institution: European Commission｜Version: Article 50 Guidelines｜Type: policy_guidance｜Disclosed technique: it explains disclosure obligations for interaction prompts, machine-readable marking, deepfakes and text on matters of public interest｜Disclosure level: official_guidelines｜Availability: published｜Official source: S_EXT_43DBE36DFBE6E288｜Independent source: none｜Limits of the impact claim: it reduces implementation ambiguity. The exact scope and exceptions still require checking against the guidelines and the legal text｜Confidence: high [@src_s_ext_43dbe36dfbe6e288]
- ECO-E027｜EU AI Act Article 50 transparency obligations begin to apply｜Occurred: 2026-08-02｜Announced: 2026-07-20｜Reported: 2026-08-02｜Institution: European Union｜Version: AI Act Article 50｜Type: policy_effective｜Disclosed technique: providers must make AI-generated or manipulated content machine-detectable. Deployers must disclose deepfakes and similar content in key scenarios｜Disclosure level: official_guidance_summary｜Availability: effective｜Official source: S_EXT_43DBE36DFBE6E288; S_EXT_402803AE8DA90AB8｜Independent source: none｜Limits of the impact claim: generation-side marking, deployment-side disclosure and platform display require end-to-end coordination. It is not equivalent to mandating adoption of a single technical standard｜Confidence: high [@src_s_ext_43dbe36dfbe6e288; @src_s_ext_402803ae8da90ab8]

## Chapter 19 Cross-Family Synthesis and Conditional Selection Guide

Input and question: the chapter covers unified fields across the six families, the conditional rules, and the system and risk boundaries.

Argumentative move: the task and its hard constraints come first. They decide which combination of mechanisms has to be audited. The next step lists the fields that must be measured and the conditions that would flip the recommendation.

**Unified comparison contract for methods. The table contains no cross-protocol numerical values or overall ranking.}

\begin{tabular}{lllll}

Family & State update & Training objective & Inference & Key failure \\

Explicit probability and latent variable/invertible flow & Latent samples or invertible mapping & ELBO or exact likelihood & Single-pass decoding or inverse transform & Posterior/invertible structure and perceptual mismatch \\
Adversarial implicit generation & Implicit mapping constrained by a discriminative game & Minimax or IPM objective & Single forward pass of the generator & Coverage, training stability, and conditional extension \\
Autoregressive and masked generation & Conditional updates over token/pixel/frame/scale & Conditional likelihood or masked reconstruction & Serial or parallel iteration & codec ceiling, serial depth, and accumulated error \\
Score and stochastic denoising & Noise or score state & Denoising, score, or equivalent parameterization & Reverse chain, SDE, or probability flow ODE & Sampling cost, guidance tradeoff, and codec distortion \\
Deterministic transport & Velocity field, flow map, or consistency mapping & Conditional flow, rectified path, or endpoint consistency & ODE integration or few-step mapping & Path, solver, and end-to-end gains uncertain \\
Hybrid and unified generation mechanisms & Two or more non-removable update operators & Joint component objectives and interfaces & Multi-stage or compositional updates & Error propagation, complexity, and attribution difficulty \\

\end{tabular}

\end{table**

|
Family | State update | Training objective | Inference | Key failure |
|---|---|---|---|---|
|
Explicit probability and latent variable/invertible flow | Latent samples or invertible mapping | ELBO or exact likelihood | Single-pass decoding or inverse transform | Posterior/invertible structure and perceptual mismatch |
| Adversarial implicit generation | Implicit mapping constrained by a discriminative game | Minimax or IPM objective | Single forward pass of the generator | Coverage, training stability, and conditional extension |
| Autoregressive and masked generation | Conditional updates over token/pixel/frame/scale | Conditional likelihood or masked reconstruction | Serial or parallel iteration | codec ceiling, serial depth, and accumulated error |
| Score and stochastic denoising | Noise or score state | Denoising, score, or equivalent parameterization | Reverse chain, SDE, or probability flow ODE | Sampling cost, guidance tradeoff, and codec distortion |
| Deterministic transport | Velocity field, flow map, or consistency mapping | Conditional flow, rectified path, or endpoint consistency | ODE integration or few-step mapping | Path, solver, and end-to-end gains uncertain |
| Hybrid and unified generation mechanisms | Two or more non-removable update operators | Joint component objectives and interfaces | Multi-stage or compositional updates | Error propagation, complexity, and attribution difficulty |
| | | | | |

The task decides the first audit target. If it demands explicit density or inversion, start with the semantic external validity of latent variable/invertible flow. If fixed-domain single-step latency is the priority, start with the coverage of GANs or adversarial distillation. For language unification and online causal state, audit the codec, serial processing, and caching of AR/token models. For open-ended text and mature control, audit the guidance, codec, and sampling of latent diffusion. If few steps are the priority, audit the true system frontier of flow/consistency.

Before selecting for video, fix the output duration and frame rate, the codec, update granularity, history maintenance, and the action/camera/audio conditions. These four are different tasks: short-window bidirectional consistency, causal online updating, long-horizon narrative planning, and action closed loops. Longer sequences alone cannot substitute for state querying and closed-loop success.

- SEL01｜The task requires explicit density, latent variable inference, or invertible inversion. Audit explicit-latent-flow first. The fields to measure are semantic_external_validity_of_likelihood, invertibility_cost, decoder_reconstruction
- SEL02｜The domain is fixed, and single-forward-pass latency is a hard constraint. Audit adversarial and hybrid first. The fields to measure are coverage, training_stability, tail_failures, end_to_end_latency
- SEL03｜The task requires language-unified sequences, discrete states, or online causal updating. Audit autoregressive-mask and hybrid first. The fields to measure are codec_reconstruction, serial_depth, kv_cache, error_accumulation
- SEL04｜The priority is open-ended text, multimodal futures, and mature conditional control. Audit stochastic-score-diffusion first. The fields to measure are guidance_tradeoff, sampling_cost, condition_conflict, codec_loss
- SEL05｜A few-step ODE/flow map is the main constraint. Audit deterministic-transport and hybrid first. The fields to measure are wall_clock, throughput, memory, energy, coverage, editability
- SEL06｜The video needs low-latency action input. Audit autoregressive-mask, deterministic-transport, and hybrid first. The fields to measure are causal_latency, state_drift, first_failure_time, closed_loop_success
- SEL07｜The task requires long-horizon narrative or world state. Audit hybrid, autoregressive-mask, and stochastic-score-diffusion first. The fields to measure are revisit_consistency, object_permanence, event_order, memory_cost, planning_success
- SEL08｜Only closed-source APIs can be accessed. Audit system-interface-only first. The fields to measure are documented_input_output, availability, terms, observed_failures; limits of the claim: it must not back-infer training data, architecture, or server efficiency. It must not rank against the mechanisms in the papers.

Five kinds of change flip the choice most easily. The tokenizer/codec is replaced. So is the text encoder or caption. So is the sampler, guidance, or update granularity. Single images/short clips give way to long-horizon, multi-subject, or action closed loops. NFE/FLOPs give way to end-to-end p50/p95, throughput, VRAM, energy, licensing, and failure rate. Currently, no unified run produces empirical weights or a global Pareto.

![F13. Conditional selection. Six defining questions on the left determine the primary generation family. The right side maps concrete task conditions to the families that should be checked first and to the required measurement vectors. Any change to the codec, data, sampler, hardware, or risk constraints can flip the conclusion. Evidence boundary: the decision tree gives candidates and the required measurement fields. It does not guarantee model quality. Neither the second-coder sensitivity analysis nor the same-protocol Pareto has been completed.](../figures/F13_conditional_decision_tree.png)

*F13. Conditional selection. Six defining questions on the left determine the primary generation family. The right side maps concrete task conditions to the families that should be checked first and to the required measurement vectors. Any change to the codec, data, sampler, hardware, or risk constraints can flip the conclusion. Evidence boundary: the decision tree gives candidates and the required measurement fields. It does not guarantee model quality. Neither the second-coder sensitivity analysis nor the same-protocol Pareto has been completed.*

Overall judgment: conditional selection returns "what to audit first, what must be measured, and when it flips." It does not return a total score, a leaderboard, or an unconditional winner.

Evidence boundary: R001/R002 verify local formulas and synthetic feature protocols only. They cannot endorse any family or product.

Transition: Chapter 20 recasts the unresolved flip conditions as a falsifiable research agenda.

## Chapter 20 Future Trends and a Falsifiable Research Agenda

Input and question: the chapter starts from earlier failures, blank cells in tables, system boundaries, risk-control gaps, and the six frozen future agenda items.

Argument move: run each trend through "observation---gap---falsifiable question---minimum verification---falsification condition." Reject wish lists.

**Falsifiable questions and falsification conditions for future trends.}

\begin{tabular}{llll}

Section & Topic & Falsifiable question & Falsification condition \\

20.1 & Native multimodal understanding--generation unification & Under the same data, same parameters, same tokenizer, and same training budget, can a shared model on understanding, image generation, video generation, and editing reach specialized model… & The advantage comes only from a larger budget, or the shared model consistently degrades on any core task. \\
20.2 & Long-horizon state, editable memory, and identity/scene persistence & Under the same generator, data, token, and compute budget, does explicit object/scene memory continuously reduce revisit errors compared with an equally long pixel context and allow local… & Explicit memory is no better than an equal-budget pixel context, or edit propagation systematically corrupts unedited state. \\
20.3 & Physics, causality, 3D/4D, and interactive closed loops & Do explicit 3D/physical states, in out-of-distribution actions and long-horizon closed loops, significantly improve predictability and task success over pixel history while not excessively sacrificing rendering quality and… & Improvements appear only in in-training visual metrics, and physical residuals or closed-loop planning do not improve. \\
20.4 & Data bottlenecks, synthetic feedback, and the licensed-data economy & With fixed independent sources and compute, how do licensed human descriptions, machine re-descriptions, and multi-round synthetic mixtures each change semantics, tail coverage, memory, bias, and rights auditability… & Gains hold only for evaluators from the same source as the generative teacher, or tail coverage continues to shrink as the number of synthetic rounds increases. \\
20.5 & Real-time on-device/edge generation, sparse experts, and codec co-design & Can sparse/expert routing, few-step sampling, and codec co-design improve the end-to-end Pareto frontier at fixed quality, rather than shifting the bottleneck or distortion to other components? & NFE or FLOPs decrease but the end-to-end frontier does not move, or the tail failure rate increases significantly. \\
20.6 & Verifiable provenance, behavior audit, and governance infrastructure & Can a combination of signed credentials, robust watermarking, service logs, and platform policies increase the provenance verifiability rate under known/unknown transformations, malicious stripping, and multi-platform forwarding, while control… & The combined control is no better than the strongest single layer, or absence is still systematically misjudged as non-AI. \\

\end{tabular}

\end{table**

|
Section | Topic | Falsifiable question | Falsification condition |
|---|---|---|---|
|
20.1 | Native multimodal understanding--generation unification | Under the same data, same parameters, same tokenizer, and same training budget, can a shared model on understanding, image generation, video generation, and editing reach specialized model… | The advantage comes only from a larger budget, or the shared model consistently degrades on any core task. |
| 20.2 | Long-horizon state, editable memory, and identity/scene persistence | Under the same generator, data, token, and compute budget, does explicit object/scene memory continuously reduce revisit errors compared with an equally long pixel context and allow local… | Explicit memory is no better than an equal-budget pixel context, or edit propagation systematically corrupts unedited state. |
| 20.3 | Physics, causality, 3D/4D, and interactive closed loops | Do explicit 3D/physical states, in out-of-distribution actions and long-horizon closed loops, significantly improve predictability and task success over pixel history while not excessively sacrificing rendering quality and… | Improvements appear only in in-training visual metrics, and physical residuals or closed-loop planning do not improve. |
| 20.4 | Data bottlenecks, synthetic feedback, and the licensed-data economy | With fixed independent sources and compute, how do licensed human descriptions, machine re-descriptions, and multi-round synthetic mixtures each change semantics, tail coverage, memory, bias, and rights auditability… | Gains hold only for evaluators from the same source as the generative teacher, or tail coverage continues to shrink as the number of synthetic rounds increases. |
| 20.5 | Real-time on-device/edge generation, sparse experts, and codec co-design | Can sparse/expert routing, few-step sampling, and codec co-design improve the end-to-end Pareto frontier at fixed quality, rather than shifting the bottleneck or distortion to other components? | NFE or FLOPs decrease but the end-to-end frontier does not move, or the tail failure rate increases significantly. |
| 20.6 | Verifiable provenance, behavior audit, and governance infrastructure | Can a combination of signed credentials, robust watermarking, service logs, and platform policies increase the provenance verifiability rate under known/unknown transformations, malicious stripping, and multi-platform forwarding, while control… | The combined control is no better than the strongest single layer, or absence is still systematically misjudged as non-AI. |
| | | | |

- 20.1 Native multimodal understanding--generation unification. Observation: current work shares a Transformer backbone, visual tokens or conditioning representations — one piece per system — and then tries to connect continuous and discrete updates. Images and video increasingly share the codec, DiT and conditioning modules. Gap: public results usually change data, parameters, tokenizer and budget together. The gains therefore cannot be attributed to unification alone, and negative transfer still lacks same-contract reporting. Falsifiable question: under one shared data set, parameter set, tokenizer and training budget, can a model reach the conditionalized Pareto of specialized models on understanding, image generation, video generation and editing? Can it do so without amplifying tail failures on any core task? Minimum verification: build shared and modular groups of equal-budget models, and fix the deduplicated data and prompt set. Use at least three random seeds, stratify per task and by language/group, and record cross-task gradient conflicts and failure samples. Metrics: per-task quality vector, tail failure rate, gradient conflict, parameter and training cost, and p50/p95 inference cost. Falsification condition: a larger budget alone explains the advantage, or the shared model consistently degrades on any core task. [@transitionmatching; @januspro; @flowinone; @video_2312_14125]
- 20.2 Long-horizon state, editable memory, and identity/scene persistence. Observation: sliding windows, fixed queues, first-frame anchors, KV caches, storyboards and retrieval memory can all extend the generation process. Forgetting outside the window, cumulative drift and revisit inconsistency still occur. Gap: most long or infinite videos report only the runnable duration or selected examples. They lack object permanence, event order, identity-state queries and time to first failure. Falsifiable question: hold the generator, data, token and compute budget fixed. Does explicit object/scene memory then reduce revisit errors continuously, against an equally long pixel context? Can state propagate correctly after local edits? Minimum verification: construct 1-, 5-, 15- and 30-minute scripts with off-screen revisits, occlusion, attribute modification, cross-shot identity and counterfactual editing. Compare sliding windows, keyframes, retrieval and explicit state. Repeat seeds and report survival curves. Metrics: event-level accuracy, object permanence, identity consistency, time to first failure, memory, latency and edit propagation error. Falsification condition: explicit memory only matches an equal-budget pixel context, or edit propagation systematically corrupts unedited state. [@video_2310_15169; @video_2405_11473; @video_2403_14773; @video_2504_13074; @video_2507_18634; @video_2412_03568; @video_2505_21996]
- 20.3 Physics, causality, 3D/4D, and interactive closed loops. Observation: action-conditioned video, simulator trajectories, physical constraints and interactive systems are growing in number. Yet the evidence spans a wide gap from visual prediction to closed-loop planning and stays mostly inside specific robots, games or material contracts. Gap: visual realism, physical conservation, counterfactual causality, 3D/4D state consistency and policy success are often conflated as world understanding. Falsifiable question: in out-of-distribution actions and long-horizon closed loops, do explicit 3D/physical states significantly improve predictability and task success over pixel history? Can they do so without excessively sacrificing rendering quality and real-time performance? Minimum verification: train pixel, geometric/physical state and hybrid groups on the same scenes. Test collisions, occlusion, object permanence, camera loop closure, counterfactual actions and downstream planning, then move to new out-of-distribution scenes and independent policies. Metrics: visual quality, physical conservation residual, state query accuracy, calibration, closed-loop task success and latency. Falsification condition: improvement appears only in in-training visual metrics, while physical residuals and closed-loop planning stay flat. [@video_2309_17080; @video_2310_06114; @video_2402_15391; @video_2408_14837; @video_2405_15223; @video_2509_20358; @video_system_58; @video_system_59; @video_system_60]
- 20.4 Data bottlenecks, synthetic feedback, and the licensed-data economy. Observation: web image-text/video indexes, automatic captions, re-description, filtering and synthetic intermediate states have become key variables. URL drift, derived-clip dependency, teacher errors, non-transfer of rights and deletion propagation remain problems. Gap: scale is often reported as the number of pairs or clips. Missing are the independent-source denominator, the rights status, the teacher version, and the error and diversity trajectories of multi-round synthetic feedback. Falsifiable question: freeze the independent sources and the compute. How do licensed human descriptions, machine re-descriptions and multi-round synthetic mixtures each change semantics, tail coverage, memory, bias and rights auditability? Minimum verification: freeze the same original media set, then stratify the caption source and the number of synthetic rounds. Train at equal budget on at least three model classes, AR, diffusion and flow. Keep per-sample rights, teacher version and deletion propagation records, plus near-duplicate ledgers. Metrics: number of independent sources, near-duplicate rate, stratified semantic/compositional metrics, tail coverage, teacher error transmission, and deletion propagation success rate. Falsification condition: the gains hold only for evaluators drawn from the same source as the generative teacher. A second failure is tail coverage that keeps shrinking as the number of synthetic rounds increases.
- 20.5 Real-time on-device/edge generation, sparse experts, and codec co-design. Observation: few-step sampling, caching, quantization, attention kernels, multi-GPU parallelism and high-compression codecs each reduce a different local cost. Open weights make measurement possible, but NFE, kernel speedup factors and README times are not the same end-to-end metric. Gap: evidence on quality--latency--memory--energy is lacking under one checkpoint, hardware, compilation and service load. The missing work covers text encoding, the generator, the codec, transmission, cold start, queues and retries. Falsifiable question: at fixed quality, can sparse/expert routing, few-step sampling and codec co-design improve the end-to-end Pareto frontier? Or do they shift the bottleneck or the distortion onto other components? Minimum verification: freeze the open checkpoint, license-available version, input set and hardware. Run single-factor ablations first, then combined ones. Record warmup, cold start, concurrency, failed retries and the full trace. Repeat at least three times. Metrics: p50/p95 latency, throughput, peak GPU/main memory, energy, failure rate, codec reconstruction, and motion/text/identity stratified quality. Falsification condition: NFE or FLOPs fall without moving the end-to-end frontier, or the tail failure rate rises significantly.
- 20.6 Verifiable provenance, behavior audit, and governance infrastructure. Observation: data documentation, output labels, C2PA credentials, invisible watermarking, platform recognition, deletion and correction form a multi-layer chain of responsibility. The 34 incidents and 21 risk-control records show that every layer has stripping, misjudgment or enforcement gaps. Gap: a specification may exist, a vendor may enable it, or one robustness demonstration may pass. None of that equals the real retention rate across editors, platforms and re-encoding chains. Nor can any of it determine whether content is true or false, or whether training is lawful. Falsifiable question: under known/unknown transformations, malicious stripping and multi-platform forwarding, can signed credentials, robust watermarking, service logs and platform policies together lift the provenance verifiability rate? Can they do so while false positives and privacy costs stay controlled? Minimum verification: freeze the generator version and the original manifest. Perform cropping, scaling, transcoding, frame-rate change, splicing, screenshots, regeneration and adversarial stripping. Verify the valid/invalid/missing three states across at least two editors and platforms, and add appeal-recovery tests. Metrics: credential retention rate, watermark detection rate, false positives, chain integrity rate, verification latency, privacy leakage and appeal recovery rate. Falsification condition: the combined control at best equals the strongest single layer, or absence is still systematically misjudged as non-AI.

![F14. The evidence chain of the six future agenda items. Each row starts from an observed phenomenon and an open gap and converts them into a falsifiable question, a minimum verification design, and an explicit falsification criterion; these are research recommendations, not confirmed trends. Evidence boundary: the claim_](../figures/F14_future_agenda_falsification.png)

*F14. The evidence chain of the six future agenda items. Each row starts from an observed phenomenon and an open gap and converts them into a falsifiable question, a minimum verification design, and an explicit falsification criterion; these are research recommendations, not confirmed trends. Evidence boundary: the claim_*

Overall judgment: the priority agenda centers on six themes — multimodal negative transfer, long-term memory, physics/closed loops, data feedback, end-to-end real-time performance and cross-platform provenance attestation.

Evidence boundary: these are test plans derived from current evidence gaps. They are not predictions of model, market or policy outcomes after 2026.

Transition: for RQ1—RQ12, Chapter 21 tightens the answers through methodological, evidential, statistical, engineering and timeliness limitations.

## Chapter 21 Limitations and Conclusion

Input and question: RQ1–RQ12, post-correction corpus counts, per-chapter stability judgments, and unclosed gaps.

Argumentative move: answer the research questions one by one. The methodological, evidential, statistical, engineering, closed-source and temporal-validity boundaries are written into the conclusion.

Methodological limitations: 1332 records were screened. Only 132 structured cards exist, and the complete full-text exclusion flow for candidates is not yet closed. The post-generation audit completed a structural scan of 132/132 cards. Only 124 of those cards have local full text, and 8 cannot be locally verified. The second coding is still a purposive sample of 24 cards and 32 edges, so the overall agreement rate cannot be estimated. [@src_s_audit_post_generation_20260809]

Taxonomy limitations: after correction there are 231 canonical edges in total. Their roles are core 74, branch 32, bridge 21, system 3 and counterexample 2. The full-card scan found critical 1, high 19 and medium 4. Critical and high findings are revised atomically by the central overlay, and they propagate only after the corrected family counts and figure contracts are synchronized. This corrected key boundaries such as ConsisID, DDIM, Janus-Pro, VACE and The Matrix. The central overlay, however, does not amount to a blinded independent re-coding of all 132 cards by the second coder. It must not be used to claim that the entire taxonomy is stable. [@src_s_audit_post_generation_20260809; @video_2411_17440; @ddim; @januspro; @video_2503_07598; @video_2412_03568]

Statistical and engineering limitations: data, prompts, resolution, duration, sampling, guidance, evaluators and hardware are heterogeneous. This survey therefore declines cross-protocol ranking and meta-analysis. R001 is a formula contract, and R002 is a synthetic feature-metric/protocol smoke test. The C2PA round trip was blocked by a tool gate. Public-weight generation, training, paper metrics and serving benchmarks were not executed. [@src_s_exp_r001_formula_metrics; @src_s_exp_r002_metric_fixture; @src_s_exp_r002_c2pa_availability]

Temporal-validity and extrapolation limitations: 2025–2026 preprints, weights, licenses and product availability will drift. Closed-source systems support only interface-level conclusions, and governance evidence is constrained by jurisdiction and enforcement environment. The event table is a snapshot as of 2026-08-09. It does not estimate incidence rates or future trajectories.

- RQ1: Technical turning points come from reorganizing representation, state update, conditioning and temporal interfaces. The 231 canonical edges are the currently auditable lineage, not a causal network.
- RQ2: Six families describe the 132 cards isomorphically. Their card counts: explicit probability and latent variable/invertible flow (5 cards), adversarial implicit generation (13 cards), autoregressive and masked generation (18 cards), score-based and stochastic denoising (60 cards), deterministic transport (23 cards) and hybrid and unified generative mechanisms (13 cards).
- RQ3: Pixels, continuous latents, discrete tokens and spatiotemporal codecs set the information and compute ceilings. They must be reported together with reconstruction and system costs.
- RQ4: Control, editing, personalization, reference images and text resolve local bottlenecks. They do so through adapters, attention, inversion, fine-tuning or additional conditioning.
- RQ5: Representation capacity, update granularity, history mechanisms and action/temporal supervision jointly determine video bottlenecks. Single-frame image quality alone is not sufficient.
- RQ6: Unification requires at least shared parameters, representations or objectives. Image pretraining, or a same-brand API, does not amount to mechanistic unification or to the absence of negative transfer.
- RQ7: Subsets are comparable only when they agree on task, data, output, sampling, evaluation, hardware and statistical contracts.
- RQ8: Few-step methods, caching, quantization, parallelism and codecs change different local costs. When an end-to-end trace is missing, no serving benefits are claimed.
- RQ9: After correction, the counts are core 74 and branch 32. The A–E rules and verifiable successors determine roles, not popularity.
- RQ10: Data lineage, output credentials, watermarks, platform transmission, organizational controls and appeal-based correction must be combined. No single layer can prove authenticity, legality or safety.
- RQ11: The 34 events support time-bounded ecosystem explanations. They do not support conclusions about adoption rates, impact rates or algorithmic performance.
- RQ12: Mechanisms can be selected only under task, control, time, resource, openness, license and risk constraints. An unconditional winner is formally rejected.

Conclusion: the main line of visual generation is not the rotation of model names. It is how representations compress visual state and how update operators transport priors to data. It is also how conditioning is attached and how video maintains time and action. Images and video increasingly share codecs, Transformers and transport objectives. Yet data, time, physics, interaction, system and governance failures still require different evidence.

This draft delivers a provenance-traceable taxonomy synthesis with sampling-based error correction and a falsifiable agenda. Candidate screening/exclusion must still be closed, core samples further double-coded, claim-level localization filled in, and comparable subsets or unified runs completed. Only then can compiled-draft be marked by subsequent validation, and only when actual rendering, compilation and visual gates pass. Even if compilation passes, this draft remains publication-blocked and cannot claim to be submission-ready, because screening, double-coding, model runs and venue remain unclosed.

Overall judgment: interfaces and failure conditions are the most stable basis for selection at present, not a single metric, model popularity or product demos.

Evidence boundary: conclusion strength is capped at cross-study qualitative synthesis, post-correction descriptive counts and the executed R1/synthetic-fixture contract. It does not include model performance reproduction.

Transition: the main text ends here. The report directory holds the detailed paper cards, the event timeline, the reproduction boundaries and the generation receipts.
---

# Appendix — Post-cutoff update (2026-08-07 ~ 2026-08-09 → 2026-09-26)

The two background surveys close at 2026-08-09 (visual generative models) and 2026-08-07
(3D spatial construction). This appendix registers later material; **the body text is unchanged.**

## A.1 — Movement on the visual-generative-models agenda

Chapter 20 freezes six agenda items (natively multimodal unification; long-horizon state and editable
memory; physics, causality and 3D/4D loops; the data bottleneck and a licensed-data economy;
out-of-distribution evaluation; provenance and rights governance). Two **industry-side** moves appeared
after the cutoff, both on the sixth item:

- **Sony and Reuters** demonstrated a near-live newsroom authenticity workflow that embeds provenance
  signing in the editorial process rather than verifying after broadcast;
- **AFP and Dalet** announced a news-video provenance and authenticity collaboration.

This is not paper evidence, but it moves provenance and rights governance from proposal to
**production workflow** — the first observable landing of the sixth agenda item.

## A.2 — A note on the 3D spatial construction agenda

The body notes that newer LaTeX packages in 3D spatial construction have no local full-text PDF or
page-level locator. Nothing found since the cutoff changes that judgement; it stays an **open item**
rather than an update.

## A.3 — Institutional consequence of the OpenAI–Hugging Face incident (cross-repository note)

On 2026-09-16 / 17 OpenAI published an account of the incident and announced a safety-incident disclosure
process; reporting describes roughly 700 agents, dozens of third-party systems and 53 leaked user images;
the US Senate opened an investigation (led by Hawley).

For these two background surveys the point is that it is **not a paper but an event that has already
happened**: security problems of generative and agentic systems have moved from research topic to
regulatory object. It also vindicates ch. 20's methodological premise that future trends cannot be
extrapolated from paper counts or news density.

## A.4 — How to use this appendix

The lineage, method families and evaluation boundaries are unaffected. What changes is the **verification
progress of the agenda**: at least one of the six items has reached production. When citing the items
above, cite both the body cutoff and this appendix's date.