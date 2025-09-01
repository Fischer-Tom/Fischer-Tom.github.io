---
layout: page
title: Unified Category-Level Object Detection and Pose Estimation using 3D Prototypes from RGB Images
short_title: Unified Object Detection & Pose Estimation
img: assets/img/publication_preview/upose.png
importance: 1
category: research
related_publications: fischer2025unified
---

Tom Fischer, Xiaojie Zhang, Eddy Ilg  
*ICCV 2025*  

[📄 Paper](https://arxiv.org/abs/2508.02157) | [💻 Code](https://github.com/Fischer-Tom/unified-detection-and-pose-estimation)

---

## Abstract

Understanding not only **what objects are present in an image** but also **where they are located and how they are oriented in 3D space** is a central challenge in computer vision. Traditional category-level approaches often rely on depth sensors or treat detection and pose estimation as two separate tasks.  
In this work, we introduce the **first unified single-stage model** that solves both problems jointly from RGB images only. By leveraging **neural mesh models as 3D prototypes**, our method learns a shared representation for categories and establishes dense 2D–3D correspondences. Combined with a robust multi-model fitting algorithm, this allows us to detect multiple objects and estimate their **9D pose (category, size, rotation, translation)** in a single pass.  

## Key Contributions


At the core of our approach lies the concept of **3D prototypes**: representative meshes that encode the geometry of object categories. The model establishes dense 2D–3D correspondences between RGB image features and prototype vertices, enabling both **object detection and 9D pose estimation in a single stage**.  


- **First single-stage RGB-only framework** for category-level multi-object detection and 9D pose estimation.  
- **Prototype-based representation:** 3D neural mesh models serve as category-level anchors, enabling robust correspondences between image features and 3D geometry.  
- **Joint inference with RANSAC PnP:** A multi-model fitting procedure detects multiple instances and refines their poses in one pipeline.  
- **State-of-the-art performance:** Achieves +22.9% improvement over previous best methods on REAL275 across scale-agnostic metrics.  
- **Robustness to noise and corruption:** Outperforms two-stage pipelines under image degradations, highlighting stability for real-world applications:contentReference.  

## Method Overview


Our framework unifies **detection and pose estimation** using a single representation.  
{% include figure.liquid path="assets/img/upose/upose.jpg" title="Overview of our unified framework with 3D prototypes" class="img-fluid rounded z-depth-1" width="100%" %}

- **Feature extraction:** A frozen DINOv2 backbone with lightweight adapters encodes image features, which are then aligned with category-level prototypes.  
- **3D prototypes:** Each object category is represented by a neural mesh model that encodes geometry and learned vertex features.  
- **2D–3D matching:** Dense correspondences between image features and prototype vertices are established, enabling detection and pose inference.  
- **Pose estimation:** Multi-model RANSAC PnP jointly detects multiple instances and estimates their initial 6D pose. A refinement stage further optimizes instance-specific size and improves accuracy:contentReference.  

This design eliminates error propagation between detection and pose stages and ensures a more **generalizable, category-level understanding** (e.g., handling unseen mugs, laptops, or bottles).

## Results

Our model sets a new **state of the art** in RGB-only category-level pose estimation:  

- **REAL275 benchmark:** Improves average accuracy by 22.9% over the previous state of the art, across all scale-agnostic metrics.  
- **CAMERA25 benchmark:** Achieves highest accuracy on 9 out of 11 metrics, showing strong generalization from synthetic to real data.  
- **Robustness:** Maintains stable performance under common corruptions (blur, noise, compression, weather), where two-stage methods degrade significantly.  
- **Qualitative results:** Demonstrate more confident detections and improved rotation accuracy, especially in challenging categories like laptops:contentReference.  

These results confirm that a **single unified model is both more accurate and more robust** than existing two-stage pipelines.

{% include figure.liquid path="assets/img/upose/upose_table.png" title="Overview of our unified framework with 3D prototypes" class="img-fluid rounded z-depth-1" width="100%" %}

## Citation

If you find this work useful in your research, please consider citing:

```bibtex
@article{fischer2025unified,
  title={Unified Category-Level Object Detection and Pose Estimation from RGB Images using 3D Prototypes},
  author={Fischer, Tom and Zhang, Xiaojie and Ilg, Eddy},
  journal={arXiv preprint arXiv:2508.02157},
  abbr={ICCV},
  year={2025},
  website={https://fischer-tom.github.io/projects/project1},
}
```

---

*This work was conducted at the University of Technology Nuremberg under the supervision of Prof. Eddy Ilg and supported by the DFG GRK 2853/1 "Neuroexplicit Models of Language, Vision, and Action".*
