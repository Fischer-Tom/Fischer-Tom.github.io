---
layout: page
title: Emergence of a Shared Canonical Object Frame from In-the-Wild Videos
short_title: Canonical Object Frame Emergence
img: assets/img/publication_preview/upose.png
importance: 1
category: research
related_publications: fischer2025unified
---

# Emergence of a Shared Canonical Object Frame from In-the-Wild Videos

**Tom Fischer**, **Martin Sundermeyer**, **Adam Kortylewski**, **Eddy Ilg**

University of Technology Nuremberg · Google · CISPA Helmholtz Center for Information Security

[Paper](https://arxiv.org/abs/XXXX.XXXXX){: .btn .btn-primary }
[Code](https://github.com/PLACEHOLDER/PLACEHOLDER){: .btn }
[Checkpoint](https://github.com/PLACEHOLDER/PLACEHOLDER/releases){: .btn }
[Project Assets](https://github.com/PLACEHOLDER/PLACEHOLDER){: .btn }

---

## Overview

A shared canonical frame allows object poses to be compared consistently across different instances and categories. Existing approaches usually require canonical pose supervision from aligned CAD models, synthetic rendering pipelines, or manually annotated real data. This creates a scaling bottleneck.

We show that a shared canonical object frame can instead **emerge from self-supervised training on object-centric videos captured in the wild**. Our method uses only noisy Structure-from-Motion camera poses and routes all training sequences through a shared geometric bottleneck: a coarse canonical mesh that carries no category-specific detail.

At test time, the model takes a single RGB image and predicts dense correspondences to this shared mesh. A continuous 6D pose is then recovered via PnP, yielding a canonical pose in a frame shared across categories.

---

## Teaser

![Teaser figure showing supervised, category-specific self-supervised, and our category-agnostic self-supervised setting.](assets/img/canonic/Teaser.pdf)

{% include figure.liquid path="assets/img/canonic/Teaser.png" title="We propose a self-supervised pose learning strategy that generalizes without any canonical pose supervision." class="img-fluid rounded z-depth-1" width="100%" %}

**Figure:** Three approaches to learning canonical object frames. Supervised methods rely on canonical pose labels or aligned CAD models. Category-specific self-supervised methods avoid canonical labels but train separate models per category. Our method learns a shared canonical frame across categories from in-the-wild videos without canonical pose labels or category conditioning.

---

## Key Idea

Our method learns dense pixel-to-mesh correspondences through a shared canonical mesh.

![Method overview.](/assets/projects/shared-canonical-frame/method_overview.png)

The training pipeline has three main components:

1. **Canonical Mesh**  
   A coarse mesh, such as a cube, defines a fixed canonical coordinate frame. The mesh is deliberately category-agnostic and contains no instance-specific geometry.

2. **Correspondence Prediction**  
   A neural network predicts dense correspondences from image pixels to vertices on the canonical mesh.

3. **Per-Sequence Alignment**  
   Each SfM reconstruction has an arbitrary coordinate frame. During training, we estimate a per-sequence rotation that aligns the SfM frame to the canonical mesh frame, allowing pseudo-labels to supervise the correspondence network.

The network and the alignments improve together during training. Over time, semantically corresponding object parts are encouraged to map to consistent regions of the shared mesh, causing a canonical frame to emerge without explicit canonical pose labels.

---

## Qualitative Results

{% include figure.liquid path="assets/img/canonic/QualitativeMain.png" title="Predicted 6D poses are visualized as canonical axes overlaid on images from diverse categories and benchmarks. A single fixed rotation is applied uniformly to all predictions for visualization, without per-category or per-dataset adjustment." class="img-fluid rounded z-depth-1" width="100%" %}

---

## Benchmark Results

We evaluate category-level rotation accuracy across five benchmarks using median geodesic rotation error and Acc@30°. Our method is trained once on real object-centric videos and evaluated without dataset-specific fine-tuning.

<div class="table-wrapper" markdown="1">

| Model | Can. | Train | REAL275 Med ↓ | REAL275 Acc30 ↑ | Omni6DPose Med ↓ | Omni6DPose Acc30 ↑ | Objectron Med ↓ | Objectron Acc30 ↑ | Pascal3D+ Med ↓ | Pascal3D+ Acc30 ↑ | ImageNet3D Med ↓ | ImageNet3D Acc30 ↑ | Avg. Med ↓ | Avg. Acc30 ↑ |
|---|:---:|:---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| QWEN3-VL | ✓ | R.+S. | 38.7 | 37.6 | 70.6 | 18.8 | 49.0 | 31.1 | 61.1 | 27.0 | 66.5 | 24.0 | 57.2 | 27.7 |
| OriAny.V1 | ✓ | S. | 28.4 | 52.2 | 62.8 | <u>36.7</u> | 18.4 | 60.4 | 18.1 | 71.0 | 29.7 | 50.3 | 31.5 | 54.1 |
| OriAny.V1† | ✓ | R.+S. | 26.7 | 54.1 | 54.5 | 31.5 | **15.1** | <u>67.7</u> | <span style="color:lightgray">**15.7**</span> | <span style="color:lightgray"><u>78.4</u></span> | <span style="color:lightgray"><u>25.7</u></span> | <span style="color:lightgray">**55.6**</span> | 27.5 | 57.5 |
| OriAny.V2† | ✓ | R.+S. | **21.3** | <u>57.0</u> | **47.7** | **39.2** | 19.8 | 66.2 | <span style="color:lightgray"><u>17.5</u></span> | <span style="color:lightgray">76.5</span> | <span style="color:lightgray">28.1</span> | <span style="color:lightgray">54.1</span> | <u>26.9</u> | <u>58.6</u> |
| **Ours** | ✗ | R. | <u>21.8</u> | **70.0** | <u>49.2</u> | 35.2 | <u>15.3</u> | **69.2** | **15.7** | **79.8** | **25.5** | <u>55.0</u> | **25.5** | **61.8** |

</div>

† Methods marked with a dagger were trained on ImageNet3D, which overlaps with Pascal3D+. Gray metrics therefore indicate settings where those methods have an additional advantage.

---

## Main Findings

- A shared canonical frame can emerge from object-centric videos without canonical pose labels.
- A shared geometric bottleneck encourages cross-sequence and cross-category consistency.
- Category-agnostic training scales better than training separate self-supervised models per category.
- The learned frame is strongest for objects with distinctive semantic axes.
- Symmetric objects remain challenging because multiple orientations can explain the same visual evidence.

---

## Ablations

![Ablation results.](/assets/projects/shared-canonical-frame/ablations.png)

We study the impact of mesh shape, PCA initialization, learned per-sequence alignment, and the number of training views per sequence. The full model uses a cube mesh, PCA initialization, learned alignment, and four training views per sequence.

The ablations show that PCA and learned alignment are complementary. PCA provides a useful geometric initialization, while learned alignment resolves semantic axes such as front/back orientation. Multi-view coverage is also critical: reducing the number of views weakens alignment, and single-view training leads to substantial degradation.

---

## Analysis

![Symmetry and frame consistency analysis.](/assets/projects/shared-canonical-frame/symmetry_frame_consistency.png)

The learned frame is largely consistent across non-symmetric categories. Remaining errors concentrate on categories with rotational symmetry or ambiguous structure, where orientation is not uniquely identifiable from appearance and multi-view consistency alone.

---

## Citation

TODO