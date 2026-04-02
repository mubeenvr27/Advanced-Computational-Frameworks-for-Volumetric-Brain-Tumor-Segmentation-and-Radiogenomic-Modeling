# Advanced Radiogenomic Framework for Volumetric Brain Tumor Segmentation

This repository implements a high-performance computational pipeline for the precise semantic segmentation of heterogeneous brain gliomas and the non-invasive prediction of **MGMT promoter methylation status**. Using the **Swin UNETR** architecture and the **BraTS 2021** dataset, this project bridges the gap between volumetric computer vision and molecular biology.

## 🧠 Clinical Context
* **Target:** Glioblastoma multiforme, the most aggressive primary brain tumor in adults with a median survival of ~15 months.
* **Objective:** Automate the labor-intensive manual segmentation process to reduce subjective inconsistencies and support surgical planning.
* **Modality:** Multi-parametric MRI (mpMRI) including T1, T1Gd, T2, and T2-FLAIR sequences.

## 🏗️ Technical Architecture: Swin UNETR
The core of this pipeline is the **Swin UNETR (Swin UNET TRansformers)**, a hybrid 3D U-shaped network.
* **Encoder:** Employs a hierarchical 3D Swin Transformer to capture long-range global spatial dependencies with linear computational complexity.
* **Decoder:** A CNN-based decoder connected via multi-resolution skip connections to preserve fine-grained spatial details.
* **Memory Efficiency:** Utilizes **gradient checkpointing** to enable training on commodity GPUs with 16 GB VRAM.

## 🛠️ Implementation Pipeline

### 1. Preprocessing & Augmentation
Data is processed using the **MONAI** framework:
* **Standardization:** Scans are resampled to $1\text{mm}^3$ isotropic resolution and oriented to RAS coordinates.
* **Normalization:** Z-score intensity scaling is applied only to non-zero brain regions.
* **Cropping:** $128 \times 128 \times 128$ sub-volumes are extracted to optimize gradient processing.
* **Augmentation:** Includes random 3D flips, rotations, and intensity shifts to prevent overfitting.

### 2. Loss Functions & Optimization
To combat extreme class imbalance where healthy tissue dominates the volume, we use a hybrid approach:
* **Loss:** Weighted combination of **Dice Loss** and **Cross-Entropy/Focal Loss**.
* **Optimizer:** AdamW with an initial learning rate of $1\times10^{-4}$.

### 3. Radiogenomic Analysis (MGMT Prediction)
Beyond segmentation, the pipeline extracts actionable molecular insights:
* **Feature Extraction:** Over 1,000 mathematical descriptors (first-order statistics, shape, and texture) are extracted using **PyRadiomics**.
* **Classification:** A machine learning layer predicts MGMT promoter methylation, a critical factor for chemotherapy response.

## 🔬 "Wow Factor" Features (Explainability & Trust)
* **Explainable AI (XAI):** Integrated **Seg-Grad-CAM** via the `M3d-CAM` library to visualize which voxel regions influenced the model's decisions.
* **Predictive Uncertainty:** Uses **Test-Time Augmentation (TTA)** to calculate Shannon entropy and Volume Variation Coefficients (VVC), identifying regions of low model confidence.

## 📊 Evaluation Metrics
| Metric | Purpose |
| :--- | :--- |
| **Dice Similarity Coefficient (DSC)** | Measures overall volumetric overlap. |
| **95th Percentile Hausdorff Distance ($HD_{95}$)** | Measures boundary/surface alignment accuracy. |
| **Area Under the Curve (AUC)** | Evaluates MGMT status classification success. |

---
*This project utilizes the RSNA-ASNR-MICCAI BraTS 2021 Task 1 dataset*
