# Methodology Summary

## Project Overview

This project applies transfer learning using ResNet18 to classify Clock Drawing Test (CDT) images into three cognitive screening categories: `Impaired`, `Mild`, and `Normal`.

The experiments compare different fine-tuning strategies to determine which setup provides the best overall and class-balanced performance.

---

## Dataset Preparation

The dataset was based on Clock Drawing Test images originally labeled using the Shulman scoring system.

The original Shulman scores were regrouped into three classes:

| Shulman Score | Final Class |
|---|---|
| 5, 4 | Normal |
| 3 | Mild |
| 2, 1, 0 | Impaired |

This regrouping was used to simplify the classification task and make it more suitable for cognitive screening.

---

## Dataset Distribution

After regrouping the original Shulman scores into three classes, the dataset contained **1,638 CDT images**.

| Class | Shulman Scores | Total Images |
|---|---|---:|
| Normal | 5, 4 | 1,200 |
| Mild | 3 | 352 |
| Impaired | 2, 1, 0 | 86 |
| **Total** | — | **1,638** |

The dataset shows class imbalance, with `Normal` having the highest number of samples and `Impaired` having the fewest. This imbalance was one reason why class-level metrics such as precision, recall, F1-score, macro average, and confusion matrices were used instead of relying on accuracy alone.

---

## Training and Validation Split

The dataset was split into training and validation sets using an 80/20 split with a fixed random seed.

| Class | Training Images | Validation Images | Total Images |
|---|---:|---:|---:|
| Normal | 953 | 247 | 1,200 |
| Mild | 288 | 64 | 352 |
| Impaired | 70 | 16 | 86 |
| **Total** | **1,311** | **327** | **1,638** |

The validation set was kept unchanged during evaluation to preserve the original class distribution and provide a realistic measurement of model performance.

---

## Preprocessing

All images were prepared using the following preprocessing steps:

- Images were resized to `224 × 224` pixels.
- Images were loaded in RGB format to match the ImageNet-pretrained ResNet18 input requirement.
- Pixel values were normalized from `[0, 255]` to `[0.0, 1.0]`.
- The dataset was split into training and validation subsets using a fixed random seed for consistency.

---

## Data Augmentation

Online data augmentation was applied only to the training set.

The purpose of augmentation was to improve generalization by exposing the model to slightly varied versions of the training images.

The validation set was not augmented to preserve a clean and realistic evaluation setup.

Common augmentation techniques included:

- Rotation
- Zoom
- Translation
- Contrast adjustment, where applicable

Since augmentation was applied dynamically, no additional augmented image files were permanently saved.

---

## Model Architecture

All experiments used an ImageNet-pretrained ResNet18 backbone from KerasHub.

The general architecture followed this structure:

```text
Input Image
↓
ResNet18 Backbone
↓
Global Average Pooling
↓
Dense Layer
↓
Dropout
↓
Softmax Classifier
```

The final softmax classifier outputs probabilities for the three classes:

- `Impaired`
- `Mild`
- `Normal`

---

## Experiment 1 — Fully Frozen ResNet18 Baseline

Experiment 1 used the ResNet18 backbone as a fixed feature extractor.

The entire backbone was frozen, meaning its pretrained weights were not updated during training. Only the custom classifier head was trained.

This experiment served as the baseline for comparing the effect of partial and full fine-tuning.

---

## Experiment 2 — Partial 30% Fine-Tuning

Experiment 2 partially unfroze the ResNet18 backbone.

The last 30% of backbone layers were made trainable, while the earlier layers remained frozen.

This setup allowed higher-level visual features to adapt to the CDT dataset while preserving general pretrained features learned from ImageNet.

---

## Experiment 3 — Full Fine-Tuning with Imbalance Handling

Experiment 3 made the entire ResNet18 backbone trainable.

This allowed all pretrained feature layers to adapt to the CDT dataset.

To address class imbalance, Experiment 3 also used:

- **Oversampling** to increase minority-class exposure during training.
- **Class weights** to increase the penalty for misclassifying minority classes.

Oversampling and class weighting were applied only during training. The validation set remained unchanged to avoid artificially inflated evaluation results.

### Experiment 3 Training Balance

For Experiment 3, oversampling was applied only to the training set. Minority classes were repeated with replacement until each class matched the majority training class.

| Class | Original Training Images | After Oversampling |
|---|---:|---:|
| Normal | 953 | 953 |
| Mild | 288 | 953 |
| Impaired | 70 | 953 |
| **Total** | **1,311** | **2,859** |

The validation set was not oversampled or augmented. This prevents artificially inflated validation results.

---

## Evaluation Metrics

The models were evaluated using both overall and class-level metrics.

The following metrics were used:

- Accuracy
- Precision
- Recall
- F1-score
- Macro average
- Micro average
- Weighted average
- Confusion matrix

Accuracy was not used alone because the dataset was imbalanced. Macro F1-score and per-class recall were emphasized to evaluate whether the model performed well across all classes, especially `Impaired` and `Mild`.

---

## Final Experiment Comparison

| Experiment | Setup | Accuracy | Macro Precision | Macro Recall | Macro F1-score |
|---|---|---:|---:|---:|---:|
| Experiment 1 | Fully frozen ResNet18 backbone | 0.8226 | 0.7868 | 0.6505 | 0.7003 |
| Experiment 2 | Partial 30% fine-tuning | 0.8838 | 0.7951 | 0.7159 | 0.7494 |
| Experiment 3 | Full fine-tuning with imbalance handling | 0.8807 | 0.7436 | 0.8079 | 0.7640 |

---

## Summary

Experiment 2 achieved the highest overall accuracy, showing that partial fine-tuning improved general classification performance.

Experiment 3 achieved the highest macro recall and macro F1-score, showing better class-balanced performance and improved sensitivity to minority classes.

Since the study focuses on cognitive decline screening, Experiment 3 is important because it improved detection of `Impaired` and `Mild` cases, even though its overall accuracy was slightly lower than Experiment 2.

Overall, the methodology demonstrates how different ResNet18 fine-tuning strategies affect CDT image classification performance. The final comparison highlights the trade-off between overall accuracy and screening-sensitive class-balanced metrics.
