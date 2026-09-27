## Chapter 13 Conditions, Control, Editing, and Personalization Side Branches

Input and question: in the control side-branch table, the structural conditions, editing, personalization, reference images, visual text, and video action conditions.

Argument move: compare along condition source, injection point, parameter update, inference dependency, and failure mode, rather than elevating every adapter into a generation family.

Structural control centers on "not breaking the base generator". ControlNet copies a trainable branch and injects it with zero convolutions, T2I-Adapter uses a smaller condition encoder, GLIGEN introduces grounding tokens, and IP-Adapter adds decoupled image cross-attention. Their failures differ in capacity, parameters, modality competition, and multi-condition conflict. [@controlnet; @t2iadapter; @gligen; @ipadapter]

Editing methods work on the starting point, the trajectory, attention, and supervision. SDEdit selects the noise starting point, Prompt-to-Prompt controls attention maps, Null-text inversion optimizes the unconditional embedding, DiffEdit estimates a mask, and InstructPix2Pix learns instruction triplets. Counting, object swapping, spatial relations, drift in non-edited regions, and multi-turn artifacts all require local and global metrics to be kept separate. [@sdedit; @prompt2prompt; @nulltext; @instructpix2pix; @diffedit]

Personalization started with the input embedding of Textual Inversion. It then reached DreamBooth's full-model fine-tuning, Custom Diffusion's partial weights, and the encoder route of InstantID/PhotoMaker. Subject fidelity, text adherence, and diversity form a triangle. A single identity similarity cannot cover background leakage, multi-subject attribute confusion, and group bias. [@textualinversion; @dreambooth; @customdiffusion; @instantid; @photomaker]

Visual text methods explicitly add layout, glyph, OCR representations, or more detailed captions. That shows data and codecs are algorithmic variables as well. Reliable text needs a joint contract spanning character-level data, spatial constraints, VAE reconstruction, OCR, and human evaluation. Misspellings, missing words, and overlaps cannot be masked by overall CLIP similarity. [@textdiffuser; @glyphdraw; @glyphcontrol; @anytext; @dalle3]

**Structured distribution of side-branch clusters such as control, editing, and personalization.**

Side-branch cluster | Record count | Examples |
|---|---|---|
temporal | 19 | Generating Videos with Scene Dynamics, Temporal Generative Adversarial Nets with Singular Value Clipping, MoCoGAN: Decomposing Motion and Content for Video Generation |
| control | 11 | Adding Conditional Control to Text-to-Image Diffusion Models, T2I-Adapter: Learning Adapters to Dig out More Controllable Ability for Text-to-Image Diffusion Models, GLIGEN: Open-Set Grounded Text-to-Image Generation |
| editing | 11 | A Style-Based Generator Architecture for Generative Adversarial Networks, Analyzing and Improving the Image Quality of StyleGAN, Alias-Free Generative Adversarial Networks |
| personalization | 7 | An Image is Worth One Word: Personalizing Text-to-Image Generation using Textual Inversion, DreamBooth: Fine Tuning Text-to-Image Diffusion Models for Subject-Driven Generation, Multi-Concept Customization of Text-to-Image Diffusion |
| audio-video | 3 | MM-Diffusion: Learning Multi-Modal Diffusion Models for Joint Audio and Video Generation, VideoPoet: A Large Language Model for Zero-Shot Video Generation, Movie Gen: A Cast of Media Foundation Models |
| text-rendering | 1 | AnyText: Multilingual Visual Text Generation And Editing |
| | | |

![F07. Control side branches are not a new generation family. They should be compared by condition carrier, injection location, scope of parameter updates, and failure mode. Evidence boundary: the injection path is an inductive label used to navigate the full text, and it does not replace the precise layer, weight, or code audit of each paper.](../../figures/en/F07_control_module_map.png)

*F07. Control side branches do not constitute a new generation family. They should be compared by condition carrier, injection location, scope of parameter updates, and failure mode. Evidence boundary: the injection path is an inductive label used to navigate the full text. It does not replace a precise audit of each paper's layer, weight, or code.*

Overall judgment: a side branch earns its value by repairing shared bottlenecks. Whether it is upgraded to a core depends on interface reuse and independent successors. It does not depend on module size or release hype.

Evidence boundary: the current control table aggregates evidence from heterogeneous authors. No unified run has yet exercised multi-condition, editing and personalization on the same base model.

Transition: Chapter 14 carries the same conditioning question into time, motion, long-range state, audio and action.

---

[← Back to contents](index.md)
