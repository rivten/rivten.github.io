---
layout: post
title:  "Compilation of 100+ 3D graphics academic papers"
date:   2018-08-03
tags: ['tldr', 'graphics', 'article', 'academic', 'paper']
author: "Hugo Viala"
---

About a little more than a year ago, I followed an intensive Computer Graphics course that studied a lot of academic materials and engaged us into reading more scientific papers. Throughout the course, we studied in-class and by ourselves several articles spanning lots of sub-topics of 3D graphics.

I have compiled that list here. Note that this list is by no means exhaustive, might not span the full knowledge (or even possible the basic knowledge) of a specific subtopic and might be lacking very important reference.

However, I would be willing to expand and improve it. If you think a particular academic article or subtopic is lacking, or that a specific reference is useless, or if you want to suggest any improvement whatsoever, do not hesitate [to shoot me a DM](https://twitter.com/hugoviala) :)

## Algebraic Topology

Title | Authors | Link
---|---|----
Structure and Stability of the 1-Dimensional Mapper | Carrière and Oudot | [Link](https://arxiv.org/pdf/1511.05823v3)

## Ambient Occlusion

Title | Authors | Link
---|---|----
Approximating Dynamic Global Illumination in Image Space | Ritschel et al. | [Link](https://people.mpi-inf.mpg.de/~ritschel/Papers/SSDO.pdf)
Dynamic Ambient Occlusion and Indirect Lighting | Bunnell | [Link](http://http.developer.nvidia.com/GPUGems2/gpugems2_chapter14.html)
Hardware Accelerated Ambient Occlusion Techniques on GPUs | Shanmugan and Arikan | [Link](http://www.okanarikan.com/assets/Papers/SSAO/paper.pdf)
Neural Network Ambient Occlusion | Holden et al. | [Link](http://www.ipab.inf.ed.ac.uk/cgvu/nnao.pdf)
Production-Ready Global Illumination | Landis | [Link](http://www.spherevfx.com/downloads/ProductionReadyGI.pdf)
Bent Normals and Cones in Screen-space | Klehm et al. | [Link](https://www.mpi-inf.mpg.de/~ritschel/Papers/ScreenSpaceBentCones.pdf)
Volumetric Ambient Occlusion | Szirmay-Kalos et al. | [Link](http://sirkan.iit.bme.hu/~szirmay/ambient8.pdf)

## Computational Geometry

Title | Authors | Link
---|---|----
Edgebreaker : Connectivity compression for triangle meshes | Rossignac | [Link](http://www.cc.gatech.edu/~jarek/papers/EdgeBreaker.pdf)
Adaptative Forward Differencing for Rendering Curves and Surfaces | Lien et al. | [Link](http://www.eecs.umich.edu/courses/eecs598-1/lien87.pdf)
Variational Shape Approximation | Cohen-Steiner et al. | [Link](http://www.geometry.caltech.edu/pubs/CAD04.pdf)
DeWall : A fast divide and conquer Delaunay triangulation algorithm in E^d | Cignoni et al. | [Link](http://vcg.isti.cnr.it/publications/papers/dewall.pdf)
Discovering Structural Regularity in 3D Geometry | Pauly et al. | [Link](http://lgg.epfl.ch/publications/2008/pauly_2008_DSR.pdf)
Fast Exact and Approximate Geodesics on Meshes | Surazhsky et al. | [Link](http://hhoppe.com/geodesics.pdf)
Geodesics in Heat : A New Approach to Computing Distance based on Heat Flow | Crane et al. | [Link](http://ddg.math.uni-goettingen.de/pub/GeodesicsInHeat.pdf)
Lloyd relaxation using analytical Voronoi diagram in the L_infinity norm and its application to quad optimization | Mouton and Béchet | [Link](http://www.imr.sandia.gov/papers/imr21/RNMouton.pdf)

## Data Structure

Title | Authors | Link
---|---|----
Efficient Data Structure for Representing and Simplifying Simplicial Complexes in High Dimension | Attali et al. | [Link](https://hal.archives-ouvertes.fr/hal-00579902/document)
QSplat : A Multiresolution Point Rendering System for Large Meshes | Rusinkeiwicz and Leroy | [Link](http://graphics.stanford.edu/papers/qsplat/qsplat_paper.pdf)
High Resolution Sparse Voxel DAGs | Kämpe et al. | [Link](http://www.cse.chalmers.se/~kampe/highResolutionSparseVoxelDAGs.pdf)

## Global Illumination

Title | Authors | Link
---|---|----
The Rendering Equation | Kajiya | [Link](http://dl.acm.org/citation.cfm?id=15902)
Mathematical Models and Monte Carlo Algorithms for Physically Based Rendering | Lafortune | [Link](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.38.3626&rep=rep1&type=pdf)
Bi-Directional Path Tracing | Lafortune and Willems | [Link](http://graphics.cs.kuleuven.be/publications/BDPT/BDPT_paper.pdf)
Metropolis Light Transport | Veach and Guibas | [Link](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.88.944&rep=rep1&type=pdf)
Instant Radiosity | Keller | [Link](http://citeseerx.ist.psu.edu/viewdoc/download;jsessionid=21FD348B4727B27F6796FB2A301F17FF?doi=10.1.1.15.240&rep=rep1&type=pdf)
Lightcuts : A Scalable Approach to Illumination | Walter et al. | [Link](http://www.graphics.cornell.edu/~bjw/lightcuts.pdf)
Deep Screen Space | Nalbach et al. | [Link](http://people.mpi-inf.mpg.de/~onalbach/DeepScreenSpace/DeepScreenSpace.pdf)
Interactive Indirect Illumination Using Voxel Cone Tracing | Crassin et al. | [Link](https://research.nvidia.com/sites/default/files/publications/GIVoxels-pg2011-authors.pdf)
Gradient-Domain Path Tracing | Kettunen et al. | [Link](https://mediatech.aalto.fi/publications/graphics/GPT/kettunen2015siggraph_paper.pdf)
Manifold Exploration : A Markov Chain Monte Carlo technique for rendering scenes with difficult specular transport | Jakob | [Link](http://www.cs.cornell.edu/projects/manifolds-sg12/manifolds-sg12.pdf)
Precomputed Radiance Transfer for Real-Time Rendering in Dynamic, Low-Frequency Lighting Environments | Sloan et al. | [Link](http://www.cs.jhu.edu/~misha/ReadingSeminar/Papers/Sloan02.pdf)
Robust Monte Carlo Methods for Light Transport Simulation | Veach | [Link](http://graphics.stanford.edu/papers/veach_thesis/thesis.pdf)
Real-Time Indirect Illumination with Clustered Visibility | Dong et al. | [Link](https://people.mpi-inf.mpg.de/~ritschel/Papers/ClusteredVisibility.pdf)
Real-Time Polygonal-Light Shading with Linearly Transformed Consines | Heitz et al. | [Link](https://drive.google.com/file/d/0BzvWIdpUpRx_d09ndGVjNVJzZjA/view)
Scalable Realistic Rendering with Many-Light Methods | Dachsbacher et al. | [Link](http://cgg.mff.cuni.cz/~jaroslav/papers/2013-mlstar/eg2013star_manylights.pdf)
Splatting Indirect Illumination | Dachsbacher et al. | [Link](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.104.2043&rep=rep1&type=pdf)
The State of the Art in Interactive Global Illumination | Ritschel et al. | [Link](https://people.mpi-inf.mpg.de/~ritschel/Papers/GISTAR.pdf)
A Practical Guide to Global Illumination using Photon Maps | Jensen |[Link](https://graphics.stanford.edu/courses/cs348b-00/course8.pdf)
Photon Mapping on Programmable Graphics Hardware | Purcell et al. | [Link](https://www.cs.umd.edu/class/spring2005/cmsc828v/papers/photongfx.pdf)
Bidirectional Instant Radiosity | Segovia et al. | [Link](http://artis.imag.fr/Projets/Cyber-II/Publications/SIMP06a.pdf)
Fast, Arbitrary BRDF Shading for Low-Frequency Lighting Using Spherical Harmonics | Kautz et al. | [Link](http://www0.cs.ucl.ac.uk/staff/J.Kautz/publications/shbrdfRW02.pdf)
Light Field Rendering | Levoy and Hanrahan | [Link](https://graphics.stanford.edu/papers/light/light-lores-corrected.pdf)

## Point Based Global Illumination

Title | Authors | Link
---|---|----
Point-Based Approximate Color Bleeding | Christensen | [Link](http://graphics.pixar.com/library/PointBasedColorBleeding/paper.pdf)
ManyLoDs: Parallel Many-View Level-of-Detail Selection for Real-Time Global Illumination | Holländer et al. | [Link](http://perso.telecom-paristech.fr/~boubek/papers/ManyLoDs/ManyLoDs.pdf)
Factorized Point Based Global Illumination | Wang et al. | [Link](http://perso.telecom-paristech.fr/~boubek/papers/FPBGI/FPBGI_lowres.pdf)
Forward Light Cuts : A Scalable Approach to Real-Time Global Illumination | Laurent et al. | [Link](http://perso.telecom-paristech.fr/~boubek/papers/FLC/FLC.pdf)
Micro-Rendering for Scalable, Parallel Final Gathering | Ritschel et al. | [Link](http://jankautz.com/publications/microrenderingSIGAsia09.pdf)
Quantized Point-Based Global Illumination | Buchholz and Boubekeur | [Link](http://perso.telecom-paristech.fr/~boubek/papers/QPBGI/QPBGI.pdf)
Wavelet Point-Based Global Illumination | Wang et al. | [Link](http://perso.telecom-paristech.fr/~boubek/papers/WPBGI/WPBGI_lowres.pdf)

## Shadow

Title | Authors | Link
---|---|----
Deep Shadow Maps | Lokovic and Veach | [Link](https://graphics.stanford.edu/papers/deepshadows/deepshad.pdf)
Imperfect Shadow Maps for Efficient Computation of Indirect Illumination | Ritschel et al. | [Link](http://resources.mpi-inf.mpg.de/ImperfectShadowMaps/ISM.pdf)
Real-Time, All-Frequency Shadows in Dynamic Scenes | Annen et al. | [Link](http://discovery.ucl.ac.uk/10005/1/10005.pdf)
Percentage-Closer Soft Shadows | Fernando | [Link](http://developer.download.nvidia.com/shaderlibrary/docs/shadow_PCSS.pdf)
Stochastic Soft Shadow Mapping | Liktor et al. | [Link](http://cg.ivd.kit.edu/publications/2015/sssm/StochasticSoftShadows.pdf)
Variance Shadow Mapping | Myers | [Link](http://developer.download.nvidia.com/SDK/10.5/direct3d/Source/VarianceShadowMapping/Doc/VarianceShadowMapping.pdf)

## Rendering

Title | Authors | Link
---|---|----
Clustered Deferred and Forward Shading | Olsson et al. | [Link](http://www.cse.chalmers.se/~uffe/clustered_shading_preprint.pdf)
Comprehensible Rendering of 3-D Shapes | Saito and Takahashi | [Link](https://www.cs.princeton.edu/courses/archive/fall00/cs597b/papers/saito90.pdf)
An Overview of BRDF Models | Montes and Urena | [Link](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.923.6560&rep=rep1&type=pdf)
X-Toon: An Extended Toon Shader | Barla et al. | [Link](http://www.cs.ucr.edu/~vbz/cs230papers/x-toon.pdf)

## Physics Simulation

Title | Authors | Link
---|---|----
Practical Animation of Liquids | Foster and Fedkiw | [Link](http://physbam.stanford.edu/~fedkiw/papers/stanford2001-02.pdf)
Large Steps in Cloth Simulation | Baraff and Witkin | [Link](https://www.cs.cmu.edu/~baraff/papers/sig98.pdf)
Light Scattering from Human Hair Fibers | Marschner et al. | [Link](http://www.graphics.stanford.edu/papers/hair/hair-sg03final.pdf)
Stable Fluids | Stam | [Link](http://www.dgp.toronto.edu/people/stam/reality/Research/pdf/ns.pdf)

## Remeshing

Title | Authors | Link
---|---|----
Anisotropic Polygonal Remeshing | Alliez et al. | [Link](http://geometry.caltech.edu/pubs/ACDLD03.pdf)
Isotropic Surface Remeshing | Alliez et al. | [Link](https://hal.inria.fr/inria-00071991/document)

## Spectral Methods

Title | Authors | Link
---|---|----
Laplacian Eigenmaps for Dimensionality Reduction and Data Representation | Belkin and Niyogi | [Link](http://yeolab.weebly.com/uploads/2/5/5/0/25509700/belkin_laplacian_2003.pdf)
General discrete Laplace operators on polygonal meshes | Herholz | [Link](https://www.informatik.hu-berlin.de/de/forschung/gebiete/viscom/thesis/final/Diplomarbeit_Herholz_201301.pdf)
Robust 3D Shape Correspondence in the Spectral Domain | Jain and Zhang | [Link](https://www.cs.sfu.ca/~haoz/pubs/jain_zhang_smi06.pdf)
Discrete Laplace operators : No free lunch | Wardetzky et al. | [Link](http://www.cs.columbia.edu/cg/pdfs/1180993110-laplacian.pdf)
Poisson Image Editing | Pérez et al. | [Link](http://www.cs.jhu.edu/~misha/Fall07/Papers/Perez03.pdf)
Laplace-Beltrami Eigenfunctions for Deformation Invariant Shape Representation | Rustanov | [Link](http://www.cs.jhu.edu/~misha/Fall07/Papers/Rustamov07.pdf)
Spectral Geometry Processing with Manifold Harmonics | Vallet and Lévy | [Link](http://www.cs.jhu.edu/~misha/ReadingSeminar/Papers/Vallet08.pdf)
Discrete Laplacians on General Polygonal Meshes | Alexa and Wardetzky | [Link](http://ddg.math.uni-goettingen.de/pub/Polygonal_Laplace.pdf)

## Surface Deformation

Title | Authors | Link
---|---|----
As-Rigid-As-Possible Surface Modeling | Sorkine and Alexa | [Link](https://www.igl.ethz.ch/projects/ARAP/arap_web.pdf)
Bone Glow: An Improved Method for the Assignment of Weights for Mesh Deformation | Wareham and Lasenby | [Link](https://www.cse.msu.edu/~cse872/papers_files/BoneGlowImprovedWeightAssignment.pdf)
Convolutional Wasserstein Distances: Efficient Optimal Transportation on Geometric Domains | Solomon et al. | [Link](http://graphics.pixar.com/library/OptimalTransportation/paper.pdf)
Variational Harmonic Maps for Space Deformation | Ben-Chen et al. | [Link](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.445.1058&rep=rep1&type=pdf)
Mean Value Coordinates for Closed Triangular Meshes | Ju et al. | [Link](http://www.cs.wustl.edu/~taoju/research/meanvalue.pdf)
Green Coordinates | Lipman et al. | [Link](http://www.math.tau.ac.il/~lipmanya/GC/gc_techrep.pdf)
Harmonic Coordinates for Character Articulation | Joshi et al. | [Link](http://graphics.pixar.com/library/HarmonicCoordinatesB/paper.pdf)
Harmonic Coordinates | DeRose and Meyer | [Link](http://graphics.pixar.com/library/HarmonicCoordinates/paper.pdf)
Local Barycentric Coordinates | Zhang et al. | [Link](http://lgg.epfl.ch/publications/2014/LBC.php)
Automatic Rigging and Animation of 3D Characters | Baran and Popovic | [Link](http://people.csail.mit.edu/ibaran/papers/2007-SIGGRAPH-Pinocchio.pdf)
Bounded Biharmonic Weights for Real-Time Deformation | Jacobson et al. | [Link](https://igl.ethz.ch/projects/bbw/bounded-biharmonic-weights-siggraph-2011-compressed-jacobson-et-al.pdf)
Conformal Mesh Deformations with Möbius Transformations | Vaxman et al. | [Link](http://web.engr.oregonstate.edu/~mjb/cs550/Projects/Papers/CConformalMeshDeformations.pdf)
Direct Skinning Methods and Deformation Primitives | Kavan | [Link](http://skinning.org/direct-methods.pdf)
Skinning with Dual Quaternions | Kavan et al. | [Link](https://www.cs.utah.edu/~ladislav/kavan07skinning/kavan07skinning.pdf)
Laplacian Surface Editing | Sorkine et al. | [Link](https://igl.ethz.ch/projects/Laplacian-mesh-processing/Laplacian-mesh-editing/laplacian-mesh-editing.pdf)
Spin Transformation of Discrete Surfaces | Crane et al. | [Link](https://www.cs.cmu.edu/~kmcrane/Projects/SpinTransformations/paper.pdf)
GPU-assisted Positive Mean Value Coordinates for Mesh Deformations | Lipman et al. | [Link](http://www.math.tau.ac.il/~lipmanya/pmvc/PMVC.pdf)
Spatial Deformation Transfer | Ben-Chen et al. | [Link](http://www.cs.technion.ac.il/~gotsman/AmendedPubl/Miri/SpatialDeformationTransfer.pdf)
Deformation Transfer for Triangle Meshes | Sumner and Popovic | [Link](http://people.csail.mit.edu/sumner/research/deftransfer/Sumner2004DTF.pdf)
Mesh Editing with Poisson-Based Gradient Field Manipulation | Yu et al. | [Link](http://www.cs.jhu.edu/~misha/Fall07/Papers/Yu04.pdf)

## Surface Parametrization

Title | Authors | Link
---|---|----
Surface Parametrization: a Tutorial and Survey | Floater and Hormann | [Link](http://vcg.isti.cnr.it/Publications/2005/FH05/survey_mingle04.pdf)
Least Squares Conformal Maps for Automatic Texture Atlas Generation | Lévy et al. | [Link](https://members.loria.fr/Bruno.Levy/papers/LSCM_SIGGRAPH_2002.pdf)
Mesh Parametrization: Theory and Practice | Hormann et al. | [Link](http://alice.loria.fr/publications/papers/2007/SigCourseParam/param-course.pdf)
Discrete Surface Ricci Flow | Jin et al. | [Link](https://cvc.cs.stonybrook.edu/Publications/2008/JKLG08/25RicciFlow.pdf)

## Surface Reconstruction

Title | Authors | Link
---|---|----
Marching Cubes: A High Resolution 3D Surface Construction Algorithm | Lorensen and Cline | [Link](http://academy.cba.mit.edu/classes/scanning_printing/MarchingCubes.pdf)
Poisson Surface Reconstruction | Kazhdan et al. | [Link](http://hhoppe.com/poissonrecon.pdf)
Voronoi-based Variational Reconstruction of Unoriented Point Sets | Alliez et al. | [Link](ftp://ftp-sop.inria.fr/prisme/dcohen/Papers/ACTD07.pdf)
Surface Reconstruction by Voronoi Filtering | Amenta and Bern | [Link](http://www.cs.jhu.edu/~misha/Fall13b/Papers/Amenta99.pdf)
Linear Surface Reconstruction from Discrete Fundamental Forms on Triangle Meshes | Wang et al. | [Link](https://www.cse.msu.edu/~ytong/DiscreteFundamentalForms.pdf)
Reconstruction Using Witness Complexes | Guibas and Oudot | [Link](http://geometrica.saclay.inria.fr/team/Steve.Oudot/papers/go-ruwc-07/go-ruwc-07.pdf)
Geodesic Delaunay Triangulation and Witness Complex in the Plane | Gao et al. | [Link](http://geometrica.saclay.inria.fr/team/Steve.Oudot/papers/ggow-gtwcp-08/ggow-gtwcp-08.pdf)
Delaunay Triangulation Based Surface Reconstruction: Ideas and Algorithms | Cazals and Giesen | [Link](http://www.cs.jhu.edu/~misha/Fall13b/Papers/Cazals06.pdf)

## Surface Segmentation

Title | Authors | Link
---|---|----
D-Charts: Quasi-Developable Mesh Segmentation | Julius et al. | [Link](https://ai2-s2-pdfs.s3.amazonaws.com/d4af/f9cccb898e94d531fbcf67c08d828e77b65d.pdf)

## Surface Simplification

Title | Authors | Link
---|---|----
Surface Simplification Using Quadric Error Metrics | Garland and Heckbert | [Link](http://cseweb.ucsd.edu/~ravir/190/2016/garland97.pdf)
Structure-Aware Mesh Decimation | Salinas et al. | [Link](https://hal.inria.fr/hal-01111203/document)

## Texture Packing

Title | Authors | Link
---|---|----
A Thousand Ways to Pack the Bin - A Practical Approach to Two-Dimensional Rectangle Bin Packing | Jylänki | [Link](http://clb.demon.fi/files/RectangleBinPack.pdf)

## Unsorted

Title | Authors | Link
---|---|----
Computing Discrete Minimal Surfaces and Their Conjugates | Pinkall and Polthier | [Link](http://page.mi.fu-berlin.de/polthier/articles/diri/diri_jem.pdf)
Efficient Spherical Harmonic Evaluation | Sloan | [Link](http://jcgt.org/published/0002/02/06/paper.pdf)
Stupid Spherical Harmonics Tricks | Sloan | [Link](http://www.ppsloan.org/publications/StupidSH36.pdf)
Filtering Distributions of Normals for Shading Antialiasing | Kaplayan et al. | [Link](https://research.nvidia.com/sites/default/files/publications/NDFFiltering.pdf)
Joint Bilateral Upsampling | Kopf et al. | [Link](https://johanneskopf.de/publications/jbu/paper/FinalPaper_0185.pdf)
Symmetry and Orbit Detection via Lie-Algebra Voting | Shi et al. | [Link](http://www.geometry.caltech.edu/pubs/SADBH16.pdf)
Procedural Noise using Sparse Gabor Convolution | Lagae et al. | [Link](http://www-sop.inria.fr/reves/Basilic/2009/LLDD09/LLDD09PNSGC_paper.pdf)
Improved Alpha-Tested Magnification for Vector Textures and Special Effects | Green | [Link](http://www.valvesoftware.com/publications/2007/SIGGRAPH2007_AlphaTestedMagnification.pdf)
Wavelet Noise | Cook and DeRose | [Link](http://graphics.pixar.com/library/WaveletNoise/paper.pdf)
