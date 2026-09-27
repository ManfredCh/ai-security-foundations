## Chapter 12 Hybrid and Unified Generation Mechanisms

Input and question: hybrid family cards, component interfaces, and native multimodal work.

Argumentative move: judge hybrid status with the test "whether removing a component changes the final generation contract". Record shared parameters, representations, objectives, and state separately.

A VQGAN system can consist of an adversarial tokenizer and an AR prior. Even so, if the next token independently decides the final token generation, its primary class is still AR. MAR uses an AR conditional decomposition plus a continuous diffusion head, ADD relies on a diffusion teacher and an adversarial student, and GAIA-1 uses AR world tokens plus a diffusion renderer. Only when these components are jointly non-removable does the system encode hybrid. [@vqgan; @add; @sana; @video_2309_17080]

Discrete diffusion and masked prediction sit at the boundary between sequence and denoising. Coarse-to-fine, multi-stage, and token + continuous latent approaches may each take on planning and rendering. The final update and the irreplaceable training dependencies determine the primary class. The simultaneous presence of a Transformer, a VAE, and cross-attention in a system is not a reason to record all of them as hybrid. [@maskgit; @flexvar; @transitionmatching]

Native multimodal "unification" also needs to be decomposed. Janus-Pro shares an AR backbone but decouples its understanding and generation encodings. FlowInOne attempts to visualize conditions and hand them to a flow. VideoPoet shares a token sequence interface. A shared brand or API is not enough to demonstrate shared parameters, representations, or objectives, let alone that multiple tasks incur no negative transfer. [@januspro; @flowinone; @video_2312_14125]

Image–video unification often shares a codec, a DiT, text conditioning, or image pretraining. Temporal state, actions, long-term memory, and audio-video synchronization remain video-specific constraints. The gains from unification cannot be attributed to sharing itself. That attribution requires comparisons under the same data, the same parameters, the same tokenizer, and the same budget. [@video_2304_08818; @video_2401_03048; @video_2503_20314]

Synthesis judgment: hybrid is not a catch-all bucket for what cannot be classified. It is an explicit contract with components, interfaces, dependencies, and joint failure. Unification must state exactly what is shared.

Evidence boundary: public results usually change the data, the scale, the codec, and the budget at the same time. There is currently no equal-budget experiment supporting "unification is inherently better".

Transition: Chapter 13 connects side branches such as control, editing, personalization, and text rendering back to these backbones.

---

[← Back to contents](index.md)
