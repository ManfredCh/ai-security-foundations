## L0--L5 Method Families

This section regroups the method families along the same input--internal state--output contract. For every family the section states the unknown it resolves, the verifiable artifact it produces, the supervision and optimization it relies on, and how failure propagates to the next layer.

### L0: Video Generation and Action-Conditioned Frame Worlds

L0 natively delivers images, videos, or action-conditioned observation sequences. Camera conditioning, 3D caches, and latent states can improve consistency. Yet if the output is only frames, queryable appearance, spatial samples, and persistent assets still cannot be inferred.

The conclusion of this chapter comes first. Researchers did not bring video models into 3D spatial construction because "continuous imagery is inherently three-dimensional". They brought them in because cameras, rays, depth, point clouds, and external caches were progressively connected into the generation process. The order of evolution can be summarized as follows. Numerical poses control the overall motion. Ray conditioning expands poses down to pixels. Depth back-projection and point cloud rendering then provide pixel-level geometric constraints. Finally, the newly generated observations go to 3DGS, mesh, and collision construction modules for consolidation. Every step adds a query capability, and every step also writes the previous step's error into the next layer.

The paper versions and public pages described in this chapter were verified as of August 6, 2026. The formulas explicitly distinguish generic expressions from paper-specific implementations. In particular, one must not generalize every camera-controllable video model into "using Plücker rays". MotionCtrl uses pose matrices directly. AC3D adopts the Plücker camera representation. ViewCrafter and GEN3C go further and use explicit point cloud rendering as a condition.

What separates a world model from ordinary video generation is the closed loop, not the name. An action has to change the internal state, and that state has to determine what is observed next. The RSSM used for control shares neither an optimization objective nor an output contract with the autoregressive token model used for playable video, the diffusion model that directly predicts the next observation, the JEPA that plans in an embedding space, or the persistent state model that explicitly maintains 3D world frames. This chapter organizes them by "what the state is" rather than listing papers one by one.

#### What must be solved is not "generating a video" but three kinds of non-identifiability

The video generator receives text or a single starting image, a target camera trajectory, and optional depth information. It must output a set of frames

$$
\hat{X}_{1:T}\sim p_\theta\left(X_{1:T}\mid X_{\mathrm{ref}},C_{1:T},c_{\mathrm{text}},c_{\mathrm{geo}}\right).
$$

This is the generic probabilistic form of camera-conditioned video generation. It matches no single paper's original formulation. Here $C_t=(K_t,R_t,t_t)$ denotes intrinsics and extrinsics, and $c_{\mathrm{geo}}$ may be empty, or may be depth, point cloud rendering, optical flow, or multi-view conditions. This problem has at least three layers of non-identifiability.

First, a single RGB image cannot determine depth and absolute scale uniquely. Objects of different sizes, placed at different distances, can produce the same perspective projection. Second, occluded regions yield no observation at all, so the generator can only complete them from training priors. The completed back side may be perceptually plausible, yet it need not be identical to the real scene. Third, pixel motion can come from camera motion, from object motion, or from the two superimposed. If the "pose-annotated videos" in the training data are almost all static real-estate videos, the model can easily mislearn camera conditioning as a signal to "suppress scene dynamics."

Therefore, the video route's native output contract is usually only a time-indexed sequence of RGB frames or latents. It can answer "what does it look like along a given path." It cannot directly answer the depth at a point, wall thickness, closed topology, collision normals, or whether a character can pass through a doorway. The capability level truly moves upward only when the system additionally outputs queryable depth, point clouds, 3DGS or meshes, and passes independent geometric checks.

#### The common foundation of video diffusion: denoising completes, conditioning constrains

Most related work builds on latent video diffusion. What follows is a generic diffusion expression, not an implementation unique to any one method. An encoder first compresses a real video $X$ into a latent variable $z_0=E(X)$. Gaussian noise is then added at a random time $\tau$:

$$
z_\tau=\alpha_\tau z_0+\sigma_\tau\epsilon, \qquad \epsilon\sim\mathcal N(0,I).
$$

If the network predicts noise, its common training objective is

$$
\mathcal L_{\mathrm{diff}} =\mathbb E_{z_0,\epsilon,\tau} \left[\left\|\epsilon-\epsilon_\theta(z_\tau,\tau,c)\right\|_2^2\right],
$$

Here the condition $c$ may include text, the first frame, camera parameters, and geometric rendering. Some papers also adopt $x_0$ prediction, velocity prediction, or rectified flow. Those choices change the prediction target and the sampling trajectory, but the core is the same: "applying a condition to a noisy state and progressively restoring the video." If the condition is only a soft embedding during training, the denoiser can compromise between the visual prior and the condition. So "a pose was input" does not equal "projection geometry is satisfied pixel by pixel."

Camera conditions can be divided into three levels of granularity. The first is a whole-frame pose matrix. A typical form flattens each frame's $3\times4$ rotation--translation matrix and feeds it into a temporal module. The second is per-pixel rays. Adopt the world-to-camera convention $x_c=R_tX_w+t_t$ and let the pixel homogeneous coordinate be $\tilde u$. The camera center and the world-frame ray direction can then be written as

$$
o_t=-R_t^\top t_t,\qquad d_t(u)=\frac{R_t^\top K_t^{-1}\tilde u} {\|R_t^\top K_t^{-1}\tilde u\|_2}.
$$

Plücker rays are often written as $r_t(u)=(d_t(u),o_t\times d_t(u))$. This is a generic ray definition. Methods such as CameraCtrl, VD3D, and AC3D adopt this family of representations, but MotionCtrl does not construct Plücker rays per pixel. A ray indicates where a pixel looks from and where it looks toward. It does not indicate at which depth the ray intersects a surface.

The third is to back-project depth and color into explicit 3D points and then render from the target camera. This generic back-projection of a source pixel $u$, and the warp to the target view, still use the above camera convention:

$$
X_w(u)=R_t^\top\left(D_t(u)K_t^{-1}\tilde u-t_t\right), \qquad \tilde u'\sim K_{t'}\left(R_{t'}X_w(u)+t_{t'}\right).
$$

A practical implementation must also perform z-buffering, frustum culling, and occlusion masking. Otherwise points behind the source view will incorrectly cover the foreground of the target view. Depth warp provides one more assumption than a pure ray embedding, namely "where along the ray," and can therefore lock camera motion more strongly. It also turns depth errors directly into 3D position errors.

A generic geometry-conditioned video pipeline can be written as the algorithm below. It describes only the structure this method family shares. It does not represent the complete code of any single paper.

**The text pipeline or pseudocode in the original manuscript**
\begin{verbatim}
Input: reference images or sparse views, camera parameters, target camera trajectory
1. Encode the reference images; if explicit geometry is needed, estimate depth and confidence.
2. Back-project valid RGB-D pixels to world coordinates, keeping the source view and confidence.
3. Render color, depth, and a visibility mask from each target camera.
4. Encode the renderings, the first frame, and the text as video denoising conditions.
5. Sample or distill to obtain video frames along the target path.
6. Filter inconsistent frames using reprojection, cyclic trajectories, and multi-view matching.
7. Feed only the frames that pass screening into point cloud fusion, 3DGS, or mesh construction.
Output: generated frames; optional depth, point cloud cache, and a downstream reconstruction representation.
\end{verbatim}

Failure propagation is also determined by this chain. Pose errors make ray directions wrong. Ray or depth errors misalign the warp. The denoiser will then "fix" the misalignment to look real, and the reconstructor may in turn solidify such visual patching into fake surfaces. Therefore, video metrics, pose re-estimation metrics, and 3D geometry metrics must be reported separately.

#### MotionCtrl: first separate camera motion and object motion from video motion

The primary problem that MotionCtrl [@srcV01] solves is not 3D reconstruction but control-signal entanglement. Conventional video conditioning often mixes camera movement and object movement into a single optical flow. Inside a latent video diffusion model, MotionCtrl adds a Camera Motion Control Module (CMCM) and an Object Motion Control Module (OMCM) as separate modules. Each type of motion can then be controlled on its own or in combination.

Its paper-specific implementation works in the following steps. First, CMCM receives a per-frame camera pose sequence of length $L$, $RT_t=[R_t\mid t_t]\in\mathbb R^{3\times4}$. It first replicates the original $L\times12$ pose values along the spatial dimension into a camera condition of $H\times W\times L\times12$. Second, this condition is concatenated along the channel dimension with the output of the first self-attention module in the temporal Transformer. A fully connected layer then projects it back to the original number of channels as the input to the second self-attention part. In other words, the projection in the paper happens after broadcasting and concatenation, rather than first encoding the 12-dimensional pose and then broadcasting. Third, OMCM rewrites the object trajectory given by the user into a sparse displacement field. If the positions of a trajectory point in adjacent frames are $(x_{i-1},y_{i-1})$ and $(x_i,y_i)$, the paper defines

$$
u_i=x_i-x_{i-1},\qquad v_i=y_i-y_{i-1},
$$

and fills zeros at pixels that are not traversed, forming a trajectory tensor of $L\times H\times W\times2$. Fourth, a convolution and downsampling network extracts multi-scale features from this tensor. It injects them only into the corresponding spatial layers of the denoising U-Net encoder. Fifth, CMCM is trained first. The base model and CMCM are then frozen to train OMCM, which avoids the later-trained camera module destroying the already-learned object trajectory control.

In this evolution, the step matters because it separates "global camera motion" and "local object motion" at the network interface. The control is still soft, however. The pose matrix is broadcast for the whole frame. It does not tell each pixel which ray it corresponds to, and it does not give the surface intersection. Therefore, when a scene contains large parallax, when occlusion appears, or when the camera combination is out of the training distribution, the model can produce roughly correct background movement. It cannot guarantee that every structure projects according to the same 3D scene.

Its output contract is still pixel video. The camera condition of CMCM cannot directly derive depth, point clouds, or collision bodies. The evolution after MotionCtrl naturally turns to two questions. How can pose conditioning be expanded into per-pixel rays? And at what time, and in which layers of the denoising process, should the condition be injected?

#### AC3D: camera motion is low-frequency structure, and the condition should not disturb all denoising layers

AC3D [@web00a2d5e10b6d] conducts a finer-grained mechanistic analysis of video DiT. It does not simply "switch to Plücker rays." It first answers when camera information appears in denoising time and network depth, and then narrows the scope of condition injection accordingly.

The paper-specific pipeline is as follows. First, the method builds a per-pixel Plücker camera representation from each frame's extrinsics and intrinsics. A fully convolutional encoder turns it into camera tokens at the same resolution as the video tokens. Second, a lightweight DiT branch processes the camera tokens and adds them before the main video DiT blocks, while the main branch is allowed to give cross-attention feedback to the camera tokens. Third, the authors analyze the motion spectrum of the generated video. They find that camera movement mainly forms low-frequency motion and is determined at a very early stage of the rectified-flow reverse trajectory. Based on this observation, training noise is concentrated in the early interval $[0.6,1]$, and inference applies camera conditioning only in the first 40% of the reverse denoising interval. This is the paper-specific schedule of AC3D; it should not be generalized into a fixed ratio for all diffusion models. Fourth, the authors perform linear pose probing on 32 video DiT blocks and find that camera information is most obvious in the middle layers. They therefore inject the condition only in the first 8 blocks, which avoids mutual interference between the already-formed camera representation and subsequent appearance and dynamic features. Fifth, in addition to RealEstate10K, about twenty thousand clips of "fixed camera but dynamic scene" video are added. The camera branch thereby stays active on dynamic content. This also reduces the data bias that "having camera annotation means a static scene".

AC3D explains the key change from MotionCtrl to the next generation of methods. Controllability no longer depends only on what the condition is. It also depends on where the condition is injected along the denoising time axis and the network-layer axis. It improves the conflict between camera following and video quality. It still does not form an explicit surface. Even if the generated video allows ParticleSfM to recover a relatively accurate trajectory, that only shows that the frames carry estimable camera motion. Scale, post-occlusion geometry, topology, and collision remain unknown.

Therefore, the failure of AC3D propagates downstream as follows. If the model ignores high-frequency camera changes, the parallax in the generated frames is smaller than that of the given trajectory. Subsequent SfM then estimates a compressed baseline, the triangulated depth becomes correspondingly too large, and the scale of the final point cloud and mesh is distorted. To block this propagation, the next step cannot be to keep adjusting the embedding alone. It must turn the coarse 3D projection directly into a pixel condition.

#### ViewCrafter: letting the coarse point cloud handle the camera and video diffusion handle hole filling

ViewCrafter [@srcV07] advances the task from "camera-controllable video" to "novel view generation conditioned on coarse 3D". Its basic division of labor is clear. Point cloud rendering provides structure that can be aligned under the target camera. Video diffusion repairs the holes, stretching, and unobserved regions of the point cloud rendering.

The paper-specific algorithm consists of six steps. First, a dense stereo model such as DUSt3R estimates a point map, cameras, and an initial point cloud $P^{\mathrm{ref}}$. Its input is a single image or sparse images. Second, under the target camera sequence $C_{1:L}$, the point cloud is rendered into a coarse video $P_{1:L}$. Here $P$ denotes the point cloud rendering condition in the paper, not a probability. Third, a point-conditioned video diffusion model is trained to learn

$$
X_{1:L}\sim p_\theta \left(X_{1:L}\mid I^{\mathrm{ref}},P_{1:L}\right),
$$

This is the conditional distribution form given by the ViewCrafter paper. The reference image provides a reliable appearance anchor. The coarse rendering provides a per-pixel camera constraint, and the denoising model completes the missing regions. Fourth, to expand the browsing range, the system samples multiple candidate poses near the current camera and renders the visibility mask of each candidate. A utility function then selects the next best view that can reveal more uncovered regions. Fifth, it generates a new video along the interpolated trajectory from the current pose to the next best pose. The generated new views then go back to the dense stereo model to update the point cloud. Sixth, it repeats "plan camera--render coarse point cloud--diffusion completion--update point cloud". Finally, it uses the extended point cloud to initialize 3DGS and supervises 3DGS optimization with synthesized views.

This loop adds the idea of "actively supplementing observations", but it also introduces an important bootstrapping risk. The utility of a candidate viewpoint comes from the visibility of the current point cloud. If the initial depth is wrong, the planner may treat erroneous regions as uncovered regions. The diffuser then generates a visually plausible patch, and the stereo model turns it into new points. The error therefore escalates from appearance hallucination to three-dimensional fact. A higher point cloud cleanup threshold can reduce flying points but increases holes. A lower threshold retains more structure and also retains more erroneous points.

ViewCrafter's native generation output is still novel view videos and an updated point cloud. 3DGS is a subsequent optimization result. It does not natively provide watertight meshes, object identity, collision proxies, or physical properties. In this evolution it marks the first upgrade of the camera condition, from "describing rays" to "directly rendering coarse geometry". The subsequent GEN3C further turns this coarse geometry into an explicit external memory in long video.

#### GEN3C: replacing limited pixel history with an updatable 3D cache

GEN3C [@srcV08] targets two problems. Multi-view conditions may contradict one another, and a limited context forgets content that leaves the frame in long videos. Its answer is a spatiotemporal 3D cache that holds the colored point cloud of every time step and every input view. Past content can then be re-rendered from a new camera instead of being recalled from latent memory alone.

The paper's own representation is an $L\times V$ point cloud array $P_{t,v}$. For the RGB image at time $t$ and view $v$ the method estimates depth, then applies the back-projection formula of Section 3.2 to build a colored point cloud. Single-image generation creates one cache element and copies it along time. Static multi-view gives each view its own cache. Dynamic multi-view makes each synchronized video stream a column of the cache. When the camera is not given, the paper's implementation can estimate it with DROID-SLAM.

Given a user camera $C_t$, the point cloud renderer outputs

$$
(I_{t,v},M_{t,v})=R(P_{t,v},C_t),
$$

Here $I_{t,v}$ is the coarse color and $M_{t,v}$ is the coverage mask. The mask explicitly marks which positions in uncovered regions "need generation". Multi-view fusion does not first force all point clouds into a single geometry. Different depth estimates and lighting would make direct stitching produce ghosting. GEN3C instead encodes each view rendering with a frozen VAE, $z^v=E(I^v)$, downsamples the mask, and multiplies it element-wise with the rendered latent. That product is concatenated with the noisy latent of the target video. Its paper-specific fusion can be summarized as

$$
z^{v\prime}=\operatorname{InLayer} \left(\operatorname{Concat}(z^v\odot M^{v\prime},z_\tau)\right), \qquad z'=\operatorname{MaxPool}_{v=1}^{V}z^{v\prime}.
$$

Cross-view max-pooling removes any need to fix the number of views or their permutation order. A fine-tuned video diffusion model then generates the target video under these rendering conditions.

Long videos use block-wise autoregression. Adjacent blocks overlap by one frame. Once a block is generated, depth is estimated only for the last frame of the block. The paper's supplementary material does not simply trust the scale of monocular depth. It solves for the scale $s$ and offset $b$ over the region that the old cache covers:

$$
(s^*,b^*)=\arg\min_{s,b} \left\|\big(sD+b-D_{\mathrm{cache}}\big)\odot M\right\|_2^2.
$$

The aligned RGB-D is then back-projected and appended to the cache. The next block renders its condition from that updated cache. This is the closed loop of "generate first, write back, then query".

The 3D cache adds a new capability, revisiting. After a wall leaves the frame, its sampled points can still be queried and rendered under a new viewpoint. The same cache also carries a risk of persistent contamination. Depth scale alignment constrains $s,b$ only in regions the cache already covers, so newly appearing regions may still be wrong. If the temporal correspondence of dynamic objects is incorrect, ghosting is left at multiple positions in the cache. Depth blending at occlusion edges forms floating layers. Max-pooling can aggregate multi-view features, but it does not resolve geometric conflicts on its own.

GEN3C natively outputs a video and an internal point cloud cache, not a deliverable mesh. To enter the asset layer it must export a point cloud that carries camera, time, confidence, and provenance, then perform fusion or 3DGS optimization. If only the final video is exported, the interface loses the spatial value that the external memory provides.

#### ReconX: first filling in observations with generation, then solidifying them into 3DGS with confidence

ReconX [@srcD01] recasts the ambiguity of sparse-view reconstruction as a video interpolation and extrapolation problem. What sets it apart from ViewCrafter is how explicitly it states the final goal. The target moves from "generating controllable novel views" to "using these novel views to optimize 3DGS".

The paper's algorithm starts from two or more sparse images that may have no known poses. First, DUSt3R predicts point maps and confidence for image pairs, and global alignment yields a unified point cloud $P={p_i}_{i=1}^{N}$. Second, farthest point sampling selects a small number of query anchors. Those anchors aggregate raw point cloud features through cross-attention, which encodes a variable-length point cloud into a fixed-length three-dimensional context $F(P)$. Third, the first and last reference images become the endpoints of video interpolation. The image condition and the three-dimensional context are each injected into the spatial cross-attention of the video U-Net. ReconX's paper-specific fusion is

$$
F_{\mathrm{out}} =\operatorname{Softmax}\left(\frac{QK_g^\top}{\sqrt d}\right)V_g +\lambda_F\operatorname{Softmax}\left(\frac{QK_F^\top}{\sqrt d}\right)V_F,
$$

The image condition supplies the first term. The second term comes from the three-dimensional point cloud condition. Diffusion training still uses the noise prediction objective. Fourth, the model generates multiple video frames between the reference views and along the outer paths. Fifth, DUSt3R runs again, this time to match the generated frames pairwise. That yields global alignment and confidence among the generated views. Sixth, 3DGS starts from the point maps of the input endpoints and is then optimized on all generated frames.

ReconX does not count every generated pixel as equally valid ground truth. Let the pixel confidence of the $i$-th frame be $C_i$. Its paper-specific 3DGS objective weights the color, SSIM, and LPIPS errors by confidence:

$$
\mathcal L_{\mathrm{conf}} =\sum_i C_i\left[ \lambda_{\mathrm{rgb}}\mathcal L_1(\hat I_i,I_i) +\lambda_{\mathrm{ssim}}\mathcal L_{\mathrm{ssim}}(\hat I_i,I_i) +\lambda_{\mathrm{lpips}}\mathcal L_{\mathrm{lpips}}(\hat I_i,I_i) \right].
$$

This is safer than treating generated frames as noise-free photographs. Even so, "low matching confidence" does not equal "the generated content is definitely wrong". Nor does it guarantee that regions which are semantically consistent but geometrically false will receive low weight. Suppose the video model keeps generating the same nonexistent window. Multi-frame matching may then assign it high confidence, and 3DGS will stably solidify the fake structure.

ReconX upgrades its output contract to a renderable 3DGS, which can answer appearance queries from arbitrary viewpoints. It still does not close topology, explicit surface normals, collision bodies, or physical materials. To connect to the next layer, it should retain, for each Gaussian, the provenance of whether it comes from real input or generated completion. It should also set different thresholds and review protocols for the two types of regions before surface extraction.

#### VideoScene: starting from coarse 3D and skipping the early steps of recovering structure from pure noise

VideoScene [@srcD02] attacks the speed bottleneck that appears when video diffusion takes part in three-dimensional reconstruction. Its "one step" refers to the distilled video generation step. It does not refer to obtaining, in one step, an engine asset that has already passed physical validation.

The algorithm first receives two images and a camera. The pose may be given, or estimated by COLMAP or DUSt3R. It then uses a feed-forward sparse-view 3DGS model such as MVSplat to construct a coarse scene. Along the interpolated camera trajectory it renders a structurally consistent but possibly blurry video $X_0^r$. Traditional diffusion starts from pure noise. VideoScene instead encodes the coarse rendering into the latent space and adds noise at an intermediate timestep

$$
x_t^r=\alpha_t x_0^r+\sigma_t\epsilon,
$$

and then trains a student model to approximate how the teacher maps $t_{n+1}$ to $t_n$. Its paper-specific consistency distillation objective is written as

$$
\mathcal L_D= \mathbb E\left[ d\left( f_\theta(x_{t_{n+1}}^r,t_{n+1}), f_{\theta^-}(\hat x_{t_n}^{\phi}(x_{t_{n+1}}^r),t_n) \right) \right],
$$

where $\phi$ is the teacher and $\theta^-$ is the exponential moving average student. The dynamic denoising policy network DDPNet treats candidate jump timesteps as contextual bandit actions. When the coarse rendering is good, it chooses less noise to preserve structure. When the coarse rendering is distorted, it allows more noise for repair. Too much noise, however, will erase the 3D prior.

Compared with ReconX, the route moves toward "feed-forward 3D initialization + local correction by a generative model + distillation acceleration". It avoids making the denoiser reinvent the entire scene from pure noise. Yet it may still let visual completion cross real obstacles. The paper's supplementary material shows a case where the two input views differ greatly in semantics. There the generated camera path passes directly through a closed door. That case shows that a continuous transition in the pixel sequence carries no feasible path constraints. It also shows that DDPNet selects only visual denoising strength, not a collision planner.

