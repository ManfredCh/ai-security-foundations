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

---

[← Back to contents](index.md)
