# XDL-IDS: CNN-Attention-GRU Intrusion Detection System

## Overview
This project extends the original **XDL-IDS (Explainable Deep Learning-Based Intrusion Detection System)** for in-vehicle networks by introducing:

- A **CNN-Attention-GRU hybrid architecture**
- **Dataset expansion** using the Car Hacking Dataset
- Improved **training efficiency and generalization**

The system detects malicious activity on the **Controller Area Network (CAN) bus** using deep learning.

---

## Key Contributions

### 1. Novel Architecture: CNN-Attention-GRU
We enhanced the baseline CNN-GRU model by inserting a **Multi-Head Self-Attention layer** between convolutional and recurrent layers.

**Pipeline:**
Input → CNN → Multi-Head Attention → GRU → Dense → Output

**Advantages:**
- Focuses on important temporal features
- Faster convergence
- Improved representation learning
- Better prediction confidence

---

### 2. Dataset Expansion
We combined:
- **CICIoV2024 Dataset**
- **Car Hacking Dataset**

### Preprocessing Steps:
- Removed irrelevant fields (Timestamp, DLC)
- Converted payload bytes from hex → decimal
- Unified feature schema:
  ID, DATA_0–DATA_7, label, category, class
- Mapped attack labels across datasets:
- Replay → Spoofing
- DoS → DoS
- RPM → Spoofing/RPM
- Gear → Spoofing/GEAR

**Outcome:**
- Increased dataset diversity
- Reduced overfitting
- Improved real-world robustness

---

## Model Configurations

### 🔹 Baseline (CNN-GRU)
- Conv1D layers for feature extraction
- GRU for temporal modeling
- Batch size: 256
- Learning rate: 1e-3
- Early stopping patience: 3

### 🔹 Proposed Model (CNN-Attention-GRU)
- Multi-head self-attention layer added
- Batch size: 512
- Learning rate: 5e-4
- Early stopping patience: 2
- Mixed precision training (float16)
- XLA JIT compilation enabled

---

## Results

### Original Dataset (CICIoV2024)
| Metric | CNN-GRU | CNN-Attention-GRU |
|--------|--------|------------------|
| Binary Accuracy | ~99.9% | **100%** |
| Multiclass Accuracy | ~99.6% | ~99.6% |
| Convergence Speed | Slower | **Faster** |

---

### Merged Dataset (CICIoV2024 + Car Hacking)
| Metric | Value |
|--------|------|
| Binary Accuracy | ~93–94% |
| Multiclass Accuracy | ~75% |

**Insight:**
- Slight accuracy drop indicates **better generalization**
- Multiclass classification becomes harder due to dataset diversity

---

## Evaluation Metrics
- Accuracy
- Precision, Recall, F1-score
- ROC-AUC
- Confusion Matrix
- Loss Curves
- SHAP Explainability

---

## Explainability (SHAP)
- Most important features: **DATA_0, DATA_1**
- Confirms meaningful feature learning
- Attention model produces clearer and more confident predictions

---

## Performance Optimizations
- Mixed Precision Training (float16)
- XLA JIT Compilation
- Larger batch sizes
- Faster convergence

---

## Experimental Setup
- Platform: Google Colab (T4 GPU)
- Framework: TensorFlow / Keras
- Python: 3.10+
- Train/Test Split: 70/30 (stratified)
- Preprocessing: StandardScaler, LabelEncoder, NaN/Inf removal

---

## Limitations
- Multiclass accuracy drops (~75%) on merged dataset
- Cross-dataset variability increases classification difficulty
- Limited explainability for multiclass predictions
- No statistical significance testing (e.g., McNemar’s test)

---

## Future Work
- Improve multiclass classification performance
- Apply domain adaptation techniques
- Increase model capacity for complex datasets
- Extend SHAP analysis to multiclass setting

---

## References
1. XDL-IDS Paper (IEEE TITS, 2023)  
2. CICIoV2024 Dataset (Canadian Institute for Cybersecurity)  
3. Car Hacking Dataset (2018)  
4. Attention Is All You Need (NeurIPS 2017)  
5. SHAP: A Unified Approach to Model Interpretability  

---

## Authors
- Haider Abbas (23i-2558)
- Hamdul Haq (23i-0081)
- Ayesha Ikram (23i-0109)

---

## Course Info
**Artificial Neural Networks (AI-3003)**  
FAST-NUCES  
Instructor: Dr. Qurat Ul Ain
- Removed irrelevant fields (Timestamp, DLC)
- Converted payload bytes from hex → decimal
- Unified feature schema:
