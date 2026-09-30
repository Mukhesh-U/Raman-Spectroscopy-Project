# Raman Spectroscopy under Distribution Shift

Research code for estimating Raman spectra from coherent anti-Stokes Raman scattering (CARS) measurements and studying prediction uncertainty when moving from synthetic data to real solvent measurements.

> **Status:** This repository documents a research workflow. The archived prediction arrays are outputs from a stochastic training run and are the numerical artifacts used for the reported analysis. Re-running training may produce different outputs.

## Research overview

The workflow trains a one-dimensional residual neural network to estimate Raman spectra from CARS spectra. Its training objective combines spectral reconstruction error with a Hilbert-transform/Kramers–Kronig consistency term and a smoothness penalty for the estimated non-resonant background. The analysis then calibrates prediction intervals on synthetic data and compares coverage on real solvent measurements, including a density-ratio-weighted calibration analysis for distribution shift.

## Repository contents

```text
.
├── README.md
├── .gitignore
├── notebooks/                  # Reviewed, cleaned analysis notebooks
├── src/                        # Reusable model and analysis code (if extracted)
├── data/                       # No raw or experimental dataset files published
│   └── README.md               # Authorized access instructions only
├── models/
│   └── README.md               # Checkpoint provenance and loading instructions
├── results/
│   └── final_report_run/       # Exact saved outputs used by the report
│       ├── README.md
│       └── ...                 # Preserve original prediction arrays and figures
├── environment/                # requirements.txt or environment.yml
└── docs/
    └── reproducibility.md
```

This is the intended organization; files should be moved only after their provenance and relationships are verified. In particular, the checkpoint currently visible under `cache/` has not yet been confirmed to match the saved predictions used in the report.

## Data

The workflow uses paired synthetic CARS and Raman spectra, and real solvent CARS/Raman spectra with corresponding wavenumber values. **This repository does not distribute raw or experimental datasets.** See [`data/README.md`](data/README.md) for provenance and authorized access instructions. Do not add `.npy` data files or dataset archives to commits.

Large or restricted datasets must not be committed to this repository. Keep local data outside the repository; the `.gitignore` should exclude relevant data directories and array/archive formats as a safeguard. Provide access instructions only if approved.

## Saved results and reproducibility

The final report is based on the saved prediction artifacts from `cache_2`. Preserve these files as a named, read-only snapshot under `results/final_report_run/` after confirming that the local snapshot is complete and matches the report. They are saved model outputs, not raw datasets; check that the arrays themselves do not expose restricted measurements before publishing. They are the record of the stochastic run used for the reported analysis; they are not expected to be recreated bit-for-bit by a new training run.

Training randomness can affect model weights and predictions. The code should record the random seed and software/hardware environment for future runs, but setting a seed does not guarantee identical results across all devices or library versions. The report's saved prediction arrays should therefore remain distinct from newly generated outputs.

The current notebook preview saves a checkpoint and six prediction arrays under `cache_3/`, while an earlier analysis cell references `cache_2/`. Keep both sets intact until their origins and roles are mapped. Do not describe `cache/model.pth` as the report model unless that link is verified.

## Workflow

1. Install the dependencies listed in `requirements.txt` (to be finalized from the reviewed notebook environment).
2. Obtain data through its authorized source and keep it outside the public repository.
3. Run the reviewed training notebook or scripts to generate a new stochastic run.
4. Save each new run to a unique output directory; do not overwrite the archived report snapshot.
5. Run the uncertainty and distribution-shift analysis using an explicit results directory.

Exact commands and dependency versions will be added after the notebook is cleaned and tested from a fresh environment.

## Methods at a glance

- One-dimensional residual neural network with separate resonant and non-resonant output branches.
- Composite objective using mean squared error, Hilbert-transform consistency, and non-resonant-background smoothness terms.
- Synthetic-data calibration and real-solvent evaluation.
- Unweighted and density-ratio-weighted conformal calibration, with empirical coverage comparison.

## Limitations

Coverage estimates depend on the dataset, split, calibration procedure, and distribution-shift assumptions. They should be interpreted as empirical results for the documented experiments, not as a guarantee for arbitrary samples or deployment conditions. Add quantitative findings only after checking them against the final report and saved outputs.

## Citation and contact

Add the associated report, publication, institution, and preferred contact/citation details here when approved for public display.
