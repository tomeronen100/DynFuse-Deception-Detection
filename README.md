# 🎭 DynFuse – Multimodal Deception Detection (Bag-of-Lies)

This repository contains the full experimental pipeline for multimodal deception detection on the **Bag-of-Lies** dataset.  
The project explores **audio**, **video**, and **text** modalities and compares **static** and **dynamic fusion** strategies.  
The final selected model is **Model 3.0 (static multimodal fusion)**.

---

## 📁 Repository Structure
├── Dynfuse-bag-of-lies.ipynb # Main notebook (end-to-end pipeline)

├── data/

│ ├── bag_of_lies/ # Extracted dataset

│ ├── features/ # Cached audio & video features

│ └── transcriptions.csv # Whisper ASR outputs

├── graphs/ # Evaluation plots and analysis figures

└── README.md

---

## 1. Dataset Preparation
- Dataset: **Bag-of-Lies**
- Each sample includes:
  - A video file (`.mp4`)
  - A binary label: *Truthful / Deceptive*
- Labels are loaded from `Annotations.csv`
- The dataset is extracted from a password-protected ZIP and stored locally

---

## 2. Feature Extraction
Feature extraction is performed once and saved to disk to avoid recomputation.

### Video Features
- Frames extracted using **OpenCV**
- Texture-based descriptors:
  - **Local Binary Patterns (LBP)**
- Features aggregated per timestep  
- Output shape:

### Audio Features
- Audio extracted from video tracks
- Processed using **Librosa**
- Spectral features (MFCC-based)
- Output shape: (N_samples, T_timesteps, D_audio)


---

## 3. Preprocessing
- Temporal alignment of audio and video features
- Padding / truncation to fixed sequence length
- Feature normalization
- Stratified **5-fold cross-validation**
- Early stopping during training

---

## 4. Audio-to-Text (ASR)
To incorporate linguistic information:

### Whisper Transcription
- Model: **OpenAI Whisper**
- Each video is transcribed into text
- Transcriptions saved to: data/transcriptions.csv

### Text Embeddings
- Transcriptions encoded using **BERT**
- Token-level embeddings
- Output shape: (N_samples, T_tokens, 768)

---

## 5. Models
We evaluate multiple model families with increasing multimodal complexity.

### Model Group 1.0 – Raw Audio & Video
- Audio-only
- Video-only
- Static fusion (feature concatenation)
- Dynamic fusion (DynFuse – learned modality weighting)

### Model Group 2.0 – Text & Video
- Text-only (BERT)
- Static text–video fusion
- Dynamic text–video fusion

### Model Group 3.0 – Final Model (Selected)
- Static multimodal fusion architecture inspired by prior literature
- Combines text and video representations
- Evaluated with:
- 5-fold cross-validation
- Paired statistical testing
- Held-out test set

---

## 6. Final Model Selection
**Model 3.0 (Static Multimodal Fusion)** is chosen as the final model.

### Reasons:
- Stable and consistent performance across folds
- Competitive accuracy (~72%)
- Strong F1-score and AUC
- Architecture aligned with published work
- No statistically significant disadvantage compared to dynamic fusion
- Better interpretability and reproducibility

---

## 7. Graphs and Visual Analysis
The `graphs/` folder contains:
- Cross-validation performance plots
- Model comparison charts
- Fusion behavior analysis
- ROC curves and metric visualizations

These figures support and illustrate the quantitative results.

---

## 8. Reproducibility
- Fixed random seeds
- Cached feature extraction
- Identical training and evaluation protocols
- Clear separation between training, validation, and test data

---

## Summary
This project demonstrates:
- The importance of **linguistic features** in deception detection
- The effectiveness of **multimodal fusion**
- That **dynamic fusion does not always outperform static fusion**
- A strong and reproducible final model via **Model 3.0**










