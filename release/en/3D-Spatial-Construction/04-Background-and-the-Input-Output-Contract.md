## Background and the Input--Output Contract

This section unifies the seven-layer pipeline and the input--internal state--output contract. It provides a common interface for the subsequent taxonomy.

### Data and Conditioning Layer

This layer specifies what the system can see and what counts as a known quantity.

Observations consist of single image, multi-image, video, panorama, RGB-D and LiDAR/point cloud. Known quantities include intrinsics, extrinsics, timestamps, IMU, and scale or GPS. Conditioning consists of text, sketch, coarse 3D layout, object list and style. It also consists of user actions, agent pose, historical frames, memory and control targets.

Input conditions directly determine identifiability. A single image does not provide enough observations to uniquely determine occluded regions and absolute scale, and any full back side or unseen space involves prior-driven inference. If multi-image input lacks overlap, texture or stable exposure, matching and pose also degrade. If a dynamic video treats object motion as camera motion, it contaminates the entire geometry chain.

### Generation or Estimation Layer

Generative models and reconstruction algorithms diverge at this layer.

The generation route learns a conditional distribution and provides plausible but not guaranteed-real content in unobserved regions. The estimation route uses observation constraints to recover pose, depth, correspondences and surfaces. The hybrid route uses generative priors to fill invisible regions and then makes the result queryable through multi-view or explicit geometric constraints.

Classical incremental SfM recovers sparse geometry and cameras through feature matching, geometric verification, camera registration, triangulation and bundle adjustment. The corresponding COLMAP paper [@srcG003] DUSt3R instead directly regresses uncalibrated image pairs into point maps and then performs global alignment. It lowers the barrier of camera priors, but going from point maps to production meshes still requires post-processing. DUSt3R [@srcG011] VGGT jointly outputs cameras, depth, point maps and point trajectories in a single forward computation. That joint output represents the development direction of 3D geometry foundation models. Its output is still geometric attributes and point-based representations, not engine assets with UV, collision and navigation completed automatically. VGGT [@srcVGGT-2025]

### Scene Representation Layer

The representation determines which queries are cheap and which edits are difficult.

Voxel/TSDF/SDF holds occupancy or signed distance to the surface, which suits fusion, isosurface extraction and distance queries, but high-resolution dense grids consume memory. Point cloud/point map/surfel preserves sampling positions, colors, normals or confidence directly, and incremental fusion is convenient, but there is no closed surface or topology. NeRF/neural implicit field is continuous and differentiable and good at appearance fitting, while per-scene optimization and surface extraction are usually expensive. 3DGS uses explicit Gaussians, trains and renders fast, and offers strong transparency and view-dependent appearance, but geometry, topology and collision still require constraints or conversion. A triangle mesh has a mature ecosystem for engines, DCC, rasterization and physics, while topology, UV, materials and LOD need maintenance. A hybrid representation lets meshes handle topology, animation and collision while Gaussians/neural textures handle high-fidelity appearance, a compromise of considerable practical value today. A latent world state is convenient for autoregressive prediction and action-conditioned generation, but if geometry cannot be exported or queried, it cannot replace an asset representation.

### Geometric Assetization Layer

This layer turns "data that can be displayed" into "assets that can be maintained". The typical steps are as follows.

Unify coordinate systems, units, orientation, cameras and scene origin. Remove outliers and low-confidence floating geometry. Estimate normals and keep their orientation consistent. Obtain a triangle mesh via alpha shape, Ball Pivoting, Poisson, TSDF or neural surfaces. Delete small connected components, fill holes, handle self-intersections, run manifold checks and correct normals. Simplify, retopologize, build LODs, unwrap UVs, and bake textures and materials. Export and validate according to target formats such as glTF/GLB, USD, FBX, OBJ, PLY and SPZ.

