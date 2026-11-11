<div align="center">

# 🛰️ CoupledSplat

### Modeling Coupled Uncertainties for Generalizable 3D Gaussian Splatting from Sparse-View Satellite Images

![3D Gaussian Splatting](https://img.shields.io/badge/3D-Gaussian_Splatting-2563EB?style=flat-square)
![Satellite Imagery](https://img.shields.io/badge/Satellite-Imagery-0891B2?style=flat-square)
![Feed-Forward Reconstruction](https://img.shields.io/badge/Feed--Forward-Reconstruction-059669?style=flat-square)
![Uncertainty Modeling](https://img.shields.io/badge/Coupled-Uncertainties-7C3AED?style=flat-square)

**Joint 3D reconstruction, elevation recovery, and novel-view synthesis from sparse-view satellite images.**

</div>

---

## 🔍 Abstract

Feed-forward 3D Gaussian splatting enables efficient satellite reconstruction, yet observation inconsistencies, ambiguous correspondences, and projection discrepancies can propagate through the reconstruction pipeline and degrade elevation accuracy. We introduce **CoupledSplat**, a unified uncertainty-aware framework combining reliability-aware observation fusion, semantic-structural matching, Newton-based RPC geometric refinement, and multi-scale elevation refinement. Together, these components address coupled uncertainties across reconstruction stages to improve geometric accuracy and novel-view rendering quality.

## 🌍 Introduction

Sparse-view satellite reconstruction faces four connected sources of uncertainty—**observation, representation, geometry, and elevation**. Radiometric variations, temporal changes, and occlusions weaken matching evidence, while correspondence ambiguities and projection errors affect Gaussian positioning and the recovered surface. CoupledSplat addresses these interactions throughout the reconstruction pipeline.

<div align="center">
  <img src="./Figure_2.png" alt="Observation, representation, geometric, and elevation uncertainties in satellite reconstruction" width="100%">
  <br>
  <em>Coupled uncertainties in satellite 3D reconstruction and their effects on surface recovery.</em>
</div>

## ⚙️ Method

<div align="center">
  <img src="./Figure_1.png" alt="Overview of the CoupledSplat framework" width="100%">
  <br>
  <em>Overview of CoupledSplat.</em>
</div>

CoupledSplat combines **semantic-guided coarse-to-fine matching** with **spatially adaptive observation fusion** to establish reliable multi-view evidence. **Newton-based inverse-RPC refinement** improves projection consistency, while a shared network jointly refines heights and predicts Gaussian attributes. **Multi-scale elevation constraints** support global height alignment and local surface fidelity.

## 🙏 Acknowledgements

This work builds upon [SkySplat](https://github.com/NanCheng2001/SkySplat-main) and uses intermediate features from Depth Anything 3. We thank the authors for sharing their research and resources.