Therefore, VideoScene's intermediate output should still go through pose re-estimation, reprojection, and visibility checks. Only then should it reach InstantSplat or another 3DGS reconstruction. Any clip that "passes through a door but looks continuous" must be removed before NavMesh and collision construction. Its evidence level must not be raised just because generation is fast.

#### Video2Game: what truly closes the spatial construction chain is asset conversion, not more video frames

Video2Game [@srcD04] is not a direct successor of the video diffusion chain above. It deals with the systems engineering problem of going from a real single video to a game environment. Yet it clearly shows which steps are still missing between "a renderable representation" and "an interactive asset". For that reason it serves as the endpoint reference for this evolutionary chain.

Its paper-specific pipeline is divided into three layers. First, train a large-scene NeRF from posed video. Beyond density and color, add surface normals, sky, and monocular geometric cues. These improve how stably surfaces can be extracted from the radiance field. Second, run Marching Cubes on the NeRF density and post-process the mesh. UV unwrapping bakes multi-channel neural textures into texture maps. At runtime, a small GLSL shading MLP combines texture features and the view direction to recover view-dependent appearance. Third, split the scene into objects, either by semantics or by bounding box, and extract a mesh for each object on its own. Where surfaces are newly exposed, complete their textures by nearest neighbor.

The interaction layer does not directly use the high-fidelity visual mesh as a unified collision surface. Each rigid-body entity is represented as

$$
e_i=(\mathcal G_i^{\mathrm{vis}},\mathcal G_i^{\mathrm{col}},m_i,f_i,T_i),
$$

This is an engineering notation for the paper's interface, not the paper's original equation. Here $\mathcal G_i^{\mathrm{vis}}$ is the visual mesh, $\mathcal G_i^{\mathrm{col}}$ is the collision proxy, $m_i$ and $f_i$ are mass and friction, and $T_i$ is the object transform. The paper supports four classes of collision geometry: box, sphere, convex polyhedron, and triangle mesh. Complex convex proxies can be generated with V-HACD. Mass and friction can be set by hand. The paper also discusses estimating them with a language model, but those estimates are not measured ground truth.

This pipeline shows why the output contract must be accepted layer by layer. NeRF optimization answers appearance and density. Marching Cubes yields a rasterizable surface. UV and neural textures yield real-time appearance. Object decomposition yields independent transforms. Only collision proxies and physical parameters yield contact and dynamics. No step can be automatically derived from the previous step. In particular, an erroneous NeRF density produces an erroneous isosurface. A simplified collision body expands or shrinks the traversable space. Erroneous mass and friction estimates make the collision response inconsistent with the real world.

#### Evolution logic of this chapter and the interface to the next layer

Placed on the same line of evolution, these methods show that each generation replaces a different link. MotionCtrl separates the global pose and the local trajectory from ordinary video motion. AC3D further decides when and at which layers the camera condition should take effect. ViewCrafter replaces purely numerical poses with coarse point-cloud rendering, obtaining pixel-level constraints. GEN3C turns the point cloud into an updatable, revisitable external cache. ReconX solidifies newly generated observations into 3DGS through confidence weighting. VideoScene uses coarse 3D and distillation to reduce generation cost. Video2Game shows that after 3DGS or NeRF, surfaces, textures, objects, collision, and a runtime contract are still required.

Therefore, the minimal interface this chapter delivers to the reconstruction and asset layer should include the following. Camera intrinsics and extrinsics, plus the coordinate convention. Provenance labels for real frames and generated frames. Per-frame depth, confidence, and occlusion mask. The provenance and time of each primitive in the point cloud or 3DGS. Markers of unobserved regions. Closed-loop reprojection residuals. And the constraint that downstream must not directly treat the visual representation as collision ground truth. Chapters 6–10 cover SfM, MVS, TSDF, surface extraction, and collision construction. Those steps are exactly what turns these incomplete observations into auditable spatial assets.

#### State, action, and observation must be kept separate

The general state transition in a partially observable environment can be written as

$$
s_{t+1}\sim p(s_{t+1}\mid s_t,a_t),\qquad o_t\sim p(o_t\mid s_t),\qquad r_t\sim p(r_t\mid s_t,a_t).
$$

This is the general form of a POMDP and of world models. The true state $s_t$ is often unobservable. From the history $h_t=(o_{\le t},a_{<t})$ the model can only build an approximate state. The fundamental difference between the various routes is where this approximation is placed. Dreamer places it in a recurrent latent state. Genie places it in discrete video tokens and historical actions. GameNGen and DIAMOND condition on a finite frame history to generate the next observation. V-JEPA 2 predicts the future in feature space. PERSIST explicitly evolves latent three-dimensional world frames, cameras, and a renderer.

Suppose the state exists only in a finite pixel history. Then doors, boxes, or terrain that leave the field of view slide away with the context window. If the state is a compact latent variable, it can support reward prediction and policy learning. An engine, however, cannot necessarily query it for "whether the door is open". If the state is object or three-dimensional scene data, only then can the system stably support saving, branching, revisiting, multi-user sharing, and collision queries.

#### RSSM and Dreamer: compressing history for imagination-based planning, not exporting 3D assets

The Nature paper on DreamerV3 [@srcW03] builds on a recurrent state-space model (RSSM). Its paper-specific structure is

$$
\substack{h_t=f_\phi(h_{t-1},z_{t-1},a_{t-1}),\\ z_t\sim q_\phi(z_t\mid h_t,x_t),\\ \hat z_t\sim p_\phi(\hat z_t\mid h_t),\\ \hat r_t\sim p_\phi(\hat r_t\mid h_t,z_t),\\ \hat c_t\sim p_\phi(\hat c_t\mid h_t,z_t),\\ \hat x_t\sim p_\phi(\hat x_t\mid h_t,z_t).}
$$

$h_t$ is the deterministic recurrent memory. $z_t$ is the stochastic discrete representation, corrected by the current observation. The prior $p(\hat z_t\mid h_t)$ lets the model roll out when no future observation exists. The reward head, the continue-flag head, and the observation decoder head force the state to retain what the task needs. Training can be summarized as a combination of reconstruction, reward, continue prediction, and the prior–posterior KL. DreamerV3 adds stabilization techniques such as KL balance, free bits, symlog, and return normalization. The objective below is a conceptual reorganization of the paper's structure; it does not reproduce every coefficient item by item:

$$
\mathcal L_{\mathrm{WM}} =\mathcal L_x+\mathcal L_r+\mathcal L_c +\beta_{\mathrm{dyn}}D_{\mathrm{KL}}(\operatorname{sg}q\|p) +\beta_{\mathrm{rep}}D_{\mathrm{KL}}(q\|\operatorname{sg}p).
$$

At run time the algorithm first writes real interactions into the replay buffer. It then obtains a posterior state from real observations. From some posterior state it rolls out multi-step "imagination trajectories" using only the prior dynamics and the actor. The critic estimates the returns of those trajectories. The actor improves accordingly. The algorithm then collects new data with the real environment again. This loop makes the model learn a compressed state useful for decision-making.

The RSSM's advantage is also its limitation for spatial construction. To predict reward, it can discard textures, distant rooms, or precise wall thickness that do not affect the current task. The policy may also exploit model error, finding high-return paths in imagination that do not exist in the real environment. Even if the decoded frames are clear, $(h_t,z_t)$ is not an editable mesh or a collision field. The Dreamer route therefore delivers no scene assets to a spatial system. What it delivers is "the latent state, reward, and termination prediction given an action." Where persistent space is needed, a geometry or object database must be connected as an independent state source.

#### Genie: discovering discrete control from video without action labels, but the state is still carried by token history

Internet video has no action labels. Genie [@srcGDM-GENIE] addresses that problem with three parts, all explicitly disclosed in the paper: a spatiotemporal video tokenizer, a latent action model, and an autoregressive Dynamics Model.

Stage one compresses the video $x_{1:T}$ into discrete tokens $z_{1:T}$ with a spatiotemporal VQ-VAE. In stage two the latent action model looks at the history frames and the next frame. An encoder infers a continuous action, then quantizes it into a small VQ codebook. The decoder reconstructs the next frame from the history and that action alone. The decoder cannot see future frames. The action code must therefore summarize the most explanatory change from the current frame to the next. The paper's experiments limit the latent action vocabulary to 8, so that a person can explore its semantics the way one learns a new gamepad. That number is Genie's experimental setting, not a universal rule for all latent action models.

The third stage is a decoder-only MaskGIT Dynamics Model. It receives historical video tokens and stop-gradient latent action embeddings, and predicts the next frame tokens. Its paper-specific main supervision is

$$
\mathcal L_{\mathrm{dyn}} =-\sum_{t=2}^{T}\sum_{j\in\mathcal M_t} \log p_\theta(z_{t,j}\mid z_{<t},z_{t,\setminus\mathcal M_t},a_{<t}),
$$

where $\mathcal M_t$ is the randomly masked token positions. During training the paper masks a relatively high ratio of middle-frame tokens at random. At inference it generates the next frame by multiple rounds of MaskGIT sampling. The latent action model is discarded at inference, except for the codebook. The user selects discrete action codes directly, and the Dynamics Model then autoregressively generates video.

Relative to the RSSM, this step changes the objective. The latent state no longer mainly serves the value function; it serves controllable observation generation. Latent actions let even unlabeled video learn interaction. However, one action code may mix "walk left," "camera pans left," and "background change" into a single discrete category, and the data distribution sets its semantics. Video tokens contain causal history, but they carry no stable object ID, map coordinates, inventory variable, or collision body. What happens after the character leaves the frame can only be guessed from the limited context and the model prior.

Genie therefore demonstrates that "low-dimensional changes that a human can control can be discovered from video." It does not demonstrate that "the game rules or the three-dimensional level have already been recovered." Research next turns to two paths. One lets a diffusion model predict the next observation directly from labeled actions, pursuing real time and visual detail. The other moves persistent state out of the pixel history.

#### GameNGen: predicting the next frame in real time with action-conditioned diffusion, and using noise augmentation to resist autoregressive drift

GameNGen [@web685daea96198] trains in two stages on DOOM. First it trains a reinforcement learning agent to play the original game, keeping that agent's action–observation trajectories from the random stage to the proficient stage. Then it adapts Stable Diffusion v1.4 into a next-frame model. Actions no longer pass through text prompts. Each key combination maps to a token, which replaces the original text cross-attention. Past frames are encoded by the VAE and concatenated with the current noisy latent along the channel dimension.

The paper uses velocity parameterization. Its specific loss is

