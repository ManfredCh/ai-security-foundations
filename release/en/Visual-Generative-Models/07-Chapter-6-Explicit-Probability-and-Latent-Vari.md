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

![F05. Training–inference contracts compared across VAE/invertible flows, GAN/WGAN, pixel or discrete token autoregression, DDPM/score-SDE, and Flow Matching/Rectified Flow/Consistency. Evidence boundary: formulas and mechanisms come from first-hand full-text paper cards. Complexity and failures serve only as a qualitative contract, and they do not support global rankings of quality, speed, or resources.](../../figures/en/F05_train_inference_comparison.png)

*F05. Training–inference contracts compared across VAE/invertible flows, GAN/WGAN, pixel or discrete token autoregression, DDPM/score-SDE, and Flow Matching/Rectified Flow/Consistency. Evidence boundary: formulas and mechanisms come from first-hand full-text paper cards. Complexity and failures serve only as a qualitative contract, and they do not support global rankings of quality, speed, or resources.*

Synthesis: the explicit latent-variable route ultimately becomes shared representation infrastructure for AR, diffusion, and video systems. Choosing it requires checking inference needs, reconstruction ceilings, and structural costs at the same time.

Evidence boundary: this chapter does not compare reconstruction scores or likelihoods across protocols. It also does not infer perceptual or semantic quality from properties of the formulas.

Transition: Chapter 7 turns to the route that abandons explicit density and learns a single-step mapping through a discriminative game.

---

[← Back to contents](index.md)
