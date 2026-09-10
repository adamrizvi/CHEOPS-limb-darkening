# CHEOPS Limb Darkening Analysis

An analysis of the stellar limb darkening parameters of some stars using open source photometric data collected from the CHEOPS satellite. This project was adapted from my final year undergraduate research project, with the templates for this project provided by my supervisor Dr Pierre Maxted at Keele University.

## Scientific Background

A star appears brighter at its centre than near its visible edge, or limb. This effect is called limb darkening and results from a different thickness of stellar being viewed through, dependent on the distance from the centre of the stellar disc.

This project models the transit of stars and estimates the power-2 limb darkening parameters, $h_1$ and $h_2$ [Maxted, 2018](https://www.aanda.org/articles/aa/pdf/2018/08/aa32944-18.pdf). The transit model is fitted using the `pycheops` pipeline and Markov Chain Monte Carlo (MCMC) sampling using `emcee`, both open source.

## Repository Contents

- `results/<TARGET>`: examples of results of each star available here, with a `summary.md` explanatory file for each; the `.multivisit` and `.h5` files used to generate these results are also included here.
- `stars/<TARGET>`: includes the `.dataset` files required to run the simulation in a `data` subfolder as well as the actual `.ipynb` analysis notebook set up for each star.
- `requirements.txt`: software requirements needed to run the simulation.
- `.gitignore`: excludes local environment and my `.h5` files (available as demos for download in `results`).
- `.gitattributes`: identifies notebooks and serialised dataset files for Git.

## Installation

The analysis uses Python 3.13 in a project-specific local virtual environment (strictly, Python 3.8 or later is required, however Python 3.9 or later is recommended). From the repository directory, run:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

In VS Code, select `.venv\Scripts\python.exe` as both the Python interpreter and the Jupyter notebook kernel.

## Running the Notebook

Open the notebook for the target you want to analyse and run its cells from top to bottom. For example, open `stars/WASP-18/WASP-18_CHEOPS_analysis.ipynb`. The notebooks:

1. Import the analysis packages.
2. Load the target's CHEOPS datasets from its star's `data` directory.
3. Define the transit parameters and priors.
4. Run a burn in and creates an `.h5` MCMC backend.
5. Continues the chain from the existing backend.
6. Produces trail plots, corner plots, and a fit report.

To extend the total number of steps of the burn, go to the 'main run' cell, select the number of new steps you wish to add, run the simulation, and the new steps will be appended to the `.multivisit` and `.h5` files. To update the plots and fit report, simply run the rest of the cells till the end of the notebook.

## Saving Results

The notebook creates the trail and corner figures. To save them for the repository, simply uncomment them which produces a `.pdf` of the trail and corner plots, as well as a `.txt` file of the fit report with all the available parameters.

## Method & Assumptions

This analysis uses 64 random walkers and an HDF5 backend MCMC chain. The model includes transit and limb darkening parameters, with the option to pre-set priors and ranges. The planetary orbital eccentricity is fixed to zero to reduce the number of parameters, given the low intrinsic eccentricities.

## Reproducibility Notes

- Use a local virtual environment rather than a global Python installation (the `.venv` environment is recommended).
- Run the notebook from the repository root so that its `stars/<TARGET>/data/` path resolves correctly.
- The notebook fetches a CHEOPS-related catalogue from the public SWEET-Cat repository `(https://github.com/iastro-pt/SWEET-Cat/tree/master)`.
- Generated `.h5` files, notebook checkpoints, and local virtual-environment files should not be committed (only done so here in a dedicated `results` section to give examples).
- Clear notebook outputs before committing unless they are intentionally included as final demonstration results.

## Acknowledgements

This project uses CHEOPS observations and the open-source `pycheops`, `emcee`, and `h5py` packages.