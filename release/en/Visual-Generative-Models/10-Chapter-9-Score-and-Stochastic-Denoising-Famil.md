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

---

[← Back to contents](index.md)
