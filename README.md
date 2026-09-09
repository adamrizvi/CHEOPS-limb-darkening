# CHEOPS Limb-Darkening Analysis

An analysis of the stellar limb-darkening parameters of WASP-18 using photometric data from the CHEOPS satellite. This project was adapted from a final-year undergraduate research project.

## Scientific background

A star appears brighter at its centre than near its visible edge, or limb. This effect is called limb darkening and results from observing different layers of the stellar atmosphere across the stellar disk.

This project models the transit of the hot Jupiter WASP-18 and estimates the power-2 limb-darkening parameters, h1 and h2. The transit model is fitted with the `pycheops` pipeline and Markov Chain Monte Carlo (MCMC) sampling using `emcee`.

## Repository contents

- `stars/WASP-18/WASP-18_CHEOPS_analysis.ipynb`: WASP-18 analysis notebook.
- `stars/WASP-18/data/`: WASP-18 CHEOPS dataset files.
- `stars/KELT-25/KELT-25_CHEOPS_analysis.ipynb`: KELT-25 analysis notebook.
- `stars/KELT-25/data/`: KELT-25 CHEOPS dataset files.
- `requirements.txt`: pinned Python dependencies.
- `.gitignore`: excludes local environments and generated HDF5 files.
- `.gitattributes`: identifies notebooks and serialized dataset files for Git.

## Installation

The analysis uses Python 3.13 in a project-local virtual environment. From the repository directory, run:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

In VS Code, select `.venv\Scripts\python.exe` as both the Python interpreter and the Jupyter notebook kernel.

## Running the notebook

Open the notebook for the target you want to analyse and run its cells from top to bottom. For example, open `stars/WASP-18/WASP-18_CHEOPS_analysis.ipynb`. The notebooks:

1. Imports the analysis packages.
2. Loads the target's CHEOPS datasets from its star-specific `data/` directory.
3. Defines the transit parameters and priors.
4. Runs a short burn-in and creates an HDF5 MCMC backend.
5. Continues the chain from the existing backend.
6. Produces trail plots, corner plots, and a fit report.

The first MCMC section contains `backend.reset(...)`. Running that section starts a new chain and replaces the existing backend contents. The later MCMC section opens the existing backend and appends further steps. Do not rerun the reset section if you intend to continue an existing chain.

The HDF5 backend is written in the current working directory, for example as `WASP-18_analysis.h5`. It is intentionally excluded from Git by `.gitignore` because it is a generated analysis file and can become large.

## Saving result plots

The notebook creates the trail and corner figures. To save them for the repository, uncomment or add:

```python
from pathlib import Path

results_dir = Path("results")
results_dir.mkdir(exist_ok=True)

trail_fig = M.trail_plot()
trail_fig.savefig(results_dir / "trail_plot.pdf", bbox_inches="tight")
trail_fig.savefig(results_dir / "trail_plot.png", dpi=200, bbox_inches="tight")

corner_fig = M.corner_plot()
corner_fig.savefig(results_dir / "corner_plot.pdf", bbox_inches="tight")
corner_fig.savefig(results_dir / "corner_plot.png", dpi=200, bbox_inches="tight")
```

PDF files are useful for high-quality figures, while PNG files display directly in the GitHub README. Link them like this after adding the files:

```markdown
![MCMC trail plot](results/trail_plot.png)

[Download the trail plot as PDF](results/trail_plot.pdf)

![MCMC corner plot](results/corner_plot.png)

[Download the corner plot as PDF](results/corner_plot.pdf)
```

## Method and assumptions

The analysis uses 64 walkers and an HDF5-backed MCMC chain. The model includes transit and limb-darkening parameters, with the allowed parameter ranges specified in the notebook. The orbital eccentricity is currently fixed to zero, and the adopted priors are stated in the analysis cells.

The trail and corner plots should be inspected together with quantitative diagnostics before making strong claims about convergence. In particular, limb-darkening parameters can be correlated and weakly constrained by the available data. A longer chain does not by itself prove convergence.

## Reproducibility notes

- Use the repository-local virtual environment rather than a global Python installation.
- Run the notebook from the repository root so that its `stars/<target>/data/` path resolves correctly.
- The notebook refreshes a CHEOPS-related catalogue cache from the public SWEET-Cat repository.
- Generated `.h5` files, notebook checkpoints, and local virtual-environment files should not be committed.
- Clear notebook outputs before committing unless they are intentionally included as final demonstration results.

## Limitations and future work

Potential extensions include:

- calculating integrated autocorrelation times and effective sample sizes;
- comparing alternative limb-darkening laws;
- testing informative and weakly informative priors;
- investigating correlated instrumental noise;
- comparing CHEOPS results with other photometric datasets such as TESS;
- fitting additional CHEOPS targets.

## Acknowledgements

This project uses CHEOPS observations and the open-source `pycheops`, `emcee`, and `h5py` packages. The relevant data, software, and published parameter sources should be cited appropriately in any formal scientific use of this analysis.
