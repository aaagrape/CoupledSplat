<div align="center">

# 🛰️ CoupledSplat

### Modeling Coupled Uncertainties for Generalizable 3D Gaussian Splatting from Sparse-View Satellite Images

![3D Gaussian Splatting](https://img.shields.io/badge/3D-Gaussian_Splatting-2563EB?style=flat-square)
![Satellite Imagery](https://img.shields.io/badge/Satellite-Imagery-0891B2?style=flat-square)
![Feed-Forward Reconstruction](https://img.shields.io/badge/Feed--Forward-Reconstruction-059669?style=flat-square)
![Uncertainty Modeling](https://img.shields.io/badge/Coupled-Uncertainties-7C3AED?style=flat-square)

**Joint 3D reconstruction, elevation recovery, and novel-view synthesis from sparse-view satellite images.**

[Abstract](#-abstract) · [Introduction](#-introduction) · [Method](#-method) · [Acknowledgements](#-acknowledgements)

</div>

---

## 🔍 Abstract

Feed-forward 3D Gaussian splatting enables efficient reconstruction from multi-view satellite imagery, yet reliable 3D recovery remains challenging due to **coupled uncertainties throughout the reconstruction pipeline**. Radiometric variations, temporal changes, and occlusions undermine observation reliability and cross-view matching. Ambiguous correspondences and projection discrepancies can further compromise Gaussian geometry and ultimately affect elevation accuracy.

We propose **CoupledSplat**, a unified uncertainty-aware framework for generalizable 3D reconstruction from sparse-view satellite images. Reliability-aware fusion adaptively weights image pairs and spatial regions to suppress inconsistent observations. Semantic-structural matching combines semantic guidance with coarse-to-fine correspondence refinement to resolve matching ambiguities and preserve local structures. Newton-based RPC geometric refinement reduces projection discrepancies, while multi-scale elevation refinement addresses residual height errors through global height alignment and high-resolution surface constraints. Together, these components mitigate uncertainty accumulation across the reconstruction pipeline and improve geometric accuracy and novel-view rendering quality.

## 🌍 Introduction

Satellite imagery supports large-scale 3D reconstruction and digital surface model generation. Recovering accurate geometry from sparse views is challenging because observations may be acquired at different times, under different illumination conditions, and from different viewing directions.

These challenges affect connected stages of reconstruction. Unreliable observations weaken correspondence estimation. Correspondence errors and projection discrepancies then affect Gaussian positioning, contributing to residual height errors in the reconstructed surface.

**CoupledSplat addresses four related sources of uncertainty within a unified feed-forward framework.**

| Uncertainty | Reconstruction challenge | Our approach |
| :--- | :--- | :--- |
| ☁️ **Observation** | Radiometric variations, temporal changes, and occlusions | Spatially adaptive, reliability-aware observation fusion |
| 🧩 **Representation** | Ambiguous correspondences and insufficient structural cues | Semantic-guided coarse-to-fine matching |
| 📐 **Geometry** | RPC projection discrepancies affecting sampling and positioning | Newton-based inverse-RPC refinement |
| 🏔️ **Elevation** | Residual height errors and local surface distortions | Multi-scale height refinement and complementary training constraints |

## ⚙️ Method

### Framework Overview

Given sparse-view satellite images and their associated rational polynomial camera (RPC) models, CoupledSplat predicts a **3D Gaussian scene representation** for novel-view rendering and elevation recovery.

<div align="center">
  <img src="./Figure_1.png" alt="Overview of the CoupledSplat framework" width="100%">
</div>

*Overview of CoupledSplat. Semantic-structural matching and reliable observation fusion establish multi-view geometric evidence. RPC-constrained refinement and joint height and Gaussian prediction support scene reconstruction and novel-view rendering.*

### 🤝 Reliable Observation Fusion

Different source views provide different levels of support for each reference pixel. CoupledSplat constructs an independent height distribution for each reference–source pair, preserving individual matching evidence before aggregation.

A learned fusion module combines height-distribution evidence, spatial context, and RPC-derived metadata. **Spatial attention** incorporates neighboring evidence, while **pair attention** compares evidence across source views. Pixel-wise reliability weights then determine each pair’s contribution to the fused distribution.

Fusion operates at both coarse and fine matching stages, adapting to local observation quality.

### 🧩 Semantic-Structural Matching

CoupledSplat combines image features with adapted intermediate features from a **frozen Depth Anything 3 (DA3) backbone**. These semantic-structural features provide contextual cues for matching weak textures, repetitive patterns, and regions with cross-view appearance changes.

The matching process proceeds through three connected steps.

1. **Coarse matching** evaluates candidate heights through RPC-guided feature warping and combines image and semantic similarities into pairwise height distributions.
2. **Fine matching** searches around the coarse estimate using learned spatial sampling offsets.
3. **Uncertainty-guided blending** combines coarse-distribution ambiguity with disagreement across source pairs to control the sampling offsets and the contribution of the fine estimate.

### 📐 RPC-Constrained Geometric Refinement

Inverse-RPC initialization can produce ground coordinates whose forward projections differ from the original image pixels. These discrepancies affect cross-view feature sampling and Gaussian positioning.

CoupledSplat applies a **Newton-based correction** to the initialized latitude and longitude at a given height, with the RPC coefficients held fixed. The correction reduces forward-projection residuals and is shared across matching, reprojection constraints, and Gaussian construction.

The corrected projection remains differentiable with respect to height, allowing image-based objectives to guide elevation learning through geometrically consistent sampling locations.

### ✨ Joint Height and Gaussian Prediction

A shared refinement U-Net combines image and semantic-structural features with reference RGB, height-distribution information, and confidence and uncertainty estimates.

These features support **full-resolution height correction** and the prediction of **Gaussian covariance, opacity, and appearance**. Refined heights and corrected ground coordinates determine Gaussian centers, completing the scene representation used for differentiable rendering.

### 🏔️ Multi-Scale Elevation Refinement

CoupledSplat upsamples the locally refined height map and predicts a bounded residual at full image resolution. This refinement recovers local height variations that can be lost during lower-resolution matching.

Complementary training objectives constrain geometry and appearance at different scales.

| Training objective | Role |
| :--- | :--- |
| **Rendered appearance** | Encourages consistency between rendered and observed images |
| **Relative-height supervision** | Preserves coherent relative surface structure |
| **Global height alignment** | Constrains the predicted height level through multi-view feature agreement |
| **High-resolution RGB reprojection** | Strengthens cross-view consistency at the refined surface |
| **Edge-aware residual regularization** | Encourages smooth height corrections while allowing variations near image boundaries |

## 🙏 Acknowledgements

This work builds upon [SkySplat](https://github.com/NanCheng2001/SkySplat-main) and uses intermediate features from Depth Anything 3. We thank the authors for making their research and resources available to the community.
