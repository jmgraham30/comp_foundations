# Python Projects and Environments (with uv)

[← Back to README](../README.md)

This guide shows how to create a Python project with its own **environment** and install the packages used for scientific and statistical computing. It assumes you finished the setup for [Mac](01-setup-mac.md) or [Windows](01-setup-windows.md) and read the [file system guide](02-filesystem-and-projects.md).

## 1. Why environments?

Python packages depend on each other, and different projects need different versions. If everything is installed into one shared place, upgrading a package for project A can silently break project B, and results you obtained last month may not be reproducible.

The solution is a **virtual environment**: a private folder (`.venv`) containing a copy of the Python interpreter and only the packages that one project needs. You make one per project, never share them, and never install project packages "globally".

**uv** handles all of it:

| Task | uv command |
|---|---|
| Install a Python version | `uv python install 3.12` |
| Create a project | `uv init` |
| Add a package | `uv add numpy` |
| Remove a package | `uv remove numpy` |
| Recreate the environment from files | `uv sync` |
| Run a command in the environment | `uv run python script.py` |
| Update a package | `uv lock --upgrade-package numpy` |

Two files record the environment, and **both go in git**:

- `pyproject.toml`: the packages you asked for (human-written/edited intent).
- `uv.lock`: the exact versions that were installed (machine-generated). A classmate who clones your project runs `uv sync` and gets the identical environment.

The `.venv/` folder is generated and is **not** committed.

## 2. Create a project

```
cd ~/projects                  # Windows: cd C:\Users\<you>\projects
uv init my-first-project
cd my-first-project
```

This creates a folder with `pyproject.toml`, a `.python-version`, a sample script, and a `README.md`. Add packages:

```
uv add numpy pandas matplotlib
```

The first `uv add` creates `.venv/` and `uv.lock`. Run something:

```
uv run python -c "import numpy; print(numpy.__version__)"
```

To choose a specific Python version for the project: `uv python pin 3.12` and then `uv sync`.

### A project that is mainly Quarto documents (no packaged code)

Most of your work will be analyses, not libraries. Use a bare project:

```
mkdir eeg-study && cd eeg-study
git init
uv init --bare
uv add jupyter
```

`jupyter` supplies the "kernel" Quarto uses to run Python code. Then render with `uv run quarto render analysis.qmd`, or open the folder in Positron, which finds `.venv` automatically (if not, use the interpreter picker in the top right to select `.venv`).

## 3. The scientific Python stack

Install these in a new project as you need them. Don't install everything at once; each package is a dependency you must understand.

### Core (almost everyone)

```
uv add numpy pandas scipy matplotlib seaborn statsmodels scikit-learn jupyter
```

| Package | What it does |
|---|---|
| **numpy** | Fast arrays and numerical computing; the foundation of everything else |
| **pandas** | Data frames: reading, cleaning, grouping, and reshaping tabular data |
| **scipy** | Scientific algorithms: statistics, optimization, signal processing, linear algebra |
| **matplotlib** | The fundamental plotting library |
| **seaborn** | Statistical graphics built on matplotlib |
| **statsmodels** | Classical statistical models (regression, GLMs, time series, tests) with R-style output |
| **scikit-learn** | Machine learning: classification, regression, clustering, cross-validation |
| **jupyter** | Notebook kernel; lets Quarto and Positron run Python code chunks |

### Useful additions

```
uv add polars pyarrow plotnine pingouin lifelines
```

| Package | What it does |
|---|---|
| **polars** | A fast, modern alternative to pandas |
| **pyarrow** | Parquet/Arrow file support, efficient storage of large tables |
| **plotnine** | ggplot2-style plotting in Python (helpful if you know R) |
| **pingouin** | Friendly statistical tests |
| **lifelines** | Survival analysis (Kaplan–Meier, Cox regression) |

### Neuroscience

```
uv add mne nibabel nilearn
```

| Package | What it does |
|---|---|
| **mne** | EEG/MEG analysis |
| **nibabel** | Reading and writing neuroimaging file formats (NIfTI, etc.) |
| **nilearn** | Statistical learning and visualization for neuroimaging data |

Others to know about: **neo** and **elephant** (electrophysiology), **pynwb** (Neurodata Without Borders format), **brian2** (spiking-network simulation).

### Bayesian and probabilistic computing

```
uv add pymc arviz
```

| Package | What it does |
|---|---|
| **pymc** | Probabilistic programming and Bayesian modeling |
| **arviz** | Diagnostics and plotting for Bayesian results |

### Development habits

Tools used for development only go in a separate group so they don't clutter the main environment:

```
uv add --dev pytest ruff
```

| Package | What it does |
|---|---|
| **pytest** | Automated tests for your functions |
| **ruff** | Very fast code checker and formatter |

## 4. Working with someone else's project

```
git clone https://github.com/<user>/<repo>.git
cd <repo>
uv sync
```

## 5. Mistakes to avoid

- **Don't use `pip install` directly** in a project; use `uv add` so the change is recorded. (`uv pip install` exists for special cases; ask first.)
- **Don't copy a `.venv` folder** between computers or projects. Recreate it with `uv sync`.
- **Don't commit `.venv/`** (see the `.gitignore` in this repository for a template).
- **Don't edit `uv.lock` by hand.**
- **Don't upgrade everything casually** in the middle of an analysis. Upgrades change results and break code. Do it deliberately, test, then commit.

## 6. When uv isn't enough: pixi and conda

Some scientific software isn't a Python package; it's a compiled program or library (certain bioinformatics tools, GIS libraries, or packages that need specific system libraries). **conda-forge** and **bioconda** distribute those, and **[pixi](https://pixi.prefix.dev/latest/)** is a modern project manager that installs from conda channels *and* PyPI, and can even manage R in the same environment. If you encounter a tool whose documentation says "install with conda", try pixi first. **Miniconda/Anaconda** is the older route; you will see it in lab instructions, and it works, but avoid installing packages into its `base` environment.

## 7. Practice

1. Create `python-practice` under `projects`, run `git init`, and `uv init --bare`.
2. `uv add numpy pandas matplotlib jupyter`.
3. Write a `practice.qmd` that simulates 1000 normal random numbers, plots a histogram, and prints the mean. Render it.
4. Commit `pyproject.toml`, `uv.lock`, `practice.qmd`, and a `.gitignore` that excludes `.venv/`. Push to GitHub.
5. Delete `.venv`, run `uv sync`, and confirm everything still renders.
