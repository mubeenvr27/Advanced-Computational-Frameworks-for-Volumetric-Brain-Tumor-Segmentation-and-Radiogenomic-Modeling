# Advanced-Computational-Frameworks-for-Volumetric-Brain-Tumor-Segmentation-and-Radiogenomic-Modeling

If you're putting this on GitHub, stop treating it like a school report. A recruiter or a researcher wants to see the **architecture**, the **logic**, and the **reproducibility**. Your README needs to scream technical competence.

Here is a professional-grade Markdown template based strictly on your provided methodology.

---

# Advanced Radiogenomic Framework for Volumetric Brain Tumor Segmentation

[cite_start]This repository implements a high-performance computational pipeline for the precise semantic segmentation of heterogeneous brain gliomas and the non-invasive prediction of **MGMT promoter methylation status**[cite: 4, 115]. [cite_start]Using the **Swin UNETR** architecture and the **BraTS 2021** dataset, this project bridges the gap between volumetric computer vision and molecular biology[cite: 36, 124].

## 🧠 Clinical Context
* [cite_start]**Target:** Glioblastoma multiforme, the most aggressive primary brain tumor in adults with a median survival of ~15 months[cite: 6].
* [cite_start]**Objective:** Automate the labor-intensive manual segmentation process to reduce subjective inconsistencies and support surgical planning[cite: 7, 8].
* [cite_start]**Modality:** Multi-parametric MRI (mpMRI) including T1, T1Gd, T2, and T2-FLAIR sequences[cite: 10, 142].

## 🏗️ Technical Architecture: Swin UNETR
[cite_start]The core of this pipeline is the **Swin UNETR (Swin UNET TRansformers)**, a hybrid 3D U-shaped network[cite: 36].
* [cite_start]**Encoder:** Employs a hierarchical 3D Swin Transformer to capture long-range global spatial dependencies with linear computational complexity[cite: 37, 39, 157].
* [cite_start]**Decoder:** A CNN-based decoder connected via multi-resolution skip connections to preserve fine-grained spatial details[cite: 37, 160].
* [cite_start]**Memory Efficiency:** Utilizes **gradient checkpointing** to enable training on commodity GPUs with 16 GB VRAM[cite: 44, 162].

## 🛠️ Implementation Pipeline

### 1. Preprocessing & Augmentation
[cite_start]Data is processed using the **MONAI** framework[cite: 147]:
* [cite_start]**Standardization:** Scans are resampled to $1\text{mm}^3$ isotropic resolution and oriented to RAS coordinates[cite: 20, 150].
* [cite_start]**Normalization:** Z-score intensity scaling is applied only to non-zero brain regions[cite: 52, 152].
* [cite_start]**Cropping:** $128 \times 128 \times 128$ sub-volumes are extracted to optimize gradient processing[cite: 51, 153].
* [cite_start]**Augmentation:** Includes random 3D flips, rotations, and intensity shifts to prevent overfitting[cite: 54, 154].

### 2. Loss Functions & Optimization
[cite_start]To combat extreme class imbalance where healthy tissue dominates the volume, we use a hybrid approach[cite: 63, 64]:
* [cite_start]**Loss:** Weighted combination of **Dice Loss** and **Cross-Entropy/Focal Loss**[cite: 66, 166].
* [cite_start]**Optimizer:** AdamW with an initial learning rate of $1\times10^{-4}$[cite: 169].

### 3. Radiogenomic Analysis (MGMT Prediction)
[cite_start]Beyond segmentation, the pipeline extracts actionable molecular insights[cite: 111]:
* [cite_start]**Feature Extraction:** Over 1,000 mathematical descriptors (first-order statistics, shape, and texture) are extracted using **PyRadiomics**[cite: 118, 119].
* [cite_start]**Classification:** A machine learning layer predicts MGMT promoter methylation, a critical factor for chemotherapy response[cite: 113, 121].

## 🔬 "Wow Factor" Features (Explainability & Trust)
* [cite_start]**Explainable AI (XAI):** Integrated **Seg-Grad-CAM** via the `M3d-CAM` library to visualize which voxel regions influenced the model's decisions[cite: 105, 172, 174].
* [cite_start]**Predictive Uncertainty:** Uses **Test-Time Augmentation (TTA)** to calculate Shannon entropy and Volume Variation Coefficients (VVC), identifying regions of low model confidence[cite: 85, 89, 131, 187].

## 📊 Evaluation Metrics
| Metric | Purpose |
| :--- | :--- |
| **Dice Similarity Coefficient (DSC)** | [cite_start]Measures overall volumetric overlap[cite: 183]. |
| **95th Percentile Hausdorff Distance ($HD_{95}$)** | [cite_start]Measures boundary/surface alignment accuracy[cite: 185]. |
| **Area Under the Curve (AUC)** | [cite_start]Evaluates MGMT status classification success[cite: 188]. |

---
[cite_start]*This project utilizes the RSNA-ASNR-MICCAI BraTS 2021 Task 1 dataset[cite: 140].*
