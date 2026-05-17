# Deep Learning-Based Approach for Cognitive Decline Screening Through Clock Drawing Test Image Analysis

## Project Overview

This project explores the use of deep learning for classifying Clock Drawing Test (CDT) images into cognitive screening categories. The study uses transfer learning with ResNet18 to compare different fine-tuning strategies for three-class image classification.

The output classes are:

- `Impaired`
- `Mild`
- `Normal`

This project was developed for the COM232 Deep Learning course by **Group Synthetix**.

---

## Group Members

- Aryata, Paul Benedict T.
- Espejo, John Paul M.
- Navarro, Rose Jean D.
- Pajarillaga, Louise V.

---

## Dataset

The dataset was based on the publicly available CDT-API-Network dataset:

```text
https://github.com/cccnlab/CDT-API-Network
```

The original Shulman score labels were regrouped into three classification categories:

| Shulman Score | Final Class |
|---|---|
| 5, 4 | Normal |
| 3 | Mild |
| 2, 1, 0 | Impaired |

This regrouping was used to make the task more suitable for cognitive decline screening rather than exact Shulman score prediction.

---

## Experimental Design

Three ResNet18-based transfer learning experiments were conducted using an ImageNet-pretrained ResNet18 backbone from KerasHub.

| Experiment | Setup | Purpose |
|---|---|---|
| Experiment 1 | Fully frozen ResNet18 backbone | Baseline feature extraction |
| Experiment 2 | Last 30% of ResNet18 backbone unfrozen | Partial fine-tuning |
| Experiment 3 | Fully trainable ResNet18 backbone with oversampling and class weights | Full fine-tuning with imbalance handling |

---

## Model Architecture

Each experiment followed the same general model structure:

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

The final classifier outputs probabilities for the three CDT classes:

```text
Impaired
Mild
Normal
```

---

## Final Results

| Experiment | Accuracy | Macro Precision | Macro Recall | Macro F1-score |
|---|---:|---:|---:|---:|
| Experiment 1 — Frozen Backbone | 0.8226 | 0.7868 | 0.6505 | 0.7003 |
| Experiment 2 — Partial 30% Fine-Tuning | 0.8838 | 0.7951 | 0.7159 | 0.7494 |
| Experiment 3 — Full Fine-Tuning with Imbalance Handling | 0.8807 | 0.7436 | 0.8079 | 0.7640 |

---

## Key Findings

Experiment 2 achieved the highest overall accuracy at **0.8838**, showing that partial fine-tuning improved general classification performance compared with the frozen baseline.

Experiment 3 achieved the highest macro recall and macro F1-score, indicating better class-balanced performance. This is important because the study focuses on cognitive decline screening, where detecting `Impaired` and `Mild` cases is more important than relying only on overall accuracy.

Although Experiment 3 had slightly lower accuracy than Experiment 2, it performed better in screening-sensitive metrics because it improved recognition of minority classes.

---

## Repository Structure

```text
SYNTHETIX-COM232-DeepLearning-CDT/
│
├── README.md
├── requirements.txt
├── .gitignore
│
├── notebooks/
│   ├── SYNTHETIX_COM232_CS_ELEC_Experiment1_FullyFrozen_ResNet18.ipynb
│   ├── SYNTHETIX_COM232_CS_ELEC_Experiment2_Partial30_ResNet18.ipynb
│   └── SYNTHETIX_COM232_CS_ELEC_Experiment3_FullFineTuning_ResNet18.ipynb
│
├── results/
│   ├── exp1_summary_metrics.csv
│   ├── exp1_per_class_metrics.csv
│   ├── exp2_summary_metrics.csv
│   ├── exp2_per_class_metrics.csv
│   ├── exp3_summary_metrics.csv
│   └── exp3_per_class_metrics.csv
│
└── figures/
    ├── exp1_confusion_matrix.png
    ├── exp2_confusion_matrix.png
    └── exp3_confusion_matrix.png
```

---

## Requirements

The notebooks were developed using Google Colab.

Main libraries used:

```text
tensorflow
keras
keras-hub
numpy
pandas
matplotlib
scikit-learn
```

---

## How to Run the Notebooks

1. Open the notebook in Google Colab.
2. Set the runtime to:

```text
Python 3 + GPU
```

3. Mount Google Drive.
4. Make sure the dataset folder follows this structure:

```text
Experiment_data_3class/
├── Impaired/
├── Mild/
└── Normal/
```

5. Update the dataset path in the notebook if needed.
6. Run all cells from top to bottom.

---

## Notes

The trained model files are not included in this repository unless specifically required, since they may be large. The notebooks include model-saving cells that can save the best model checkpoints to Google Drive.

The validation set was kept unchanged during training and evaluation. Oversampling and class weighting in Experiment 3 were applied only during training to avoid artificially inflating validation results.

---

## Conclusion

The experiments show that ResNet18 transfer learning can classify Clock Drawing Test images into `Impaired`, `Mild`, and `Normal` categories.

Partial fine-tuning achieved the highest overall accuracy, while full fine-tuning with imbalance handling achieved the best class-balanced performance.

The results suggest that deep learning may support preliminary CDT image classification. However, the model should be interpreted as a screening-support approach rather than a diagnostic tool.
