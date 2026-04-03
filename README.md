# CNN-GRU Intrusion Detection System — Reproducibility Study

Reproducibility assignment for **Artificial Neural Networks (AI-3003)**  
FAST-NUCES, Department of Artificial Intelligence & Data Science  
Instructor: Dr. Qurat ul Ain | Due: April 5, 2026

---

## Paper Being Reproduced

**"Towards Secure IoT-Enabled Transportation: An Explainable AI and Deep Learning-Based Approach for Efficient Threat Detection"**  
Khan, A., Li, Y., Shoukat, S., Javeed, D., & Adil, M. (2025). *Cluster Computing*, 28, 699.  
https://doi.org/10.1007/s10586-025-05473-z

---

## Team

| Name | Student ID |
|---|---|
| Haider Abbas | 23i-2558 |
| Hamd Ul Haq | 23i-0081 |
| Ayesha Ikram | 23i-0109 |

---

## Overview

This repository contains a full reproduction of the CNN-GRU based Intrusion Detection System (XDL-IDS) proposed by Khan et al. (2025). The model combines a 1D Convolutional Neural Network with Gated Recurrent Units for threat detection in IoT-enabled vehicles, augmented with SHAP-based Explainable AI.

The reproduction was conducted on the **CICIoV2024** dataset (Canadian Institute for Cybersecurity) covering 6 traffic classes: BENIGN, DoS, Spoofing-RPM, Spoofing-SPEED, Spoofing-STEERING-WHEEL, and Spoofing-GAS.

---

## Results Summary

### Binary Classification

| Metric | Paper | Reproduced |
|---|---|---|
| Accuracy | 100% | 100% |
| Precision | 100% | 1.00 |
| Recall | 99.99% | 1.00 |
| F1-Score | 100% | 1.00 |
| ROC AUC | 1.0000 | 1.0000 |
| False Negatives | 5 | 3 |

### Multiclass Classification

| Metric | Paper | Reproduced |
|---|---|---|
| Accuracy | 99.64% | 99.64% |
| Macro Precision | 97.23% | ~97% |
| Macro Recall | 98.48% | ~98% |
| Macro F1-Score | 97.69% | ~98% |

---

## Model Architecture

```
Input (8, 1)
    → Conv1D (32 filters, kernel=2, ReLU)
    → GRU (32 units, return_sequences=True)
    → GRU (16 units)
    → Dropout (rate=0.2)
    → Dense (30 neurons, ReLU)
    → Output: Sigmoid (binary) / Softmax (multiclass)

Total parameters: 9,373
```

---

## Repository Structure

```
├── ANN_A2.ipynb              # Main Colab notebook (all experiments)
├── experiment_logs.json      # Training history for binary and multiclass
├── README.md                 # This file
└── report/
    └── final_report.pdf      # Full reproducibility report (13 pages)
```

---

## Dataset

**CICIoV2024** — Canadian Institute for Cybersecurity  
Download: https://www.unb.ca/cic/datasets/iov-dataset-2024.html

Use the **Decimal** format. Place all 6 CSV files in a folder called `CICIoV2024/` in your Google Drive before running the notebook.

| Class | Instances |
|---|---|
| BENIGN | 1,223,737 |
| DoS | 74,663 |
| Spoofing-RPM | 54,900 |
| Spoofing-SPEED | 24,951 |
| Spoofing-STEERING-WHEEL | 19,977 |
| Spoofing-GAS | 9,991 |

---

## How to Run

1. Open `ANN_A2.ipynb` in Google Colab
2. Set runtime to **GPU (T4)**
3. Mount Google Drive and place CICIoV2024 CSV files at `MyDrive/CICIoV2024/`
4. Run all cells top to bottom (**Runtime → Run all**)

### Dependencies

All pre-installed on Colab except SHAP:

```python
pip install shap
```

Other libraries used: `tensorflow`, `numpy`, `pandas`, `scikit-learn`, `matplotlib`

---

## Experimental Setup

| Aspect | Paper | This Reproduction |
|---|---|---|
| Hardware | Core i7 CPU, 16GB RAM | Google Colab T4 GPU |
| Framework | TensorFlow / Keras | TensorFlow 2.18 / Keras 3 |
| Train/Test Split | 70% / 30% | 70% / 30% (stratified) |
| Batch Size | Not specified | 256 |
| Max Epochs | 20 | 20 |
| Early Stopping | Not specified | patience=5 on val_loss |
| SHAP Method | GradientExplainer | KernelExplainer (Keras 3 compatibility) |

---

## Key Reproducibility Notes

- The paper does not provide a code repository — model was implemented from scratch based on Section 3 of the paper
- Column names in the dataset differ from the paper (`label` not `Label`, three label columns present)
- SHAP GradientExplainer is incompatible with Keras 3 GRU — KernelExplainer was used instead with equivalent results
- Random seed set to 42 (not specified in paper)
- Dataset format (decimal/hex/binary) not specified in paper — decimal format was used

---

## References

1. Khan et al. (2025). Towards secure IoT-enabled transportation. *Cluster Computing*, 28, 699.
2. Neto et al. (2024). CICIoV2024. *Internet of Things*, 26, 101209.
3. Lundberg, S. (2017). A unified approach to interpreting model predictions. arXiv:1705.07874.
