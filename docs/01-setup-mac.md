# Setup: macOS

*Draft last reviewed: November 2026. Tools change often; if a command fails, see [Troubleshooting](#troubleshooting).*

[← Back to README](../README.md) · Using Windows? See the [Windows guide](01-setup-windows.md).

This guide installs everything you need, in a deliberate order. Each step explains **what the tool is** and **why we use it**, then gives the commands. Do the steps in order and complete the [final check](#10-verify-your-setup) at the end.

**Time estimate:** 2–4 hours, mostly waiting for downloads. **You need:** a Mac running a reasonably recent macOS, an administrator account, and a good internet connection.

## 0. Before you start: the Terminal

**What it is.** The Terminal is a program where you type commands instead of clicking. Many developer tools are installed and run this way. It will feel strange for a day or two, then become fast and natural.

**Open it:** press `⌘ Space`, type `Terminal`, press Return.

A few ground rules:

- Lines starting with `$` below are commands to type; **don't type the `$`**.
- Press Return after each command and wait for the prompt to come back before typing the next.
- Copy-paste is fine, but read each command first so you know what it does.
- When asked for a password, nothing appears as you type. That is normal.

## 1. Xcode Command Line Tools

**What it is.** A bundle of basic developer tools from Apple, including a compiler. Many R and Python packages are built from source on your machine and need it. It also supplies `git`.

```
$ xcode-select --install
```

Click **Install** in the dialog and wait. If it says the tools are already installed, continue.

## 2. Homebrew (package manager)

**What it is.** A *package manager* installs, updates, and removes software from the command line. Homebrew is the standard one for macOS. Using it means upgrading everything later takes one command.

Go to <https://brew.sh> and copy the install command shown on the home page into the Terminal. **Follow the "Next steps" Homebrew prints at the end** (they add Homebrew to your shell's path). Then close and reopen the Terminal and check:

```
$ brew --version
```

## 3. git and GitHub

**What git is.** Git is *version control*: it records snapshots of your project so you can see what changed, undo mistakes, and try ideas on separate branches. **GitHub** is a website that stores git repositories online, giving you a backup, a way to share, and a portfolio.

**What the GitHub CLI is.** `gh` is a command-line tool that logs you in to GitHub securely. It is much simpler for beginners than setting up SSH keys by hand.

1. Create a free account at <https://github.com> (use a username you would be comfortable putting on a résumé; consider using your school email so you can claim student benefits).
2. Install git and `gh`:

```
$ brew install git gh
```

3. Tell git who you are (use the same email as your GitHub account, or the private "noreply" address in GitHub settings under *Emails*):

```
$ git config --global user.name "Your Name"
$ git config --global user.email "you@example.com"
$ git config --global init.defaultBranch main
```

4. Log in to GitHub:

```
$ gh auth login
```

Choose **GitHub.com**, **HTTPS**, say **yes** to authenticating git with your credentials, and log in with the web browser.

5. Check it worked:

```
$ gh auth status
```

## 4. Folder for your work

Before installing more, create the place your projects will live. Read [the file system guide](02-filesystem-and-projects.md) first if you haven't; the short version:

```
$ mkdir -p ~/Documents/projects
```

**Do not** keep projects inside iCloud Drive, Dropbox, or OneDrive folders. Syncing software conflicts with git and with package environments and can corrupt both. (On many Macs, `Desktop` and `Documents` are synced to iCloud by default. Check *System Settings → Apple ID → iCloud → iCloud Drive* and, if "Desktop & Documents Folders" is on, create `~/projects` instead and use that everywhere you see `~/Documents/projects`.)

## 5. R, with rig

**What R is.** A programming language designed for statistics and data analysis. It is the lingua franca of biostatistics.

**What rig is.** The *R Installation Manager*. R releases a new minor version every year, and projects sometimes need an older one. `rig` lets you install several versions side by side and choose which one is the default. It's the recommended way to install R, rather than downloading it from a website.

```
$ brew tap r-lib/rig
$ brew install --cask rig
```

Install the current R release and look at what you have:

```
$ rig add release
$ rig list
```

The version marked with `*` is your default. To switch later: `rig default <version>`.

Test it:

```
$ R --version
```

### Install core R packages

Start R in the Terminal by typing `R`, then run the following, and quit with `q()` (answer `n` when asked to save the workspace image):

```r
install.packages(c("tidyverse", "renv", "quarto", "rmarkdown", "knitr", "usethis", "here"))
```

(This takes several minutes. Packages are mostly installed as pre-built binaries on macOS.)

**What renv is.** `renv` gives each project its own private library of R packages plus a *lockfile* recording exact versions, so your analysis keeps working over time. You'll use it in every project; see the [R guide](04-r-packages.md).

## 6. Python, with uv

**What Python is.** A general-purpose language with an enormous scientific ecosystem (NumPy, pandas, SciPy, scikit-learn, and the neuroimaging libraries). It is standard in neuroscience and machine learning.

**What uv is.** A fast tool that installs Python versions, creates a **virtual environment** for each project (an isolated set of packages, so projects don't interfere with each other), installs packages, and records exact versions in a lockfile. It plays the combined role of rig and renv for Python.

> **Why not Anaconda/conda?** It is still common in labs, but it is heavier and a frequent source of confusing "which Python am I using?" problems for beginners. Most scientific Python packages install fine with `uv`. If you later need software that isn't a Python package (some bioinformatics tools, for example), look at [pixi](https://pixi.prefix.dev/latest/), which can manage both. See the [Python guide](03-python-environments.md).

```
$ brew install uv
$ uv --version
```

You do **not** install Python separately; `uv` does it for you per project. To install a specific Python and make it available now:

```
$ uv python install 3.12
$ uv python list --only-installed
```

## 7. Quarto

**What it is.** A publishing system that weaves together text, code (R, Python, and more), and the results of running that code in a single document, which can be rendered to HTML, PDF, Word, slides, or a website. All the tutorials here are Quarto documents (`.qmd`), and you will write your reports and your project in Quarto too.

```
$ brew install --cask quarto
$ quarto --version
$ quarto check
```

`quarto check` reports whether Quarto can find R and Python. Don't worry if Python/Jupyter shows as missing yet; you'll set that up per project in the [Python guide](03-python-environments.md). (Positron also bundles its own copy of Quarto, but installing it yourself means you can use it from the Terminal and control the version.)

## 8. Positron (and RStudio)

**What an IDE is.** An *integrated development environment* is an editor designed for programming. It combines a code editor, a console where code runs, a file browser, a plots pane, a data viewer, and git tools.

**Positron** is a newer IDE from Posit built specifically for data science in **both R and Python**, with first-class Quarto support. It is our primary editor.

```
$ brew install --cask positron
```

**RStudio** is the long-established R-focused IDE. It's optional, but you will see it everywhere (in textbooks, lab computers, and online help), so it's worth installing:

```
$ brew install --cask rstudio
```

Open Positron from the Applications folder. On first launch it should find your R (installed with rig) and Python. Use *File → Open Folder…* to open a project folder, and explore the **Console**, **Variables**, **Plots**, and **Terminal** panes.

## 9. Recommended Positron settings

- Open the Command Palette (`⇧⌘P`) → "Preferences: Open Settings" and search for these:
  - Turn on **auto-save** (`Files: Auto Save` → `afterDelay`).
  - Set **Editor: Rulers** to `80` as a gentle line-length guide.
- Make hidden files visible in Finder with `⇧⌘.` (period); you'll need this for `.gitignore` and `.venv`.
- Always open a **project folder** (not individual loose files) in Positron. This is central to good habits; see the [file system guide](02-filesystem-and-projects.md).

## 10. Verify your setup

Create a throwaway test project and render a Quarto file in each language.

```
$ cd ~/Documents/projects
$ mkdir setup-check && cd setup-check
$ uv init --bare
$ uv add jupyter pandas matplotlib
```

Create a file `check.qmd` with the following contents (in Positron: *File → New File*; or ask your mentor for a copy):

````
---
title: "Setup check"
format: html
jupyter: python3
---

## Python

```{python}
import sys, pandas, matplotlib
print(sys.version)
print("pandas", pandas.__version__)
```
````

Then render it:

```
$ uv run quarto render check.qmd
```

A `check.html` file should appear and open in a browser. Next, in R, confirm packages load:

```
$ Rscript -e 'library(tidyverse); library(renv); sessionInfo()'
```

Finally confirm git and GitHub:

```
$ git --version
$ gh auth status
```

If all of these work, **you're done**. Email your mentor the output of `quarto check`, `rig list`, `uv --version`, and `gh auth status`. Delete the `setup-check` folder when finished.

## Troubleshooting

- **"command not found: brew" (or rig, uv, quarto).** Close and reopen the Terminal. If it persists, the installer's "next steps" (path setup) weren't completed; re-run that part.
- **A package fails to install with compiler errors.** Confirm Step 1 finished (`xcode-select -p` should print a path). Copy the *first* error, not the last, when asking for help.
- **Apple silicon vs. Intel.** Both work; `rig` and `uv` choose the right builds automatically. Run `uname -m` (`arm64` or `x86_64`) if someone asks.
- **Permission denied errors.** Do not fix these by running commands with `sudo` unless an official instruction says to. Ask first.
- **Keeping things up to date** (do this a few times per semester):
  ```
  $ brew update && brew upgrade
  $ rig add release     # when a new R version is released
  ```

## Where to go next

Read [Your computer's file system and how to organize projects](02-filesystem-and-projects.md).
