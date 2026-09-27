## Introduction

The core problem of spatial construction is not whether an image has a three-dimensional look. It is whether the system can connect provenance, coordinates, representation, geometry, physical proxies and runtime state into a verifiable transformation chain.

In this survey, 3D spatial construction means turning a set of observations or generation conditions into a spatial system that can be used sustainably. Such a system does not necessarily reach the same depth in every project. It must at least make its delivery contract explicit. Does it output only a video clip, or a scene that can be queried from any camera? Does it have measurable geometry? Can it become a topologically stable surface? Can it support collision, navigation and dynamics? Can a game engine, XR or a robot simulator load it reliably?

The definition deliberately keeps the following results apart.

A video model generates a pixel sequence with parallax and motion. A world model can continuously generate the next frame according to actions. NeRF or 3DGS can render high-quality images from new cameras. A point cloud provides a large number of surface samples in three-dimensional coordinates. A mesh provides triangular faces with connectivity relations. A collision proxy allows objects to maintain stable contact in a physics solver. A navigation mesh can describe the reachable region under a given agent size, slope and step constraint.

Everyday language can call all of these objects "3D", but their inputs, states and verifiable properties differ. Placing them side by side in a leaderboard produces erroneous conclusions. For example, the original NeRF paper optimizes a function from known camera images to continuous density and view-dependent color, and the paper's direct goal is novel view synthesis. It does not treat watertight meshes or collision bodies as native outputs. Original NeRF paper [@srcNERF-2020] 3D Gaussian Splatting (3DGS) represents a scene as a set of optimizable, rapidly projectable anisotropic Gaussians and achieves real-time novel view synthesis. Those Gaussians have no topological relations of a triangle mesh. Original 3DGS paper [@src3DGS-2023-N005] SuGaR requires an additional surface alignment regularizer and Poisson reconstruction. That requirement shows precisely that going from high-quality Gaussian rendering to an editable mesh remains a separate line of work. SuGaR [@srcN006]

### Three commonly misused notions of "realism"

Visual realism answers whether a rendered image looks like a real photograph. PSNR, SSIM, LPIPS, FID/FVD and human preference usually measure different facets of this layer.

Geometric realism answers whether surface positions, depth, normals, camera and topology are close to the reference. The usual metrics are absolute/relative depth error, camera rotation/translation error, Chamfer distance, F-score, normal consistency, completeness and accuracy.

Physical realism answers whether objects can evolve according to auditable contact, friction, mass, constraints and dynamics. Stable frame rates and a lack of visual interpenetration are no substitute for contact error, penetration depth, tunneling rate, energy drift, reachability rate and task success rate.

A method may be strong at the first layer and average at the second, yet produce no output at the third. This survey will not normalize the metrics of the three layers and combine them into a "composite realism" score. Such a total would hide genuine engineering gaps.

---

[← Back to contents](index.md)
