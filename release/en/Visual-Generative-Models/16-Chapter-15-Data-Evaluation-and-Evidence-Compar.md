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

![F08. Correspondence among data, metrics and capabilities. Each metric is assigned to one capability column by its main measurement object. Reference-set dependence and known blind spots are also shown. The grouping does not expand one metric into general validity. Evidence boundary: capability grouping is for navigation only. Metrics with the same name cannot be compared directly when the extractor, preprocessing, sample count, prompt, aggregation and seed differ.](../../figures/en/F08_data_metric_capability_matrix.png)

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

---

[← Back to contents](index.md)
