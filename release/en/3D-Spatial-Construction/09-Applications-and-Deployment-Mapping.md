## Applications and Deployment Mapping

Application mapping starts from the final query and works backward to the construction pipeline. Whether one needs to view, measure, edit, collide, navigate, plan or roll back determines the choice of a different primary representation and verification gate.

No single optimal pipeline applies to all inputs and applications. Selection should work backward from the query that ultimately needs to be answered: viewing images only, free viewing, precise measurement, editing surfaces, real-time collision, agent navigation or long-horizon action response. Six common construction tasks are given below.

### Classification and Verification Figures

These figures derive from the earlier project's structured data or generation scripts, and their captions also give the readable range. The main text does not use comparison tables as a substitute for argument.

![Classification figure that back-infers the construction pipeline from the final query contract. The selection order fixes the query that needs to be answered first. Only then does it choose the representation, assetization, collision and navigation, and engine runtime layers.](../../figures/en/fig09_pipeline_selection.png)

*Classification figure that back-infers the construction pipeline from the final query contract. The selection order fixes the query that needs to be answered first. Only then does it choose the representation, assetization, collision and navigation, and engine runtime layers.*

### High-Fidelity Digitization of Real Scenes

Recommended pipeline: planned capture → camera/color preprocessing → SfM/SLAM poses → MVS or feed-forward point maps/depth → 3DGS/NeRF appearance layer + TSDF/neural SDF/mesh geometry layer → mesh repair and materials → independent collision and navmesh → engine verification.

Why keep two representations? A 3DGS/NeRF layer usually fits complex appearance and view-dependent effects better near the training viewpoints. The geometry layer is better suited to scale, measurement, editing and collision. A project that only needs a virtual tour can put more budget on the appearance layer. A project for robots or digital twins is different. There, geometry and scale verification take priority over PSNR.

Key checks are the revisit loop, reprojection, depth and point-cloud error, uncovered regions, thin or transparent objects, and coordinates and scale. They also cover mesh self-intersections and holes, collision penetration, navigation connectivity, and peak engine resources.

### Rapid World Generation from a Single Image or Text

Recommended pipeline: text/image → layout or coarse 3D conditions → multi-view/panoramic/3D world generation → feed-forward geometry or scene representation → object decomposition → separation of the visual mesh/Gaussians from collision proxies → manual semantic correction → engine baking.

There is one fact that has to be accepted. Invisible content has no unique ground truth. A generative system should output uncertainty, version and provenance. It should not describe prior-based completion as reconstruction. In gameplay and art prototypes, plausibility matters more than observational fidelity. When the goal is to replicate real locations, generated regions and observed regions must be labeled in separate layers.

Services such as Marble should be used in one specific way. Their splats can serve as a high-fidelity visual layer, and their exported coarse collider as a prototype physics layer. Collision for key gameplay regions can then be rebuilt or manually corrected. The official documentation already states that high-quality visual meshes can also fail at uncovered regions, reflections and thin structures. Asset inspection and game testing therefore cannot be skipped. Marble mesh export [@srcWL-MARBLE-MESH]

### Interactive Video Worlds and Agent Training

Recommended pipeline: action-conditioned world model → explicit state/memory interface → pixel or 3D state rollout → safety shell/action gating → explicit maps or object tables for key states → comparison with conventional physics/task logic → agent evaluation.

Where is the application boundary? Pixel world models are suited to generating diverse, continuous visual experience and counterfactual scenes. A task that depends on precise distance, contact or reward, however, needs a verifiable state channel. One feasible hybrid scheme lets the generative model handle appearance and unseen content. An explicit simulator then handles collision, dynamics, task termination and safety constraints.

Key checks cover the replay of the same actions, branch consistency, revisit memory and object permanence. They also cover state leakage, long-horizon drift, action latency, closed-loop task success rate and safety constraint violations. Short-video preference scores cannot substitute for these metrics.

### Games, XR, and Content Production

Recommended pipeline: generated/scanned assets → object and semantic separation → mesh repair/retopology → UV/PBR materials/baking → multi-level LOD/streaming → simple/complex collision → navmesh and interaction markers → platform builds and frame budget testing.

How do the representations combine? Static backgrounds can use Gaussian or neural rendering plug-ins. Close-range interactive objects use conventional meshes, skeletons and materials. Vision and collision stay separate. Distant scenery, sky and non-interactive regions should not pay the cost of high-precision collision.

Key checks cover GPU, CPU and memory budgets for desktop, mobile and XR. They also cover transparent and reflective materials, lighting and shadows, and LOD popping. Then come collision layers and triggers, continuous collision and high-speed objects, and navigation agent size. Finally, asset loading and unloading, and failure fallback.

### Robot Simulation and Digital Twins

Recommended pipeline: calibrated sensor capture → reconstruction with scale → semantics/objects and affordances → contactable surfaces and physical parameters → robot collision model → sensor simulation → task/safety constraints → back-labeling with real data.

The priority here is different. Geometry, scale, reachability and sensor error rank above photorealistic appearance. Generated content can serve domain randomization and long-tail scenarios. But the project must make explicit which variables are synthetic, how the distribution is sampled, and whether task difficulty is changed.

