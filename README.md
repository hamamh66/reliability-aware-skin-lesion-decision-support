# Reliability-Aware Skin Lesion Decision Support

This repository provides a reproducible machine-learning workflow for reliability-aware skin lesion classification using dermoscopic image data. The framework is designed to evaluate not only predictive accuracy, but also confidence, uncertainty, calibration, robustness, selective prediction, and class-aware referral behavior.

## Overview

Conventional classification pipelines often report aggregate accuracy, weighted F1-score, or AUC as primary outcomes. In safety-sensitive decision-support settings, these metrics are insufficient because they do not indicate when an individual prediction should be trusted or deferred for expert review.

This repository implements a reliability-aware pipeline that separates three stages:

1. **Predictive modeling** using transparent ensemble classifiers.
2. **Reliability assessment** using confidence, entropy, calibration, robustness, and error-detection metrics.
3. **Decision support** using selective prediction, accept–defer rules, clinical decision zones, and class-aware referral logic.

The workflow is intended for research on trustworthy medical AI, uncertainty-aware classification, and human-in-the-loop decision support.

## Main Features

- Supervised classification on dermoscopic image-derived feature representations.
- Transparent ensemble baselines including ExtraTrees, Random Forest, Logistic Regression, and soft-voting models.
- Stratified train/test splitting and cross-validation stability analysis.
- Class-weighted training to account for severe class imbalance.
- Probabilistic reliability assessment using:
  - maximum confidence,
  - predictive entropy,
  - expected calibration error,
  - maximum calibration error,
  - Brier score,
  - negative log-likelihood.
- Post-hoc temperature scaling for probability calibration without retraining.
- Misclassification detection using confidence- and entropy-based scores.
- Selective prediction through risk–coverage analysis.
- Accept–defer decision rules for routing uncertain cases to review.
- Class-aware and malignancy-aware referral evaluation.
- Clinical decision-zone mapping based on empirical reliability.
- Robustness testing under controlled perturbations such as noise, brightness, and contrast shifts.
- Feature-importance and perturbation-sensitivity analysis for explanation consistency.
- Automated generation of figures, tables, and summary outputs.

## Repository Structure

A typical repository organization is:

```text
.
├── notebooks/
│   └── reliability_aware_skin_lesion_pipeline.ipynb
├── src/
│   ├── data_preprocessing.py
│   ├── model_training.py
│   ├── calibration.py
│   ├── selective_prediction.py
│   ├── robustness.py
│   └── explainability.py
├── figures/
├── tables/
├── outputs/
├── requirements.txt
└── README.md
```

The exact structure may be adapted depending on whether the implementation is provided as a single notebook or as modular Python scripts.

## Dataset

The workflow is designed for public dermoscopic skin lesion datasets, particularly the HAM10000 dataset and its tabular 28 × 28 RGB representation. The dataset should be downloaded from its official public source or through an approved data-access platform.

No dataset files are included in this repository by default.

Expected input format:

- one row per image sample,
- pixel or feature columns,
- one diagnostic class label column,
- seven-class skin lesion classification setting when using HAM10000.

## Installation

Clone the repository:

```bash
git clone https://github.com/<user>/<repository-name>.git
cd <repository-name>
```

Create and activate a Python environment:

```bash
python -m venv venv
source venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

A minimal dependency list includes:

```text
numpy
pandas
scikit-learn
matplotlib
scipy
joblib
```

Additional packages may be required depending on the notebook configuration.

## Usage

Run the main notebook or script after placing the dataset in the expected data directory.

Example notebook workflow:

```bash
jupyter notebook notebooks/reliability_aware_skin_lesion_pipeline.ipynb
```

Example script workflow:

```bash
python src/model_training.py
python src/calibration.py
python src/selective_prediction.py
python src/robustness.py
python src/explainability.py
```

The workflow produces:

- model performance tables,
- calibration metrics,
- reliability diagrams,
- risk–coverage curves,
- accept–defer operating points,
- class-aware referral results,
- robustness plots,
- feature-importance maps,
- perturbation-sensitivity maps,
- consolidated output summaries.

## Methodological Components

### 1. Predictive Modeling

The predictive backbone estimates class probabilities for each lesion sample. Transparent tree-based ensembles are used to support reproducibility and model-agnostic reliability analysis.

### 2. Calibration

Calibration evaluates whether predicted probabilities match empirical correctness. Temperature scaling is used as a post-hoc correction method that changes probability sharpness while preserving predicted class labels.

### 3. Uncertainty and Error Detection

Maximum confidence and predictive entropy are evaluated as indicators of whether a prediction is likely to be correct or incorrect. These signals are used to support selective prediction.

### 4. Selective Prediction

The model is not forced to classify every case automatically. Instead, predictions may be accepted or deferred based on reliability thresholds. Risk–coverage analysis quantifies how selective accuracy changes as uncertain cases are deferred.

### 5. Class-Aware Referral

Global confidence thresholds may improve aggregate accepted-case accuracy while failing to protect minority or clinically critical classes. The repository therefore includes class-aware and malignancy-aware referral logic to evaluate whether high-risk cases are routed to expert review.

### 6. Robustness and Explanation Consistency

Controlled perturbations are used to evaluate the stability of predictions under input variation. Feature importance is compared with perturbation-based sensitivity to test whether static explanations reflect actual model behavior.

## Outputs

Generated outputs may include:

```text
outputs/
├── outputs_summary.txt
├── classification_metrics.csv
├── calibration_metrics.csv
├── risk_coverage_results.csv
├── class_aware_referral_results.csv
├── robustness_results.csv
└── explanation_consistency_results.csv
```

Figures may include:

```text
figures/
├── reliability_diagram.png
├── calibration_gap.png
├── risk_coverage_curve.png
├── selective_accuracy_curve.png
├── melanoma_referral_curve.png
├── robustness_noise.png
├── feature_importance_map.png
└── perturbation_sensitivity_map.png
```

## Reproducibility

The workflow is designed to be reproducible through:

- fixed random seeds,
- documented train/test splitting,
- stratified cross-validation,
- automated metric generation,
- saved figures and tables,
- consolidated output summaries.

Users are encouraged to report the dataset version, preprocessing choices, random seed, and software environment when reproducing or extending the experiments.

## Intended Use

This repository is intended for research and educational use in reliability-aware machine learning and medical decision-support evaluation. It is not a clinical diagnostic device and should not be used for patient care without appropriate validation, regulatory review, and expert oversight.

## License

A permissive open-source license such as MIT, BSD-3-Clause, or Apache-2.0 may be used depending on the intended reuse conditions.

## Citation

If this repository supports academic work, cite the repository and any dataset or software resources used in the experiments.
