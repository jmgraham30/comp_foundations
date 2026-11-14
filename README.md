# Computational Foundations for Biostatistics and Neuroscience

Self-guided tutorials for learning the computing and coding skills that matter most for graduate study in **biostatistics, neuroscience, and related fields**, using **R**, **Python**, and **Quarto**.

Originally developed for an Honors Tutorial in the Department of Mathematics at the University of Scranton, and written so that any student with similar goals can work through it.

> **Status:** under active development. Setup guides and orientation documents come first; tutorial modules are added over time (see [Topics](#topics)).

---

## Who this is for

Undergraduates (typically in mathematics, statistics, neuroscience, biology, or computer science) who expect to do quantitative research or graduate work and want a solid, practical grounding in scientific and statistical computing. No prior programming experience is assumed, but curiosity and patience are.

## How to use this repository

1. **Do the [Setup](#setup)** (this can take several hours; do it before the term begins and ask for help early).
2. **Read the [orientation guides](#orientation-guides)** on your computer's file system and on organizing projects. These habits matter more than any single package.
3. **Work through the [Topics](#topics) in order**, using the Quarto tutorials. Each tutorial is meant to be *run and edited*, not just read: type the code yourself, change it, break it, and fix it.
4. **Keep a learning log** and build your **semester project** as you go (see [Project and expectations](#project-and-expectations)).

## Setup

Choose the guide for your computer. Both guides install the same toolkit in the same order and explain what each tool is and why we use it.

| Your computer | Guide |
|---|---|
| macOS | [Mac setup](docs/01-setup-mac.md) |
| Windows 10/11 | [Windows setup](docs/01-setup-windows.md) |

### The toolkit at a glance

| Tool | What it is | Why we use it |
|---|---|---|
| **Terminal** | A text interface for giving your computer commands | Many tools below are driven from it; it is also how you work on remote servers |
| **Homebrew** (Mac) / **winget** (Windows) | A *package manager* for your operating system | Install, update, and remove software with one command instead of hunting for installers |
| **git** | Version control: a complete history of every change to your files | Lets you undo mistakes, work on ideas safely, and share work |
| **GitHub** and **GitHub CLI (`gh`)** | A website that hosts git repositories, and a command-line tool to log in to it | Backup, collaboration, and a portfolio of your work |
| **rig** | The *R Installation Manager* | Installs and switches between R versions without breaking things |
| **R** | A language built for statistics and data analysis | The standard in biostatistics |
| **renv** | An R package that gives each project its own package library and a lockfile | Reproducibility: your project works the same next year, or on someone else's computer |
| **uv** | A fast manager for Python versions, virtual environments, and packages | Does for Python what rig and renv do together for R |
| **Python** | A general-purpose language with a rich scientific ecosystem | Standard in neuroscience, machine learning, and many data pipelines |
| **Quarto** | A publishing system that combines text, code, and results in one document | Every tutorial here is a Quarto document, and you will write your reports with it |
| **Positron** | An IDE (integrated development environment) for data science in R *and* Python | Our primary editor |
| **RStudio** | A long-standing IDE focused on R | Optional alternative; widely used, so you should recognize it |

Two other tools you will hear about are covered as short side notes in the setup guides rather than installed on day one:
**pixi** (an alternative environment manager that also handles non-Python software, useful for some bioinformatics tools) and **conda/Miniconda** (older, still common in labs).

## Orientation guides

Read these early and return to them often.

- [Your computer's file system and how to organize projects](docs/02-filesystem-and-projects.md): where files live, naming rules, project folder structure, and habits that prevent lost work.
- [Python projects and environments](docs/03-python-environments.md): creating a project with its own environment and installing the scientific Python stack with `uv`.
- [R projects and packages](docs/04-r-packages.md): working with R projects and `renv`, plus a guide to the most useful R packages for statistics and neuroscience.
- [Using AI responsibly as a coding assistant](docs/05-using-ai-responsibly.md): how to use AI tools to learn faster instead of learning less.

## Topics

This is the planned syllabus. Items link to tutorials as they are written; unlinked topics are planned. **Core** topics are for everyone; **Elective** topics are chosen with your mentor based on your interests.

| # | Topic | Track | Tutorial |
|---|---|---|---|
| 1 | The command line and the file system | Core | *planned* |
| 2 | Git and GitHub workflow | Core | *planned* |
| 3 | R fundamentals | Core | *planned* |
| 4 | Python fundamentals | Core | *planned* |
| 5 | Data wrangling with tidyverse (R) and pandas/polars (Python) | Core | *planned* |
| 6 | Data visualization: ggplot2 (R) and matplotlib/seaborn (Python) | Core | *planned* |
| 7 | Reproducible reporting with Quarto | Core | *planned* |
| 8 | Writing functions, debugging, and testing | Core | *planned* |
| 9 | Responsible use of AI coding assistants (revisited throughout) | Core | [guide](docs/05-using-ai-responsibly.md), tutorial *planned* |
| 10 | Statistical computing: simulation, bootstrap, permutation tests | Core | *planned* |
| 11 | Regression and generalized linear models in R and Python | Core | *planned* |
| 12 | Data formats and file I/O (CSV, Parquet, HDF5) | Core | *planned* |
| 13 | Data ethics, privacy, and working with human-subjects data | Core | *planned* |
| 14 | Survival analysis and clinical-trial data | Elective (biostatistics) | *planned* |
| 15 | Mixed-effects models | Elective (biostatistics) | *planned* |
| 16 | Bioconductor and genomic data | Elective (biostatistics) | *planned* |
| 17 | Time series and signal processing; EEG/MEG with MNE-Python | Elective (neuroscience) | *planned* |
| 18 | Neuroimaging data with nibabel and nilearn | Elective (neuroscience) | *planned* |
| 19 | Introductory machine learning with scikit-learn and tidymodels | Elective | *planned* |
| 20 | Bayesian computation (brms/Stan, PyMC) | Elective | *planned* |
| 21 | SQL basics; working on remote servers and HPC clusters | Elective | *planned* |

*Topics will be reordered and refined as the materials are written.*

## Project and expectations

For an Honors Tutorial (or any extended use of these materials) we suggest:

- **A learning log**: a short dated entry each week on what you did, what confused you, and what you would do differently.
- **A semester project**: analyze a public dataset (for example a clinical or cohort dataset, or an open EEG/fMRI dataset), developing it as you complete each module. Keep it in its own git repository on GitHub.
- **A weekly meeting** with your mentor to discuss your code and decisions, not just your results.
- **A final reflection or short presentation** of the project.

Details are set by the mentor for each student.

## Repository layout

```
comp_foundations/
├── README.md             # this file
├── docs/                 # setup and orientation guides (Markdown)
├── tutorials/            # Quarto tutorials (.qmd)
├── _quarto.yml           # Quarto website configuration
└── .github/workflows/    # publishes the website to GitHub Pages
```

The same materials can be rendered as a website with `quarto preview` (see [Contributing](CONTRIBUTING.md)).

## Getting help and reporting problems

Tools change quickly, and an instruction that worked last term may be out of date. If a step fails, first read the error message carefully, then ask your mentor. Please also [open an issue](https://github.com/jmgraham30/comp_foundations/issues) describing your operating system, the step, and the exact error so the guide can be fixed for the next student.

## License

Text and tutorials are licensed under [CC BY 4.0](LICENSE.md); code is under the MIT License. See [LICENSE.md](LICENSE.md).