Open3D's official surface reconstruction documentation clearly reflects the algorithmic assumptions. Ball Pivoting depends on oriented normals and a suitable ball radius. Poisson solves a regularization problem to obtain a smooth surface, and the octree depth determines detail and resource consumption. Open3D surface reconstruction [@srcOPEN3D-SURFACE-P006] So "point cloud to mesh" is not a parameter-free format conversion. It is a reconstruction step, and it carries assumptions about scale, density, noise and topology.

### Physical Proxy Layer

Visual meshes aim at the image. Physical proxies aim to answer contact and reachability queries in a stable, cheap and conservative way. The two should usually be separated.

Static buildings can use simplified triangle meshes or height fields. Dynamic rigid bodies preferably use boxes, spheres, capsules, convex hulls or multiple convex pieces. Complex concave objects can undergo approximate convex decomposition or adopt SDF/voxel distance queries. Characters usually use stable proxies such as capsules rather than skintight high-poly meshes. The walkable ground surface is determined by proxy radius, height, slope, steps and clearance. It cannot be directly equated with visible ground triangles.

Unity's official documentation explicitly states that Mesh Collider can fit complex shapes more closely than primitives but is usually more expensive. Convex Mesh Colliders have a triangle count limit, and mesh cooking also cleans up degenerate triangles and preprocesses runtime queries. Unity Mesh Collider [@weba9de639ddac5] Unreal draws the same line: simple collision (primitives/convex hulls) and complex collision (triangle meshes) are two independent query representations. Unreal simple/complex collision [@srcUE-COLLISION-C006] That split is not an incidental engine limitation. It is a fundamental trade-off of real-time physics among stability, memory, build time and precision.

### Engine Runtime Layer

Once a scene enters the engine, several further jobs still need to be completed.

Convert coordinates, scale, handedness and axes. Set PBR materials, color space, transparency, two-sided and shadow rules. Implement LOD, meshlet/Nanite-style geometry streaming, occlusion culling and texture streaming. Configure collision layers, triggers, continuous collision detection and physics materials. Build the navigation mesh, dynamic obstacles, off-mesh links and agent configuration. Manage scene partitioning, world origin rebasing, network replication and save/load. Isolate scripts, shaders, plugins and the asset supply chain.

Recast's public implementation voxelizes the input triangle mesh, filters out non-walkable voxels, partitions regions and regenerates a polygon navigation mesh. Tiled navmesh supports re-baking and streaming for large scenes. Recast Navigation [@srcRECAST-NAV-V001] Godot's documentation further emphasizes that navigation and rendering/physics are independent systems. The navmesh is an approximate walkable area for a specific agent, and it cannot be expected to fit the ground precisely. If a character needs to stick to the ground precisely, it must also rely on physics or other ground queries. Godot NavigationMesh [@webfbcb09318894]

### Validation and Security Layer

Each layer needs its own gate.

Input gates check file type, size, provenance, license, hash, camera and timestamp consistency. Reconstruction gates check pose loop closure, reprojection, depth/point cloud error, confidence and anomalous frames. Representation gates check novel view, geometry, temporal and unobserved-region uncertainty. Mesh gates check finite numbers, triangle/vertex caps, degenerate faces, self-intersection, manifold, watertightness, connected components, normals, UV. Collision gates check proxy error, penetration, continuous collision, stacking, resting jitter and query latency. Navigation gates check connectivity rate, unreachable points, narrow passages, slope/steps and dynamic replanning. Engine gates check import warnings, frame budget, peak memory, streaming, script/shader allowlists and sandbox.

Only when these gates are explicitly recorded can a project know where a failure occurred. That failure may be "the model did not generate", "the geometry is incorrect", "the asset is unusable" or "the runtime configuration is wrong".

### Technical Lineage: New Representations Expand Capabilities Without Eliminating Older Layers

### Geometry-First Stage

