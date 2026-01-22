---
theme: seriph
title: Comparing Classical Machine Learning and Deep Temporal Models for Human Activity Recognition from Wearable Time Series
author: Ogün Yilmaz
transition: fade
background: linear-gradient(135deg, #f8f9fa, #ffffff)
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

- Validation subset sampled from P001–P100 (**seed = 42**) for CNN+HMM and tuned CNN–LSTM

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

**Shared CNN extracts window features; LSTM uses consecutive windows to make predictions more consistent.**

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

## Research Questions — Summary

<br>

- **RQ1:** How do feature-based models compare with deep learning models for HAR in free-living data?  
  → Models that incorporate temporal structure outperform window-based classifiers across both classical and deep learning approaches. Deep models provide competitive performance and enable end-to-end temporal modelling.

- **RQ2:** Does the use of temporal information improve performance and prediction consistency?  
  → Yes. Both CNN–HMM and CNN–LSTM consistently outperform window-based CNNs.

- **RQ3:** How do HMM-based smoothing and end-to-end recurrent models differ in performance and prediction stability?  
  → In our experiments, **CNN+HMM performs best overall**, while **CNN–LSTM improves temporal consistency** but is more sensitive to configuration.

---


## Limitations & Future Work

<br>

### Limitations
- Fixed 10s windows can miss very short activity changes
- CNN–LSTM performance depends on sequence length and parameter choices
- Activity transitions are not modelled explicitly

<br>

### Future Work
- Use models or losses that focus on activity transitions
- Combine short and long time windows
- Apply the approach to ageing and mobility variability studies

---

## Conclusion

<br>

- Temporal modelling substantially improves HAR performance on CAPTURE-24
- **CNN + HMM** provides the strongest and most stable results
- **CNN–LSTM** is a viable end-to-end alternative
- Results highlight the benefit of accounting for temporal structure in free-living HAR


---

## References

<br>

- Chan et al. (2024). _CAPTURE-24_. Scientific Data.
- OxWearables (2024). CAPTURE-24 Benchmark Repository.
- Hossain et al. (2025). Benchmarking HAR models.

---

# Thank You

Questions?
