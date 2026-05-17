# Code Explanation Guide

## Experiment 1 — Frozen Baseline

The ResNet18 backbone is fully frozen. Only the custom classifier head is trained. This experiment establishes the baseline transfer learning performance.

## Experiment 2 — Partial 30% Fine-Tuning

The last 30% of the backbone is unfrozen. This allows high-level pretrained features to adapt to CDT images while preserving earlier general visual features.

## Experiment 3 — Full Fine-Tuning with Imbalance Handling

All backbone layers are trainable. Oversampling and class weights are applied to improve minority-class detection.

## Key Concepts to Explain

- Transfer learning
- Frozen, partial, and full fine-tuning
- Online augmentation
- Oversampling
- Class weights
- Accuracy vs macro F1-score
- Confusion matrix interpretation
- Why Experiment 3 may have lower accuracy but better screening-sensitive performance

## Final Model Interpretation

Experiment 2 achieved the highest overall accuracy, while Experiment 3 achieved the highest macro recall and macro F1-score. Since the project focuses on cognitive decline screening, Experiment 3 is important because it improved detection of Mild and Impaired cases.
