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

![F04. Unified generation interface. Different families may share a codec, a Transformer backbone or a conditioning module, yet the final generative update, the success criterion, the budget and the failure must be reported separately. Evidence boundary: this figure is a field-level unified interface. It does not mean that all systems disclose every field, and missing reports should be encoded as NR/UV.](../../figures/en/F04_unified_interface.png)

*F04. Unified generation interface. Different families may share a codec, a Transformer backbone or a conditioning module, yet the final generative update, the success criterion, the budget and the failure must be reported separately. Evidence boundary: this figure is a field-level unified interface. It does not mean that all systems disclose every field, and missing reports should be encoded as NR/UV.*

Overall judgment: what is truly comparable is the unified interface and its failure propagation, not architecture labels. Representation, update, conditioning, decoding and evaluation must be attributed separately.

Evidence boundary: passing the formula contract proves only the measured algebraic or numerical implementation. It does not prove network training, generation quality, efficiency or reproduction of the paper's metrics.

Transition: Chapter 4 follows the time axis and examines how these interfaces are established, transferred and reorganized.

---

[← Back to contents](index.md)
