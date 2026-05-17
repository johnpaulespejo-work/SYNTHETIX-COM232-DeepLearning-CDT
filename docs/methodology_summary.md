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
