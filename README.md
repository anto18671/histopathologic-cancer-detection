# 🩺 Histopathologic Cancer Detection — TIF-Focused Notebook

---

## 📌 Project Overview

This project implements a deep learning pipeline for detecting **metastatic cancer** in small histopathology image patches.
It focuses on handling `.tif` images efficiently, training a binary classifier, and generating predictions for Kaggle’s **Histopathologic Cancer Detection** competition.

* **Task:** Binary classification — predict if the center 32×32 px region of a 96×96 px patch contains tumor tissue.
* **Input:** `.tif` pathology image patches
* **Output:** Probability of tumor presence
* **Metric:** ROC AUC

---

## 📁 Dataset

* All images are provided as `.tif` files.
* Labels are in `train_labels.csv` with columns:

  * `id`: image ID, mapping to `train/{id}.tif`
  * `label`: `1` for tumor, `0` for normal

Example:

| id                               | label |
| -------------------------------- | ----- |
| 0005f7aaab2800f6170c399693a96917 | 0     |
| 000620e7e8e9ff9e6b4b27a84ebf2f8c | 1     |

---

## 🔄 Pipeline Steps

### 1. TIF Utilities

* Custom loaders handle 8/16-bit `.tif` images and normalize them to float32 `[0,1]`.
* Automatically ensures RGB output.

### 2. Exploratory Data Analysis (EDA)

* Loads and checks label distribution.
* Validates that all `.tif` files exist.
* Visualizes sample positive and negative patches.

### 3. Dataset & Augmentations

* Uses **Albumentations** for strong data augmentation.
* Custom `Dataset` class reads `.tif` files and converts them to tensors.

### 4. Model Architecture

* **EfficientNet-B0** (from `timm`) is used as the backbone.
* Outputs a single logit for binary classification.

### 5. Training & Validation

* Stratified K-Fold cross-validation.
* Binary cross-entropy loss and ROC AUC metric.
* Saves the best checkpoint per fold.

### 6. Inference

* Test-time augmentation (TTA) improves performance.
* Combines predictions from multiple folds.
* Generates a final `submission.csv` for Kaggle.

---

## 📊 Results

| Experiment | Model           | Augmentations | Epochs | OOF AUC |
| ---------- | --------------- | ------------- | ------ | ------- |
| Baseline   | EfficientNet-B0 | Standard      | 3      | ~0.95   |
| +TTA       | EfficientNet-B0 | Extended      | 3      | ~0.96   |

---

## 🔬 Future Improvements

* Larger backbones or higher resolution inputs
* Stain-aware normalization or augmentation
* Class weighting or focal loss
* Semi-supervised learning with pseudo-labeling
* Model ensembling

---

## 📤 Deliverables

* ✅ `submission.csv` — ready for Kaggle submission
* ✅ Full `.ipynb` training & inference notebook
* ✅ Example visualizations and metrics

---

## 📎 Acknowledgements

* [Kaggle: Histopathologic Cancer Detection](https://www.kaggle.com/competitions/histopathologic-cancer-detection)
* [timm](https://github.com/huggingface/pytorch-image-models)
* [Albumentations](https://albumentations.ai/)