Photogrammetry, SfM/MVS, SLAM, and RGB-D fusion built the backbone that turns correspondences, pose, and depth into point clouds and meshes. Geometric quantities mean something definite, errors can be measured, and the results land close to CAD or engine assets. That is the advantage. The disadvantage is dependence on texture, lighting, calibration, view coverage, and dynamic scene handling. Fusion methods such as TSDF average multi-frame depth into voxels as truncated distances, so stable surfaces are easy to extract. Memory, though, constrains resolution and spatial extent.

### Neural Implicit and Neural Rendering Stage

NeRF treats a scene as a continuous radiance field. That significantly improved novel-view quality and opened branches for acceleration, dynamics, reflection, semantics, editing, and surface reconstruction. Its core value is that rendering error optimizes the scene function directly. Its core limitations are per-scene optimization, dense ray queries, and the ambiguity that runs from density to surface. Neural SDF/occupancy fields write geometry into implicit functions and improve mesh extraction, but they still struggle with materials, lighting, scale, and thin structures.

### Explicit Differentiable Primitive Stage

3DGS marks a leap in training and rendering efficiency, built on explicit anisotropic Gaussians and differentiable splatting. Explicit centers, scales, rotations, opacity, and spherical harmonic appearance make culling, compression, and streaming easier. Yet "explicit" does not equal "surfaced". Occlusion relationships and color optimization are enough to produce beautiful images, but they do not guarantee that every Gaussian lies on the true surface. Follow-up work such as SuGaR, 2DGS, depth-supervised GS, and mesh-aligned GS is filling precisely this gap.

### Feed-Forward Geometry Foundation Model Stage

DUSt3R, MASt3R, VGGT, and their successors partially compress camera, matching, depth, and point map estimation into the forward inference of a shared Transformer. Those steps previously required several modules and iterative optimization. These models significantly lower the barrier for casually captured input. They also bridge rapid 3D extraction from video generation results. Geometry foundation model outputs, however, still require validation on confidence, scale, drift across long sequences, dynamic objects, and extrapolated regions. A fast point cloud is still not a final game asset.

### Generative Video and World Model Stage

Video diffusion has improved content diversity and camera control. Autoregressive world models add user actions to state transitions, so frames can respond in real time. The research version of Genie learns a controllable environment from videos that carry no action labels. It relies on spatiotemporal tokenization, autoregressive dynamics, and a latent action model [@srcGDM-GENIE]. Genie 3's official public page reports 720p, 24 fps, and a duration of several minutes. It explicitly lists limitations. These include a limited action space, limited interaction duration, multi-agent interaction, and accuracy of real locations. This round found no public technical report or weights for Genie 3 as of the survey's cutoff date [@srcGDM-GENIE3-W09]. The native output of such models is still an action-conditioned pixel sequence. The user-experienced "walking around in a 3D world" therefore does not automatically imply a downloadable static mesh, a collision body, or a deterministic physics state.

### The Parallel Backbone of Procedural Rules and Constraints

3D spatial construction does not begin with photogrammetry or generative models alone. OpenSCAD, parametric CAD, Blender/Houdini scripting, Geometry Nodes, procedural level generation, and engine editor automation take dimensions, sketch constraints, CSG trees, node graphs, object rules, and random seeds straight into geometry and scene graphs. This route is older than neural representations, yet it still matters for batch content, digital twins, synthetic data, and agent-driven modeling. Known structure does not have to be inferred back from pixels. Wall widths, door openings, and part relations can therefore be preserved explicitly. Stable object IDs are a further step, and they become constructible only when the schema derives IDs from semantic paths and stable parameters. Tool default numbering, exported face indices, or "the Nth face" provide no such guarantee. The route describes design intent, and it does not automatically prove on-site reality.