Key checks cover corresponding real and simulated coordinates, joint and contact parameters, occlusion and sensor noise, trajectory executability, and collision margin. They also cover reset repeatability, simulator determinism and sim-to-real deviation.

### Rule-Based Spaces, Modular Levels, and Batch Synthetic Data

Recommended pipeline: freeze the unit/coordinate/parameter schema → construct geometry with OpenSCAD, CadQuery, FreeCAD, or Blender/Houdini nodes/scripts → semantic IDs and material/instance rules → generate the visual, LOD, and collision proxies separately, each from the intent → idempotent import and layout via Unity/Unreal/Godot editor scripts → NavMesh/sensor/annotation generation → export read-back, query regression, and atomic release.

When should the procedural route win? When the target has repeated modules, explicit dimensions, assembly relations or manufacturability constraints, or when it needs large-scale controlled variation. In that case one should not generate images first and then reconstruct structure that is already known. Architectural rooms, roads, pipe networks, industrial parts, warehouse shelving, mazes, training obstacle courses and parametric cities can all encode design intent directly as rules. A real digital twin can use scans to estimate on-site deviation. Parametric CAD then solidifies walls, doors, equipment and editable structures. The generative model handles materials, furnishings or unobserved candidates, but it must not overwrite measurement residuals.

Key checks are these. The parameter range and its constraints must be satisfiable. The output must be deterministic once the seed and the toolchain are fixed. Booleans and B-rep must be valid, and tolerance must not swallow the smallest feature. Object IDs must remain stable after an insertion or a deletion. Visual, collision and navigation derivatives must share coordinates, yet each must be accepted independently. The hierarchy, units, transforms and materials must read back consistently from the exporter and from the target engine. Untrusted scripts, plug-ins and source files must run in a worker with no network access, no credentials and limited resources. When only Python AST or textual bracket checks are available, the status must be written as "syntax/static check". One must not claim that it has already run in a Blender, CAD or engine host.

### Choosing the Primary Representation by the Final Query

Suppose the task is only film and television, or shot previsualization. The primary representation can then remain a video or an action-conditioned pixel world. The entry point is text, images and shot conditions. The physics proxy can be omitted or kept minimal. Acceptance focuses on condition adherence, cross-frame continuity and replayability of the same shot. The main risks are long-horizon drift and camera trajectories that cannot be reproduced. A virtual tour calls for a dual representation of 3DGS/NeRF appearance plus simplified geometry. The entry point is multiple images, video or panoramas, and coarse collision is built for the ground and the obstacles. Novel views, revisits, floaters, coordinates and uncovered regions form the minimum threshold.

When the result must support real measurement or a digital twin, the primary representation has to shift to scaled point clouds, TSDF, SDF or meshes. Parametric CAD can serve as design intent and editable structure. On-site deviation, though, must be verified with calibrated RGB-D, LiDAR or multi-view data with sufficient coverage. Collision proxies can be refined in layers. Scale, registration, thin surfaces, reflective surfaces, topology and version must still be verified item by item. Game prototypes and XR are better served by a hybrid of procedural meshes and Gaussian/neural appearance. Generation or scanning handles the content entry point. Blender/Houdini/engine scripts handle constrained assets and layout. Primitives, convex decomposition and NavMesh handle interaction. Acceptance must cover idempotent import, LOD, collision, navigation, device frame budgets, narrow passages and occlusion at the same time. It cannot look only at editor screenshots.

Robot training requires meshes/SDF with true scale, semantics and affordances. It must explicitly maintain robot and environment collision, contact and sensor models. Controlled generation can extend long-tail scenarios, but it cannot replace checks on contact, reachability, reset determinism and sim-to-real. Online streaming of large worlds turns the problem into spatial partitioning. Chunked meshes, hierarchical Gaussians or voxels handle vision and geometry. Partitioned collision and a tiled NavMesh handle local queries. Memory peaks, coordinate precision, world origin, LOD, dynamic re-baking and load-failure fallback must all be tested.

### Decision Rules

If the output must support collision, require a surface/distance query contract first. When there is only video, novel views or Gaussians without topology, the plan must include a mesh/SDF/proxy construction step. If the output must be editable, require object, semantic, topology and material layering first. If the whole scene is baked into one indivisible large mesh, later editing costs move forward as technical debt. If the output must be measurable, require scale, coordinates and uncertainty first. Single-image generation and purely appearance-based optimization cannot carry this responsibility alone. If the output must run in real time, budget rendering, physics, navigation and streaming as separate items. Rendering at 60 fps does not mean that collision or replanning is also within budget. When the output feeds safety-related decisions, refuse to rely on a demo alone. Data, protocol, failure samples, variance, adaptive attacks and independent reproduction must be explicit. If the structure is already known and batch variants are needed, prefer writing design intent as parameters and constraints. At the same time, lock the host/kernel/plug-in versions, the random seeds and the export contract. Validate generated code as an untrusted supply chain input.

---

[← Back to contents](index.md)
