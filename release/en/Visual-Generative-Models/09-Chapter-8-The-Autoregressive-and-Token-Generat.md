## Chapter 8 The Autoregressive and Token-Generation Family

Inputs and questions: pixel-level AR, discrete/continuous tokens, masked restoration, next-scale, and video token cards.

Argument move: compare next-token, masked-refine, next-scale, and multi-stage video sequences. The comparison axes are the granularity of each state update and serial depth.

PixelRNN and PixelCNN retain exact normalized sequence probabilities, but pixel ordering brings a long serial path. Once VQ-VAE/VQGAN compress vision into discrete tokens, a Transformer prior can handle global structure. Reconstruction error and prior error also multiply. [@pixelrnn; @vqvae; @vqgan]

> **ALG04 Pixel/discrete-token autoregressive generation [@pixelrnn; @vqvae2; @video_2104_10157**]
> 1. >
1. Training input: an ordered pixel/token sequence and a condition c
>
1. State and objective: the state is the generated prefix x_<i or z_<i. The objective is -Σ_i log pθ(z_i|z_<i,c).
>
1. One parameter update: minimize the token cross-entropy under teacher forcing.
>
1. Inference initialization: a start token / text prefix / optional first frame.
>
1. Single-step state update: sample the next token from pθ(z_i|z_<i,c).
>
1. Termination and complexity: once a fixed sequence length is reached, output passes through the tokenizer decoder. Decoding is strictly serial, O(N). The KV cache grows with context.
>
1. Typical failures and limits of the claim: slow decoding, error accumulation, tokenizer distortion, and long-video context cost. The approach supports exact conditional factorization and a unified sequence interface. It does not equate token accuracy with visual/physical correctness. Formulas and mechanisms come from first-hand full-text cards. Complexity and failures serve only as a qualitative contract; they do not constitute a performance ranking.
>

DALL·E places text and image tokens in a unified sequence. MaskGIT reduces serial depth through parallel prediction and confidence-based remasking. VAR makes coarse-to-fine explicit via next-scale. MAR uses a diffusion head over continuous tokens and is therefore a composite mechanism. The Transformer itself cannot determine the family. [@dalle; @maskgit; @flexvar; @sana]

> **ALG05 Masked parallel restoration and next-scale generation [@maskgit; @video_2210_02399; @flexvar**]
> 1. >
1. Training input: randomly masked tokens or a multi-scale discrete representation
>
1. State and objective: the state is a mask set or a completed coarse-scale map. The objective is cross-entropy at masked positions. Next-scale is factorized by coarse-to-fine conditioning.
>
1. One parameter update: predict the masked tokens / next scale and update the Transformer by backpropagation.
>
1. Inference initialization: a full mask or the coarsest-scale tokens.
>
1. Single-step state update: predict in parallel, keep by confidence and reduce the mask; or generate the next scale.
>
1. Termination and complexity: the mask is emptied, or the highest resolution is reached. The number of iterations is fewer than per-token, but each round still processes the entire representation.
>
1. Typical failures and limits of the claim: confidence bias, scale error propagation, codebook reconstruction ceiling. This shows that the state update unit differs from next-token and cannot be lumped together as the same Transformer algorithm. Formulas and mechanisms come from first-hand full-text cards. Complexity and failures serve only as a qualitative contract; they do not constitute a performance ranking.
>

VideoGPT uses a 3D VQ-VAE + AR Transformer. Phenaki uses a causal video tokenizer + MaskGIT. VideoPoet supports multimodal tasks with a unified token prefix. The sequence interface naturally supports streaming conditions. Codec, context, KV cache, and error feedback, however, jointly determine the long-horizon ceiling. [@video_2104_10157; @video_2210_02399; @video_2312_14125]

> **ALG13 Discrete video tokens and long-sequence generation [@video_2104_10157; @video_2204_03638; @video_2210_02399; @video_2312_14125**]
> 1. >
1. Training input: short-video codec tokens, text/audio conditions
>
1. State and objective: the state is a spatiotemporal discrete token prefix, a mask, or hierarchical keyframes. The objective is token NLL or a random-mask restoration objective.
>
1. One parameter update: the video tokenizer and the sequence model are trained in stages or jointly.
>
1. Inference initialization: a conditional prefix, a first frame, or a full mask.
>
1. Single-step state update: next token, masked refine, or keyframe-interpolation update.
>
1. Termination and complexity: decode after reaching the target token/segment length. Complexity is determined by the T×H×W compression rate and the context length.
>
1. Typical failures and limits of the claim: codec loss of motion/contact state, sliding-window distribution shift, and long-range error feedback. Being able to extend the generation loop does not equal persistent semantic state or online interaction. Formulas and mechanisms come from first-hand full-text cards. Complexity and failures serve only as a qualitative contract; they do not constitute a performance ranking.
>

Overall judgment: the core selection variables for the AR family are the tokenizer, update granularity, context maintenance, and serial depth. Parameter count and the Transformer label alone are not.

Evidence boundary: different codecs, sequence orders, and cache implementations make authors' scores and speed not directly rankable. Successful long-sequence examples do not prove that errors will not accumulate.

Transition: Chapter 9 examines the stochastic route that learns multimodal distributions from multi-noise-level scores or denoising objectives.

---

[← Back to contents](index.md)
