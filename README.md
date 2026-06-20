# 2026-6-20
# EUI-OCEI Beijing Hotel Research Package

This repository contains the reproducible code package for a revised manuscript on Beijing hotel-building EUI prediction and EUI-OCEI coupling analysis. The workflow combines EnergyPlus parametric simulation, sensitivity analysis, machine learning, and carbon-intensity coupling.

## Formal Workflow

Run the four root-level notebooks in order:

1. `01_Parametric_Simulation_Database_Construction.ipynb`: LHS sampling, feasibility screening, IDF generation, EnergyPlus or debug execution, EUI dataset construction, and measured-EUI benchmarking.
2. `02_SRC_Sensitivity_and_Variable_Selection.ipynb`: VIF, bootstrap SRC, SHAP nonlinear cross-check, SRC-SHAP divergence analysis, and variable-cutoff justification.
3. `03_ML_Model_Training_and_Evaluation.ipynb`: 17-model comparison, hyperparameter reporting, non-core-variable impact test, and best-model export.
4. `04_EUI_OCEI_Coupling_and_Carbon_Analysis.ipynb`: OCEI construction, emission-factor sensitivity, EUI-OCEI ranking shifts, SRC comparison, and OCEI surrogate validation.

`tools/` is optional QA support. It is not a fifth research step.

## Environment

Recommended environment:

- Windows 10/11
- Python 3.11
- EnergyPlus 25.2.0 at `C:/EnergyPlusV25-2-0/energyplus.exe`
- `numpy`, `pandas`, `scipy`, `matplotlib`, `seaborn`, `scikit-learn`, `statsmodels`, `xgboost`, `lightgbm`, `shap`, `joblib`, `jupyter`, `nbclient`

Verify EnergyPlus before a full simulation:

```powershell
C:\EnergyPlusV25-2-0\energyplus.exe --version
```

## Running Notebook 01

For the formal full simulation, keep:

```python
CONFIG["n_samples"] = 20000
CONFIG["run_energyplus"] = True
```

For fast Python-logic debugging, use:

```python
CONFIG["run_energyplus"] = False
```

Debug mode validates notebook logic and downstream data handling, but it is not full EnergyPlus evidence.

## Optional Tools

Run static repository checks:

```powershell
python tools/check_repository_quality.py
```

This checks notebook readability, Python syntax, separated Chinese/English note cells, stale placeholder text, saved warning/error outputs, and known old bug patterns.

Run isolated validation without overwriting formal outputs:

```powershell
python tools/validate_notebooks.py --notebooks 02,03,04
```

Run all four notebooks in an isolated workspace with Notebook 01 forced to debug mode:

```powershell
python tools/validate_notebooks.py --notebooks all --samples 200
```

By default, isolated validation also down-samples `data/step1_simulation_dataset.csv` to the requested `--samples` count so code paths can be checked quickly without overwriting formal outputs. To validate against the full copied dataset, add:

```powershell
python tools/validate_notebooks.py --notebooks 02,03,04 --full-data
```

Run a full EnergyPlus validation only when enough time is available:

```powershell
python tools/validate_notebooks.py --notebooks 01 --full-step1
```

Validation copies files to `archive/validation_runs/` and should not be committed unless requested.

## Revision Boundary

Current changes are intended to strengthen reviewer-facing evidence: simulation-to-measurement plausibility, feasibility-screening effects, SRC limitations, SHAP supplementation, emission-factor sensitivity, EnergyPlus reproducibility, hyperparameter transparency, variable-cutoff justification, and EUI-OCEI ranking divergence.

Do not change model families, parameter ranges, units, feature names, carbon-accounting assumptions, random seeds, or the four-notebook order unless the manuscript and response logic are updated at the same time.
