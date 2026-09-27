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

![F06. Migrating image architectures to video requires adding spatiotemporal representations, temporal interaction, frame/camera/action conditioning and out-of-window state maintenance. The final update operator still determines the primary family. Evidence boundary: the aggregate counts are non-mutually-exclusive labels. This figure describes architectural change and does not claim that an image model is necessarily the direct ancestor of every video model.](../../figures/en/F06_image_to_video_migration.png)

*F06. Migrating image architectures to video requires adding spatiotemporal representations, temporal interaction, frame/camera/action conditioning and out-of-window state maintenance. The final update operator still determines the primary family. Evidence boundary: the aggregate counts are non-mutually-exclusive labels. This figure describes architectural change and does not claim that an image model is necessarily the direct ancestor of every video model.*

Synthesis: the four prior questions for video are whether the codec preserves motion, what each update changes, how history is maintained, and whether the conditioning includes action/camera/audio.

Evidence boundary: the video cards are still positioned mainly at the chapter level, and error correction sampled only 12 cards. Public demonstrations cannot prove long-range, physical or planning capabilities.

Transition: Chapter 15 audits image and video data, metrics and protocol comparability.

---

[← Back to contents](index.md)
