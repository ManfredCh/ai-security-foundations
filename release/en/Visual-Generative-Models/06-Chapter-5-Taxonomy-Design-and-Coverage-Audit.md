## Chapter 5 Taxonomy Design and Coverage Audit

Input and question: the unified interface, corrected role coding, decision rules and boundary cases.

Argumentative move: decide hybrid first, then assign one of the six families by the final update operator. Representation, backbone, conditioning, modality and product are all retained as secondary labels.

The decision order is as follows. If two or more non-removable generative operators jointly produce the final output, code it as hybrid and record the components. Otherwise check, in turn, latent sample/invertible density, adversarial game, sequence/masked update, score denoising, and velocity/flow map. When full-text localization cannot support a coding, label it UV. Do not guess from the model name.

After correction, the 132 cards split across the families as follows. Explicit probability and latent variable/invertible flow: 5 cards. Adversarial implicit generation: 13 cards. Autoregressive and masked generation: 18 cards. Score-based and stochastic denoising: 60 cards. Deterministic transport: 23 cards. Hybrid and unified generative mechanisms: 13 cards. [@video_2408_06072]

The counts are also subject to the constraints of the post-generation audit. Structural scan is complete for 132/132 cards, 124 have local full text, and 8 cannot be locally verified. Findings are critical 1, high 19 and medium 4. Critical/high findings are revised atomically via the central overlay and propagate only after corrections and figures are synchronized. The numbers in the table are therefore dynamic counts of the current correction layer, not a frozen old snapshot. This audit still falls short of full independent double coding. [@src_s_audit_post_generation_20260809]

Roles and families are orthogonal. The post-correction role distribution is: branch 32 cards; bridge 21 cards; core 74 cards; counterexample 2 cards; system 3 cards. Under the B/D shared-interface rule, ControlNet and DreamBooth are upgraded to core. [@controlnet; @dreambooth]

**Post-correction single-axis family coding and representative works; the counts describe only the 132 structured cards.

Mechanism family | Cards | Representative work_id |
|---|---|---|
Explicit probability and latent variable/invertible flow [@vae; @glow] | 5 | WIMG0001, WIMG0007 |
| Adversarial implicit generation [@gan; @stylegan] | 13 | WIMG0002, WIMG0010 |
| Autoregressive and masked generation [@pixelrnn; @maskgit; @video_2104_10157] | 18 | WIMG0014, WIMG0020, VID-W005 |
| Score-based and stochastic denoising [@ddpm; @ldm; @video_2204_03458] | 60 | WIMG0028, WIMG0031, VID-W010 |
| Deterministic transport [@flowmatching; @rectifiedflow] | 23 | WIMG0039, WIMG0040 |
| Hybrid and unified generative mechanisms [@sana; @video_2309_17080] | 13 | WIMG0046, VID-W050 |
| | | |

**Post-correction paper role distribution; roles are orthogonal to the primary family.

Role | Cards |
|---|---|
branch | 32 |
| bridge | 21 |
| core | 74 |
| counterexample | 2 |
| system | 3 |
| | |

Boundary cases are handled by component responsibility. VQ/VAE codec + AR prior takes its primary class from next-token, and latent codec + DiT + rectified flow is decided by velocity-flow. Diffusion teacher + adversarial student is hybrid if both operators are non-removable. Image pretraining + temporal layers are only a cross-modal label. A closed-source brand that does not disclose its operator can only be recorded as system/UV. [@vqgan; @sd3; @instaflow; @video_2304_08818]

The post-generation review turns this boundary rule into practice. DDIM's deterministic sampling does not change the diffusion training family, and ConsisID's identity conditioning does not replace CogVideoX's reverse diffusion. Janus-Pro is coded by the final AR visual-token update. VACE's conditioning adapter does not establish a separate generative operator. The Matrix is distinguished by its real-time SCM student path and its non-real-time teacher path. [@src_s_audit_post_generation_20260809; @ddim; @video_2411_17440; @januspro; @video_2503_07598; @video_2412_03568]

![F03. Six-family single-axis taxonomy. The taxonomy is based on the construction of the generative distribution and the mechanism of the final visual state update, not on Transformer, VAE codec, conditioning control, or product name. Evidence boundary: the family counts describe only the 132 structured cards. The secondary coding of the stratified sample is not yet complete and is not used as a field prevalence rate.](../../figures/en/F03_operator_taxonomy.png)

*F03. Six-family single-axis taxonomy. The taxonomy is based on the construction of the generative distribution and the mechanism of the final visual state update, not on Transformer, VAE codec, conditioning control, or product name. Evidence boundary: the family counts describe only the 132 structured cards. The secondary coding of the stratified sample is not yet complete and is not used as a field prevalence rate.*

Overall judgment: the single axis can code all 132 structured cards. It treats hybrid mechanisms as a category with a component contract rather than a miscellaneous bucket.

Evidence boundary: 132/132 structural scans do not equal full independent double coding. The agreement rate of the 24-card purposive sample cannot prove that the whole classification is stable, and the 8 cards without local full text still retain a verification ceiling.

Transition: Chapters 6–12 take up the six families and their bridging mechanisms, field by field.

---

[← Back to contents](index.md)
