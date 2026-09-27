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

---

[← Back to contents](index.md)
