## Chapter 19 Cross-Family Synthesis and Conditional Selection Guide

Input and question: the chapter covers unified fields across the six families, the conditional rules, and the system and risk boundaries.

Argumentative move: the task and its hard constraints come first. They decide which combination of mechanisms has to be audited. The next step lists the fields that must be measured and the conditions that would flip the recommendation.

**Unified comparison contract for methods. The table contains no cross-protocol numerical values or overall ranking.**

Family | State update | Training objective | Inference | Key failure |
|---|---|---|---|---|
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

![F13. Conditional selection. Six defining questions on the left determine the primary generation family. The right side maps concrete task conditions to the families that should be checked first and to the required measurement vectors. Any change to the codec, data, sampler, hardware, or risk constraints can flip the conclusion. Evidence boundary: the decision tree gives candidates and the required measurement fields. It does not guarantee model quality. Neither the second-coder sensitivity analysis nor the same-protocol Pareto has been completed.](../../figures/en/F13_conditional_decision_tree.png)

*F13. Conditional selection. Six defining questions on the left determine the primary generation family. The right side maps concrete task conditions to the families that should be checked first and to the required measurement vectors. Any change to the codec, data, sampler, hardware, or risk constraints can flip the conclusion. Evidence boundary: the decision tree gives candidates and the required measurement fields. It does not guarantee model quality. Neither the second-coder sensitivity analysis nor the same-protocol Pareto has been completed.*

Overall judgment: conditional selection returns "what to audit first, what must be measured, and when it flips." It does not return a total score, a leaderboard, or an unconditional winner.

Evidence boundary: R001/R002 verify local formulas and synthetic feature protocols only. They cannot endorse any family or product.

Transition: Chapter 20 recasts the unresolved flip conditions as a falsifiable research agenda.

---

[← Back to contents](index.md)
