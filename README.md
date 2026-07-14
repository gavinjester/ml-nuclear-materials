# Rad-Corr-ML: Predicting Failure Mechanisms in Nuclear Reactor Materials from Combined Radiation and Corrosion
> **Note:** This project is in early development. I am currently building the data pipeline and exploratory analysis. Models and results will be added as the project progresses. This README reflects the planned scope, not completed work.

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
---
## Repository Structure
> **Note:** Planned Structure
```
RadCorr-ML/
├─ README.md                        
├── requirements.txt                 ← Python dependencies
├── data/
│   ├── raw/                         ← original downloaded datasets (not committed if large)
│   └── processed/                   ← cleaned feature matrices
├── notebooks/
│   └── 01_data_exploration.ipynb    ← initial dataset analysis
├── src/
│   ├── features/                    ← physics-informed feature engineering
│   └── models/                      ← baseline ML models
└── docs/
    └── DATA_SOURCES.md              ← annotated bibliography of all datasets
```

---
## Data Sources

This project uses only publicly available data. No proprietary or export-controlled information is included.

**Primary sources:**
- **NRC ADAMS** — NUREG/CR-6960, NUREG/CR-7027: compiled crack growth rate and mechanical property databases for irradiated stainless steels
- **OSTI.gov** — Chopra & Rao (2011), ANL/ENG/NE-11-001: large compiled IASCC dataset with ~200 specimens
- **Peer-reviewed literature** — Was et al. (2017), Mamivand et al. (2021): supplementary tables digitized from published reviews
- **IAEA** — TECDOC-1090, TECDOC-1502: irradiation effects compilations

See [`docs/DATA_SOURCES.md`](docs/DATA_SOURCES.md) for a full annotated bibliography with access URLs.

---

## The Physics (Brief Overview)
IASCC requires three conditions to be met simultaneously:

1. **Radiation damage** — neutron bombardment causes radiation-induced segregation (RIS), depleting chromium at grain boundaries and reducing the alloy's corrosion resistance. Damage accumulates in units of displacements per atom (dpa).

2. **Aggressive environment** — high-temperature water chemistry (particularly electrochemical potential, dissolved oxygen, and conductivity) determines whether the grain boundary surface is in an oxidizing or reducing state.

3. **Mechanical stress** — stress intensity factor K must exceed a threshold (~15 MPa√m for irradiated 304SS) for a crack to propagate.

Machine learning is useful here because the interactions between these three domains are nonlinear and depend on dozens of variables simultaneously — beyond what simple empirical rules can capture.

---

## Planned ML Approach

**Problem framing:** Binary classification (cracked / uncracked) as the primary task, with crack growth rate regression as a secondary target for specimens where quantitative measurements exist.

**Key features:**
- Alloy composition (Cr, Ni, Si, Mo content; stacking fault energy)
- Irradiation history (dpa, dose rate, irradiation temperature)
- Water chemistry (electrochemical potential, dissolved oxygen, conductivity, temperature)
- Mechanical loading (stress intensity factor K, yield strength after irradiation)

**Baseline models (planned in order):**
1. Logistic regression — interpretable; coefficient signs validate against physical expectations
2. Random forest — captures nonlinear interactions; SHAP values for feature importance
3. XGBoost — best predictive performance; handles missing data natively

**Validation strategy:** Leave-One-Lab-Out cross-validation. Because data comes from many different test reactors and laboratories, standard random k-fold CV would leak information across labs and produce overly optimistic metrics. Holding out each lab in turn gives a more honest estimate of generalization.

---

## Progress

- [ ] Identify and document all public data sources
- [ ] Extract and standardize data into a common feature schema
- [ ] Exploratory data analysis and missingness characterization
- [ ] Physics-informed feature engineering
- [ ] Baseline model training and evaluation
- [ ] SHAP-based physics validation of feature importances

---

## About This Project

I am an undergraduate student developing this project independently while completing coursework in machine learning (Stanford Machine Learning Specialization, DeepLearning.AI). The technical framework draws on the nuclear materials and materials literature.

This repository reflects genuine ongoing work, not a finished product. Commit history documents the actual progression of the project. If you are a researcher working on related problems and have questions or suggestions, I welcome contact.

---

## Key References

- Was, G.S. (2017). Irradiation assisted stress corrosion cracking. *Journal of Nuclear Materials*, 499, 471–495.
- Chopra, O.K. & Rao, A.S. (2011). *Review of IASCC of austenitic stainless steels in LWR environments*. ANL/ENG/NE-11-001. Argonne National Laboratory.
- Bruemmer, S.M. et al. (1999). Microstructural and microchemical mechanisms controlling IASCC. *Journal of Nuclear Materials*, 274(3), 299–314.
- Mamivand, M. et al. (2021). Predicting yield stress of irradiated austenitic stainless steels. *Computational Materials Science*, 191, 110222.

---

## License

MIT — see [LICENSE](LICENSE). Note: Some referenced datasets may have redistribution restrictions; raw data files are not committed to this repository.
