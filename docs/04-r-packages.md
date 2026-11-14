# R Projects and Packages

[← Back to README](../README.md)

This guide covers how to set up an R project with reproducible packages (`renv`) and then surveys the packages most useful for statistics and neuroscience. It assumes you finished the setup for [Mac](01-setup-mac.md) or [Windows](01-setup-windows.md).

## 1. R vs. packages vs. libraries

- **R** is the language and its base installation (managed with `rig`).
- A **package** is a bundle of functions, data, and documentation written by others. Most of R's power comes from packages.
- A **library** is the folder where installed packages live. `install.packages("x")` puts a package in a library; `library(x)` loads it for your current session.

You install a package **once** per library, but you load it **every session**.

## 2. Reproducible projects with renv

Packages change over time, sometimes in ways that alter results. `renv` gives each project its own private library and records exact versions in `renv.lock`, the R counterpart of `uv.lock`.

### Start a project

1. In Positron: *File → Open Folder…* on your (new) project folder. In RStudio: *File → New Project…*.
2. In the R console:

```r
renv::init()
```

This creates `renv/`, `renv.lock`, and an `.Rprofile`.

3. Install packages as usual (they go into the project library):

```r
install.packages("tidyverse")
```

4. After installing or updating packages and checking your code still works:

```r
renv::snapshot()     # record the current package versions in renv.lock
```

5. On another computer, or after cloning a project:

```r
renv::restore()      # install exactly the versions in renv.lock
```

Commit `renv.lock` and `.Rprofile` to git. Don't commit `renv/library/` (the `.gitignore` template here excludes it).

### Helpful habits

- Run `renv::status()` if something seems out of sync.
- Update deliberately: `renv::update("ggplot2")`, test, then `renv::snapshot()`.
- Add `usethis::use_blank_slate()` once so R never saves or restores a hidden workspace.
- Use `here::here("data", "raw", "file.csv")` to build file paths from the project root.

## 3. Finding and judging packages

- **CRAN** is the main repository: packages are reviewed and tested. `install.packages()` uses it.
- **Bioconductor** is a repository of bioinformatics and genomics packages; install with `BiocManager::install("DESeq2")`.
- **GitHub** hosts development versions: `pak::pak("user/repo")`. Use sparingly; they are less stable.

Before relying on a package, check: Is it maintained (recent releases)? Is it documented (vignettes)? Is it widely used? Does it have a paper or a reputable author? An AI assistant may recommend a package that doesn't exist or is abandoned; always verify it on CRAN or GitHub.

## 4. Package guide

Install only what you need for the task at hand.

### The tidyverse: everyday data science

`install.packages("tidyverse")` installs a family of packages designed to work together. Core members:

| Package | Purpose |
|---|---|
| **dplyr** | Filter, select, mutate, group, summarize, and join data frames |
| **tidyr** | Reshape data between wide and long formats; handle missing values |
| **ggplot2** | Layered, publication-quality graphics (the grammar of graphics) |
| **readr** | Fast reading and writing of CSV and other text files |
| **tibble** | A modern data frame |
| **stringr** | String manipulation |
| **forcats** | Working with categorical variables (factors) |
| **lubridate** | Dates and times |
| **purrr** | Functional programming: apply a function across many things |

### Reproducible reporting and project tools

| Package | Purpose |
|---|---|
| **quarto** | Render Quarto documents from R |
| **knitr**, **rmarkdown** | The engines behind code-in-document reporting |
| **renv** | Project package management (above) |
| **here** | Project-relative file paths |
| **usethis** | Helpers for setting up projects, git, and GitHub from R |
| **targets** | Pipeline tool that reruns only what changed (advanced) |
| **gt**, **kableExtra** | Beautiful tables |

### Statistical modeling

| Package | Purpose |
|---|---|
| **stats** (built in) | `lm()`, `glm()`, `t.test()`, `nls()` and more; learn these first |
| **broom** | Turns model output into tidy data frames |
| **tidymodels** | A consistent framework for modeling and machine learning (parsnip, recipes, rsample, yardstick, tune) |
| **lme4**, **lmerTest** | Linear and generalized linear mixed-effects models |
| **nlme**, **mgcv** | Mixed models and generalized additive models (GAMs) |
| **emmeans** | Estimated marginal means and post-hoc comparisons |
| **car** | Regression diagnostics and ANOVA tools |
| **performance**, **parameters**, **easystats** | Model checking and reporting |
| **marginaleffects** | Interpreting model predictions and effects |
| **glmnet** | Lasso, ridge, and elastic net regression |

