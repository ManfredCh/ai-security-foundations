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

---

[← Back to contents](index.md)
