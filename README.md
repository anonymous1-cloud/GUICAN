# GUICAN: Deep GUI Cross-Modal Alignment Network

![Overview](https://github.com/SnapUI1/GUIAlignFusion/blob/main/overall.jpg)

> **A Novel Approach for Multimodal GUI Retrieval Based on CLIP** > 📝 [Paper](你的论文链接) | 💾 [Dataset](#resources) | 📦 [Pretrained Models](#resources)

## 📖 Introduction
GUICAN addresses the challenges in GUI retrieval (high sensitivity to image quality, fine-grained alignment issues) by adapting large-scale vision-language models to downstream tasks. 

**Core Contributions:**
- **SemAlign-GUI Framework:** A novel two-stage pipeline incorporating Task-Oriented Fine-Tuning and Attention-Guided Gated Fusion.
- **SOTA Performance:** Achieves **69% higher Recall@10** and **37% improvement in MRR** compared to baselines.
- **Large-Scale Dataset:** We introduce **GL3D**, containing 62,530 triplets for composed GUI retrieval.

---

## 🛠️ Method Overview

Our approach consists of two main stages to bridge the gap between general pre-training and GUI tasks.

### Stage 1: Feature Alignment
We freeze CLIP encoders and train the **AGGF module**.
<details>
<summary><b>Click to view Stage 1 Architecture (Figure 2)</b></summary>

![Stage 1](https://github.com/SnapUI1/GUIAlignFusion/blob/main/stage.jpg)
*Figure 2: Task-oriented fine-tuning with progressive unfreezing strategy.*
</details>

### Stage 2: Feature Fusion
We train the **MEDR-Combiner network** from scratch to fuse multimodal features.
<details>
<summary><b>Click to view Stage 2 & Model Architecture (Figure 3 & 4)</b></summary>

| Stage 2 Pipeline | MEDR-Combiner Structure |
| :---: | :---: |
| ![Stage 2](https://github.com/SnapUI1/GUIAlignFusion/blob/main/stage2.jpg) | ![Model](https://github.com/SnapUI1/GUIAlignFusion/blob/main/model.jpg) |

</details>

---

## 💾 Resources (Datasets & Checkpoints)

We release all datasets and pre-trained weights for reproducibility.

| Resource Name | Description | Download Link |
| :--- | :--- | :---: |
| **GL3D Dataset** | 62,530 triplets (Ref, Text, Target) for composed retrieval. | [**[`Google Drive`]**](https://drive.google.com/drive/folders/1SUR1Tzp0BixmFNH4YIjJNgNId8IoarMf?usp=drive_link) |
| **Rico-Topic** | 5,562 curated screenshots across 10 themes. | [**[`Google Drive`]**](https://drive.google.com/file/d/11TPlera7HjaF4O_8s0dd8OPgvfd0AeWh/view?usp=sharing) |
| **Model Weights** | Pre-trained GUIAlignFusion weights. | [**[`Google Drive`]**](https://drive.google.com/file/d/1u43DZe968v9Fs9xMvUbUnn4jQbkAM-uZ/view?usp=drive_link) |

### 🔍 Dataset Construction Details
We constructed GL3D using a hybrid pipeline involving OpenCV and GPT-2. 

<details>
<summary><b>Click to expand detailed construction pipeline (HSV, GPT-2, etc.)</b></summary>

#### 1. GUI Element Extraction
- Used HSV conversion and contour detection via OpenCV.
- Mapped BGR colors to component types (e.g., Green -> TextView).
- Detection includes thresholding, morphological operations, and contour analysis.

#### 2. Natural Language Description (GPT-2)
- Employed a hybrid rule-based + GPT-2 approach.
- GPT-2 refined mechanical stats into natural English (Temp=0.7, Repetition Penalty=1.1).

#### 3. Difference Matching
- Calculated Euclidean distance for component differences.
- Filtered top-3 significant changes based on weight and area.

</details>

---

## 💻 Implementation & Usage

### Prerequisites
- Single Tesla V100-SXM2-32GB GPU (Recommended)
- Python 3.x / PyTorch (Add your versions here)

### Key Parameters
- **Optimizer:** AdamW (lr=3e-5 for fusion, 3e-6 for CLIP)
- **Input Size:** 288×288 pixels
- **Batch Size:** (Add your batch size)

```bash
# Example: How to run training (if you have code)
python train.py --dataset gl3d --epochs 50


