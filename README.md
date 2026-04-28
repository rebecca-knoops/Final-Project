# Early-Time Classification of Tidal Disruption Events vs Active Galactic Nuclei

Astro 416 Final Project  
Rebecca Knoops — University of Michigan (2026)

---

## Overview

This project applies machine learning techniques to distinguish between **tidal disruption events (TDEs)** and **active galactic nuclei (AGN)** using photometric time-series data.

The primary goal is not only to classify these sources, but to determine:

> **How early in a light curve can we reliably distinguish TDEs from AGN?**

This is motivated by time-domain surveys, where early identification of transients is critical for follow-up observations.

---

## Scientific Background

- **TDEs** occur when a star is disrupted by a supermassive black hole, producing a bright, transient flare with a smooth rise and decay.
- **AGN** exhibit persistent, stochastic variability due to ongoing accretion.

While these behaviors are distinct in ideal cases, **real observations can be noisy, incomplete, and ambiguous**, making classification challenging.

---

## Data Sources

This project combines multiple datasets:

- **TDEs** from the Transient Name Server (TNS)
- **AGN** from:
  - TNS (AGN-like transients)
  - Milliquas catalog (quasar/AGN sample)
- **Features and light curves** from the ALeRCE database (ZTF survey)

> ⚠️ Note: The Milliquas catalog files (`milliquas.fits`, `milliquas.txt`) are not included in this repository due to GitHub file size limits. The notebook expects these files to be available locally.

---

## Feature Extraction

Two types of features are used:

### 1. Full Light Curve Features (ALeRCE)
- Color features (e.g., g − r)
- Variability amplitude and standard deviation
- Rise and decay times
- TDE-specific model fits (e.g., `TDE_decay`, `fleet_*`)
- Host-galaxy offset metrics

### 2. Early-Time Features
Constructed from only the first **N days after first detection**:
- Detection counts and cadence
- Light curve slopes
- Variability amplitude
- Time to peak brightness
- Color evolution
- Monotonicity and sharpness metrics

---

## Methods

Two machine learning models are used:

### Logistic Regression
- Linear classifier
- Requires feature scaling
- Serves as an interpretable baseline model

### Random Forest
- Nonlinear ensemble of decision trees
- Captures complex feature interactions
- More flexible for astrophysical data

---

## Evaluation Metrics

Model performance is evaluated using:

- **ROC-AUC** (primary metric)
- **F1 score**
- **Confusion matrix**

A **benchmark classifier** (predicting the majority class) is used for comparison.

---

## Key Results

### Full Light Curve Classification
- Logistic Regression: ROC-AUC ≈ 0.84–0.86  
- Random Forest: ROC-AUC ≈ 0.92  

Random forest consistently outperforms logistic regression, indicating the importance of nonlinear feature relationships.

---

### Early-Time Classification

Performance depends strongly on how much data is available:

- **7 days:** Limited performance due to sparse data  
- **14 days:** Best performance (~0.85–0.87 ROC-AUC)  
- **30+ days:** Performance stabilizes  

This suggests that **meaningful classification is possible within ~2 weeks of detection**, which is highly relevant for real-time transient surveys.

---

## Repository Structure
Final_Project_Notebook.ipynb # Main analysis notebook
README.md # Project description

---

## How to Run

1. Install required packages:

```bash
pip install numpy pandas matplotlib scikit-learn astropy alerce

2. Open the notebook:
jupyter notebook Final_Project_Notebook.ipynb

3. Run all cells to:
build datasets
extract features
train models
generate plots

## Limitations
Small sample size
Missing feature values (handled via imputation)
Differences between AGN datasets (TNS vs Milliquas)
Early-time features are noisy and incomplete

## Future Work
Expand dataset size
Incorporate time-series modeling techniques
Apply to real-time classification pipelines

## Author
Rebecca Knoops
B.S. Astronomy & Astrophysics
University of Michigan (2026)