### Biostatistics

| Package | Purpose |
|---|---|
| **survival** | Core survival analysis: Kaplan–Meier, Cox proportional hazards |
| **survminer**, **ggsurvfit** | Survival plots |
| **cmprsk**, **tidycmprsk** | Competing risks |
| **gtsummary** | Publication-ready summary and regression tables ("Table 1") |
| **pwr**, **powerSurvEpi** | Power and sample-size calculation |
| **MASS** | Classic statistical functions and datasets |
| **meta**, **metafor** | Meta-analysis |
| **epiR**, **epitools** | Epidemiology tools |
| **mice** | Multiple imputation of missing data |
| **ggdist**, **patchwork** | Visualizing distributions; combining plots |

### Bayesian

| Package | Purpose |
|---|---|
| **brms** | Bayesian regression models in familiar formula syntax (uses Stan) |
| **rstanarm** | Pre-compiled Bayesian regression models |
| **rstan**, **cmdstanr** | Direct interfaces to Stan |
| **bayesplot**, **posterior**, **tidybayes** | Diagnostics, summaries, and plotting |

*Bayesian packages need a C++ compiler toolchain: Xcode tools on Mac, Rtools on Windows (installed by `rig`).*

### Bioconductor (genomics and high-dimensional data)

Install the installer first: `install.packages("BiocManager")`. Then for example:

| Package | Purpose |
|---|---|
| **Biobase**, **SummarizedExperiment** | Core data structures for omics data |
| **limma** | Linear models for microarray/RNA-seq expression |
| **DESeq2**, **edgeR** | Differential expression for RNA-seq counts |
| **GenomicRanges** | Intervals on genomes |
| **clusterProfiler** | Gene set / pathway enrichment |

### Neuroscience and signal processing

| Package | Purpose |
|---|---|
| **oro.nifti**, **neuroim2**, **RNifti** | Reading and manipulating NIfTI neuroimaging files |
| **fslr**, **ANTsR** | Interfaces to FSL and ANTs neuroimaging tools (ANTsR is typically installed from GitHub) |
| **eegUtils**, **eeguana** | EEG processing and visualization (check whether each is on CRAN or only GitHub before installing) |
| **signal**, **seewave**, **wavelets** | Filtering, spectral, and wavelet analysis |
| **STAR** | Spike-train analysis |
| **R.matlab** | Read MATLAB `.mat` files |
| **reticulate** | Call Python from R, and use Python objects in R. Lets one document mix both languages |

*Neuroscience package availability changes; verify each on CRAN/GitHub.*

(In neuroscience, Python is generally stronger for imaging and electrophysiology; R is generally stronger for statistical modeling of the resulting measurements. Learning to hand data from one to the other is a valuable skill.)

### Data import and formats

| Package | Purpose |
|---|---|
| **readxl**, **writexl** | Excel files |
| **haven** | SPSS, Stata, and SAS files |
| **arrow** | Parquet and large datasets |
| **DBI**, **RSQLite**, **duckdb** | Databases and SQL |
| **janitor** | Cleaning messy column names and tabulations |
| **data.table** | Very fast data manipulation (alternative to dplyr) |

### Testing, debugging, and code quality

| Package | Purpose |
|---|---|
| **testthat** | Automated tests |
| **styler**, **lintr** | Code formatting and style checks |
| **devtools** | Building and checking your own packages |
| **bench**, **profvis** | Measuring speed and finding bottlenecks |

## 5. Practice

1. Open a new folder `r-practice` in Positron, run `git init`, and `renv::init()`.
2. Install `tidyverse` and `broom`, then `renv::snapshot()`.
3. Write a `practice.qmd` that simulates a linear relationship with noise, fits `lm()`, tidies the output with `broom`, and plots with `ggplot2`. Render it.
4. Commit `renv.lock`, `.Rprofile`, and `practice.qmd`. Push to GitHub.
5. Delete `renv/library`, run `renv::restore()`, and confirm it still renders.
