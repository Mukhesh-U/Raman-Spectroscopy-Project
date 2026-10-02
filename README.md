# Raman Spectroscopy under Distribution Shift

This repository contains notebooks and saved artifacts for synthetic CARS–Raman data generation, Raman spectrum estimation, and prediction-uncertainty analysis under synthetic-to-real distribution shift.

## Project overview

The project explores estimating Raman spectra from coherent anti-Stokes Raman scattering (CARS) measurements. It also evaluates how prediction coverage changes when a model calibrated on synthetic data is applied to real solvent measurements.

The workflow includes:

1. Generating synthetic CARS and Raman spectra.
2. Training a one-dimensional residual neural network to estimate Raman spectra.
3. Calibrating prediction intervals on synthetic data.
4. Evaluating empirical coverage on synthetic and real solvent data.
5. Comparing unweighted calibration with density-ratio-weighted calibration.

The training notebook uses a composite objective involving spectral reconstruction, Hilbert-transform consistency, and smoothness of the estimated non-resonant background.

## Repository structure

```text
.
├── Datasets/
│   ├── Synthetic_data/
│   │   ├── X_cars_2000x640.npy
│   │   └── y_raman_2000x640.npy
│   └── dataset_Mukesh/
│       └── dataset_Mukesh/
│           └── solvent spectra and wavenumber files
├── Results/
│   ├── Final_Predictions/
│   │   ├── model.pth
│   │   ├── y_pred_norm_cal.npy
│   │   ├── y_true_norm_cal.npy
│   │   ├── y_pred_norm_eval.npy
│   │   ├── y_true_norm_eval.npy
│   │   ├── y_pred_real.npy
│   │   └── y_true_real.npy
│   └── Weighted_and_Unweighted_Distribution.png
├── notebooks/
│   ├── CARS and Raman Synthetic data .ipynb
│   └── Raman Training and Uncertainty in Distribution_Shift.ipynb
├── .gitignore
└── README.md
```

## Files

- **Synthetic-data notebook:** `notebooks/CARS and Raman Synthetic data .ipynb` documents synthetic CARS and Raman data generation.
- **Synthetic arrays:** `Datasets/Synthetic_data/X_cars_2000x640.npy` and `Datasets/Synthetic_data/y_raman_2000x640.npy` contain the generated arrays. Their filenames indicate 2,000 rows and 640 values per row.
- **Training and analysis notebook:** `notebooks/Raman Training and Uncertainty in Distribution_Shift.ipynb` contains the model training, prediction, and uncertainty-analysis workflow.
- **Solvent dataset:** `Datasets/dataset_Mukesh/dataset_Mukesh/` contains the solvent spectra and wavenumber files used for real-data evaluation.
- **Saved report run:** `Results/Final_Predictions/` contains the checkpoint and prediction arrays from the stochastic run used for the final report.
- **Coverage plot:** `Results/Weighted_and_Unweighted_Distribution.png` visualizes the empirical coverage comparison.

## Coverage results

The figure shows empirical coverage for the run used in the report. The target coverage was 90%.

| Evaluation data | Unweighted | Weighted using predicted Raman | Weighted using raw CARS |
|---|---:|---:|---:|
| Synthetic | 90.0% | 94.2% | 90.5% |
| Real solvent | 81.0% | 87.3% | 81.6% |

For this run, weighting based on predicted Raman increased real-data coverage from 81.0% to 87.3%. It remained below the 90% target. These are empirical results for this experiment, not guarantees for other datasets or future runs.

![Coverage comparison for unweighted and weighted conformal prediction](Results/Weighted_and_Unweighted_Distribution.png)

*Figure: Empirical coverage for the saved run. The dashed line marks the 90% target.*

## Running the notebooks

The notebooks are intended to be opened with Jupyter Notebook or JupyterLab.

1. Clone or download this repository.
2. Install Python and the packages imported by the notebook, including PyTorch, NumPy, Matplotlib, and scikit-learn.
3. Open the notebook you want to run.
4. Check the data-loading paths near the start of the notebook. The notebooks are in `notebooks/`, while the datasets are in `Datasets/`.
5. Check that the array names and dimensions expected by the training notebook match the included arrays before running all cells.

The notebooks were developed as research notebooks; exact dependency versions and a fully automated setup file have not yet been added.

## Reproducibility

Training involves stochastic operations, so rerunning it may produce different model weights and predictions. The saved artifacts in `Results/Final_Predictions/` preserve the run used for the final report. They are included so the reported analysis can be inspected without expecting a new training run to reproduce identical values.

For future runs, record the random seed, package versions, data split, and hardware alongside the outputs.

## Data and reuse

The repository includes synthetic arrays and solvent dataset files. Please consult the dataset documentation and its original source for provenance and reuse conditions before using the data in another project.

No license file is currently included. Code and data may have different reuse terms; add appropriate licenses and source citations once those terms are confirmed.

## Citation

Add the citation for the associated report, publication, or project source here when available.
