---
theme: seriph
title: Comparing Classical Machine Learning and Deep Temporal Models for Human Activity Recognition from Wearable Time Series
author: Ogün Yilmaz
transition: fade
---

# Temporal Models for HAR

## CNN, CNN–HMM and CNN–LSTM

**ANN2 Project**

Ogün Yilmaz

---

## Problem & Motivation

**Human Activity Recognition (HAR)**  
Classifying short segments of wearable sensor data  
(e.g. wrist-worn accelerometers) into activity classes.

Free-living HAR is challenging due to:

- strong inter-individual variability
- sensor noise
- severe class imbalance
- frequent short activity transitions

**Key issue:**  
Window-based models ignore temporal structure.

---

## Dataset: CAPTURE-24

<br>

- **151** adult participants
- **~24h** free-living recordings per participant
- wrist accelerometer at **100 Hz**
- camera-verified activity annotations

We follow the **official CAPTURE-24 benchmark pipeline**:

- preprocessing & windowing
- participant-level train/test split
- benchmark metrics

(Chan et al., 2024)

---

## Experimental Setup

<br>

- **Non-overlapping 10s windows** (1000 samples)
- **Four-class activity taxonomy**:
  - Sleep
  - Sedentary
  - Light
  - Moderate–Vigorous
- **Participant-aware split**:
  - P001–P100: training / validation
  - P101–P151: held-out test

No cross-participant leakage.

---

## CAPTURE-24 Processing Pipeline

<img src="/pipeline.png" style="width:85%; margin:auto;" />

_Window-based classification with optional temporal smoothing on held-out participants._

---

## Models Compared

<br>

### Classical baselines

- Random Forest
- XGBoost  
  (with and without HMM smoothing)

<br>

### Deep learning models

- **CNN** (window-based)
- **CNN + HMM** (post-hoc temporal smoothing)
- **CNN–LSTM** (end-to-end temporal modelling)

All CNNs share the same ResNet-style backbone.

---

## Temporal Modelling Strategies

<br>

### CNN + HMM

- CNN outputs = emission probabilities
- HMM models activity transitions
- Viterbi decoding enforces temporal consistency

<br>

### CNN–LSTM

- CNN extracts window-level features
- LSTM models sequences of windows
- End-to-end temporal learning

---

## CNN–LSTM Architecture

<img src="/tuned_cnn_lstm_architecture.png" style="width:40%; margin:auto;" />

**Shared CNN extracts window-level features; LSTM models temporal dependencies across windows.**

---

## Evaluation Metrics

To handle class imbalance:

- **Macro F1-score** (primary, all models)
- Balanced accuracy _(classical models only)_
- Cohen’s κ
- Matthews Correlation Coefficient (MCC)
- Confusion matrices (class-wise analysis)

All metrics reported on **held-out participants**.

---

## Results: Classical Models

<img src="/classical_models.png" style="width:70%; margin:auto;" />

_Temporal smoothing via HMM substantially improves feature-based models._

---

## Results: Deep Models

<img src="/deep_models.png" style="width:65%; margin:auto;" />

**CNN + HMM achieves the best overall performance across all metrics.**

---

## Class-wise Prediction Patterns

<img src="/conf_matrices.png" style="width:95%; margin:auto;" />

_Temporal modelling reduces confusion between sedentary, light and moderate-to-vigorous activities._

---

## Discussion

<br>

- Treating windows independently is insufficient for free-living HAR
- Explicit transition modelling (HMM) aligns well with daily activity patterns
- End-to-end temporal models offer flexibility but require careful tuning
- Temporal modelling mitigates:
  - participant variability
  - class imbalance
  - noisy boundaries between activities

---

## Conclusion

<br>

- Temporal modelling substantially improves HAR performance on CAPTURE-24
- **CNN + HMM** provides the strongest and most stable results
- **CNN–LSTM** is a viable end-to-end alternative
- Results highlight the importance of modelling activity transitions explicitly

---

## References

<br>

- Chan et al. (2024). _CAPTURE-24_. Scientific Data.
- OxWearables (2024). CAPTURE-24 Benchmark Repository.
- Hossain et al. (2025). Benchmarking HAR models.

---

# Thank You

Questions?