$$
\mathcal L=\mathbb E_{t,\epsilon,T} \left[ \left\| v(\epsilon,x_0,t)- v_{\theta'}(x_t,t,{\phi(o_{i<n})},{A_{\mathrm{emb}}(a_{i<n})}) \right\|_2^2 \right].
$$

Training uses teacher forcing, so the history frames are real game frames. At inference the history gradually becomes the model's own output, and the two produce a distribution shift. GameNGen's key correction adds Gaussian noise of random strength to the history frame latents during training and tells the network the noise level. The model then learns to recover from a slightly corrupted history. This is not persistent state, only a kind of training against rollout error. The paper reaches real-time generation with a 64-frame context and a small number of DDIM steps. It also explicitly observes that predicted trajectories diverge rapidly from real ones, because of tiny velocity differences.

GameNGen can maintain phenomena such as health, ammo, doors, and enemies in the frame. That does not mean these quantities exist as queryable variables. They may be merely implicit results of pixel patterns and limited history. The same visual frame may map to different ammo, enemy states, or map positions. The model has no external key-value to tell them apart. The output contract is the next observation conditioned on actions, not a level file, script state, or collision geometry.

#### DIAMOND: the diffusion observation model preserves detail, while reward and termination are still handled by independent models

DIAMOND [@web3b350556ce83] applies diffusion to reinforcement learning world models. The motivation is that an overly compressed discrete latent may lose small objects, score items, and obstacle details that are important to the policy. DIAMOND does not adopt Genie's discrete video token dynamics. It learns the conditional next-observation distribution directly.

In a POMDP, DIAMOND approximates the state with a finite history and trains a conditional EDM denoiser. The paper-specific objective can be written as

$$
\mathcal L(\theta)= \mathbb E\left[ \left\| D_\theta(x_{t+1}^{\tau},\tau,x_{\le t}^{0},a_{\le t}) -x_{t+1}^{0} \right\|_2^2 \right].
$$

EDM preconditioning mixes the noisy next observation with the U-Net prediction according to the noise strength. Past observations are concatenated along the channel dimension, and actions enter the residual blocks through adaptive group normalization. The paper solves the reverse process with the Euler method. It emphasizes long-rollout stability under very few function evaluations. Reward and termination are scalar tasks, so a separate CNN-LSTM model $R_\psi$ predicts them. The actor-critic is then trained entirely in the imagination of this world model. The real environment only collects data at intervals.

Relative to GameNGen, this structure adds a closed loop of reward, termination, and policy for reinforcement learning. It also demonstrates that better visual detail may improve control. But observation diffusion, the reward model, and the termination model stay separate, and the three may contradict one another. The frame shows the reward object still present, yet the reward head has already issued the reward. The enemy in the frame has disappeared, yet the termination head still believes the episode continues. Facts outside the history window are likewise not explicitly stored. DIAMOND therefore still belongs to "limited pixel history + action-conditioned observation generation", and it cannot deliver three-dimensional state directly to the geometry layer.

#### V-JEPA 2: planning without generating pixels, but latent goal distance is not geometric distance

V-JEPA 2 [@srcW11] takes another route. It does not ask the model to reconstruct all pixels. Instead it predicts occluded or future video representations in a joint embedding space. Action-agnostic pretraining first lets the encoder learn motion and semantics from large-scale video. The encoder is then frozen, and a roughly 300-million-parameter, block-causal action-conditioned predictor, V-JEPA 2-AC, is trained on DROID robot data. It then autoregressively predicts the next-frame representation from the history representation, the end-effector state, and the action.

Planning uses Equation 5 of the paper. The current image and the goal image are encoded as $z_k,z_g$. The predictor $P$ rolls out candidate action sequences, and the energy is given by

$$
E(\hat a_{1:T};z_k,s_k,z_g) =\left\|P(\hat a_{1:T};s_k,z_k)-z_g\right\|_1, \qquad a_{1:T}^*=\arg\min_{\hat a_{1:T}}E.
$$

The paper samples candidates from a Gaussian action distribution with the Cross-Entropy Method. It keeps the low-energy elite trajectories to update the mean and variance, and executes only the first action each time. It replans after observing the new image. This is closed-loop MPC, not one-shot generation of an entire robot action segment.

The JEPA route has one advantage over pixel diffusion: it does not spend computation reconstructing texture detail that is irrelevant to the task. Latent-space planning is also faster than video diffusion planning. Its limitations are equally concrete. The L1 in the energy is a representation distance, not meters, collision distance, or joint safety margin. If the encoder is insensitive to transparent obstacles, thin ropes, or precise gripper clearances, low-energy trajectories may still fail physically. The paper also notes sensitivity to camera position and the accumulation of error over long rollouts. V-JEPA 2-AC outputs future representations and candidate action energies, not a renderable three-dimensional world.

#### How errors roll along the world model and enter spatial assets

RSSM error first appears as divergence between the prior $p(z_t\mid h_t)$ and the true posterior. The reward head and the actor are then optimized jointly on that erroneous latent state. Genie's error appears when erroneous tokens are treated as the history of the next frame, so latent action semantics drift as rollout proceeds. GameNGen and DIAMOND pass their errors directly into the next observation condition, and small artifacts in the frame may be amplified frame by frame. V-JEPA 2's error distorts the distance between future representations and the goal representation, which leads the CEM to select the wrong actions. An explicit three-dimensional state model may write a single erroneous generation into the long-term world frame, which makes the error more persistent than a pixel artifact.

Propagation cannot be prevented by better single-step PSNR alone. Each of these must be tested separately: single-step prediction, long rollouts, multiple action branches, revisiting after leaving, multiple cameras for the same state, object permanence, collision decisions, deterministic replay. A world model that outputs content to a reconstruction or engine layer must at minimum carry the state version, action history, random seed, visible region, generated region, confidence, and rollback point. Visual observation can only ever be one projection of state. It must not, conversely, become the sole basis for collision and scripting.

### L1: NeRF, Neural Rendering, and Queryable Appearance of 3DGS

For L1 the promotion gate is whether the appearance of the same scene can still be queried from a new camera. The comparison here covers NeRF, the rendering side of neural SDFs, and the original 3DGS. High rendering quality, real-time speed, or explicit Gaussian centers do not equal an explicit surface.

#### The Core Contradiction of Representation Evolution: Pixel Fitting Does Not Uniquely Determine the Surface

Classical MVS picks depths for pixels first, then fuses points or voxels. Implicit neural representations write the whole space as a continuous function. Through differentiable rendering, all sampled points jointly explain the image. That can share statistical information in sparse regions, and it also avoids fixing the grid resolution in advance. Yet it produces a new unidentifiability. A single set of training images can be explained jointly in several ways: by a thin surface, thick density, view-dependent color, or multiple semi-transparent layers. 3DGS in turn replaces the many MLP queries along each ray with sortable explicit ellipsoidal primitives, markedly improving training and rendering speed. Yet it defines no mesh adjacency for the primitives. Anyone tracing this evolution must ask two separate questions: "how does it render" and "how does it define a surface."

#### NeRF: Discrete Integration Along Rays, Not Attaching Pixels Directly to 3D Points

The original NeRF paper and its project page [@srcNERF-2020] represent the mapping from position and direction to volume density and color with a network $F_\theta$:

$$
(\sigma,c)=F_\theta(\gamma(x),\gamma(d)),
$$

$x\in\mathbb R^3$ is the spatial position and $d$ is the viewing direction; $\gamma$ denotes a high-frequency positional encoding. The original design makes density a function of position alone. Color, by contrast, depends on position and direction together. A camera ray $r(t)=o+td$ is sampled at $t_1<\cdots<t_N$ between the near and far bounds, where $\delta_i=t_{i+1}-t_i$. Discrete volume rendering in the NeRF paper can be written as

$$
\hat C(r)=\sum_{i=1}^{N}T_i\alpha_i c_i,\qquad \alpha_i=1-\exp(-\sigma_i\delta_i),\qquad T_i=\prod_{j<i}(1-\alpha_j) =\exp\left(-\sum_{j<i}\sigma_j\delta_j\right).
$$

$\alpha_i$ is the discrete probability that light terminates within this small segment. $T_i$ is the transmittance accumulated before that segment is reached. Training is supervised by pixel color, for example

$$
\mathcal L_{\mathrm{rgb}} =\sum_{r\in\mathcal R}\|\hat C(r)-C^{gt}(r)\|_2^2.
$$

The original NeRF runs a hierarchical coarse sampling pass, then performs importance resampling according to the coarse network weights. Occupancy grids, hash encodings, and proposal networks appear in later implementations. They are acceleration or parameterization evolutions, and should not be written back as components of the original paper. At runtime the method holds at least a camera/ray batch, sample points, network parameters, spatial bounds, and sampling/acceleration state. The basic procedure is:

**Indented procedure or pseudocode in the original draft**
\begin{verbatim}
Generate rays o,d from the pixel and the camera
→ sample t_i between the near and far bounds to obtain x_i=o+t_i d
→ query the network for sigma_i and c_i
→ compute alpha_i, the accumulated transmittance T_i, the composite color, and the expected depth
→ compare with the ground-truth RGB and back-propagate
→ update the network and the optional camera parameters
\end{verbatim}

NeRF can output color from arbitrary views, plus expected depth and density samples. Density is not a signed distance, however. Provided $\hat C$ is correct, optimization may accept floating density, thick surfaces, or view-dependent color that masks geometric errors. Beyond the sparse views, the function extrapolates color. Choosing a density threshold arbitrarily and running Marching Cubes is another surface estimate: shift the threshold and the shell inflates, splits, or disappears. No natural inside/outside exists. The native NeRF output contract should therefore be written as "radiance field + camera + bounds + sampling/acceleration structure + rendered depth and uncertainty." It must not be written as a "collidable mesh."

#### VolSDF and NeuS: Two Different SDF Differentiable Rendering Mechanisms

A neural network $f_\theta(x)$ can represent the SDF, and that is one way to make the surface a first-class object of the model. The surface is then defined as

$$
\mathcal S={x\mid f_\theta(x)=0}.
$$

That sign separates inside from outside. The normal follows as $n=\nabla f/\|\nabla f\|$. A true signed distance function satisfies $\|\nabla f\|_2=1$ almost everywhere, which is why an Eikonal regularizer is often added

$$
\mathcal L_{\mathrm{eik}} =\mathbb E_{x\sim\Omega} \left(\|\nabla_x f_\theta(x)\|_2-1\right)^2.
$$

Eikonal is a general constraint on SDFs. The density/opacity mappings below belong instead to individual papers. The two must not be conflated.

VolSDF [@srcN003] turns the SDF into volume density through the cumulative distribution of a Laplace distribution. One of the paper's sign conventions allows the map to be written as

$$
\sigma(x)=\alpha \Psi_\beta(-f_\theta(x)),
$$

Here $\Psi_\beta$ is the zero-mean Laplace CDF with scale $\beta$. Density magnitude is governed by $\alpha$, and the surface transition bandwidth by $\beta$. NeRF-style volume rendering integration is still used afterwards. VolSDF induces density from the SDF, and it offers an error-bound idea for sampling. Near the zero surface, however, it still explains the image through a density of finite width.

NeuS [@srcN002] does not simply reuse the Laplace density above. It derives weights with better unbiasedness from two sources: the logistic distribution and the variation of the SDF along the ray. Expressed with the logistic CDF $\Phi_s$, its discrete mechanism can construct adjacent samples as

$$
\alpha_i= \max\left( \frac{\Phi_s(f_i)-\Phi_s(f_{i+1})} {\Phi_s(f_i)},0 \right),
$$

Color is then composited with the forward transmittance. This simplified form rests on conventions about the orientation of the SDF, and about local monotonicity along the ray. The actual NeuS also estimates interval endpoints through the SDF gradient, and it carries an annealing strategy. Its core purpose is to give the main weight to the interval that crosses the zero level set. It also reduces the problem of a density peak that is systematically biased to one side of the surface. The label "VolSDF with a different CDF" loses the derivational differences between the two papers.

Both families usually combine RGB, mask, Eikonal, sparse depth, or normal terms. When the camera is inaccurate, the pose can also be jointly fine-tuned. Once the SDF is available, one queries $f$ on a bounded three-dimensional grid or an adaptive octree, runs Marching Cubes on the zero isosurface, and computes gradient normals. Compared with extracting a surface from a NeRF density threshold, this is more definitional. It is not automatically true, however. Eikonal favors regular, smooth distance fields, and thin sheets smaller than the sampling scale may be erased. An incorrect camera can be absorbed jointly by the smooth surface and the color network. Unobserved back sides may be filled in as a closed shell. When the training bounds are too small, the bounding box truncates the isosurface. A neural SDF's output contract must be accompanied by the coordinate scale, sampling bounds, inside/outside sign, zero-surface threshold, training mask, observation coverage, gradient quality, and surface-extraction resolution.

#### Original 3DGS: Covariance Projection, Forward Alpha Compositing, and Adaptive Densification

The original 3D Gaussian Splatting paper [@src3DGS-2023-N005] drops the continuous MLP in favor of a set of anisotropic Gaussians. Each $k$-th primitive carries a mean $\mu_k\in\mathbb R^3$, a rotation $R_k$, a scale $s_k$, an opacity parameter $o_k$, and spherical-harmonic color coefficients. To keep the covariance positive semidefinite, the paper parameterizes it by scale and rotation:

$$
\Sigma_k=R_k\operatorname{diag}(s_k^2)R_k^\top.
$$

The camera transform comes first, and the perspective projection is linearized at the mean. Let $W$ denote the linear part of the world-to-camera transform, and $J_k$ the Jacobian of the projection function at the camera-space mean. The screen-space covariance is then approximated as

$$
\tilde\Sigma'_k=J_kW\Sigma_kW^\top J_k^\top,\qquad \Sigma'_k=\bigl[\tilde\Sigma'_k\bigr]_{1:2,1:2}.
$$

In practice, the screen ellipse keeps the upper-left 2×2 block of this matrix, and adds numerical stabilization such as a minimum screen footprint. Once these low-pass details are ignored, the elliptical Gaussian response and the effective alpha at a pixel $x$ can be written as

$$
G_k(x)= \exp\left[-\tfrac12(x-\mu'_k)^\top(\Sigma'_k)^{-1}(x-\mu'_k)\right], \qquad a_k(x)=\operatorname{sigmoid}(o_k)G_k(x).
$$

Gaussians that cover the same tile are sorted by view-point depth. Color is then composited forward, front to back:

$$
C(x)=\sum_{k\in\mathcal N_x} c_k(d)a_k(x)\prod_{j<k}(1-a_j(x)).
$$

Viewing direction enters through the spherical harmonics, which let $c_k(d)$ vary. The original paper initializes the means from SfM sparse points. It then optimizes position, scale, rotation, color, and opacity under a combined loss of $L_1$ on RGB and D-SSIM. The GPU rasterizer culls by the view frustum and the screen bounding box, and writes the Gaussian instances into the key--value lists of the tiles they cover. It sorts by "tile ID + depth," then forward-composites and backpropagates in per-tile thread blocks. Its data structures are the Gaussian parameter arrays, tile ranges, sort keys, and optimizer state. No edge table or face table exists.

A fixed number of primitives struggles to cover flat regions and high-frequency detail at once. The original method therefore performs adaptive density control at intervals, driven by view-space position gradients and scale. Smaller primitives with high gradients can be cloned. Larger primitives with high gradients can be split into smaller ones. Primitives with low opacity, or an abnormally large size, are pruned, and opacity is periodically reset for redistribution. The mechanism can be written as:

**Indented procedure or pseudocode in the original draft**
\begin{verbatim}
Initialize Gaussian(mu, scale, rotation, opacity, SH) from SfM points
for each iteration:
frustum cull and assign to tiles
sort by tile_id and depth
front-to-back alpha compositing
compute L1 and D-SSIM, and back-propagate to update all parameters
accumulate view-space gradients, visibility, and scale statistics
at densification steps:
clone small primitives with high gradients
split large primitives with high gradients
prune low-opacity or pathologically large primitives
\end{verbatim}

Densification is a rendering-quality mechanism. It is also a source of resource risk and geometric instability. Incorrect poses, or training images that contradict each other, leave persistent pixel residuals; the optimizer may then memorize individual views with more floating Gaussians. Elongated ellipsoids can cover color along a ray while corresponding to no real thin surface. Sky and reflections form distant or large-scale primitives. Aggressive pruning deletes thin lines and semi-transparent objects. Weak pruning lets GPU memory, sort volume, and rendering time grow. The 3DGS output contract should therefore include the Gaussian parameters, spherical-harmonic order, coordinate/scale, cameras, training-image version, densification/pruning configuration, Gaussian count and resource curves, visibility, and low-confidence regions. Storing primitives explicitly is not the same as storing a surface explicitly. That a file is viewable as a PLY does not prove that its topology or collision is usable.

#### Chapter 7 Output Contract and the Link to Surfacing from Point Clouds

NeRF's native capability is continuous novel-view querying. Neural SDFs add an explicit zero level set, and Gaussians add real-time explicit differentiable rendering. From all of them one can export depth, normals, and surface samples, but they become a candidate mesh only after threshold, coverage, and topology validation. The next layer needs Chapter 7 to deliver: sample point positions, normals, and confidence; which cameras or primitives each point/face comes from; the observed regions and the filled-in regions; the implicit field bounds and the isovalue threshold; a reproducible version of the Gaussians or the network; and the coordinates, scale, and defect report of the visual mesh.

The next chapter no longer assumes that these points connect naturally into a surface. It asks instead "how to infer a surface from unordered, noisy samples of uneven density whose normals may point the wrong way." BPA, Poisson, alpha shape, and TSDF/Marching Cubes differ essentially in the assumptions they make about local sampling, global closure, and unknown space.

### L2: Camera, Depth, Point-Map, and Point-Cloud Spatial Samples

L2 natively delivers measurable spatial samples such as cameras, depth, point maps, point clouds, or surfels. Acceptance focuses on scale, pose, confidence, multi-view consistency, and free- versus unknown-space semantics, not on the number of points or on whether a file can be loaded.

#### Observability: Reconstruction Is First a Constraint Problem, Not a Model-Name Problem

The $i$-th image is $I_i$, with camera intrinsics $K_i$ and world-to-camera extrinsics $T_{cw,i}=[R_i|t_i]$; the 3D points are $X_j\in\mathbb{R}^3$. The ideal pinhole projection takes the form

$$
u_{ij}=\pi\left(K_i(R_iX_j+t_i)\right),
$$

where $u_{ij}$ is the 2D coordinate of point $j$ in image $i$, and $\pi([x,y,z]^\top)=[x/z,y/z]^\top$. One pixel yields a single ray, and cannot give the distance along that ray. Two pure-rotation images share no triangulation baseline. Without a known object size, a stereo baseline, IMU/GNSS, or a depth anchor, a monocular sequence can be recovered only up to an overall similarity transformation. So "1 unit" in the output of a classical reconstruction cannot be treated as "1 meter" in an engine without calibration. Reflective and transparent surfaces break the assumption that the same 3D point has a stable appearance. Dynamic objects break the static-world assumption.

An auditable reconstruction task must first declare whether the intrinsics are known, whether distortion is corrected, whether the images are synchronized, and whether the scene is approximately static. It must also state whether an absolute scale exists, how much unobserved area is allowed, and whether the downstream need is novel views, measured surfaces, or collision assets. The algorithm that follows does no more than make those assumptions concrete.

#### Incremental SfM: How Features, Geometric Verification, Triangulation, and Bundle Adjustment Connect

Structure-from-Motion Revisited [@srcG003] represents incremental SfM, which is not "a point cloud obtained from a single network inference." It is a graph algorithm with fallbacks and repeated optimization. Its core steps are as follows.

Step 1: Detect local features and build candidate matches. Within each image, detect keypoints $u_{ik}$ that are relatively stable in scale and rotation, and compute descriptors $d_{ik}$. Candidate image pairs can be produced by temporal adjacency, GPS, global bag-of-words retrieval, or exhaustive matching. For a candidate pair $(i,j)$, the nearest-neighbor ratio and mutual nearest-neighbor tests filter out obviously wrong descriptors. What is stored here is not a dense tensor. It is an image table, keypoint arrays, descriptor matrices, and a sparse edge table of image pairs. Wrong matches also sit very close together in descriptor space when textures are highly repetitive, so a large number of matches does not mean strong geometric constraints.

Step 2: Verify two-view geometry with RANSAC. When the cameras are uncalibrated, estimate the fundamental matrix $F$. When they are calibrated, the essential matrix $E$ can be estimated. Correct matches should satisfy the epipolar constraint

$$
\tilde u_j^\top F\tilde u_i\simeq0.
$$

In practice, the geometric distance is commonly approximated by the Sampson error:

$$
e_{\mathrm{samp}}= \frac{(\tilde u_j^\top F\tilde u_i)^2} {(F\tilde u_i)_1^2+(F\tilde u_i)_2^2+ (F^\top\tilde u_j)_1^2+(F^\top\tilde u_j)_2^2}.
$$

The original RANSAC paper [@srcG012] describes one general mechanism. Draw a minimal sample, fit a model, count inliers against a threshold, then re-estimate from all inliers. Let the minimal sample size be $s$ and the inlier ratio be $w$. To draw at least one all-inlier sample with probability $p$, the theoretical number of iterations is approximately $N=\log(1-p)/\log(1-w^s)$. A low inlier ratio therefore makes computation grow sharply. The same expression also shows that a fixed pixel threshold must be adjusted with resolution and noise. Pure rotation, planar scenes and extremely small baselines may make a homography fit better than an essential matrix. A system that does not compete among degenerate models will mistake image pairs that cannot be triangulated for good initializations.

Step 3: Select the initial image pair and triangulate. Decomposing the essential matrix yields candidate $R,t$. The four solutions are ambiguous. The one with the most positive depths resolves that ambiguity. For a feature track $\mathcal O_j$ formed by multiple images, a linear DLT gives an initial 3D point. Minimizing the multi-view reprojection error then refines it. A triangulation angle that is too small makes depth extremely sensitive to pixel noise. One that is too large may bring appearance changes and occlusion. Practical systems check positive depth, angle, reprojection error and track length together. They do not keep a point as soon as two rays intersect.

Step 4: Register new cameras and extend the observation graph. For an unregistered image, collect 2D–3D correspondences between its 2D features and existing 3D points, then solve for $R_i,t_i$ with PnP-RANSAC. Once registration succeeds, continue triangulating tracks that have not yet become points, matching the new image against its already-registered neighbors. The next image is usually chosen as the one with enough visible 3D points that are well distributed in space. Looking only at the number of correspondences concentrates points in one corner of the image and yields a numerically unstable pose.

Step 5: Local and global bundle adjustment. The expression below is a generic BA objective. It is not equivalent to all the robustification, camera models and scheduling details of any particular software package:

$$
\min_{{R_i,t_i,K_i},{X_j}} \sum_{(i,j)\in\mathcal O} \rho\left( \left\|u_{ij}-\pi\left(K_i(R_iX_j+t_i)\right)\right\|_{\Sigma_{ij}^{-1}}^2 \right).
$$

$\mathcal O$ is the edge set of the bipartite observation graph. $\pi$ includes the chosen perspective and distortion models. $\Sigma_{ij}$ is the pixel measurement covariance, and $\rho$ is a robust kernel such as Huber or Cauchy. The optimization variables carry gauge freedom in overall scale, rotation and translation. An implementation must therefore fix the first camera and the scale, remove the corresponding degrees of freedom, or add an identifiable prior. Otherwise the Hessian is singular. Distortion parameters and focal length can both be free at once. If the viewing angles are insufficient, the two will compensate for each other. The Jacobian matrix is sparse, because each observation depends on only one camera and one point. Engineering implementations usually eliminate point variables first via the Schur complement, then solve the smaller camera normal equations. Camera blocks, point blocks, observation edges, robust weights and residual statistics are therefore formal data structures, not debugging information.

The incremental loop can be written as:

**Indented workflow or pseudocode in the original manuscript**
\begin{verbatim}
features = detect_and_describe(images)
candidate_pairs = retrieve_pairs(images)
for each pair:
matches = mutual_ratio_match(pair)
model, inliers = ransac_F_or_E(matches)
if nondegenerate(model, inliers):
add_verified_edge(pair, inliers)
seed = choose_seed_by_baseline_and_coverage()
initialize_poses_and_triangulate(seed)
while an image has enough 2D–3D correspondences:
pose = pnp_ransac(next_image)
if pose passes quality gates:
triangulate_new_tracks()
local_bundle_adjustment()
prune_by_reprojection_angle_and_cheirality()
else:
quarantine_image()
global_bundle_adjustment_with_gauge_fix()
\end{verbatim}

How failures propagate. A wrong descriptor correspondence that passes through RANSAC forms a wrong track. A wrong track shifts the PnP pose. During triangulation, the shifted pose in turn interprets originally correct pixels as wrong depths. Bundle adjustment may reduce the overall residual while "jointly explaining" systematic errors by moving cameras and points, especially under repeated facades, rolling shutter and unmodeled distortion. Once downstream MVS treats these cameras as ground truth, the same plane falls at different depths in different views. Fusion then produces double walls or thick shells. The output contract of SfM must therefore include the coordinate system and scale state, plus each camera's intrinsics and extrinsics with quality/uncertainty. It must also record each point's 3D coordinates, color, track length, triangulation angle and reprojection statistics, along with the observation edges and the rejected images. A standalone PLY sparse point cloud is an incomplete contract.

#### MVS: From Plane Sweep to Cost Volume, Then From Depth Maps to Fused Surface Samples

SfM resolves "where the cameras are" and a small number of stable points. Only MVS estimates the depth of each reference pixel under known cameras. Classical plane sweep selects a set of fronto-parallel or general spatial planes for the reference camera. Consider a plane in reference camera $r$ with normal $n$ and distance hypothesis $d_k$. A plane-induced homography can transform source view $s$ into the reference view. Under one common coordinate convention, the form is

$$
H_{s\leftarrow r}(d_k) =K_s\left(R_{sr}+\frac{t_{sr}n^\top}{d_k}\right)K_r^{-1}.
$$

If the literature uses the opposite plane equation, normal or camera transformation direction, the signs in the expression change. An implementation must therefore write the coordinate convention into its synthetic-plane tests, and must not copy the matrix verbatim. For each pixel $x$ and depth layer $k$, the colors or features of multiple source images are sampled through $H(d_k)$ onto the same hypothesized 3D point. SSD, NCC or learned feature variance is then computed. At the correct depth, the multi-view appearance should be more consistent:

$$
C(x,k)=\frac{1}{N}\sum_s \left(f_s(H_{s\leftarrow r}(d_k)x)-\bar f(x,k)\right)^2.
$$

This is the generic cost-volume expression of plane sweep. It does not mean that all MVS methods use the same features, aggregation or depth regression. Under its world-to-camera parameter convention, MVSNet [@srcG004] writes it specifically as

$$
H_i(d)=K_iR_i \left(I-\frac{(t_1-t_i)n_1^\top}{d}\right) R_1^\top K_1^{-1},
$$

Camera 1 is the reference camera and $n_1$ is its principal axis. After unifying the convention, this is equivalent to the relative-pose form above. The paper warps deep features of the source views through this differentiable homography. Cross-view variance yields an $H\times W\times D\times C$ cost volume, which a 3D CNN regularizes. It then performs expectation regression on the depth probabilities and refines the result with the reference image. PatchMatch, traditional photometric propagation, cascaded cost volumes and subsequent Transformer MVS have different search and aggregation mechanisms, and cannot all be called MVSNet.

The depth estimate in discrete depth-probability implementations of the MVSNet type can be written as

$$
\hat d(x)=\sum_{k=1}^{D}P(k\mid x)d_k.
$$

Cost-volume GPU memory grows approximately with $HWD$. A depth range that is too wide wastes resolution. One that is too narrow excludes the true surface from the search space. Cascaded MVS uses coarse-level results to narrow the fine-level frustum interval. That is essentially allocating a limited depth sampling budget.

Each reference depth map is still not the final surface. Four types of checks are usually performed before fusion. The depth probability peak or entropy gives a photometric confidence. Reference points are projected into source views and back-projected, to check the reprojection position and relative depth difference. A visibility test excludes occlusions. Samples are merged only when multiple reference views support them. A depth $d$ is back-projected through

$$
X_w=T_{wc,r}\left(dK_r^{-1}\tilde x\right)
$$

into a world point. Neighboring points are aggregated by spatial distance, normal and color, to obtain a dense point cloud or to be fed into a TSDF. Practical data organization includes depth/normal/confidence maps stored per reference view, view-selection lists, depth ranges and sampling strategies. It also keeps the visibility relation from pixels to supporting views, and provenance indices from fused points to the original observations.

How failures propagate. In textureless regions the cost volume is nearly flat along the depth direction, so the probability expectation outputs a distribution average rather than the true surface. Repeated textures produce multiple peaks. Specular surfaces vary with viewpoint, which makes features at the true depth less consistent instead. Occlusion boundaries mix foreground and background features into the same voxel. A confidence threshold that is too loose yields double layers at fusion. One that is too strict removes thin poles, edges and weakly textured walls. If the "novel views" completed by video diffusion are inconsistent across views, the cost volume will also forcibly explain them as geometry. MVS output must therefore distinguish direct observations, model completion and low-confidence regions, and must retain the number of view supports for each point.

#### RGB-D, TSDF, and SLAM: Fusing History Also Requires the Ability to Revise History

RGB-D or LiDAR already provides a distance for each frame, but per-frame noise, holes and pose errors still require fusion. Curless–Levoy volumetric fusion [@srcG001] projects the spatial voxel center $x$ into the current depth map. The convention adopted here is "positive when the predicted depth from the camera to the voxel is smaller than the observed surface depth":

$$
s_i(x)=D_i(\pi(T_{cw,i}x))-z_i(x),\qquad f_i(x)=\operatorname{clip}(s_i(x)/\mu,-1,1).
$$

$D_i$ is the observed depth, $z_i(x)$ is the voxel's depth in camera coordinates, and $\mu$ is the truncation bandwidth. Some implementations define the opposite sign, but the zero level set is the same. When deriving normals and inside/outside, the sign convention must be stored along with them. The common per-voxel update of the historical TSDF $F$ and weight $W$ is

$$
F'(x)=\frac{W(x)F(x)+w_i(x)f_i(x)}{W(x)+w_i(x)},\qquad W'(x)=\min\left(W(x)+w_i(x),W_{\max}\right).
$$

$w_i$ may vary with sensing noise, incidence angle and depth, and not all implementations use constant weights. The neighborhood of $F=0$ is the candidate surface. Under the convention of this paragraph, the observed truncation band in front of the surface is positive. Voxels that have never been updated are unknown space. Unknown is not free space, a semantics that ordinary point clouds do not have.

KinectFusion [@srcG002] performs ICP between the current depth and the predicted surface obtained by raycasting the global TSDF. A common point-to-plane tracking objective is

$$
\min_{\xi}\sum_k w_k \left[n_k^\top\left(\exp(\hat\xi)p_k-q_k\right)\right]^2,
$$

$p_k$ is a point in the current frame, $q_k,n_k$ are the corresponding model point and normal, and $\xi\in\mathfrak{se}(3)$ is the pose increment. After linearizing for small motion, a 6×6 normal equation is solved. A coarse-to-fine pyramid enlarges the region of convergence. The engineering loop is "preprocess depth → predict the global model → establish correspondences → solve for pose → quality gating → fusion → update the surface". When quality gating fails, fusion must stop. Otherwise a single tracking failure will be permanently written into the map.

A large space cannot be allocated as a dense cube, so voxel block hashing is common. A hash table maps integer block coordinates to fixed-size voxel blocks. Within each block it compactly stores the TSDF, weights, color and the most recent update time. An octree or sliding submap may also be used. Online SLAM further maintains keyframes, feature/dense constraints, a covisibility graph and a pose graph. After a loop closure is detected, the general objective of the pose graph can be written as

$$
\min_{{T_i}}\sum_{(i,j)\in\mathcal E} \left\|\log\left(Z_{ij}^{-1}T_i^{-1}T_j\right)\right\|_{\Omega_{ij}}^2,
$$

$Z_{ij}$ is the measured relative pose and $\Omega_{ij}$ is the information matrix. The key point is that the old depth was written into the TSDF under the old pose, before the trajectory was corrected. Updating only the camera trajectory will not make a thick wall become thinner by itself. BundleFusion [@srcG006] demonstrates surface re-integration after global optimization. ElasticFusion [@srcG005] maintains surfels and a deformation graph, so that historical surfaces deform with the loop closure. The two follow different implementation routes, but both show that the map state must be able to be undone, deformed or recomputed.

How failure propagates. A depth bias changes the TSDF zero level set. Errors in normals and correspondences shift the ICP pose. The pose shift in turn aligns the next frame with the erroneous model, which forms positive feedback. A truncation band that is too wide erases thin walls and narrow gaps. One that is too narrow cannot form a continuous zero surface under noise. After weights saturate, new observations find it difficult to correct old errors. If a dynamic person is fused as a static surface, it leaves a ghost volume. A false loop closure may force two similar corridors to coincide. A complete output should include versioned poses, keyframes, replayable depth, voxel resolution, truncation distance, sign conventions, weights/coverage, free—occupied—unknown states, loop-closure edges, re-integration versions and the final isosurface.

#### Learned TSDF, DUSt3R, and VGGT: what feed-forward geometry changes is the initialization cost

Learned reconstruction writes the data prior into the traditional state. NeuralRecon [@srcG009] back-projects multi-view features into a sparse 3D volume. It uses sparse convolutions and a GRU to update the hidden state across fragments, predicts local occupancy/TSDF, and then extracts surfaces with Marching Cubes. It partly avoids the accumulation of "per-frame depth error followed by independent fusion". If the voxels completed by the network and the direct observations are not kept in separate layers, however, training-domain bias will be treated as a definite surface. NICE-SLAM [@srcG010] uses multi-level local neural feature grids to represent geometry and color, and continues to optimize them during tracking and mapping. It replaces the fixed grid function of the TSDF with a learnable implicit function, yet still requires poses, keyframes and local/global consistency management.

DUSt3R [@srcG011] does not need camera intrinsics or extrinsics up front. For an image pair $(I_i,I_j)$, it regresses two per-pixel pointmaps and their confidence directly. Each pixel no longer outputs only a scalar depth, but a 3D point in a reference frame of the image pair. The 3D relations among those pointmaps serve the matching role as well. For multiple images, every edge $e=(n,m)$ gives two pointmaps $X^{n,e},X^{m,e}$ in one local coordinate frame. The paper solves for a world-coordinate pointmap $\chi^v$ per image, and for a rigid transformation $P_e$ and a positive scale $\sigma_e$ per edge. Its global alignment objective is

$$
\min_{\chi,P,\sigma} \sum_{e=(i,j)}\sum_{v\in{i,j}}\sum_p c^v_{e,p} \left\|\chi^v_p-\sigma_eP_eX^{v,e}_p\right\|_2, \qquad \prod_e\sigma_e=1.
$$

$c$ is the network confidence. The constraint $\prod_e\sigma_e=1$ rules out the trivial solution in which all scales shrink to zero at once. The same $P_e$ maps both pointmaps of the pair to the world pointmap at once, so the predictions of a shared image must stay consistent across its different edges. The paper also gives an extension in which a pinhole model parameterizes $\chi^v$ and recovers the camera and depth. The core change compresses the past discrete chain of "descriptor matching→essential matrix→triangulation" into a feed-forward pointmap. Edges are then merged by 3D alignment rather than traditional 2D reprojection BA.

VGGT [@srcVGGT-2025] places multi-frame patch tokens and camera tokens into a shared Transformer. One forward pass then jointly predicts the camera, depth, pointmaps, and point tracks. The model reduces pairwise module boundaries, so sparse, low-texture inputs can also obtain strong initial values from the training prior. Its native data structure is still "frame-level camera tensors + pixel-level depth/pointmaps/confidence + tracks." It is not a half-edge mesh or a physical scene graph.

A more reliable production pipeline does not treat classical optimization and feed-forward models as alternatives. Instead it does this:

**Indented workflow or pseudocode in the original manuscript**
\begin{verbatim}
The feed-forward network outputs cameras, depth/point maps, trajectories, and confidence
→ build an observation graph from high-confidence trajectories and isolate low-confidence and dynamic pixels
→ perform local BA / pose graph correction with reprojection, scale anchors, and loop-closure constraints
→ re-fuse depth or align the point maps according to the corrected poses
→ store the direct-observation mask and the model-completion mask separately
→ proceed to the implicit field, Gaussian, or point-cloud surfacing stage
\end{verbatim}

This "feed-forward initialization + explicit constraint correction" explains how the optimization approach evolved. The network lowers the threshold for cold start and low-texture matching, while geometric optimization re-imposes the real observations of the current scene onto the result. The same framing also preserves honest boundaries. The training prior may fill a common wall over a real doorway. Confidence may be uncalibrated. The scale of long sequences will drift, and dynamic objects may produce mutually contradictory pointmaps. If these points are sent directly into Poisson, model hallucination will become a watertight fake wall. If they directly generate colliders, visual bias will escalate into gameplay errors.

#### The Chapter 6 output contract and the connection to the representation layer

The formal deliverable of Chapter 6 should be a replayable reconstruction package, not a single model file. Such a package covers camera coordinate conventions and units, intrinsics, distortion, extrinsics, timestamps, and quality. It also carries raw image/depth references and hashes, sparse tracks, and an observation graph. Per-pixel depth, pointmaps, normals, confidence, and dynamic/completion masks follow. The package also holds the TSDF's voxel size, truncation band, weights, and unknown space, plus versions for global alignment, loop closure, BA, and re-integration. Its export is the point cloud/initial mesh with the provenance mapping.

What this layer adds is "queryable distance and multi-view geometry." It still has no stable appearance function, and it does not necessarily have a closed, oriented, editable surface. NeRF, neural SDF, and 3DGS arrive in the next chapter to enhance novel-view and surface estimation with continuous fields or differentiable primitives. They must inherit the camera, scale, confidence, and observation coverage. At the interface they must not zero out the uncertainty of Chapter 6.

#### What point clouds lack is not a triangle file but neighborhood, orientation, and free-space semantics

A point cloud is usually written as $\mathcal P={(p_i,c_i,w_i,\ldots)}$, where $p_i\in\mathbb R^3$, and it may carry color, confidence, time, and provenance. It does not capture "which points are adjacent," "which side is the interior," or "whether there really is a surface between the points." Laser scanning, MVS, DUSt3R pointmaps, and 3DGS surface sampling have different noise distributions. Depth sensing is often biased along the line of sight. MVS produces mixed depth at occlusion edges. Feed-forward pointmaps may use a prior to complete low-confidence regions. Gaussian sampling, in turn, is controlled by the level-set threshold. Before surfacing, the provenance and the line of sight must be retained. Otherwise the algorithm can only guess at missing measurements from the spatial distribution of the points.

Neighborhood queries are the foundation of every subsequent algorithm. A static point cloud can use a k-d tree for k-nearest-neighbor or radius queries. When density varies strongly, the physical scale of a fixed $k$ shifts with the region. A fixed radius will fail to obtain enough points in sparse places. Voxel hashing suits large-scale nearest-neighbor search and deduplication, and an octree suits multi-scale solving. Preprocessing should first remove NaN/Inf and points outside the sensor range. It should then filter by confidence, line-of-sight consistency, and statistical outlier score. Voxel downsampling should aggregate color, normal, time, and provenance rather than keep an arbitrary point. For multi-layer thin structures, one should also avoid letting a single voxel average the two layers in front and behind into a nonexistent middle surface.

Point count also cannot directly represent surface quality. A dense region may simply be where the camera stayed longer. A sparse region may be reflective rather than empty of objects. Subsequent methods need a common input contract. At minimum it must store position, optional color, the raw line of sight or camera ID, confidence, sampling time, local density, and the "direct observation/model completion" label.

#### Normal PCA: the smallest eigenvector gives the axis, the viewpoint or graph propagation gives the orientation

Take the neighborhood $\mathcal N_i$ of point $p_i$. Compute the weighted centroid $\bar p_i$ and the covariance

$$
C_i=\frac{1}{\sum_jw_{ij}} \sum_{j\in\mathcal N_i} w_{ij}(p_j-\bar p_i)(p_j-\bar p_i)^\top.
$$

Let the eigenvalues be $0\le\lambda_0\le\lambda_1\le\lambda_2$. The eigenvector $v_0$ for the smallest eigenvalue is the normal axis of the locally fitted plane, because the points vary least in that direction. The local curvature index is often taken as

$$
\kappa_i=\frac{\lambda_0} {\lambda_0+\lambda_1+\lambda_2}.
$$

When $\lambda_0$ and $\lambda_1$ are very close, the local region does not look like a stable plane, so the normal confidence should be reduced. A neighborhood that is too small leaves the normal following noise. One that is too large averages sharp corners and the two sides of a thin layer together. Normal stability should be checked at several physical radii. The scale should be chosen according to the sensor resolution, not by point count alone.

PCA determines only the axis. It does not say which of $n$ and $-n$ is the outer side. Where the camera center $o_i$ is known for each point, one can enforce $n_i^\top(o_i-p_i)>0$ on visible surfaces, so the normal points toward the camera. If the application's convention is that the outward normal points away from the camera, flip all of them as a whole. Multi-view points should be oriented by the most trustworthy observation or by line-of-sight voting, not by an arbitrary choice of first frame. When no viewpoint is available, one can build an adjacency graph and a minimum spanning tree whose edge cost is $1-|n_i^\top n_j|$. Then flip adjacent normals from the root normal so that the dot product is positive, and use a known exterior point to decide the overall sign.

Normal propagation does not fail from local noise alone. When the two sides of a thin shell are mutual nearest neighbors in Euclidean distance, the graph will connect them across the gap. One side is then flipped incorrectly as a whole, and Poisson will propagate that orientation error into a large-scale spurious shell. The two sides of a sharp corner should start with different normals. Forcing global smoothing will round the edges. The normal stage must therefore output the neighborhood scale, the three eigenvalues, and the orientation confidence. It must also report the source of the orientation, edge markers, and all flip events.

### L3: Explicit Surfaces, Production Meshes, and Procedural DCC/CAD

L3 turns spatial samples, implicit fields, 3DGS, or design intent into editable explicit surfaces and production assets. The same geometric quality gate must apply to point-cloud-to-surface conversion, 3DGS surfacing, mesh topology and LOD/UV, and code-driven DCC/CAD. Marble is analyzed here as a multi-artifact product case.

The Marble part must adopt an evidentiary standard different from that of academic papers. As of August 6, 2026, the public World Labs materials are enough to verify product inputs, editing operations, and part of the generation time. They also verify the splat and mesh export specifications and the sample files. They do not publicly disclose the network architecture, training objective, training data, geometric benchmark, or physical benchmark in enough detail to reproduce the model. This chapter therefore discusses only the observable product contract and integration algorithms. It does not speculate about the vendor's internal model.

#### First Determine the Entity and Version: Marble Is a Product, Not a Public Algorithm Paper

In this survey, Marble refers specifically to the world-generation product that the official World Labs Marble documentation describes [@srcWL-MARBLE-OVERVIEW]. It does not refer to the spatial-reasoning dataset of the same name, and it does not refer to a multi-agent software framework. On April 2, 2026, the official documentation listed Marble 1.1 and 1.1 Plus, and it retains 1.0 and 1.0 Draft. For the internal network of these versions, the public pages give no layer count, no parameter count, no loss function, and no training corpus. Therefore one cannot write video diffusion, NeRF, 3DGS optimization, or any known world-model architecture directly as Marble's internal pipeline.

What can be established is the external capability boundary. Vendor documentation states that the product can create explorable worlds from text, a single image, multiple images, 360-degree panoramas, short videos, and coarse three-dimensional structure. It also states that the product can perform panorama editing, expansion, variants, world composition, and camera recording. It states that Gaussian splats, visual meshes, collision meshes, and image/video assets can be downloaded. These are feature claims in vendor documentation. They are not equivalent to independently measured geometric accuracy.

#### Public Input Contract: The Closer the Information Is to Multi-View Capture, the Stronger the Spatial Constraints

Text input carries semantics alone. It provides no true scale, no cameras, and no hidden surfaces. A single image adds one perspective observation. Depth and the regions behind occlusions remain unidentifiable. With multiple images, the user can specify front, back, left, and right, or use an automatic layout. Coverage can increase, but conflicts in focal length, exposure, and content may arise. A 360-degree panorama covers directions most completely, but a single-center panorama still lacks a translation baseline. Short videos provide continuous parallax, and the current official limit is within 100 MB. Chisel coarse three-dimensional structure explicitly provides layout constraints. This ordering is an analysis based on input observability. It is not an inference about Marble's internal weights.

The official pages give a typical product flow. It breaks into the following public I/O steps:

The user submits one or more permitted inputs and selects a product model version. The service first generates a panorama or a draft. At the panorama layer, the user can make local edits. The service then generates an explorable world. The official documentation currently estimates about 20 seconds for a draft and 5 minutes for a complete world. The times vary with the service version and load. The user can expand boundaries, generate variants, or compose multiple worlds in Studio. For export, the user chooses splats, visual meshes, collision meshes, a panorama, or recorded video.

No step here can prove that a particular neural network is used internally. From the exported primitives, all we can judge is that the final high-fidelity visual representation contains Gaussian splats. From the GLB we can judge that the service provides triangle mesh assets. We cannot infer backward which diffusion, reconstruction, or meshing algorithm generated these assets.

#### Public Output Contract: Splats, Visual Meshes, and Collision Meshes Must Be Used Separately

The official export specifications [@srcWL-MARBLE-EXPORT-M04] list four main asset classes. The panorama is a 2560×1280 equirectangular PNG. The Gaussian representation can be exported as an SPZ of about 2 million splats or of about 500,000 splats. A PLY export is also available at the same scale. SPZ is optimized for Marble rendering and file size, while PLY targets broader software compatibility. The Collider Mesh is a coarse GLB mesh with about 100,000 to 200,000 triangles. It is officially defined as intended for simple physics computation. High-quality visual meshes include a GLB with about 600,000 triangles and textures. Another GLB carries about 1 million triangles and vertex colors. According to the Mesh export documentation [@srcWL-MARBLE-MESH], generation takes up to about one hour, and typical files are about 100 to 200 MB.

The query contracts of these three representations differ. Splats answer camera queries for high-fidelity color and opacity, and they usually have no triangle topology. DCC and rasterization pipelines can edit, simplify, and UV-unwrap high-quality meshes. Such meshes are not guaranteed to be watertight, manifold, or physically correct. A collider mesh sacrifices visual detail for physics queries and cannot be used for high-quality rendering. The vendor also explicitly requires that visual rendering prefer splats or high-quality meshes, not the collider.

Coordinate conventions depend on the asset version. The export specifications page still carries the statement “default OpenCV coordinates”. The changelog [@webd179b99c4e6c] records a different rule. From December 11, 2025, the splats and meshes produced by world generation default to OpenGL. From January 1, 2026, users may choose OpenGL or OpenCV, and approximate scale and grounding handling were added. The two documents disagree in time, so integrators cannot apply any single fixed conversion unconditionally. Read the export task options together with the asset version. Confirm first that the source asset uses the OpenCV convention. Only then negate the Y and Z axes as documented, which converts that asset to OpenGL.

#### The Executable Marble Integration Algorithm Is Not Marble's Internal Generation Algorithm

The pseudocode below shows how to integrate the public exports into an engine safely. It describes a client-side asset pipeline proposed by this survey. It is not Marble's internal model algorithm.

**The text flow or pseudocode in the original manuscript**
\begin{verbatim}
Input: Marble task ID, export manifest, target engine coordinate system and units
1. Save the model version, task time, input types, and export options.
2. Download SPZ/PLY, visual GLB, collider GLB, and textures; compute the byte sizes and SHA-256.
3. Parse each file and reject out-of-range indices, abnormal node hierarchies, missing buffers, and non-finite coordinates.
4. Confirm OpenGL/OpenCV from the task record; perform the axis conversion only after confirmation.
5. Verify the “approximate scaling” with known scale anchors; the approximate scale must not be treated as a measured scale.
6. Attach the splat to the visual rendering layer; attach the HQ GLB to the editable visual asset layer.
7. Copy the collider GLB to a separate physics layer, generate a BVH, and check for degenerate triangles, self-intersections, and thin sheets.
8. Rebuild the NavMesh from the validated collision layer according to the specific agent radius, slope, and step parameters.
9. Run penetration, falling, door-width, slope, revisit, and coordinate-orientation tests.
10. If any item fails, isolate the asset, keep the original files and logs, and do not automatically replace the collider with the visual mesh.
Output: layered visual/physics assets, a provenance manifest, a validation report, and a rollback-capable version.
\end{verbatim}

The algorithm insists above all on separating the visual and physical layers. A visual mesh of about 600,000 faces, set directly as a dynamic triangle mesh collider, would raise broad-phase and narrow-phase costs. Holes and floating surfaces may also produce abnormal contacts. Rendering with the coarse collider of 100,000 to 200,000 faces, conversely, would lose materials and detail. A NavMesh cannot be generated directly from splats either. Walkability requires a continuous surface, slope, clearance and agent size.

#### Official Samples Can Prove the Format Is Usable, Not That the Geometry and Physics Are Correct

This project previously ran a read-only format spot check on the `rustic-kitchen` sample that the official export specifications page provides. The sample collider GLB parses as GLB v2 and contains 114,183 triangles, with no materials. The high-quality GLB contains 595,429 triangles plus materials and textures. The SPZ of about 500,000 splats is identifiable as a compressed SPZ file. The project keeps the full byte counts and digests in its source notes (open materials: `../sources/world_video_marble_notes.md`). These counts are of the same order of magnitude as the official specifications. They can prove that the file contract of this sample is parseable.

This spot check has no collision ground truth, scale benchmark, surface scan, navigation task or independent reference mesh. It therefore cannot prove that the collider error is small, that the visual mesh is watertight, or that the splats have physical meaning. It did not call a paid API or obtain model weights either. That also rules out calling it a reproduction of the Marble algorithm. The conclusion it supports is narrow: “the official sample matches the public format and the approximate scale statement at the file level.”

#### How Disclosed Failure Modes Propagate to the Engine Layer

The official Mesh export documentation explicitly lists the structures that are more prone to unevenness, holes, clumps and floating surfaces. Those are thin or complex structures, transparent or reflective surfaces, sky and background, and regions not adequately covered by the input. Their propagation paths are concrete. A missing thin railing makes holes appear in the visual mesh. If the collider is missing in the same place, a character may pass through. If the collider instead seals the hole, a passage visible in the image becomes impassable. A reflective floor reconstructed as an undulating surface causes jitter in falling bodies or vehicle contacts. Floating sky surfaces that enter the collision layer create invisible obstacles in the air. Clumps in uncovered regions generate walkable polygons when used for a NavMesh. Those polygons lead to platforms that do not exist.

The January 2026 changelog only says that assets are “roughly scaled and grounded,” that is, approximately scaled and grounded. It cannot replace metric calibration. Robotics or digital-twin applications must recalibrate using known lengths, gravity direction and the ground plane. Triangles in a GLB do not mean that it contains mass, friction, restitution, joints and object semantics. Those properties still require external measurement, rules or manual configuration.

Defenses should be enforced in place at each interface. At the input end, check video continuity, camera shake, exposure jumps and dynamic objects. At the export end, validate the file structure, coordinates, non-finite values and resource limits. At the geometric end, check manifoldness, self-intersections, connected components, normals and scale. At the physics end, use an independent collision proxy for falling, penetration and continuous collision tests. At the navigation end, rebuild and replay paths for the agent radius and door width. These different units cannot be merged into a single “overall quality score.”

#### Marble's Position in the Method Lineage and Its Interface to the Next Layer

Judged by the public contract, Marble productizes several steps that were originally scattered. They are multimodal input, world generation and editing, Gaussian visual representation, and visual mesh and collision mesh export. It comes closer to production assets than models that output only video. It also supplies one more coarse physics proxy than reconstructors that output only splats. The public materials cannot yet prove, however, that it has queryable object state, deterministic dynamics, NavMesh, scripting logic or long-term interactive state. An officially exported collision mesh also does not mean that physics acceptance in the target engine has been completed.

Marble is therefore reasonably classified as “a product system that provides multimodal world creation and multi-layer asset export.” It is not a public algorithm that can be reproduced from paper formulas. When it delivers to the next layer, splats, visual meshes and colliders should be treated as three independent versioned assets. That handover should also complete coordinate and scale confirmation, geometry repair, collision validation, NavMesh generation, object semantics, physics parameters and runtime safety gates. Its internal architecture should remain unknown until it is made public. It should not be filled in on its behalf from adjacent papers.

---

The four chapters that follow are not a set of unrelated papers. They describe one continuous spatial-construction chain. It starts from 2D observations with occlusion, noise and appearance ambiguity. From those observations it first recovers cameras and measurable spatial samples. It then organizes the samples into queryable appearance or continuous surfaces. Finally it converts them into production meshes with topology, UV and multiple levels of detail. Two changes happen at the same time as the methods evolve. The representation expands from "sparse points and discrete voxels" to "continuous implicit fields, differentiable explicit primitives, and oriented meshes." The solving moves from "repeated optimization for each scene" toward "learning a prior that gives an initialization in a single feed-forward pass, then locally correcting it with reprojection, closed loops, and surface constraints." Later methods reduce some bottleneck of the previous generation. They do not automatically supply the asset contract that the previous generation lacked.

Throughout this survey, four kinds of output need to be distinguished. The first is cameras and observation maps. They answer which camera a given pixel came from, and which images observed the same point. The second is sampled geometry, including depth, point maps, point clouds and surface samples with confidence. The third is rendering representations, including NeRF and Gaussians. These first answer what is seen from a novel view. Only the fourth is a production surface, with vertex-edge-face adjacency, stable normals, material parameterization, level of detail and explicit defect markers. Many problems trace back to renaming one kind of output directly as the next. The result is an asset that “appears to already have 3D but cannot be used once it enters the engine.”

#### Gaussian surfacing: adding geometric constraints, not swapping in a different export button

Gaussian surfacing methods evolve along three mechanisms. The first is to make 3D Gaussians adhere to a common surface. SuGaR [@srcN006] adds a surface-alignment regularizer to trained Gaussians, which makes them flatter and closer to the level set approximated by Gaussian density. It then samples points and normals from this field and reconstructs a mesh with Poisson. New Gaussians are bound to the triangle faces so that high-quality rendering and editing can continue. Here Poisson is a lossy geometric decision. Mesh quality depends on the level set, the normals and hole filling. Rendering PSNR does not guarantee it automatically.

The second mechanism changes the primitives themselves into local surface elements. 2DGS [@srcN007] replaces full 3D ellipsoids with 2D Gaussian disks. It defines depth more explicitly, through the intersection of rays with the local tangent plane. It also adds depth-distortion and normal-consistency regularizers. This reduces the degrees of freedom that stretch along the viewing direction, and it improves local geometry. The disk elements still share no topology, though. Intersecting disks, gaps, both sides of thin objects and unobserved regions still need a surface-generation algorithm to handle them.

The third mechanism defines or assists a continuous surface field from the Gaussians. Gaussian Opacity Fields [@srcN008] constructs an extractable level set from accumulated opacity. 3DGSR [@srcN009] combines implicit surface reconstruction with the Gaussian representation. PGSR [@srcN010] imposes geometric constraints on approximately planar regions. Together they push information beyond color fitting into the objective, in the form of depth, normal, plane or implicit-field terms. Achieving “looking right” through incorrect thickness then becomes harder. The price is extra weights, scales and surface-extraction thresholds. The pipeline must still pass through isosurface sampling, connectivity checks, hole repair and simplification.

How failures propagate. A tiny error in the input camera can make the same edge form two clusters of Gaussians. The surface regularizer compresses the two clusters into a double-layer mesh. Poisson then closes the gap between the two layers into an inner shell. Relying on the regularizer can flatten a low-texture wall, and a doorway may then be treated as missing data and filled in. The rendering Gaussians and the collision mesh may instead be simplified independently, without shared coordinates, object IDs and versions. The visual wall and the physical wall will then be misaligned. The surfacing output is therefore divided into at least three artifacts. They are raw/compressed rendering Gaussians, a visual mesh with provenance and confidence markers, and collision candidates to be validated separately later. The same coordinates, units, object tiles and content hash link the three.

#### BPA: building triangles from the local visibility of a rolling ball

The original Ball-Pivoting Algorithm paper [@srcP002] assumes a sufficiently smooth source surface, sampled densely enough to be compatible with the ball radius $r$. Three points can form a candidate triangle if they can jointly support a ball of radius $r$. The ball must touch all three points, and no other samples may lie inside it. The algorithm first finds a seed triangle whose orientation is compatible with the input normals, and puts its three edges into the active front. For a directed edge, the ball keeps touching both endpoints of the edge and rotates around that edge. The first new point it touches forms an adjacent triangle with that edge. The front is then updated until no edge can be pivoted. Multi-radius BPA uses small balls to recover detail, then larger balls to connect sparser regions.

**Indented workflow or pseudocode in the original manuscript**
\begin{verbatim}
Build a k-d tree and estimate consistent normals
for r in the sequence of radii from small to large:
find seed triangles that are not yet covered and whose supporting ball is empty
add the three directed edges to the active front
while the active front is non-empty:
take one directed edge e
query candidate points in the rolling-ball sweep neighborhood of e
select the point q that is touched first, keeps the ball empty, and is orientation-compatible
if q exists:
output triangle (e.start, e.end, q)
update or cancel the active edge
output boundary loops, unused points, and non-manifold conflicts
\end{verbatim}

The data structures are a spatial index, an active half-edge front, the set of generated triangles, and point/edge usage state. The precondition for correctness is not that “any point cloud can roll out a surface.” It is sampling dense enough, noise small relative to $r$, consistent normals, and a local surface that can accommodate the ball. If $r$ is smaller than the point spacing, the surface fragments. If $r$ is larger than a real hole, the ball will cross the hole. If it is larger than the spacing of a thin layer, the ball will bridge the two layers. A noise point can become the pivoting contact point first. Wrongly oriented normals will reject correct triangles or produce flipped faces.

BPA does not solve a global equation. It preserves local holes and boundaries, and it does not naturally close unknown regions. This is both an advantage and a limitation. The output contract must carry the radius sequence and the boundary loops. It must also carry the proportion of uncovered points, the triangle aspect ratios, and the supporting points of each face. An open boundary may be a capture gap or a real doorway. It cannot be uniformly recorded as an error to be repaired.

#### Alpha shape: using the scale parameter of the Delaunay complex to decide which empty balls to keep

Alpha shape begins with the 3D Delaunay tetrahedralization of a point set. Every Delaunay simplex has a circumsphere that contains no other sample points. Fix a scale parameter $\alpha$, and keep those vertices, edges, triangles and tetrahedra whose circumsphere radius satisfies the threshold. The boundary of the kept complex is the alpha shape. Implementations parameterize this differently, using either the radius $r\le\alpha$ or the squared radius $r^2\le\alpha$, so read the library convention. The CGAL 3D Alpha Shapes documentation [@srcP007] draws a further distinction between general and regularized results. Regularized results can remove isolated lower-dimensional simplices that belong to no 3D cell.

Alpha shape shares a "ball scale" with BPA, but the two methods reason differently. BPA rolls the ball locally along an existing front. Alpha shape builds the global Delaunay complex first, then filters it by the empty-ball scale. A small $\alpha$ preserves detail but yields many fragments. A large $\alpha$ fills in grooves and eventually approaches the convex hull. Delaunay requires exact predicates or controlled symbolic perturbation on cospherical and nearly coplanar points. Direct tetrahedralization of a million points also usually requires far more memory than local BPA. Alpha shape suits one question particularly well: is a topology stable with respect to scale? A doorway that shows up in only a very narrow parameter range is not a stable asset feature. The original sightlines and images should then be consulted for verification.

A single "best alpha" mesh should not be the only output of this method. It should also save how connected components, cavities, boundaries and volume change across the parameter sweep. Choosing alpha by tuning until it "looks best" puts the selection process into the asset provenance as well.

#### Poisson: treating oriented points as the gradient of an indicator function to solve for a global implicit surface

The original Poisson Surface Reconstruction paper [@srcP003] starts from the ideal solid indicator function $\chi(x)$, which jumps from inside to outside at the surface. Its gradient in the distributional sense is related to the surface normal field. Splat and smooth the normal of every oriented point into a spatial vector field $V(x)$, then solve

$$
\min_\chi\int_\Omega\|\nabla\chi(x)-V(x)\|^2dx.
$$

The Euler--Lagrange equation of that objective gives the Poisson equation

$$
\Delta\chi=\nabla\cdot V.
$$

The actual algorithm represents $\chi=\sum_kx_kB_k$ with local basis functions on an adaptive octree. It assembles a sparse linear system $Ax=b$, solves for the coefficients with a multiscale solver or an iterative method, then extracts the surface at the isovalue $\chi=\tau$. The octree depth controls the smallest cell and the memory. The number of samples per node affects adaptive refinement. Sample density estimation can help trim extrapolated surfaces supported by very few points. The original Screened Poisson paper [@srcP004] adds a point-value constraint beyond gradient fitting, which it abstracts as

$$
\min_\chi \int\|\nabla\chi-V\|^2dx +\lambda\sum_i(\chi(p_i)-\chi_0)^2,
$$

This term brings the isosurface closer to the samples and reduces the over-smoothing of the original Poisson. A $\lambda$ that is too large will also follow the noise. The continuous objective above explains the mechanism. The specific octree basis functions, the discretization of the screening term and the solver should come from the paper and the implementation version.

**Indented workflow or pseudocode in the original manuscript**
\begin{verbatim}
Input oriented points, confidence weights, and the reconstruction bounding box
→ build an adaptive octree and splat the normals into a vector field V
→ assemble the Laplacian sparse matrix A and the divergence right-hand side b
→ if Screened Poisson is enabled, add the point-value term
→ iteratively solve Ax=b and record the residual
→ determine the iso-value tau from function statistics at the samples
→ extract the iso-surface on the octree and clip by support density
→ output the mesh, per-vertex density, boundaries, connected components, and parameters
\end{verbatim}

Poisson is correct only when the input has fairly consistent oriented normals and the user accepts the global smoothing and closure prior. It can bridge small gaps and suppress noise, and it tends to produce watertight surfaces. Without the original sightlines, however, there is no way to know whether a gap is a scanning hole or a door or window. A wrong normal does not merely flip one local triangle. It changes $\nabla\cdot V$, and the error propagates through the global equation into a large shell or a wrong inside/outside. The bounding box, octree depth and isovalue threshold also change volume and thin structures. Watertightness is a property of the algorithm, not evidence of scene authenticity. Surfaces filled in from low sample density must be marked separately. Their provenance must be retained as well, so that the surfaces can be verified against the original rays.

#### TSDF＋Marching Cubes: retaining free/unknown semantics before extracting the isosurface

Suppose the point cloud comes from a sensor sequence that still retains cameras and depth. Reconstructing a TSDF from the observation SDF and weight formulas in Chapter 6 is then usually more auditable than discarding the sightlines and running Poisson. A TSDF distinguishes voxels in front of the surface that were observed as free from voxels that were never seen. The fusion weights can also enter the surface-extraction confidence.

The original Marching Cubes paper [@srcM001] reads the signs of the eight corners of each regular voxel cell against the threshold $\tau$. Those signs give an 8-bit index, which selects one of 256 configurations. When the scalar values at the endpoints $x_a,x_b$ of an edge are $f_a,f_b$, the intersection point is the linear interpolation

$$
x=x_a+\frac{\tau-f_a}{f_b-f_a}(x_b-x_a).
$$

An edge cache must deduplicate the vertices that edges of adjacent cells share. Without it, the result is a triangle soup that is geometrically coincident but topologically disconnected. On saddle or face-ambiguous cells, classic table lookup may choose different connections. Consistent disambiguation rules can reduce the cracks that result. When resolution is insufficient, zero crossings on both sides fall in the same cell and thin walls disappear. Truncation and weights may in turn form unreliable isosurfaces in unobserved regions. Surface extraction should skip cells with insufficient weight or with unknown corners. It should also allow open boundaries in the output rather than filling the entire volume by default.

Marching Cubes data structures include regular volumes or sparse voxel blocks, a per-cell case index and a cross-cell edge vertex cache. The same structures also carry vertex position/normal arrays and triangle indices. Normals can come from the central-difference gradient of the TSDF. At block boundaries, however, neighboring blocks must be accessed or a consistent halo used; otherwise visual seams and incorrect face orientations appear.

#### Method Selection, Failure Propagation, and the Output Contract

No single method of surfacing point clouds is optimal for all inputs. If genuine open boundaries are to be preserved and sampling is approximately uniform, multi-radius BPA can be tried first. If concave–convex topology is to be explored across scales, alpha shapes can be used. If consistently oriented points are available and globally smooth completion of unmeasured regions is acceptable, Poisson/Screened Poisson can be used. If depth, pose and line of sight are still available, TSDF plus Marching Cubes can preserve free–unknown semantics. Results from multiple methods can be inspected side by side. Majority voting still cannot replace observation, because the methods share the same input errors while their priors differ.

The error chain here is clear. Depth/pose noise changes point positions. Neighborhood scale interprets that noise as curvature, or blends thin layers together. Wrong normals make BPA reject correct connections and make Poisson flip shells globally. An oversized ball or alpha spans genuine holes. An overly deep octree chases noise, while an overly shallow octree loses detail. TSDF resolution and the truncation band erase thin objects. Marching Cubes ambiguity or edge-cache errors create cracks. A log that claims “automatic surfacing succeeded” but checks only that the file is non-empty has covered none of these mechanisms.

The surfacing stage should deliver the reference point cloud and its attributes, the spatial index configuration, and normal scale and orientation records. It should also report the algorithm used and all of its scale parameters, plus per-face support/observation density. A completion face mask, boundary loops, connected components and non-manifold edges belong in the delivery as well. So do an initial self-intersection screening and the bidirectional distance to the original point cloud. Surfaces extracted from a TSDF must also retain the voxel weights and unknown space. Surfaces extracted from Poisson must retain the octree depth, iso-value threshold and density clipping.

The output may by now already be a set of triangles, yet it is not necessarily a production mesh. Duplicate vertices, misoriented faces, T-junctions and self-intersections all remain. So do extremely thin triangles, semantically meaningless hole filling, missing UVs and excessively high face counts. The task of Chapter 9 is not to guess the surface again. It is to organize, repair, simplify and parameterize the candidate surface into a visual asset that can enter DCC tools and engines, without concealing these provenance risks.

#### The Half-Edge Structure Upgrades “What a Face Looks Like” to “How Faces Connect”

Exchange files often represent a mesh with a vertex array $V\in\mathbb R^{n\times3}$ and triangle indices $F\in\mathbb N^{m\times3}$. However identical their vertex coordinates are, two triangles remain topologically disconnected as long as the indices do not refer to the same vertex. Inappropriate welding does the reverse, gluing together geometrically close but semantically independent sheets. The first step of assetization is to build explicit adjacency from the triangle soup.

The half-edge structure splits each undirected edge into two oppositely directed half-edges. Each half-edge stores its origin, its twin half-edge, the next half-edge in the same face, and the face it belongs to, at a minimum. A vertex stores one outgoing half-edge, and a face stores one edge. Following next walks around a face, and following twin.next walks around a vertex. A boundary half-edge has no twin face, or connects to a dedicated boundary loop. For a closed two-dimensional manifold, each undirected edge must be incident to exactly two faces with opposite directions, and the vertex neighborhood must be homeomorphic to a disk. For a manifold with boundary, an edge may be associated with only one face, and the boundary must form a single sector at a vertex. Three faces sharing one edge are non-manifold, and so are two independent face fans meeting at only one vertex.

During construction, place the directed key $(a,b)$ in a hash table. Look up $(b,a)$ to build the twin. When the same undirected key appears more than twice, record the conflict. Do not pair two of them arbitrarily. At the same time, independent attribute indices must be stored. The two sides of a UV seam at the same position usually need different texture coordinates and tangents. Rendering formats may duplicate vertices as well. The topology layer can share geometric vertices. The rendering layer instead expands by the combination of position, normal, UV, material and skin. If the two layers are confused, “welding duplicate vertices” destroys hard edges and UVs.

A consistency audit can draw on the Euler characteristic $\chi=V-E+F$, on connected components, on the number of boundary loops and on the number of incident faces per edge. A single Euler number cannot point to the location of a defect, however, nor can it determine whether a hole is genuine. The half-edge structure pays off because every later repair and edge collapse can query its topological impact locally.

#### Repair Order: First Diagnose Defects, Then Decide by Asset Intent Whether to Modify

A reliable repair pipeline follows an order that runs from semantics-free steps to semantics-bearing ones.

Parse and numeric gate. Validate index ranges, NaN/Inf in vertices/normals/UVs, element counts, and the file budget. Apply axis, handedness, unit and transform hierarchy, then fix the reference space. Negative scale flips face winding, and non-uniform scale requires transforming normals with the inverse-transpose matrix. When an importer silently drops illegal faces, later reports must still be able to say what was lost. Degenerate and duplicate. Compute the triangle area $A_f=\frac12\|(v_1-v_0)\times(v_2-v_0)\|$. Delete duplicate indices and faces whose area is below a threshold relative to the bounding box. Merge geometrically duplicate vertices within a controlled tolerance, but lock object boundaries, material edges, UV seams and skeleton seams. Set the tolerance according to units and local scale. The same absolute value cannot apply to a city and to a cup. Connectivity and orientation. Build the half-edge structure and enumerate connected components, boundary loops and non-manifold edges. For each manifold component, propagate consistent winding with BFS. Closed components can use signed volume.

$$
V_s=\frac16\sum_{(a,b,c)\in F}a\cdot(b\times c)
$$

Determine the overall orientation, but for open surfaces the outside cannot be decided by volume alone. The original normals and the camera line of sight should be consulted.

Non-manifold handling. Distinguish duplicate shells, T-junctions, multi-face shared edges and contacts at a single vertex. Splitting vertices by face fan can restore local manifoldness. Another option is to revisit the reconstruction layer and recompute. A split like this makes the data structure legal, but it may create cracks in collision, so it must be recorded as a repair event. Holes and self-intersections. For boundary loops compute size, planarity, observation support and semantic labels. Only automatically fill small scan gaps that satisfy the policy. Doors, windows, pipe openings and garment edges must not be sealed shut in pursuit of watertightness. Small holes can be projected onto a local plane for triangulation, then smoothed and projected back onto the constraints. Large holes should be handed to provenance-constrained hole filling. Use an AABB tree or BVH to find candidate triangle pairs. Exact triangle intersection then excludes shared adjacency and identifies genuine self-intersections. Recompute attributes. Decide shared or split normals according to hard-edge angle and material/UV seams, and generate tangents. Retain the original scan colors and material provenance. Finally, verify boundaries, non-manifold edges, self-intersections, connected components and bidirectional surface distance once more.

The MeshFix paper [@srcM004] and CGAL Polygon Mesh Repair [@srcM005-CGAL-REPAIR] supply tools for local defects. Those tools cannot know the semantics of a hole, however. Methods such as voxel watertightening can reconstruct a two-dimensional manifold from the boundary between occupied voxels and free space connected to the outside. They then project it back onto the reference surface. Topological legality improves, but a voxel minimum feature scale enters the result. The method may also seal structures that should have remained open. A repair report must not conflate three operations: “deleting illegal degenerate elements,” “restoring obvious breaks,” and “adding surfaces based on priors.” The last must not be disguised as a cleanup operation.

Repair also needs a stopping condition. Consider a component with many interpenetrating thin layers, an unknown inside/outside, and faces that carry no provenance. Repeated local patching will let the file pass manifold checks while it drifts ever further from the real scene. At that point, return to TSDF/point-cloud surfacing or re-acquire data rather than continue beautifying.

#### QEM Simplification: Why Edge Collapse Is Fast, and Why Semantic Constraints Must Be Locked

Surface Simplification Using Quadric Error Metrics [@srcM002] writes each triangle's plane as $p=[a,b,c,d]^\top$, with $a^2+b^2+c^2=1$, and a point's homogeneous coordinates as $\bar v=[x,y,z,1]^\top$. The squared distance from a point to the plane is

$$
(p^\top\bar v)^2=\bar v^\top(pp^\top)\bar v.
$$

A face therefore contributes a 4×4 symmetric matrix $K_p=pp^\top$. The quadric error matrix of a vertex $v$ sums over its incident faces, $Q_v=\sum_{p\ni v}K_p$. The cost of collapsing a candidate vertex pair $(v_i,v_j)$ to a new point $\bar v$ is

$$
\Delta(v_i,v_j\rightarrow\bar v) =\bar v^\top(Q_i+Q_j)\bar v.
$$

If the upper-left 3×3 block of $Q=Q_i+Q_j$ is invertible, the optimal position can be solved under the constraint that the homogeneous last component equals 1. If the block is singular, choose the lowest-cost option among finite candidates such as the two endpoints and the midpoint. The original paper also allows vertex pairs not necessarily connected by an existing edge, to promote aggregation. Engineering implementations aimed at topology-preserving assets usually restrict candidates to edges and additionally check local topology. The algorithm places all legal candidates in a min-heap. It pops the lowest cost each time, then checks the entry version and topological legality. The collapse follows, $Q_i+Q_j$ accumulates, and only the local adjacent edges are updated.

**Indented Procedure or Pseudocode in the Original Manuscript**
\begin{verbatim}
Compute the plane quadric Kp for each face
Accumulate Qv for each vertex and add boundary/feature constraints
For each edge allowed to collapse, compute the candidate position and cost, and push them into the min-heap
while the face count is above the target and the minimum error is below the budget:
pop the lowest-cost edge whose version is still valid
if it violates the link condition or locked attributes, reject the edge
perform the collapse and update half-edges, faces, attributes, and Q
recompute the local candidates and write versioned heap entries
\end{verbatim}

QEM's correctness presupposes that the local planar quadric distance can represent the geometric quality to be preserved. It does not automatically protect texture, semantics, or physical function. Without restrictions, boundaries shorten, sharp corners round off, two closely spaced thin layers may become connected, and door gaps narrow. Merging the two sides of a UV seam stretches the texture. Material edges and hard normals are lost, and skinning weight blending makes joints collapse. Common constraints forbid collapses across material, object, and semantic edges. Boundary points may move only along the boundary. Silhouettes and feature edges receive high-weight constraint planes. Others limit normal flips, minimum triangle area, local Hausdorff error, UV distortion, and bone weight change. The link condition is checked before collapsing, to avoid creating non-manifold geometry.

Collision-critical features must also be locked separately. A step of a few millimeters that QEM smooths away may be visually imperceptible, yet it changes whether a character can step over it. A narrow door that becomes wider lets a large agent pass through incorrectly. Visual LODs and collision proxies must not share a single simplification result driven only by triangle count.

#### LOD and HLOD: Converting Geometric Error into Screen-Space and Gameplay Error

A single low-poly model is not a complete LOD. The offline stage generates $M_0,M_1,\ldots,M_L$. Each level should record the bidirectional distance to the reference surface $M_0$. It should also record normal and silhouette differences, UV/material error, topological change, and face count. A world-space error $\epsilon_w$ at focal length (in pixel units) $f$ and depth $z$ has an approximate projected error

$$
\epsilon_{px}\approx\frac{f\epsilon_w}{z}.
$$

This is the basis for selecting LODs by screen-space error, which adapts better to different FOVs and resolutions than a fixed distance threshold. Switching should have a hysteresis band, so the camera does not repeatedly change levels near the threshold. Abrupt geometric changes can use short dithering or cross-fading, but transparency, shadows, and collision cannot necessarily be blended without artifacts. Progressive meshes can store the inverse of each edge collapse, the vertex split, and stream detail continuously. Cluster or meshlet streaming instead cuts the mesh into chunks with bounding volumes and error bounds, selected by visibility and budget.

HLOD packs multiple distant objects into proxy meshes with baked materials. Draw calls drop, but independent object IDs, animation, and occlusion relationships are lost. The mapping from HLOD back to the source objects must be preserved. It must also be made explicit when an interactive object switches back from the proxy to the real entity. Screenshots alone do not constitute acceptance of a visual LOD. Silhouettes, normal maps, shadows, reflections, skinned poses, and switch-frame peaks must be covered too. Collision and navigation should be versioned independently, and every visual asset update should run an alignment regression.

Each LOD level should set two budgets. A visual budget limits projection error and material distortion. A runtime budget limits vertices, triangles, meshlets, VRAM, and draw calls. Meeting only the latter three without the former is "a faster wrong asset". Meeting only the former while running out of VRAM at runtime is likewise not a deliverable asset.

#### UV, Tangents, and Texture Baking: The Last Step That Turns Geometry into a Shadable Asset

Reconstructed meshes often carry per-vertex colors. Colors can also be queried from a NeRF/Gaussian. Neither route gives a stable two-dimensional parameterization. UV authoring answers four questions: where to cut seams, how to unwrap each chart, how to pack charts into a texture atlas, and which observations supply the textures on a new surface.

First, the face graph is cut into topological disk charts, based on high curvature, occlusion, material boundaries, and invisible regions. Unwrapping solves for each vertex $u_i\in\mathbb R^2$ and aims to reduce angular, area, or length distortion. Methods of the LSCM family pin at least two vertices and minimize the residual of the local conformal equations. No unwrapping can preserve every length and area perfectly at once, so each chart should report its angular/area distortion and flipped triangles. Charts are then scaled by texel density and packed under a given padding. The padding must cover mipmap and compression filter dilation. Too little causes color bleeding at distance.

Texture baking starts at each surface texel and traces back to a three-dimensional point. That point is then projected onto candidate source images, or it queries the radiance field/Gaussians. Source image selection should trade off visibility, incidence angle, resolution, exposure, and dynamic occlusion. Multi-view colors require exposure compensation and seam blending. Completely unobserved faces must be labeled as inpainting/generation, and must not silently copy their neighborhood. Normal maps require a consistent tangent space, and a vertex's position, normal, UV, tangent, and handedness must match the target engine's conventions. Positions may be identical at a UV seam, but rendering vertices must be split.

The asset data structure is therefore more than positions and indices. It should include a topological half-edge mesh; vertex buffers laid out by render attribute; submesh/material slots; UV0 and optional lightmap UVs; tangents and normals; textures and color spaces; skeleton/skinning; object hierarchy and transforms; LOD/HLOD relationships; and provenance and repair records. Exchange formats such as glTF can serialize only the subset of render assets that their specifications support. Half-edge topology, collision semantics, LOD/HLOD relationships, and provenance and repair records usually still require dedicated extensions, `extras`, sidecars, or engine conventions. They must be verified by read-back with the target consumer. Writing custom fields cannot be equated with semantics being preserved.

#### Procedural Modeling: Constructing Space Directly from Parameters, Constraints, and Code

Procedural modeling is not an "automation button" after reconstruction. It is a construction route standing alongside observational reconstruction and generative models. Reconstruction uses images, depth, or point clouds as evidence to solve for unknown cameras and surfaces. Generative models fill in plausible content from a data distribution. Procedural modeling instead treats dimensions, topology rules, assembly relationships, semantic constraints, and random seeds as known intent, and directly solves for a scene that satisfies the contract. This route often suits rooms, roads, pipelines, shelving, level modules, building facades, industrial parts, and synthetic training scenes. Their repeated structures and design rules are more concise than per-vertex coordinates. The cost is that what the code generates is "the world defined by the rules," not an automatic measurement of the real world. Wrong rules can generate wrong assets in bulk, stably and deterministically.

Let the parameter vector be $\theta$, the host and dependency versions be $v$, the random seed be $s$, and the procedural builder be $G$. Then the output asset is not an abstract "model" but should be written as

$$
A = G(\theta,s,v) = {M_{visual},M_{collision},H,U,P,Q},
$$

where $M_{visual}$ is the visual mesh and $M_{collision}$ is the independent physics proxy. $H$ is the object hierarchy and stable IDs, $U$ is UV/material, $P$ is provenance, parameters, and the build manifest, and $Q$ is the verification result of geometry and runtime queries. The determinism requirement is not that the asset "looks the same". Once normalized parameters, versions, and seed are fixed, the key output hash or normalized geometry signature must be identical. If the host evaluation, floating-point kernel, or exporter version changes, $v$ must also enter the artifact identity.

#### Blender Python: The Data API, BMesh, Modifiers, and the Evaluated Scene Are Not One Layer

Blender embeds Python and exposes data blocks, scenes, objects, modifiers, nodes, import/export, and rendering interfaces through `bpy`. Scripts have two operational surfaces that are often conflated. `bpy.data` and the object/mesh data API modify data-blocks in Blender's main database, and they are better suited to implementing explicit, headless construction. Whether those edits are written to `.blend` depends on the save action, however, and the application script itself must also guarantee idempotence. `bpy.ops` calls the same operators as the user interface, and whether execution succeeds may depend on the active object, selection, mode, area, and view layer context. A batch script that leans heavily on implicit `bpy.context` may produce different results in an interactive window and in `--background` mode. Engineering defaults should therefore prefer creating and linking explicit data-blocks. Only when an operator must be reused should context be established explicitly and its `poll` conditions checked. Blender Python API [@web11c361c8ab15]

Direct mesh construction writes vertices, edges, and faces into `bpy.types.Mesh`. It can also drive connectivity operations on the editable topology of `bmesh`, such as split, collapse, dissolve, and extrude. The official BMesh documentation states explicitly that this API will not enforce the maintenance of all valid states on the script's behalf. Duplicate edges/faces, faces with fewer than three vertices, and inconsistent selection states are all the script's responsibility to clean up. After an edit-mode BMesh modification, the Mesh should be updated through `bmesh.update_edit_mesh`, and loop triangle tessellation recomputed as needed. A standalone BMesh is first written back with 〈bm.to_mesh(mesh)〉, and then 〈mesh.update()〉 is called. Blender BMesh [@srcBDCC-003] So "the API call raised no error" is not a sufficient condition for mesh correctness. Every generation function must still check face indices, area, winding order, incidence relationships, boundary loops, transforms, and units.

Modifiers and Geometry Nodes introduce a second key distinction. The base mesh in the object data-block is not necessarily equal to the evaluated mesh of the dependency graph. Arrays, booleans, mirrors, subdivision, curves, Geometry Nodes instances, and the dependency graph change topology after evaluation. A correct contract must make explicit whether what is delivered is the base parametric model or the evaluated geometry. It must read and check the latter from the dependency graph's evaluated object. An exporter should be expected to output the corresponding evaluated result only when 〈Apply Modifiers〉 or an equivalent option is enabled. The target consumer's read-back must still be the standard. Otherwise the script counts 8 base cube vertices, while the exported file may contain tens of thousands of instances or subdivided faces. It may also still be the base mesh, because the option was off. Geometry Nodes is essentially a program graph with typed sockets, fields, attribute domains, instances, and control flow zones. Its node group version, input sockets, attribute names, whether instances are realized, random IDs, and seeds should be fixed, rather than saving only a screenshot of the nodes. Geometry Nodes Manual [@web11b0dbc34b98]

One reliable order for a headless Blender build runs as follows. Clear the scene or open a fixed template. Set units, axes, renderer, and color management. Create collections, objects, and meshes from parameters, then add and configure modifiers and node groups. Evaluate in the dependency graph. Derive visual, collision, and LOD geometry separately. Run geometry checks, export to a temporary path, and read the result back with the target importer. If the read-back passes, move the result atomically into the release directory. The command line should run in background mode, name the script path explicitly, and return a non-zero exit code on Python exceptions. Parse business parameters after `--`. Blender's command-line documentation also reminds us that arguments execute in the order they appear, and that loading a `.blend` overrides scene options set earlier. The command order is therefore part of the reproduction contract itself. Blender Command Line [@webab7044d6a1d4]

**Text Workflow or Pseudocode in the Original Manuscript**
\begin{verbatim}
blender --background --factory-startup \
--disable-autoexec --python-exit-code 2 \
trusted_template.blend \
--python build_scene.py -- --config room.json --out build/
\end{verbatim}

`--disable-autoexec` must appear before the untrusted `.blend` path, because Blender processes command-line arguments in order. The flag is a necessary auto-execution control. It is not a Python sandbox, and it is not a complete security boundary. In particular, it does not restrict a 〈--python build_scene.py〉 specified explicitly on the command line, so that script itself must already have been reviewed. Blender's official security documentation explains that a `.blend` file can contain registered text blocks and Python driver expressions. The same documentation notes that Python itself does not limit what a script can do. Downloaded source files, add-ons, and build scripts must therefore first enter a worker with no credentials, no network, low privileges, read-only inputs, a bounded output root, and resource quotas. Auto-execution is disabled there by default. Blender Script Security [@srcS020]

#### CSG and OpenSCAD: Concise Rules Do Not Equal a Robust Boolean Kernel

OpenSCAD uses declarative code to describe primitives and set operations, and it is especially suited to parameterizable enclosures, brackets, pipes, room modules, and printed parts. A solid set $S\subset\mathbb R^3$ can be represented by a CSG tree composed of unions, intersections, and differences:

$$
S=(S_1\cup S_2)\setminus\bigcup_k H_k,
$$

Here $S_1,S_2$ are the main bodies, and $H_k$ are doorways, holes, or slots. The code stores the tree and the parameters. A triangle mesh such as STL/3MF is merely the evaluation result at a particular resolution and kernel version. OpenSCAD's official command-line interface supports overriding variables with `-D` and exports according to the `-o` suffix, which makes it suitable for batch-building parameter combinations in CI. OpenSCAD Command Line [@srcOSC-CLI] Fast preview is usually an approximate display produced by OpenCSG/OpenGL. It cannot replace the final `render`/export evaluation and checking of the solid. The solid backend also varies with version and configuration, and the 2025 development snapshot has made Manifold the default while retaining the CGAL option. The manifest must therefore pin the OpenSCAD version, the actual backend, discrete parameters such as 〈$fn/$fa/$fs〉, and the complete command. Neither an artifact-free preview nor a vague "using OpenSCAD" can be treated as reproducible evidence. OpenSCAD Backend Announcement [@srcOSC-MANIFOLD-DEFAULT]

Boolean failures cluster around geometric predicates and boundary representations. Coplanar overlaps, zero-thickness contacts, extremely thin slivers, excessive scale spans, near-duplicate faces, and self-intersecting inputs make inside/outside classification or topology splitting unstable. Expanding each cutting body by an arbitrary epsilon can sometimes avoid coplanarity, but it also changes dimensions and thin walls that closure should have preserved. A more reliable strategy has four parts. Specify minimum features and tolerances relative to the bounding box. Verify before the boolean that the inputs are closed oriented solids. Record component and volume changes at each step. Re-check boundaries, non-manifoldness, self-intersections, and target dimensions after export. A valid CSG tree proves only that the operation can be described. It does not prove that the discrete mesh is suitable for rendering or collision.

#### CadQuery and FreeCAD: Parametric B-rep, Feature History, and Topological Naming

CadQuery models with `Workplane`, sketch, selector, feature, and assembly on the OpenCascade geometry kernel. Typical code first draws a two-dimensional profile on a workplane, then extrudes, revolves, or sweeps it into a B-rep solid. It then selects entities by the direction or geometric type of faces and edges, and performs hole, fillet, chamfer, or shell operations. The official documentation describes `Workplane` as the core object that carries the object stack, the modeling context, and a chained API. CadQuery Concepts [@web02108419a530] A B-rep is not merely a list of triangles. It stores curve and surface geometry — planes, cylinders, and B-splines/NURBS among them — as well as the topological relationships of edges, wireframes, faces, shells, and solids. That makes it suitable for exact dimensions, STEP exchange, and manufacturing constraints. Before entering a game engine it still must be tessellated and assetified according to OpenCascade's linear deflection, angular deflection, normals, and material grouping. OpenCascade Meshing Pipeline [@srcOCCT-MESH] Assembly constraint solving is essentially adjusting child object positions to minimize constraint cost. Under-constrained or multi-solution systems are also affected by initial positions, so "the solver returned success" alone cannot prove that the assembly is unique. CadQuery Assemblies [@srcCQ-ASSEMBLY]

FreeCAD's Python interface allows creating documents, Part/PartDesign objects, expressions, and parameter tables, and then evaluating the document via `recompute`. It is suited to turning interactive CAD workflows into auditable scripts, and it also carries the different object models of workbenches such as assembly and BIM. The most dangerous engineering misunderstanding here is to treat "parameters can be changed" as "references are always stable". Suppose a later feature references an earlier result through an anonymous Nth face or the topmost edge. After a boolean or fillet changes the topology, the selection may bind to a different face. This is the well-known topological naming problem. FreeCAD 1.0 has merged a topological naming mitigation mechanism based on modification/generation/deletion history mapping, and stability has improved somewhat. Its official positioning nevertheless remains a mitigation, not an absolute guarantee against arbitrary boolean and feature changes. A displayed Label is likewise not equal to a stable sub-shape ID. FreeCAD 1.0 release notes [@srcFC-TNP-1] Defense should preferentially use datum planes, datums, sketch constraints, object-level links, and semantic references. After a parameter sweep, re-check the count, area, normal, centroid, solid count, volume, key dimensions, and export read-back of the selected sub-shapes.

The truthfulness of parametric CAD comes from the design contract, not from photographic evidence. A digital twin of a real building or piece of equipment can use scans and point clouds as measurement constraints and CAD parameters as intent and an editable prior. Even so, post-fit residuals, unobserved surfaces, and human decisions must be retained. Treating CAD surfaces unconditionally as ground truth hides wrong drawing revisions or on-site modifications inside a "precise model".

#### Houdini and Geometry Nodes: Turning the Generation Process into a Dataflow Graph

Houdini SOPs and Blender Geometry Nodes both organize geometry operations as directed graphs. Houdini can additionally scale to large-scale dependency scheduling through VEX, Python, HDA, and PDG/TOP. SideFX officially exposes each SOP's result as `hou.Geometry` containing point, vertex, primitive, group, attribute, and intrinsic. PDG/TOP goes further and represents tasks as work items with attributes and dependencies. Houdini Geometry API [@srcHOU-003] PDG/TOP overview [@srcHOU-005] Node graphs suit laying roads along splines, splitting building facades, scattering vegetation, terrain erosion, tiling large worlds, and batch LOD. The reason is that each node explicitly consumes upstream geometry and attributes. Houdini should distinguish point, vertex, primitive, and global/detail owners, where vertex corresponds to the face corner of each primitive. Geometry Nodes, in turn, distinguishes the point, edge, face, corner, and instance domains. Cross-domain reads, interpolation, or promote rules must be recorded separately per host. Otherwise materials, normals, IDs, or collision labels will quietly change.

The reproducible unit of a node graph is not the final `.blend`/`.hip` file. It is "host version + node/HDA version + input schema + parameters + seed + external file hashes + cook result". `hython` configures Houdini Python and imports `hou`, but licenses and site environment are also involved. Being syntactically correct does not mean the SOP has cooked. SideFX command-line scripting [@srcHOU-001] Random scattering should derive sub-seeds from a stable `entity_id` or spatial cell ID. Otherwise inserting one object upstream reorders all instances. Instancing can significantly save scene memory, but exporters, collision builders, or downstream engines do not necessarily preserve instance semantics. The contract must separately state when Geometry Nodes executes 〈Realize Instances〉 and when Houdini keeps or unpacks packed primitives. It must also state how each prototype and transform is serialized, and whether colliders are generated per instance, merged, or partitioned.

#### BlenderProc: Generating Images and Geometry Annotations from the Same Program State

BlenderProc turns Blender scene scripts into a purpose-built synthetic data pipeline. Code loads or constructs assets, randomizes materials, lights, objects, and cameras, and performs collision checks or physical placement when necessary. It then outputs RGB, depth/distance, normal, segmentation, flow, and annotations such as HDF5 and COCO/BOP from the same scene state. BlenderProc official repository [@srcBPROC-001] BlenderProc2 JOSS paper [@srcBPROC-002] Its advantage is that images and labels share the same camera, object transforms, and occlusion state. Manual annotation therefore needs no secondary alignment. But "same-source" only proves that labels are faithful to the synthetic scene. It does not prove that asset semantics, sensor models, or domain randomization are close to the real world.

The input contract must save camera intrinsics $K$, resolution, camera-to-world extrinsics, OpenCV/OpenGL coordinate conversion, near/far clipping, and object `category_id`/instance/name. It must also save material and pose distributions, renderer, physics step size, output channels, and writer version. `depth` and the `distance` from the optical center to a point are not the same array. The two cannot both be called "depth". If a visible background or object lacks a semantic attribute, a default must also be given explicitly, or the build must fail. The per-frame manifest should bind the scene hash, camera matrices, object poses, channel dtype/shape/units, instance mapping, and randomization rejection records to a common `frame_id`.

Fixing `BLENDER_PROC_RANDOM_SEED` unifies Python `random` and NumPy sampling, and it is one of the necessary conditions for reproduction. Yet it does not guarantee bit-level consistency across Blender/Cycles, GPU, physics, or plugin versions. The synthetic data gate should re-run on a fixed golden scene, compare camera projection, RGB/depth/mask/box cross-modal consistency and overall distributions, and then measure sim-to-real on a real validation set. "generated one hundred thousand labeled images" cannot substitute for coverage, bias, and real task performance.

#### Open3D, trimesh, PyMeshLab, and CGAL: They Are Better Suited to Geometry Conversion and Acceptance Gates

Geometry libraries and DCC/CAD hosts play different roles. Open3D provides point cloud, RGB-D, TSDF, normal, ICP, BPA, alpha shape, Poisson, and TriangleMesh operations, and suits connecting captured data to a surface pipeline. Its Poisson interface explicitly requires oriented normals and returns density to support trimming low-support regions. Open3D surface reconstruction [@srcO3D-SURFACE] `trimesh` focuses on loading, transforms, topology/watertightness checks, cross-sections, adjacency, sampling, collision, and format exchange, but it delegates boolean operations to the Manifold or Blender backend. Its collision and some acceleration capabilities depend on optional packages. PyMeshLab wraps MeshLab filters, and its results are affected by the current `MeshSet` layer, filter order, version, and parameters. CGAL provides polygon mesh algorithms with selectable numeric kernels, explicit topology concepts, and preconditions. Some configurations can use exact predicates, but not all constructions, simplification results, and outputs can be generalized as automatically exact or automatically free of self-intersections. trimesh boolean interface [@srcTM-BOOLEAN] CGAL mesh simplification [@srcCGAL-SIMPLIFY] These libraries can automate repair and conversion, but function names such as `fill_holes`, `make_watertight`, or `repair` cannot supply scene semantics. The manifest must record the actual backend, optional dependencies, and filter chain.

A reliable library-style pipeline should retain the immutable source mesh and then sequentially generate normalization, repair, simplification, visual, and collision derivatives. Each step should record input/output hashes, library and kernel versions, parameters, vertex/face/component counts, AABB, and area/volume. It should also record boundary/non-manifold/self-intersection, the mapping of removed or added elements, and bidirectional distance to the reference surface. Different libraries may handle the scene hierarchy, units, textures, and negative scale of the same OBJ/glTF differently. At least a second parser or the target engine must therefore read it back, and geometry queries must be compared, rather than merely comparing "export succeeded".

#### The Minimal Contract for Code-Generated Visual Surfaces, Collision Surfaces, and Scene Graphs

The main function of a procedural build should be a pure parameter interface, with host operations converging into a boundary adapter. The pseudocode below deliberately separates visual from collision and reads back again before publishing:

**Text workflow or pseudocode in the original manuscript**
\begin{verbatim}
build(config, seed, toolchain_lock):
assert validate_schema_units_ranges(config)
intent = canonicalize(config, seed, toolchain_lock)
visual = build_visual_geometry(intent)
collision = build_collision_from_intent(intent) # do not blindly copy from the high-poly model
scene = attach_ids_materials_transforms(visual, collision)
assert geometry_gates(scene)
temp = export_to_isolated_directory(scene)
roundtrip = import_with_target_or_second_parser(temp)
assert query_equivalence(scene, roundtrip)
manifest = hash_inputs_outputs_and_metrics(intent, temp, roundtrip)
atomic_publish(temp, manifest)
\end{verbatim}

`build_collision_from_intent` matters. The collision clearance of a door opening is best reconstructed from the same door width/height parameters, rather than by performing unconstrained simplification on the high-poly visual model. This way visual trim strips can exist while the collision proxy still maintains a clear passage contract. Object IDs should also be derived from semantic paths and stable parameters, not by relying on "which object in the current array". Otherwise a single insertion misaligns saved files, network replication, and differential updates.

This survey provides a minimal runnable mechanism that depends on no third-party libraries. `reproduction/programmatic_modeling/generate_parametric_room.py` takes metric room and door-opening parameters and generates a visual OBJ, a collision OBJ, and a machine-readable manifest. The three files from two isolated builds are byte-for-byte identical. The visual mesh has 120 triangles and the collision mesh 84 triangles. The door-opening clearance, finite-value, index, and degenerate-face assertions pass. This result only demonstrates the parameters–dual-mesh–manifest mechanism. It is not a reproduction on a Blender, CAD, Houdini, or engine host, and all examples whose host is not installed are marked in the status file as `SYNTAX_ONLY_NOT_HOST_EXECUTED`. Reproduction instructions (open material: `../reproduction/programmatic_modeling/README.md`) Status and boundaries (open material: `../reproduction/programmatic_modeling/STATUS.md`)

#### Choice Boundaries for Procedural Modeling

Procedural DCC/CAD works best on targets that have explicit dimensions, repeated modules, assembly relationships or manufacturable constraints, or that need tens of thousands of controllable variants. Such targets need only a few inputs. Edits stay traceable. Semantic IDs can be generated from an explicit schema. Visual, collision and navigation parameters can be coupled. When the target is an unmodeled real site, reconstruction still carries the measurement work. When unobserved regions need visual richness, generative models can propose candidates. When runtime matters, the engine carries state and performance. Real systems commonly mix these routes. Scans calibrate dimensions, CAD solidifies structure, generative models supplement materials and content, and DCC/node graphs turn the result into assets. The engine then builds collision and navigation.

This hybrid route has a fixed acceptance order. First ask whether the rules satisfy the design constraints. Then ask whether the output fits the observations. Then ask separately whether the mesh, the collision, the navigation and the engine queries hold. Rules, images and runtime each carry independent evidence. No layer can self-certify the next one.

#### The Full Flow from Candidate Surface to Production Asset and Failure Propagation

**Indented flow or pseudocode in the original manuscript**
\begin{verbatim}
Load the candidate mesh, the provenance mapping, and the coordinate contract
→ validate the parse budget, indices, finite numbers, units, handedness, and hierarchical transforms
→ remove exact duplicates and degeneracies by a scale threshold, preserving attribute seams
→ build half-edges and report connected components, boundaries, and non-manifold face fans
→ orient each component using adjacency and source views
→ repair only the holes and self-intersections allowed by policy, and tag newly added faces with provenance
→ preserve hard edges and seams, and recompute normals and tangents
→ unwrap charts, pack UVs, bake by visibility, and record source image IDs
→ generate constrained QEM LODs and HLOD provenance mappings
→ run geometry, topology, rendering, attribute, and import round-trip tests
→ export the visual asset and a separate downstream collision candidate
\end{verbatim}

In this chain, errors amplify rather than vanish on their own. A pose deviation in Chapter 6 makes MVS/TSDF produce double-layer surfaces. The color fitting or densification in Chapter 7 reads that double layer as a stable primitive. The Poisson step in Chapter 8 seals it into a shell. The welding and QEM steps in Chapter 9 may then join the inner and outer layers into self-intersections, or block the openings. Once UV baking covers geometric seams with blended colors, visual inspection becomes even harder. Conversely, chasing topological perfection too hard can also repair genuinely open structures into watertight fake objects. Every conversion should record its provenance mapping and its difference metrics. A problem can then be traced back to the stage that produced it.

Geometry gates and asset gates must be set separately. A geometry gate checks bidirectional surface distance, normals, boundaries, topology, observation coverage and completion surfaces. An asset gate checks units, coordinates, UVs, materials, LOD, import read-back and runtime budget. PSNR, Chamfer, non-manifold edge count and triangle count carry different units, so they cannot be summed into a single total score. A mesh can sit very close in geometric distance and still be unusable because its UVs are flipped. It can also be watertight and low-poly while sealing every door opening.

#### Chapter 9 Output Contract and the Connection to the Collision, Navigation, and Engine Layers

A production visual mesh should deliver at least the following. Coordinate axes, handedness, units and origin must be explicit. The object hierarchy carries stable IDs, and the half-edge topology audit reports its results. The asset must report statistics on open boundaries, non-manifold edges, self-intersections, degeneracies and connected components. It must log all automatic and manual repair events and the added-face masks. The reference surface and each LOD must give their error, triangle count and switching strategy. UV charts, texel density, padding, materials, tangents and texture color space must be recorded, along with source image, point cloud and field versions and their content hashes. Import read-back results must cover the target format.

Even then the result is only an "editable, renderable surface." The next stage must build a separate set of collision proxies from the visual mesh. It chooses primitives, convex hulls or convex decomposition, SDFs, or triangle meshes. The right form depends on whether the objects are dynamic or static, and on their convexity, velocity and error budget. It then bakes a NavMesh from agent radius, height, slope and step parameters. A common object ID and transform must link the visual LOD, the collision LOD and the navigation surfaces. They must not impersonate one another.

Only at this point does the logic of the method evolution become complete. SfM turns images into a camera-point observation graph. MVS and TSDF add depth, free space and fusible geometry, and feed-forward point maps lower the initialization barrier. NeRF provides continuous appearance queries, a neural SDF writes the zero level set into the representation, and 3DGS trades differentiable explicit primitives for real-time rendering. BPA, alpha shape, Poisson and Marching Cubes make different connectivity and closure assumptions about the samples. Half-edge, repair, QEM, LOD and UV then turn candidate surfaces into editable visual assets. Physical interactivity still requires a next, separate layer that builds collision, navigation, scene graphs and the engine runtime. That layer must be demonstrated through contact, reachability and performance regressions. It cannot be inferred from "the mesh has been exported."

---

### L4: Collision Surfaces, Distance Proxies, and Navigation Surfaces

L4 delivers collision, distance or navigation queries natively, for a given proxy and runtime budget. A file named collider, a single bounding-box pass or a successful NavMesh bake are all only candidates. Each one still needs penetration, tunneling, stability, cost and reachability tests.

The meshes, Gaussians or implicit fields obtained earlier solve appearance replay and geometric representation. A collision system, by contrast, must answer a different set of questions. It reports whether two objects intersect, when they first come into contact, how deep the penetration is and what the contact normal is. It must also report whether that query completes stably within every physics step. Navigation asks more again. An agent has a body size, a slope capability and a step-over capability. Navigation must report which free spaces are connected for that agent and what the path cost is. It must also report which paths remain valid after dynamic obstacles change. The visual mesh, the collision proxy and the NavMesh must therefore be three artifacts in the same world coordinates, linked by version relationships. They are not three aliases of a single file.

#### Visual Meshes and Collision Proxies Optimize Different Errors

Visual meshes usually target pixel error, silhouette, material continuity, normal smoothness and triangle budget. A window frame can carry a fine groove in a normal map. A leaf can be a zero-thickness face with a transparent texture. A distant wall can rely on occlusion to cull its back faces. Those tricks hold as long as rendering is correct. A collision proxy, by contrast, must define a spatial set. Let the reference entity occupy $S$ and the proxy occupy $C$. The two most basic geometric errors are then

$$
E_{\mathrm{false}}=\mu(C\setminus S),\qquad E_{\mathrm{miss}}=\mu(S\setminus C),
$$

where $E_{\mathrm{false}}$ denotes obstacles that do not exist, such as a convex hull sealing off a cup handle, a doorway or a drawer handle. $E_{\mathrm{miss}}$ denotes real obstacles that are missed, such as a thin wall swallowed by voxelization. The first produces invisible walls and false unreachability. The second produces wall penetration and falls. Practical engineering should also weight the errors under a task-relevant sampling distribution $q(x)$. Doorways, step edges, grasp slots and high-speed motion trajectories matter more than surfaces far from the interaction region.

$$
L_{\mathrm{proxy}}= \lambda_f\int_{C\setminus S}q(x) dx+ \lambda_m\int_{S\setminus C}q(x) dx+ \lambda_n E_{\mathrm{normal}}+ \lambda_c C_{\mathrm{query}}.
$$

Here $E_{\mathrm{normal}}$ measures the contact normal error, and $C_{\mathrm{query}}$ measures the number of candidate pairs, the narrow-phase time and the memory. Safety robots usually make $\lambda_m$ larger, preferring to treat uncertain regions conservatively as obstacles. Competitive games may care more about $\lambda_f$, so that players are not blocked by invisible faces. Precision grasping must protect grooves and holes, and cannot optimize only the overall volume error. This difference in objectives explains why a visual LOD cannot automatically serve as a collision LOD. It also explains why "watertight" is not a sufficient condition. A proxy that is completely watertight but seals off real openings is topologically legal and functionally wrong.

The input contract for a collision asset should include at least the following. It names a normalized reference mesh and says whether the object is static, kinematic or dynamic. Mass and material belong here, together with the allowed maximum miss and false-hit distances and the query types (ray, overlap, sweep, simulation). The contract also states the speed limit, the minimum feature scale and whether inner-side contact is allowed. Observed-face and completion-face masks complete the list. The output contract is not a bare mesh. It is

〈shape set + local transform + collision layer/mask + material + cooking parameters + error report + version hash〉.

If the upstream provides only NeRF or 3DGS, an acceptable reference surface or signed occupancy must be obtained first. If the upstream mesh contains self-intersections, non-manifold edges, open boundaries or an unknown scale, then "automatic convexification" will only freeze the uncertainty into the physics layer. The topology, scale, normal and observation coverage checks in Chapter 9 are therefore a prerequisite gate for this chapter. They are not optional cleanup.

#### AABB and BVH: Broad Phase Produces Only Candidates, Not Contacts

A scene with $n$ shapes needs $O(n^2)$ candidates if every pair is checked. Broad phase first uses cheap bounding volumes to exclude pairs that cannot possibly intersect. For a vertex set $P_i$, the axis-aligned bounding box is written as

$$
B_i=[\ell_i,u_i],\quad \ell_{ik}=\min_{p\in P_i}p_k,\quad u_{ik}=\max_{p\in P_i}p_k.
$$

Two AABBs overlap in three dimensions if and only if, for every axis $k\in{x,y,z}$,

$$
\ell_{ik}\le u_{jk}\ \land\ \ell_{jk}\le u_{ik}.
$$

Those six interval comparisons are very fast. They only indicate a "possible collision." An AABB overlap between a curved wrench and a sphere does not mean their real surfaces touch. The official PhysX documentation explicitly defines AABB overlap as broad phase, and computes real contacts only in the next stage. Its SAP, MBP, ABP, PABP and GPU broad phase suit different static/moving ratios, region partitions and parallel scales.[@srcC004]

Static triangle environments usually add a bounding volume hierarchy, BVH. Leaf nodes store triangles or small clusters. Internal nodes store the union of the left and right child bounding boxes. During construction, one may split at the median of the longest axis, or use the surface area heuristic:

$$
C_{\mathrm{split}}=C_t+ \frac{A(B_L)}{A(B_P)}N_LC_i+ \frac{A(B_R)}{A(B_P)}N_RC_i,
$$

where $A(B)$ is the box surface area, $N_L,N_R$ count the primitives of the subtrees, and $C_t,C_i$ are the traversal cost and the primitive-test cost. A query starts from the root. If the query bounding volume does not overlap a node box, the node prunes. Only on overlap does the query visit the children, until a leaf node is handed to the narrow phase. Rays, overlaps and sweeps can reuse the same BVH, but their query bounding volumes differ. What is adopted here is the SAH form that MacDonald and Booth developed to approximate intersection probability by surface area. It is a construction heuristic, not a theorem that is optimal for every query distribution. That form comes from the original SAH paper[@srcSAH-1990].

Dynamic objects have three common update strategies. A bottom-up refit follows a rigid transform. A dynamic AABB tree suits frequent insertions and deletions. Asynchronous rebuild follows long-term degradation. To keep tiny motions from moving the tree every frame, an object can use a "fat AABB" expanded by the contact offset and the short-term velocity. High-speed objects use a swept AABB from $t$ to $t+\Delta t$. Too small an expansion misses candidates. Too large an expansion makes narrow-phase candidates explode. For the PhysX MBP broad phase, a large-world partition must use regions that cover all active space. The official documentation explicitly says that only eMBP requires regions. Objects that fall outside all regions will not join the broad phase, and their collisions are disabled. SAP, ABP, PABP and GPU broad phase do not depend on this set of regions. The relevant PhysX type is PxBroadPhaseRegion[@srcPHYSX-MBP-REGION]. Therefore, only when MBP is selected must scene streaming update the visual cells, broad-phase regions and object proxies as a single transaction. This requirement must not be extrapolated into a fixed precondition for all PhysX broad-phase algorithms.[@srcC004]

The broad-phase data structure should record at least 〈node_aabb, child_or_primitive, layer_mask, dynamic_flag, generation〉. Here generation prevents an old handle from being reused after asynchronous deletion. Layer mask filters out categories that need no contact before candidates are generated. NaN/Inf, reversed-order bounds caused by negative scale, an un-updated transform or an extremely large AABB will contaminate the entire tree. At the mildest, candidates surge. At the worst, objects are missed entirely. Before all shapes enter the broad phase, finite numbers, $\ell\le u$, a scale limit and world bounds checks should be enforced.

#### Convex Narrow Phase: Convex Hulls, GJK, EPA, and SAT

The value of convex shapes is not merely "fewer faces." The support function can turn collision queries into low-dimensional iteration. The support point of a convex set $A$ in direction $d$ is

$$
s_A(d)=\arg\max_{a\in A} d^\top a.
$$

Two convex sets intersect if and only if the origin belongs to the Minkowski difference $A\ominus B={a-b\mid a\in A,b\in B}$. GJK does not explicitly construct this difference set. Instead it repeatedly calls

$$
s_{A\ominus B}(d)=s_A(d)-s_B(-d),
$$

and maintains a simplex composed of one to four support points. The algorithm first takes the difference of the two object centers as the direction. Each round it adds a new support point, finds the point on the simplex closest to the origin, and takes the opposite direction as the next search direction. GJK used for intersection decisions can declare separation when the support point cannot pass the origin. It declares intersection when the simplex contains the origin. The variant used for distance queries keeps updating the closest simplex until the improvement in the closest distance falls below the tolerance. It then returns the distance and the witness points on both sides. The two stopping conditions must not be mixed. Practical implementations must also handle degenerate line segments, nearly coplanar tetrahedra, duplicate support points and floating-point tolerances. Otherwise they oscillate, or they misjudge tangency as deep penetration. The original Gilbert—Johnson—Keerthi paper[@srcGJK-1988] is the source.

After GJK confirms intersection, the penetration depth is still unknown. EPA is a common follow-up. It builds a convex polyhedron from the simplex that contains the origin. It then repeatedly selects the face closest to the origin and calls the support function along that face's normal. If the gain from the new support point over that face falls below a threshold, the face normal approximates the minimum translation direction and the face distance approximates the penetration depth. Otherwise EPA deletes the faces visible from the new point, adds new faces along the horizon and keeps expanding. EPA is sensitive to extremely thin convex bodies, to a bad initial simplex and to scale spans. Convex cooking must therefore clean up coplanar points, guarantee a minimum volume and unify the tolerance scale. The algorithm described here belongs to van den Bergen's EPA family. It does not mean that Unity, Unreal, Godot or PhysX always execute GJK followed by EPA for every shape pair. A specific implementation may use dedicated analytic tests, SAT, persistent contact manifolds or other convex queries for spheres, capsules, boxes and triangles. A contact solver's choice of candidate generation path is also a separate matter from its step of solving impulses. EPA original technical report[@srcEPA-2001] Unreal Chaos EPA interface description[@webe6f49bae1832] [C004—C007]

SAT provides another convex-body test. Two objects must be separated if some axis leaves their projection intervals disjoint. Intersection can be declared only when the complete candidate axes for convex polyhedra have been enumerated and all projections overlap. For boxes, the candidate axes come from the face normals of the two boxes and from cross products of their edge directions. For general convex polyhedra they come from both sides' face normals and from edge—edge cross products. Let the projection interval be

$$
I_A(n)=[\min_{a\in A}n^\top a,\max_{a\in A}n^\top a],
$$

Any $n$ with $\max I_A(n)<\min I_B(n)$, or with the reverse inequality, is a separating axis. SAT is straightforward for box—box and triangle—box tests, and the minimum-overlap axis can also approximate the contact normal. The number of candidate axes, however, grows with the number of polyhedron edges. Cross products of parallel edges approach zero, so deduplication and tolerance are needed. The OBBTree paper by Gottschalk, Lin and Manocha gives the 15 complete candidate axes for two three-dimensional OBBs and hierarchical traversal. The statement here about "general convex polyhedra" holds only when face normals and valid edge—edge cross products form a complete axis set. The original OBBTree paper[@srcOBBTREE-1996]

A convex hull is the smallest convex set that contains the input point set, so it naturally fills in concave grooves. It suits near-convex dynamic objects such as boxes, spheres and capsules. It also suits splitting a character's body into several primitives. Cups, rooms, scissor holes and drawer handles are all poor fits for a single convex hull. "convex=true" merely satisfies the solver interface, which is not the same as preserving interaction functionality. Unity, Unreal, Godot and PhysX distinguish convex proxies, per-triangle static queries and dynamic simulation in different ways, and the corresponding official contracts are given in [C004—C007].

#### From V-HACD to CoACD: Why Convex Decomposition Moves from Volume Approximation to Collision Function

Approximate convex decomposition expresses a concave solid $S$ as the union of several convex pieces $C=\bigcup_{j=1}^{m}C_j$. A larger $m$ usually makes holes and fine grooves more accurate. It also makes the broad-phase leaf, the GJK/EPA calls, the memory and the contact pairs of each object grow. The problem is not to minimize $m$ or the geometric error alone. It is to find a Pareto point under functional constraints.

The typical V-HACD pipeline voxelizes a closed or nearly closed mesh and approximates the solid by voxel occupancy. It computes the concavity of a part relative to its convex hull. For the part with the greatest concavity it searches for a cutting plane and bisects recursively. Each terminal part yields a convex hull. A final stage merges, simplifies and limits the vertex count and the convex-piece count.[@srcC001] Voxelization lets the algorithm tolerate complex triangle soup. It also ties the minimum expressible feature to the voxel size. A cup-handle hole narrower than the voxel size gets sealed off. Slanted thin sheets show stair steps. Raising the resolution in turn raises memory and splitting time. Where volume difference dominates the concavity measure, deep grooves and openings that are "small in volume but functionally critical" may also be underestimated.

CoACD's evolution addresses exactly this weakness. Let the solid be $S$ and the convex hull be $CH(S)$. The paper computes the Hausdorff distance separately for the boundary sample point set and for the interior sample point set:

$$
H_b(S)=H(\mathrm{Sample}(\partial S),\mathrm{Sample}(\partial CH(S))),
$$

$$
H_i(S)=H(\mathrm{Sample}(\mathrm{Int}S),\mathrm{Sample}(\mathrm{Int}CH(S))),
$$

and defines collision-aware concavity

$$
\mathrm{Concavity}(S)=\max(H_b(S),H_i(S)).
$$

The boundary term detects the worst deviation of the shape from its convex hull. The interior term specifically penalizes hollow structures or deep holes filled in by the convex hull. Using the maximum instead of the average keeps one small but critical hole from being diluted by a large area of normal surface.[@srcC002] Its decomposition procedure treats the input as a 2-manifold solid. Parts whose concavity exceeds the threshold $\varepsilon$ are enqueued. The mesh is cut directly with a 3D plane rather than by cutting voxels. For candidate cutting planes, multi-step Monte Carlo tree search simulates several future cuts, so as to minimize the maximum concavity among the future parts. The selected plane then undergoes continuous position refinement. The recursion continues until all parts pass the threshold. At the end it attempts to merge adjacent parts that still satisfy the threshold. The paper's drawer experiments show that preserving the handle hole changes whether a robot can form a shape-closure grasp. This is evidence about “collision function” rather than pure visual distance. Its 49 drawers, training protocol and success determination, however, cannot be extrapolated into a uniform improvement across all scenarios.[@srcC002]

Neither family of methods is an unconditional fixer. CoACD assumes a solid manifold, so when the input is open, self-intersecting or has disordered normals, its interior sampling and cutting semantics are themselves unreliable. V-HACD absorbs bad meshes better, but voxel closure may still modify doors and windows on its own. A production pipeline should generate candidates over a set of thresholds rather than passing once with default parameters. It should record the number of convex parts, the number of vertices per part, bidirectional surface distance, false-occupancy/missed-occupancy volume, clearance of critical openings, the GJK/EPA time distribution and downstream task success rate. The threshold must follow jointly from “how much thickening is allowed” and the query budget.

#### Triangle meshes, SDF, discrete collision, and continuous collision

In static scenes, the proxy that stays closest to the reference surface is usually a triangle mesh. The narrow phase first uses a triangle BVH to find candidates, then performs sphere/capsule/convex–triangle or triangle–triangle tests. A contact point on a triangle can be written in barycentric coordinates $p=\alpha a+\beta b+\gamma c$, where $\alpha+\beta+\gamma=1$ and all three are non-negative. The normal comes from the oriented triangle or from a smoothed geometric normal. A normal map modified for rendering cannot be used directly. A closed, consistently oriented 2-manifold mesh can define inside and outside. An arbitrary triangle soup, or a triangle-mesh shape that the engine queries only by surface, does not automatically provide a reliable solid interior. A per-triangle mesh preserves concave shape, yet still has three engineering limitations. Whether backfaces make contact depends on engine settings. Tiny triangles create contact-normal jitter. The contact pairs and mass properties of a moving concave mesh are very hard to keep stable. The official PhysX contract therefore by default does not allow TriangleMesh, HeightField or Plane as the simulation shape of a non-kinematic dynamic actor. A dynamic triangle mesh with SDF is a specific path, subject to cooking, valid mass/inertia and resolution requirements.[@srcC004]

SDF uses a scalar field to represent the signed distance to the surface:

$$
\phi(x)= -\operatorname{dist}(x,\partial S),x\in S, +\operatorname{dist}(x,\partial S),x\notin S.
$$

A point, or a sphere of radius $r$, makes contact when $\phi(x)\le r$. The contact normal can be approximated by $n=\nabla\phi/\|\nabla\phi\|$. A regular voxel grid supports constant-time trilinear sampling, while sparse bricks, octrees or narrow-band hashes store only the region near the surface. SDF is especially suited to particle–complex-surface interaction, distance queries and certain complex dynamic objects. “having a distance volume”, however, does not mean the distance ground truth holds. The sign of an open mesh is uncertain, and self-intersection and thin walls conflict. Voxels that are too coarse swallow thin structures, and voxels that are too fine raise memory and bandwidth. The PhysX documentation is explicit here. An SDF resolution that is too low misses thin parts, and one that is too high increases memory and collision time. The cooker may also close holes in a non-watertight mesh on its own.[@srcC004] For the engineering basis and discretization boundaries of GPU SDF construction, see [@srcC008].

Discrete collision detection (DCD) checks shapes only at $t_k$ and $t_{k+1}$. Suppose a wall is thinner than the object's own travel distance in one step. If the object crosses it, neither endpoint intersects, and tunneling occurs. A smaller $\Delta t$ alleviates the problem, but the cost grows linearly and there is still no formal guarantee. Continuous collision detection (CCD) solves for the earliest contact time

$$
t^*=\inf{t\in[0,\Delta t]\mid A(t)\cap B(t)\ne\emptyset}.
$$

For convex bodies, conservative advancement can be used. At the current instant, GJK gives the distance $d_k$, so the advance is $\Delta t_k\le d_k/(v_n+\epsilon)$. Here $v_n$ is the upper bound of the relative velocity along the separating normal. The step repeats until contact or until the time window is exceeded. For point–SDF, hierarchical queries and conservative stepping can be performed along the trajectory, [@srcC003]. For triangles, one can also solve the vertex–face and edge–edge time of impact. CCD must consider angular velocity, shape inflation, numerical tolerance and initial overlap together. Sending only a swept AABB into the broad phase is not enough, because it still only generates candidates.

The selection rule can be summarized as follows. For large static environments, use a triangle mesh that has been repaired and BVH-cooked. For ordinary dynamic objects, prefer primitives or a small number of convex parts. When a complex dynamic shape must be preserved, use the dynamic SDF path if the engine explicitly supports it and the SDF quality passes. For small high-speed objects, enable an appropriate CCD and validate it with known thin walls and corner trajectories. Every rule must be justified by measured query latency, missed-contact rate and contact stability, rather than inferred from an asset extension.

#### Recast/NavMesh: computing the feasible region of a class of agent from collision space

A NavMesh is not a “set of ground triangles” but a discrete approximation of configuration space. Take a cylindrical agent of radius $r$ and height $h$. Its walkable free space is approximated as the complement of the Minkowski inflation of the obstacle set $O$:

$$
F_{r,h}=W\setminus (O\oplus A_{r,h}).
$$

The same room therefore has different NavMeshes for a child character, a wheelchair, a vehicle and a flying unit. A larger agent radius makes narrow passages disappear. A larger height deletes low-ceiling passages. Max slope and max climb determine whether ramps and steps can connect. These are semantic parameters, not image-quality parameters.

The official Recast implementation splits map construction into the following auditable steps.〔source identifier V001〕

Triangle marking and voxelization. Move the input triangles into a unified world coordinate system. Mark candidate walkable faces from the angle between the face normal and the up axis. Rasterize the triangles into a heightfield with horizontal cell size $c_s$ and vertical cell height $c_h$. Each $(x,z)$ column stores multiple spans, representing the occupied intervals from `smin` to `smax`. Walkable filtering. Slope marking is already written into the triangle area before rasterization. After voxelization, Recast can reassess spans near low-hanging obstacles and mark them walkable according to `walkableClimb`. A ledge, or a span whose clearance is below `walkableHeight`, is marked non-walkable. The walkable part alone is compacted into a compact heightfield. The compact spans receive heights, adjacency connections, and area ids. The label of “whether the agent can stand” is what changes here. The original obstacle geometry is not removed from the world.〔source identifier V001〕 Radius erosion. Expand the obstacle boundary into free space by $r$. This realizes the clearance constraint of a cylindrical agent. The discrete radius is usually approximated as $\lceil r/c_s\rceil$ cells. Because of quantization, a small parameter change may open or close a passage by a whole cell. Region partitioning. The watershed path first computes the discrete distance field from each walkable span to the obstacle boundary. It then grows regions outward from local high values. Monotone and layer are separate partitioning paths. Neither requires exactly the same distance-field step as watershed. The chosen strategy and parameters then decide how small regions are handled and which regions are allowed to merge. The distance field helps watershed place boundaries in the middle of passages, but watershed may produce holes/overlaps. Monotone builds faster and produces no holes/overlaps. In exchange, it may generate elongated regions and increase the subsequent polygons. Layer suits non-overlapping layer partitioning. Whichever path is chosen, too coarse a grid may wrongly connect upper and lower layers, or the gap of a door.〔source identifier V001〕 Contours and polygonization. Extract contours from the region boundaries. Simplify the polylines by an error threshold. Then triangulate the contours and merge them into a polygon mesh. No polygon may exceed the specified number of vertices. A detail mesh is then generated. It keeps runtime height queries close to the input surface. Tiling and runtime queries. Large worlds are output tile by tile. Detour keeps polygon adjacency, external links, and area flags/costs. It performs A* on the polygon graph first. Then it obtains the path within the corridor through portal/funnel. Tile cache updates or local rebaking can be triggered by dynamic obstacles. Elevators, jumps, and teleports are expressed by explicit off-mesh links. Ordinary surfaces are not expected to discover them automatically.

The core construction can be written as the pseudocode below:

**Text procedure or pseudocode in the original manuscript**
\begin{verbatim}
build_navmesh(reference_collision, agent, grid, tiles):
assert finite(reference_collision) and units_locked()
for tile in tiles:
tris = query_triangle_bvh(tile.expanded(agent.radius))
areas = mark_triangles_by_slope(tris, agent.max_slope)
hf = rasterize(tris, areas, grid.cell_size, grid.cell_height)
filter_low_headroom_and_ledge(hf, agent.height, agent.max_climb)
chf = compact(hf)
erode_walkable(chf, ceil(agent.radius / grid.cell_size))
if partition_mode == WATERSHED:
dist = build_distance_field(chf)
regions = watershed_partition(chf, dist, min_area, merge_area)
elif partition_mode == MONOTONE:
regions = monotone_partition(chf, min_area, merge_area)
else:
regions = layer_partition(chf, min_area)
contours = simplify(trace_contours(regions), max_error)
poly = triangulate_and_merge(contours, max_vertices_per_poly)
detail = sample_detail_height(poly, chf)
validate_tile(poly, detail, trusted_free_space)
stage(tile, poly, detail)
atomic_commit(all_staged_tiles)
\end{verbatim}

Failure propagation can be localized step by step. Suppose the input collider misses a wall. After voxelization no obstacle exists there, erosion cannot make it back, and the final path goes through the wall. If the input proxy seals a door shut, all agents are unreachable. Cells that are too coarse quantize away thin walls or narrow bridges. A height that is too coarse merges upper and lower layers. A radius that is too small makes a corridor passable on the graph, while the body scrapes the wall at runtime. A minimum region area that is too large deletes valid small platforms. A contour simplification error that is too large cuts concave corners into shortcuts. Insufficient tile border produces seams. A misconfigured off-mesh link or area cost produces impossible jumps or long detours. Unity AI Navigation, Unreal Navigation System, and Godot NavigationMesh each expose different parameters and dynamic-update interfaces [V002—V004]. None of them can go beyond these configuration-space constraints.

Validation must not depend on “the same builder reading its own output once again”. A reference free space should be built from a trusted collider, under a second voxel configuration. The comparison should cover the number of components, start–goal reachability, shortest-path length, minimum clearance, and upper/lower-layer misconnection. It should also cover tile seams, and the cache state after dynamic obstacles are removed. An existing preprint checks the NavMesh of a production area with an independent voxel walkable space and prioritized exploration [@srcV005]. The work demonstrates the direction of the method. It does not yet constitute, however, a mature security guarantee against an attacker who knows the validator.

#### How this layer's output enters the engine

What the collision and navigation layer finally delivers is not “one optimized mesh”. It delivers interrelated runtime query assets. `render_mesh` handles appearance, `reference_geometry` handles traceability, `collision_shapes` handles ray/overlap/sweep/contact, and `navmesh[agent_profile]` handles reachability and paths. All four must share world coordinates, stable object IDs, source versions, and generation parameters. Yet they are allowed different topologies and LODs. The game-engine construction of the next chapter is therefore not a simple import. It loads these different contracts into the scene graph, physics scene, navigation server, and streaming system. Conversion and asynchronous updates must not confuse them again.

### L5: Game Engines and Persistent Interactive Worlds

L5 requires auditable object identity, state, actions, scripts, physics, streaming, saving, branching, and rollback in the target host. Finite-context memory, action-conditioned video, or static checking of editor scripts are all insufficient for promotion.

A game engine's job is to turn static artifacts into a long-running state system. At the same time it must maintain the scene hierarchy, rendering assets, physics actors, navigation tiles, animation, scripts, network replication, and streaming lifecycles. A mesh that passes an offline check can still produce severe spatial errors. It happens when the mesh is axis-flipped on import, or mirrored by a negative scale at runtime. It also happens when an LOD switch gives it a different collider, or a cell unloads and leaves a NavMesh link behind. The core of the engine layer is therefore not some editor button. It is a deterministic asset conversion graph and transactional runtime state.

#### Explicit persistent state: moving world facts out of the context window

The evolutionary contradiction of world models is already clear by this point. Pixel histories preserve appearance. They are expensive, though, and will slide out of the window. RSSM and JEPA latent states suit decision-making, but they cannot necessarily be queried from outside. Spatial construction needs a fact layer of its own, one that can be saved, edited, and branched.

One implementation comes from PERSIST: Beyond Pixel Histories [@srcPERSIST-2026], with peer-reviewed evidence available as of August 6, 2026. Its method does not retrieve past frames. Instead, it initializes a latent 3D world frame from a single frame. At each step it denoises according to the action first, then updates an agent-centric 3D environment. A feed-forward Transformer then predicts the camera. It projects the world into a depth-sorted stack of latent features. Finally, it denoises pixel latents conditioned on that pixel-aligned 3D feature stack. The project was validated in a Minecraft-style voxel environment. Its 3D state semantics therefore cannot be directly extrapolated to arbitrary real-world scenes. The demonstration is clear, however. Once the "environment—camera—renderer" separation is in place, space that leaves the field of view can still be queried back and edited.

MultiGen [@srcW15], in turn, splits a diffusion game engine into Memory, Observation, and Dynamics. It keeps external memory updated continuously, independently of the model context. That supports editable levels and multiplayer shared state. MultiGen is a 2026 preprint, with a lower evidence level than the already peer-reviewed PERSIST. Its interface direction is the same, however. The generator should not turn into the sole source of truth.

For production systems, an auditable persistent-state contract takes the form of the engineering pseudocode below. That contract is this survey's design recommendation. It is not the original algorithm of the papers above.

**Text flow or pseudocode in the original manuscript**
\begin{verbatim}
PersistentWorld W = {
coordinate_system, metric_scale,
camera_state,
geometry_layers, object_registry,
physical_state, semantic_state,
provenance, confidence, version
}

At each interaction step:
1. Validate the action and the current world version.
2. Update objects, contacts, and script state using deterministic rules or constrained dynamics.
3. Let the learned model propose only unobserved appearance, latent dynamics, or state increments.
4. Run geometry, physics, permission, and consistency checks on the proposal.
5. If it passes, commit it as a new version; otherwise roll back or degrade it to a read-only visual layer.
6. Render observations from the committed world state and return the observations to the agent or user.
\end{verbatim}

This contract keeps "generation proposals" apart from "committing world facts." Object IDs, coordinates, collision shapes, mass, script variables, and random seeds no longer exist only in the image. Multiple cameras can query the same world version, and it can also branch from a snapshot. The price is that the system must handle state merging, version conflicts, provenance tracking, and rejection policies for model proposals.

#### The right way to connect world models to the next layer

The evolution in this field is not "larger video models will eventually replace game engines automatically". It is the gradual externalization of state. Dreamer shows that latent states can be used for imagination-based planning. Genie shows that control can be discovered from unlabeled video. GameNGen and DIAMOND show that action-conditioned diffusion can form real-time or trainable observation environments. V-JEPA 2 shows that feature prediction is sufficient to support goal planning. PERSIST and MultiGen, in turn, move spatial facts into external 3D state or memory.

The world model and the geometric asset layer should therefore meet at a bidirectional interface. The world model outputs state deltas, cameras, depth, or object hypotheses downstream, and marks uncertainty. The downstream side returns constraints or rejection reasons through reconstruction, collision, and physics checks. The world model upgrades from "will keep generating observations" to "can maintain a world" only under one condition. The scene representation, the object state, and the physics proxies must be queryable independently.

#### The scene asset graph and four non-interchangeable artifacts

glTF 2.0 organizes transmission assets with scene/node/mesh/primitive/accessor/buffer/material/skin/animation [@srcE001]. OpenUSD goes further, providing layer, reference, payload, variant, and composition semantics [@srcE003]. Those semantics suit collaboration and lazy loading of large worlds. Both formats describe "how data is organized." Neither defines collision, navigation, and trust policies for a project. The recommendation is to model each object instance as a record with a stable ID:

**Text flow or pseudocode in the original manuscript**
\begin{verbatim}
EntityRecord {
entity_id, parent_id, local_transform, world_anchor,
render_asset_hash, reference_geometry_hash,
collider_asset_hash, nav_source_policy,
material_set, lod_group, stream_cell,
semantic_tags, provenance, build_generation
}
\end{verbatim}

`render_asset_hash` can change with the quality LOD, whereas `collider_asset_hash` should not change unconditionally along with it. `nav_source_policy` specifies which collision layers can participate in which kind of agent baking. `build_generation` lets asynchronous tasks know whether a result is already stale. The asset graph must also explicitly record external URIs, textures, shaders, scripts, plug-ins, and derived artifacts. Otherwise a seemingly standalone model will, through dependency resolution, access the network, inflate memory, or load unaudited code.

A typical offline build graph looks like this:

〈raw package → isolated parsing → normalized scene → reference geometry → visual LOD/materials → collision cooking → NavMesh baking → chunked packaging → signed manifest〉.

Every arrow carries the input/output SHA-256, tool and engine versions, parameters, coordinate transforms, count statistics, and validation reports. The same generation is published only when all sub-artifacts pass. After the visual package succeeds, a not-yet-finished collider/NavMesh must not be treated as "to be filled in later." The runtime may load an incomplete world first.

#### Coordinates, units, and transforms: one error flips rendering, normals, and inertia at the same time

Axes, handedness, and units are not unified across formats and engines. glTF defines right-handed coordinates, with metric linear units and $+Y$ up. The camera looks along local $-Z$ [@srcE001]. Unity 6's official coordinate description [@web63e3b82be661] gives a left-handed system with $+X$ to the right, $+Y$ up, and $+Z$ forward. Unreal's official coordinate description [@web13b524de5f08] gives a left-handed editor world with $+Z$ up. The units documentation [@web751e3dac8b3b] states that the default unit of length is centimeters, and that projects can adjust display units. The Godot stable documentation [@web1be2050fd5de] gives right-handed $Y$-up, with the built-in camera forward as $-Z$. The conventional front of oriented models is $+Z$ instead. The two cannot be written interchangeably. PhysX 5.5's official API description [@web69cac7e876c8], by contrast, does not mandate meters or centimeters. It only requires that inputs remain consistent, and uses `PxTolerancesScale` to set typical length and speed. The same scale applies to the physics, cooking, and scene configuration. Conversion therefore cannot merely "swap two components" on the vertices. It should define a homogeneous transform from source to world:

$$
T_{W\leftarrow S}= \left[sRt 01\right],qquad p_W=T_{W\leftarrow S}p_S.
$$

If $\det(R)<0$, the transform contains a reflection. The triangle winding order must be flipped, and normals transformed with the inverse transpose:

$$
n_W=\frac{(sR)^{-T}n_S}{\|(sR)^{-T}n_S\|}.
$$

The handedness in the tangent basis must also be converted. So must skeleton bind poses, animation curves, cameras, lights, collision centers, joint axes and the NavMesh up axis. These have to be converted together. Non-uniform scaling turns spheres into ellipsoids, distorts normals and changes the results of convex mesh cooking. Negative scale flips inside and outside as well. A more robust strategy is to bake geometric transforms during the normalization stage. Keep runtime objects at a positive uniform scale as far as possible. Then compare world AABBs, volume signs, basis-vector orthogonality and known anchor distances before and after baking.

Unit errors are especially insidious once they propagate. Treat a meter as a centimeter and inertia changes by multiples according to length squared and mass distribution. Contact offset, gravity step size, agent radius, cell size and camera near/far clipping distort at the same time. The asset gate should therefore choose at least two real-scale anchors, such as door height and a calibration rod, and check

$$
\left|\frac{d_{\mathrm{engine}}}{d_{\mathrm{reference}}}-1\right|<\epsilon_s,
$$

It should also make the unit a manifest field rather than something the importer guesses. Large worlds also need to separate geographic double-precision positions from local floating-point positions. During origin rebasing, render objects, physics actors, NavMesh tiles, particles and network replication must switch anchors in the same logical tick.

#### The runtime chains of Unity, Unreal, Godot, and PhysX are not the same set of defaults

The Unity route. An isolated worker parses source files such as glTF/FBX first, then outputs the intermediate assets the project allows. Once the assets enter the project, each side acts on its own. The rendering side forms Mesh, Material, Texture and Renderer. The physics side independently forms primitive/convex MeshCollider or static non-convex MeshCollider. The navigation side determines the source set and the baking result through NavMeshSurface, agent type, modifier, link and obstacle configuration. 〔Source identifier C005〕[@srcV002] The runtime glTF loader supports network sources and extension callbacks, [@srcE004] so arbitrary URIs must be disabled and extensions and total assets limited at the host layer. Unity's `convex` is not a general switch for "improving collision quality". It changes what kind of rigid-body interaction a mesh can be used for, and approximate convexification also loses concave grooves. Import tests should cover non-uniform scaling, cooking options, the collision layer matrix, the trigger/simulation distinction and scene asynchronous loading order.

The Unreal route. Source assets enter Static Mesh/Skeletal Mesh and the material system. The visual side can generate regular LODs or HLODs, or take the corresponding virtualized geometry path. The physics side maintains primitives/convex bodies for simple collision and per-triangle queries for complex collision. The navigation system generates a tile-organized polygon graph from the collision geometry that is allowed to affect navigation, and applies area cost/link. 〔Source identifier C006〕[@srcV003] 〈Use Complex Collision As Simple〉 lets a per-triangle surface handle complex queries. The official documentation, however, states clearly that it cannot become a general dynamic simulation body. When World Partition or another streaming mechanism loads an actor, the corresponding physics shapes and navigation tiles must be generated consistently as well. Seeing a Static Mesh actor does not mean that server-side collision and path data have already been committed.

The Godot route. Rendering assets usually enter MeshInstance3D. Physics proxies are carried by nodes such as StaticBody3D/RigidBody3D and CollisionShape3D, and navigation is carried by NavigationRegion3D/NavigationMesh together with the map/region/link relationships of NavigationServer. The official recommendation is to prefer primitives or convex bodies for dynamic objects, and to aim concave triangle collision mainly at static objects. Those concave shapes are hollow, so fast small objects may still pass through. [@srcC007] Godot's navigation baking is likewise constrained by cell size/height, agent radius/height, slope, steps and tile border. An overly fine mesh drives up build cost and even causes freezing, and wrong borders produce seams. [@srcV004] Verify the visibility of the node tree, the process state and the RID lifecycles of the physics/navigation servers separately. Do not let "the node exists" substitute for "the server object is valid."

The PhysX route. PhysX reveals the underlying distinction directly. A shape contains `PxGeometry` and a material, and it can be set separately as a simulation shape, a scene-query shape or a trigger. TriangleMesh/HeightField has support constraints for dynamic actors, and a dynamic triangle mesh requires cooking an SDF and setting valid mass, inertia and solver parameters. [@srcC004] The high-precision mesh used for ray queries, the convex piece used for rigid-body contact and the trigger region can therefore be three shapes. If an engine wrapper merges them into a single switch, the project should still preserve the original roles in the asset description. Otherwise, adjusting the collision filter can mistakenly turn a trigger into a solid wall.

The common principle of the four routes is "explicit mapping, not relying on import defaults." The same normalized object should generate one mapping table on each platform: coordinate transform, rendering assets, collision shape type, dynamic category, query/simulation/trigger flags, collision mask, navigation source, agent profile, streaming cell and version. Cross-engine consistency tests compare world query results, not editor screenshots.

#### LODs, materials, and streaming must preserve physical semantics

Visual LOD selects triangle counts, texture mips, material complexity or geometry clusters according to screen error. Collision LOD selects proxies according to the maximum allowed contact error, velocity and object class. The two may share distance intervals, but they must not share switching events. A distant visual wall can have its door frame simplified away while collision stays stable. If collision also changes with camera distance, different clients in a multiplayer game may end up with different physical worlds. Server-authoritative collision must be decided by game state, not by camera distance.

Materials likewise fall into two layers. A PBR material determines rendering parameters such as base color, roughness, metallic, normal and alpha. A physics material determines friction, restitution, density or contact combinations. A transparent texture does not automatically cut a hole in collision. Two-sided rendering does not automatically confer a two-sided solid, and a displacement map does not automatically change the collider. If a visual surface contains a genuinely traversable hole, the reference geometry and the collision proxy must express it explicitly. If the hole is merely a fence alpha mask, choose among a simplified railing, a blocking plane or no collision according to the task, and record the choice.

The smallest atom of large-world streaming is not a single texture but a spatial commit unit:

**Text flow or pseudocode in the original manuscript**
\begin{verbatim}
WorldCellBundle {
cell_id, world_bounds, generation,
render_chunks[], collider_chunks[], nav_tiles[],
entity_state[], boundary_links[], dependency_hashes[]
}
\end{verbatim}

During loading, decode and validate each sub-resource in a staging area first. Then register the render/physics/nav objects, validate boundary links and generation, and commit visibility in one step. During unloading, block new queries from entering first, migrate or freeze dynamic objects, and remove cross-chunk links. Only then revoke the NavMesh, colliders and rendering resources. If the collider is unloaded before the NavMesh, an agent may plan a path through an area that has not yet been updated. If rendering is shown before physics is ready, the player will fall. If asynchronous rebaking mixes old tiles with new colliders, path results have no consistent snapshot.

#### Runtime state, queries, and rollback

An interactive world needs to distinguish static asset versions from dynamic state. One entity state machine should drive a door's render mesh, its closed/open collider, its NavMesh obstacle or off-mesh link, and its script state. The animation must not open the door first while the collider lags by one frame, and the NavMesh must not permanently cache the closed state. Events should adopt two-phase commit. Compute and validate the candidate physics/navigation changes first, then switch the entity generation at the same fixed tick. For failed updates, retain the previous signed proxy rather than using "half a new NavMesh."

Runtime queries should be traceable. A single anomalous raycast should at least report the hit 〈entity_id, shape_id, shape_role, source_hash, build_generation, triangle_or_feature〉. A single path query should report the agent profile, the tile generations used, area cost and off-mesh links. Without these fields, going through a wall or being unreachable can only be guessed at from video recordings. It is also impossible to distinguish a geometry error, a filtering matrix error, a stale tile or an error in dynamic state.

Engine acceptance should at least include known raycast hits and misses. It should also include static falling bodies, corners, stacking, high-speed sweeps and different time steps. Clearance through door openings and narrow passages belongs on that list, along with connected components and task start/goal points for each agent class, tile seams and cell loading/unloading, and coordinate anchor resets. The same gate should cover collision unchanged before and after LOD switching, authoritative queries consistent between server and client, and peak CPU/GPU/memory and failure atomicity. Only when these runtime results pass does the space constructed in the preceding chapters truly become a "usable world." At the same time, importers, converters and runtime packages expand the attack surface. The next chapter follows the same state chain to explain how an attack propagates all the way from input to parsers, collision, navigation and the host.

#### Building engine space with code: editor automation, USD, and synthetic data

When procedural modeling enters the engine, the smallest input should not be "run this script" but a versioned `BuildSpec`. That spec carries scene/asset IDs, units and axes, parameters and constraints, material/semantic vocabularies, collision strategy, LOD error, NavMesh agent parameters, random seed, tool/plugin/dependency locks, and allowed input and output roots. It also carries a joint budget for vertices, triangles, instances, textures, memory, wall clock and total output bytes. The normalized input, the toolchain and the dependency closure together form the build identity. The build identity proves that "the input is the same," not that the content is correct. Geometry and runtime queries are therefore still required.

Unity editor automation. When code constructs a `Mesh` directly, the simple API performs more index and bounds validation. The advanced vertex/index buffer API may skip some checks for throughput and shift the responsibility for validity to the caller. Unity Mesh API [@srcEA-U001] The reliable route validates attribute array lengths, finite numbers, index ranges, submeshes and budgets first, and only then creates the visual mesh. Afterwards it generates the collider from the original intent or from a controlled proxy, rather than unconditionally copying the high-poly model. `AssetPostprocessor` can set the importer before model import and check the hierarchy and meshes afterwards. For custom formats, a versioned `ScriptedImporter` can generate primary/sub-resources. These callbacks all execute within the editor trust boundary and are not a sandbox. Unity AssetPostprocessor [@srcEA-U002] Unity ScriptedImporter [@srcEA-U003]

Unity batch builds should specify the project, platform, static execute method, log and staging output explicitly, in a one-shot process whose failures propagate through `BuildReport`. The same process should not switch among multiple target platforms in sequence, to avoid contamination from active configurations and assembly reloads. Build methods must be idempotent. If the target asset already exists, compare the generated manifest and update the controlled fields first. Do not accumulate duplicate meshes, colliders or scene nodes by "creating another object with the same name." Unity command-line builds [@srcEA-U004]

Unreal editor automation. Unreal Python and Editor Utility suit asset batching, layout, LOD and validation, but the official documentation still marks Python support as Experimental. Asset moves and saves should go through AssetTools/EditorAssetLibrary, so that internal references are updated with the transaction. A filesystem rename must not be used directly. Unreal Python [@srcEA-E001] Geometry Scripting uses `UDynamicMesh` as its working representation. It can add primitives, construct triangle by triangle, perform booleans, remesh, simplify and run intersection queries. Yet the official documentation also makes clear that some of its capabilities are editor-only. The same documentation states that DynamicMeshComponent does not automatically possess final shipping properties such as Nanite, LOD, distance fields and instancing, and that Blueprint/Editor Utility calls usually occupy the game thread synchronously. Unreal Geometry Scripting [@srcEA-E003] A script must therefore write the DynamicMesh out as a controlled StaticMesh/asset, and then rebuild collision, LOD, materials and navigation. But "it displays successfully in the editor" is not cook success.

Unreal's offline gate uses `UnrealEditor-Cmd`, commandlet/cook and Data Validation. A commandlet should not depend on the map currently open in the editor, the selected objects or a user-launched script. The required world, plugins and package paths should all be declared explicitly. Before release, check for unexpected dependencies, package count/size, simple/complex collision strategy and the path matrix of the target agent, and then reload the cooked artifacts from an empty process. Unreal Cooking [@srcEA-E006] Unreal Data Validation [@srcEA-E007]

Godot lightweight branch. `EditorScript`/`@tool`, `EditorPlugin`, `EditorImportPlugin` and `ArrayMesh` can likewise establish procedural asset entry points. ArrayMesh accepts attribute arrays and indices, but correct array lengths do not prove that the manifold, collision or navigation is correct. Tool scripts also run at the boundary of editor capabilities. Godot EditorScript [@srcEA-G001] Godot ArrayMesh [@srcEA-G002] The headless pipeline explicitly uses 〈--path --headless --script〉 or import/export arguments, and the wrapper should also reject unknown arguments and check the expected output. A command being accepted does not mean that the target script and export preset actually took effect. Godot command line [@srcEA-G004]

OpenUSD scene composition. USD's procedural objects are not flat meshes but Stage, layer, prim, variant, reference, payload, sublayer, resolver and edit target. A builder must explicitly write `metersPerUnit` and the up axis at the root layer. It must pin the variant and payload load set, and record the path and hash that each logical asset path actually resolves to. The fallback value when USD has no declared linear unit cannot substitute for a project contract. OpenUSD linear units [@srcEA-USD006] Composition arcs facilitate collaboration and lazy loading, and they also permit deep references, dependency escapes and products of counts. After resolution, a publishing worker should reconfirm that paths lie within the allowed roots. It should also limit reference depth and expansion bytes, disable network resolvers by default, and save the original composition manifest together with a consumable snapshot. `usdchecker` can check format/configuration, but it cannot substitute for resource limits and script isolation. OpenUSD Ar Resolver [@srcEA-USD004]

Omniverse/Isaac Sim Replicator synthetic data. Procedural scenes let randomized state draw on the same source as annotations such as RGB, depth, normals, segmentation and boxes. The input contract should include the controlled USD, semantic prim mappings, camera intrinsics and extrinsics, the renderer, and the physics step/settling period. It should also include random distributions and constraints, global and component seeds, the annotator/writer schema and the output root. Every frame must share `frame_id`, time, camera matrices, a scene state summary and the prim-to-instance mapping. A fixed seed cannot eliminate differences in GPU, physics, extension versions or scheduling. The official Replicator documentation warns further that different step/wait settings may leave a writer or annotator corresponding to the previous frame. Supervision data must therefore wait explicitly for rendering and perform cross-modal projection checks. Isaac Sim Replicator [@srcEA-R001]

These five automation routes share the same nine-step publishing algorithm. Normalize the spec and hash it. Use a one-shot isolated workspace. Generate/import visual geometry. Generate collision independently. Generate and accept LOD. Bake each agent's NavMesh from the approved collision source. Export engine/USD/training data. Read back and compare queries in a fresh consumer process. Promote atomically from a control plane separated from user scripts. Any failure destroys only the staging generation. It must not mix half a visual asset, a stale collider or misaligned-frame labels into the official world.

---

[← Back to contents](index.md)
