# Cross-Modal Misleading Video Detection

Official implementation of the hierarchical multimodal framework for
misleading short-video detection.

The framework integrates visual, audio, and textual information through an
ambiguity-aware cross-modal fusion mechanism. It first distinguishes Safe
from Misleading videos and then classifies misleading videos into four
fine-grained categories.

> **Paper:** An Explainable Hierarchical Multimodal Framework for Misleading Short-Video Detection  
> **Journal:** Information Fusion  
> **Code:** This repository contains the experimental implementations used in the study.

---

## Overview

The proposed framework follows a two-stage hierarchical classification
strategy:

- **Stage 1:** Safe vs. Misleading
- **Stage 2:** Fine-grained classification of misleading videos into:
  - Identity Fabrication (IF)
  - Perception Manipulation (PM)
  - Scientifically Unrealistic Scene (SUS)
  - Surreal Content (SC)

Three information sources are jointly modelled:

- **Visual:** uniformly sampled video frames
- **Audio:** speech and acoustic information
- **Text:** ASR transcripts, OCR text, captions, and descriptions where available

---

## Framework

The main pipeline consists of:

1. Multimodal preprocessing
2. Unimodal feature extraction
3. Temporal attention over video frames
4. Projection into a common feature space
5. Ambiguity-aware cross-modal fusion
6. Hierarchical classification
7. Explainability analysis

The final fusion mechanism combines a cross-attention pathway with
ambiguity-aware modality weighting based on pairwise cross-modal
disagreement.

---

## Feature Encoders

| Modality | Encoder | Representation |
|---|---|---|
| Visual | CLIP ViT-B/32 | 512-D per frame |
| Audio | wav2vec 2.0 | 768-D |
| Text | XLM-RoBERTa | 768-D |
| Text | Qwen3-Embedding-0.6B | 1024-D |
| Combined Text | XLM-R + Qwen3 | 1792-D |

For each video, 16 frames are uniformly sampled and aggregated using temporal
attention before multimodal fusion.

---

## Fusion Strategies

Nine multimodal fusion strategies are evaluated:

1. Concatenation
2. Gated Fusion
3. Attention-Based Fusion
4. Cross-Attention
5. Combined CA-GF-AMW
6. QuaFusion
7. CCIN-SA
8. Ambiguity-Aware Fusion
9. AtCAF

The ambiguity-aware mechanism is adopted in the final framework because of
its performance on the fine-grained classification task and the independent
real-world evaluation.

---

## Datasets

### MisVid

MisVid is the curated dataset used for hierarchical evaluation.

It contains five categories:

- Safe
- Identity Fabrication (IF)
- Perception Manipulation (PM)
- Scientifically Unrealistic Scene (SUS)
- Surreal Content (SC)

The dataset contains **2,400 short-videos**, including 1,200 Safe videos and
1,200 Misleading videos distributed equally across the four misleading
subcategories.

### FakeTT

FakeTT is used as an independent real-world benchmark to evaluate
generalisation beyond the curated MisVid dataset.

Because FakeTT provides binary labels but no fine-grained labels matching
the MisVid taxonomy, it is used for Stage 1 evaluation only.

> Dataset availability and access information should follow the terms and
> licences of the corresponding dataset sources.

---

## Explainability

The framework includes several complementary explainability methods:

- **Grad-CAM** for gradient-based visual localisation
- **Score-CAM** for score-based visual localisation
- **LIME** for word-level textual importance
- **Modality ablation** for estimating visual, audio, and text contributions
- **Qwen2.5-VL-7B-Instruct** for evidence-grounded natural-language synthesis

The LLM is used only to summarise the evidence produced by the framework. It
does not make the classification decision.

---

## Repository Structure

```text
CrossModal-Misleading-Video-Detection/
│
├── Hierarchical_models/
│   └── Hierarchical model experiments
│
├── FakeTT_misleading-video-experiments.ipynb
│   └── Independent evaluation on FakeTT
│
├── crossmodal-misleading-fusion-v1.ipynb
├── crossmodal-misleading-fusion-v2.ipynb
│   └── Cross-modal fusion experiments
│
├── hierarchical-multimodal-*.ipynb
│   └── Hierarchical classification experiments
│
├── misleading-fusion-combined.ipynb
├── misleading-fusion-strategies.ipynb
│   └── Fusion strategy comparison
│
├── misleading-grad-score-cam.ipynb
│   └── Grad-CAM and Score-CAM analysis
│
├── misleading-llm-xai.ipynb
│   └── LLM-based explanation synthesis
│
├── vision-audio-ablation.ipynb
│   └── Modality ablation experiments
│
├── vision-audio-text-llm.ipynb
│   └── Trimodal experiments
│
├── paper-comparison.ipynb
│   └── Baseline comparison experiments
│
└── README.md
