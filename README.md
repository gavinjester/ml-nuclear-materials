# Rad-Corr-ML: Predicting Failure Mechanisms in Nuclear Reactor Materials from Combined Radiation and Corrosion
> A physics-grounded machine learning framework for predicting 

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/)
[![Status: Research](https://img.shields.io/badge/Status-In_Development-yellow)]()
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## Motivation
When austenitic stainless steels and nickel-based alloys are used inside nuclear reactor cores, they face three simultaneous stressors: neutron bombardment that displaces atoms and alters the alloy's microstructure, hot high-pressure water that drives electrochemical corrosion, and sustained mechanical stress from the reactor structure itself.

The interaction of all three, known as Irradiation-Assisted Stress Corrosion Cracking (IASCC), is one of the most complex degradation mechanisms in nuclear engineering. Existing mechanistic models require detailed input parameters rarely available in operating reactors. This project explores whether machine learning, trained on compiled experimental data from the public literature, can serve as a practical predictive surrogate for material susceptibility.

---

## Research Goals
| Stage | Goal|
|---|---|
| 1 | Compile and clean publicly available IASCC datasets from NRC, IAEA, and peer-reviewed literature |
| 2 | Engineer physics-grounded features capturing alloy chemistry, radiation history, water chemistry, and mechanical loading |
| 3 | Build and evaluate baseline ML models (logistic regression, random forest, XGBoost) to predict cracking susceptibility |
| 4 | Validate learned feature importances against known physical mechanisms using SHAP analysis |
