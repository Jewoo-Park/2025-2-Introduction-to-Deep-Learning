# Cross-Domain Contrastive Alignment for MobileNetV4
### Domain Generalization via Cross-Domain Supervised Contrastive Learning

<p align="center">
  <img src="./assets/architecture.png" width="800">
</p>

---

## Overview

This project studies **Domain Generalization (DG)** by explicitly enforcing **cross-domain feature alignment** during training.

Instead of relying solely on cross-entropy loss, we introduce a **cross-domain supervised contrastive loss** that aligns representations of samples belonging to the **same class across different source domains**.  
The goal is to suppress domain-specific shortcuts and learn **domain-invariant semantic features**.

The method is lightweight and is evaluated using **MobileNetV4** on the **PACS** benchmark.

---

## Key Idea

Given multiple source domains (Art, Cartoon, Sketch) and an unseen target domain (Photo), we aim to learn representations that satisfy:

- Same class → similar features  
- Different domains → no domain-specific separation

To achieve this, we add a **cross-domain supervised contrastive objective** on **intermediate features** of the backbone.

---

## Method

### Architecture

- Backbone: **MobileNetV4**
- Feature used for contrastive alignment: **Intermediate feature**
- Classification head: standard linear classifier
- Optional auxiliary classifier on mid-level features

---

### Loss Function

The total training objective is:

\[
\mathcal{L}_{total} = \mathcal{L}_{CE} + \lambda \mathcal{L}_{DG}
\]

- **Cross-Entropy Loss (\(\mathcal{L}_{CE}\))**  
  Ensures class discrimination on source domains.

- **Domain Generalization Contrastive Loss (\(\mathcal{L}_{DG}\))**  
  A supervised contrastive loss where:
  - Positive pairs: same class, different domain
  - Negative pairs: different class

This loss explicitly enforces **cross-domain class-level alignment**.

---

## Experimental Setup

- Dataset: **PACS**
  - Domains: photo, art_painting, cartoon, sketch
- Evaluation: Leave-One-Domain-Out
- Backbone: MobileNetV4 (Small / Large)
- Optimizer: AdamW
- BN running statistics: frozen

---

## Results

### Baselines

**MobileNetV4 (Small)**  
- photo: 35.75  
- art_painting: 26.86  
- cartoon: 34.22  
- sketch: 36.19  
- **Average: 33.25**

**MobileNetV4 (Large)**  
- photo: 38.50  
- art_painting: 29.74  
- cartoon: 35.71  
- sketch: 39.20  
- **Average: 35.79**

---

### Proposed Method

**Final-layer Feature Alignment**  
- photo: 37.13  
- art_painting: 28.71  
- cartoon: 32.08  
- sketch: 38.66  
- **Average: 34.14**

**Mid-level Feature Alignment**  
- photo: 39.76  
- art_painting: 29.30  
- cartoon: 28.41  
- sketch: 39.98  
- **Average: 34.36**

**Mid-level Feature + Auxiliary Classifier**  
- photo: 41.32  
- art_painting: 28.52  
- cartoon: 31.95  
- sketch: 43.06  
- **Average: 36.21**

---

## Key Observations

- Cross-domain contrastive alignment improves generalization over ERM
- Aligning **intermediate features** is more effective than final-layer features
- Auxiliary supervision further stabilizes learning
- The proposed method outperforms MobileNetV4-Small and is competitive with MobileNetV4-Large

---

## Contributions

- Introduce **cross-domain supervised contrastive loss** for DG
- Highlight the importance of **mid-level feature alignment**
- Demonstrate effectiveness on lightweight CNNs
- Provide a simple and extensible DG framework

---

## References

- **MobileNetV4**  
  Chen et al., *MobileNetV4: Universal Models for the Mobile Ecosystem*, 2024.

- **PACS Dataset**  
  Li et al., *Deeper, Broader and Artier Domain Generalization*, ICCV 2017.

- **Supervised Contrastive Learning**  
  Khosla et al., *Supervised Contrastive Learning*, NeurIPS 2020.
