## Chapter 4 Technical Lineage: From Single-Frame Distributions to the Spatiotemporal World

Inputs and problem: the 132 cards that remain after error correction, the 231 normative lineage edges, and full-text localization of the image and video branches.

Argumentative move: periodize by technical interface rather than by year-based rankings, and distinguish direct inheritance, parallel development, combination and replacement.

Before 2014 and 2014–2017: generative contracts coexist. VAE turns an intractable posterior into the ELBO and reparameterization. GAN learns an implicit mapping through a discriminative game. PixelRNN/PixelCNN retains normalized pixel likelihoods, and RealNVP/Glow develops invertible density. VQ-VAE turns vision into discrete codes. Video GAN in turn begins to split static appearance from motion. The stage produces no single winner but four reusable contracts: latent-sample, adversarial-map, next-token and invertible-map. [@vae; @gan; @pixelrnn; @realnvp; @vqvae; @video_1609_02612; @video_1707_04993]

2018–2020: representation, scale, and score denoising. Progressive GAN turns a resolution curriculum into a component. BigGAN does the same for scale-conditional training, and StyleGAN for an editable latent space. A VQ hierarchy and a perceptual tokenizer shorten generation sequences. NCSN and DDPM rebuild multi-noise-level score/denoising regression into a stochastic generation backbone. On the video side, spatial/temporal dual discrimination still separates frame quality from dynamics. [@pggan; @biggan; @stylegan; @vqgan; @scoresde; @ddpm; @video_1907_06571]

2021–2022: text–vision, latent space, and video diffusion. DALL·E jointly models text and image tokens. Imagen strengthens text encoding and cascading. LDM moves denoising into a perceptual latent and wires in conditioning through cross-attention. DDIM, Score-SDE, EDM and CFG separate training parameterization, sampling path and conditioning strength. VideoGPT, Phenaki, Video Diffusion and Video LDM in turn bring discrete sequences, temporal layers and spatiotemporal codec into video. [@dalle; @ldm; @ddim; @cfg; @video_2104_10157; @video_2210_02399; @video_2204_03458; @video_2304_08818]

2023–2024: DiT, flow, few-step, and the control ecosystem. DiT hands latent patches to a Transformer. Flow Matching and Rectified Flow regress a vector field or a path, while Consistency, LCM and adversarial distillation compress sampling. ControlNet, IP-Adapter, DreamBooth and others share control-interface extensions. On the video side, DiT, 3D causal VAE, long windows and action conditioning diverge rapidly. A second encoding confirms CogVideoX as a v-pred diffusion DiT. It must not be changed to flow merely because it uses ODE-related terminology or a DiT backbone. [@dit; @flowmatching; @rectifiedflow; @consistency; @controlnet; @dreambooth; @video_2401_03048; @video_2408_06072]

2025–2026: few-step, long-horizon, native multimodality, and open—closed divergence. New work attempts one-step average velocity, flow maps over arbitrary intervals, unification of continuous and discrete transitions, causal few-step video, retrieval memory, interactive worlds and joint audio–video. Version drift, insufficient independent successors and closed-source evidence still limit these efforts. This survey therefore records only verifiable mechanisms and falsification conditions. It does not declare that the routes have already been unified, nor that a general world model has been reached. [@meanflow; @alignflow; @transitionmatching; @januspro; @video_2505_07344; @video_2412_03568; @video_system_60]

![F02. Visual generation technical lineage. The figure fully includes 132 work nodes and 231 edges retained by second-pass encoding. Colors denote the generation-update family of each work, and circles/diamonds denote image/video, respectively. Evidence boundary: edges denote verifiable mechanism relations, not citation counts, influence, or performance rankings. Medium- and low-confidence edges still require precise citation review.](../../figures/en/F02_lineage_dag.png)

*F02. Visual generation technical lineage. The figure fully includes 132 work nodes and 231 edges retained by second-pass encoding. Colors denote the generation-update family of each work, and circles/diamonds denote image/video, respectively. Evidence boundary: edges denote verifiable mechanism relations, not citation counts, influence, or performance rankings. Medium- and low-confidence edges still require precise citation review.*

Overall judgment: the normative lineage contains 231 edges. Its turning points come from the reorganization of representation, state update, conditioning and temporal interfaces, not from linear replacement by model names.

Evidence boundary: lineage edges are technical relations in the current encoding, not a causal attribution network. The sampling-based error correction covers only 32 edges, and unsampled relations still require localization and re-checking.

Transition: Chapter 5 explains how historical material is stably encoded into six families, and how boundary cases are handled.

---

[← Back to contents](index.md)