The procedural route also reveals two different meanings of the word "generation". Probabilistic generative models sample content from a learned distribution. Even with conditioning held fixed, reproducing the result may require the sampling strategy and the model version. A procedural generator instead evaluates from explicit rules. Once its parameters, seed, and toolchain are fixed, it should return the same normalized asset. The two can be combined. Language or video models propose layout and style. DCC/CAD code then fixes measurable dimensions and topological constraints. Geometry libraries perform differential validation. Engine scripts finally build collision, navigation, and runtime state. Code can produce a mesh directly. That does not mean the mesh has correct units, manifold topology, collision semantics, or trustworthy script provenance. Later layers still accept these contracts independently.

### Persistent 3D State and the Production Asset Stage

Representative work from 2025–2026 shows that explicit or latent 3D state beyond pixel history is re-entering world models. PERSIST writes the environment, camera, and renderer into a latent 3D scene, and it directly targets the spatial forgetting and long-horizon inconsistency caused by limited context [@srcPERSIST-2026]. WorldGen starts from text and combines layout reasoning, procedural generation, 3D generation, and object-level decomposition. It moves the goal from "viewable" to "traversable and interactive" [@srcWORLDGEN-2026].

Marble takes another path, a product-type one. Its official documentation states that it accepts text, a single image, multiple images, panoramas, video, and coarse 3D structures. It can export SPZ/PLY at about 2 million or 500,000 Gaussians, high-quality GLB meshes, and coarse collision GLB (Marble overview [@srcWL-MARBLE-OVERVIEW]; Marble export specification [@srcWL-MARBLE-EXPORT-M04]). More critically, the documentation explicitly distinguishes the highest-fidelity splats, visual meshes of about 600,000/1,000,000 triangles, and collision meshes of about 100,000–200,000 triangles. It acknowledges that visual meshes may show holes, clumps, and floating faces on thin structures, transparent/reflective surfaces, sky, and uncovered regions (Marble mesh export [@srcWL-MARBLE-MESH]). This is a very concrete piece of product evidence for this survey's central argument. Visual, surface, and physical proxies remain three different assets, even if one system integrates generation, reconstruction, and export in a single interface.

### The Unified Input--Internal State--Output Contract

The unified contract does not compress methods into a single total score. Instead it keeps asking five questions. What observables does the input provide? What does the internal state store? What queries can the native output answer? What assumptions does training or solving depend on? Which asset layers are still missing in the end? Video generation starts from text, image, or shot conditions and predicts frames in a pixel or latent video state. It usually still lacks pose, scale, surfaces, and physical assets. Action-conditioned world models add actions and memory. If that state cannot be queried and exported, deterministic geometry still cannot be obtained on that basis. SfM/MVS and RGB-D/TSDF begin with overlapping observations, cameras, or depth. What they recover natively is cameras, point clouds, depth, or isosurfaces. They still have to deal with uncovered regions, dynamics, texture, and assetization.

Feed-forward geometry models move the estimation of cameras, depth, point maps, or point trajectories into the network forward pass and amortize it there. That reduces the per-scene solving cost, but global scale, long-sequence loop closure, and the final surface still require validation. NeRF natively answers color and density queries along rays, and 3DGS natively answers efficient projection and synthesis queries. Neither obtains stable topology automatically just because rendering quality is high. Procedural DCC/CAD, by contrast, constructs B-rep, meshes, and scene hierarchies directly from parameters, constraints, CSG trees, or node graphs. It natively answers "what asset should be generated given the rules". It does not automatically answer whether the asset matches real observations, whether the host is safe, or whether collision is correct. Point clouds store spatial samples but not face connectivity. Meshes store vertex--edge--face adjacency relations, but manifoldness, UV, materials, and LOD need maintenance. The game engine then organizes these assets, physical proxies, and logic into a runnable scene.

The color coding in the figure indicates only "native output", "obtainable through explicit conversion", or "not natively provided". It does not indicate high or low performance, and it cannot be summed along a row. The subsequent analysis always uses this input--state--output--gap contract. It asks which solving step each work actually replaces, what kind of output is measured, and which responsibilities are left to the next layer.

---

---

[← Back to contents](index.md)
