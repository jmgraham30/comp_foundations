# Setup: Windows

*Draft last reviewed: November 2026. Tools change often; if a command fails, see [Troubleshooting](#troubleshooting).*

[← Back to README](../README.md) · Using a Mac? See the [Mac guide](01-setup-mac.md).

This guide installs everything you need, in a deliberate order. Each step explains **what the tool is** and **why we use it**, then gives the commands. Do the steps in order and complete the [final check](#10-verify-your-setup) at the end.

**Time estimate:** 2–4 hours, mostly waiting for downloads. **You need:** Windows 10 (recent) or Windows 11, an account that can install software, and a good internet connection. This guide uses native Windows (not WSL) to keep things simple.

## 0. Before you start: the terminal

**What it is.** A terminal is a program where you type commands instead of clicking. Many developer tools are installed and run this way. It will feel strange for a day or two, then become fast and natural.

**Open it:** press the Windows key, type `Terminal` (Windows 11) or `PowerShell` (Windows 10), press Enter. We use **PowerShell** throughout.

A few ground rules:

- Lines starting with `>` below are commands to type; **don't type the `>`**.
- Press Enter after each command and wait for the prompt to return before typing the next.
- After installing a tool, **close and reopen the terminal** so Windows picks up the new program.
- Do not use "Run as administrator" unless a step says to.

## 1. winget (package manager)

**What it is.** A *package manager* installs, updates, and removes software from the command line. `winget` ships with current versions of Windows 10/11. Using it means upgrading everything later takes one command.

```
> winget --version
```

If that prints a version, continue. If not, install **App Installer** from the Microsoft Store and try again.

## 2. git and GitHub

**What git is.** Git is *version control*: it records snapshots of your project so you can see what changed, undo mistakes, and try ideas on separate branches. **GitHub** is a website that stores git repositories online, giving you a backup, a way to share, and a portfolio.

**What the GitHub CLI is.** `gh` is a command-line tool that logs you in to GitHub securely. It is much simpler for beginners than setting up SSH keys by hand.

1. Create a free account at <https://github.com> (use a username you would be comfortable putting on a résumé).
2. Install git and `gh`, then **close and reopen the terminal**:

```
> winget install --id Git.Git -e
> winget install --id GitHub.cli -e
```

3. Tell git who you are (use the same email as your GitHub account, or the private "noreply" address in GitHub settings under *Emails*):

```
> git config --global user.name "Your Name"
> git config --global user.email "you@example.com"
> git config --global init.defaultBranch main
```

4. Log in to GitHub:

```
> gh auth login
```

Choose **GitHub.com**, **HTTPS**, say **yes** to authenticating git with your credentials, and log in with the web browser.

5. Check it worked:

```
> gh auth status
```

## 3. Folder for your work

Create the place your projects will live. Read [the file system guide](02-filesystem-and-projects.md) first if you haven't. The short version:

```
> mkdir C:\Users\$env:USERNAME\projects
```

**Do not** keep projects inside OneDrive (including the default `Documents` and `Desktop` on many Windows PCs), Dropbox, or iCloud. Syncing software conflicts with git and with package environments and can corrupt both. A folder directly in your user directory, like `C:\Users\<you>\projects`, is safe. Also avoid spaces in your folder names and, if possible, in your Windows username folder.

## 4. R, with rig

**What R is.** A programming language designed for statistics and data analysis. It is the lingua franca of biostatistics.

**What rig is.** The *R Installation Manager*. R releases a new minor version every year, and projects sometimes need an older one. `rig` lets you install several versions side by side and choose the default. On Windows it also installs the matching **Rtools**, the compiler toolchain needed to build some packages from source. It's the recommended way to install R, rather than downloading it from a website.

```
> winget install posit.rig
```

Close and reopen the terminal, then:

```
> rig add release
> rig list
```

The version marked with `*` is your default. To switch later: `rig default <version>`.

Test it:

```
> R --version
```

If R builds packages from source and complains about missing tools, run `rig system rtools list` to see Rtools status and `rig system rtools add` to install it.

### Install core R packages

Start R in the terminal by typing `R`, then run the following, and quit with `q()` (answer `n` when asked to save the workspace image):

```r
install.packages(c("tidyverse", "renv", "quarto", "rmarkdown", "knitr", "usethis", "here"))
```

(This takes several minutes. Packages are mostly installed as pre-built binaries on Windows.)

**What renv is.** `renv` gives each project its own private library of R packages plus a *lockfile* recording exact versions, so your analysis keeps working over time. You'll use it in every project; see the [R guide](04-r-packages.md).

## 5. Python, with uv

**What Python is.** A general-purpose language with an enormous scientific ecosystem (NumPy, pandas, SciPy, scikit-learn, and the neuroimaging libraries). It is standard in neuroscience and machine learning.

**What uv is.** A fast tool that installs Python versions, creates a **virtual environment** for each project (an isolated set of packages, so projects don't interfere with each other), installs packages, and records exact versions in a lockfile. It plays the combined role of rig and renv for Python.

> **Why not Anaconda/conda?** It is still common in labs, but it is heavier and a frequent source of confusing "which Python am I using?" problems for beginners. Most scientific Python packages install fine with `uv`. If you later need software that isn't a Python package (some bioinformatics tools, for example), look at [pixi](https://pixi.prefix.dev/latest/), which can manage both. See the [Python guide](03-python-environments.md).

```
> winget install --id astral-sh.uv -e
```

Close and reopen the terminal, then:

```
> uv --version
```

You do **not** install Python separately; `uv` does it for you per project. To install a specific Python and make it available now:

```
> uv python install 3.12
> uv python list --only-installed
```

> **Do not install Python from the Microsoft Store.** If typing `python` opens the Store, that is fine; we run Python through `uv run` instead.

## 6. Quarto

**What it is.** A publishing system that weaves together text, code (R, Python, and more), and the results of running that code in a single document, which can be rendered to HTML, PDF, Word, slides, or a website. All the tutorials here are Quarto documents (`.qmd`), and you will write your reports and your project in Quarto too.

```
> winget install --id Posit.Quarto -e
```

Close and reopen the terminal, then:

```
> quarto --version
> quarto check
```

`quarto check` reports whether Quarto can find R and Python. Don't worry if Python/Jupyter shows as missing yet; you'll set that up per project in the [Python guide](03-python-environments.md). (Positron also bundles its own copy of Quarto, but installing it yourself means you can use it from the terminal and control the version.)

## 7. Positron (and RStudio)

**What an IDE is.** An *integrated development environment* is an editor designed for programming. It combines a code editor, a console where code runs, a file browser, a plots pane, a data viewer, and git tools.

**Positron** is a newer IDE from Posit built specifically for data science in **both R and Python**, with first-class Quarto support. It is our primary editor.

```
> winget install --id Posit.Positron -e
```

**RStudio** is the long-established R-focused IDE. It's optional, but you will see it everywhere (in textbooks, lab computers, and online help), so it's worth installing:

```
> winget install --id Posit.RStudio -e
```

Open Positron from the Start menu. On first launch it should find your R (installed with rig) and Python. Use *File → Open Folder…* to open a project folder, and explore the **Console**, **Variables**, **Plots**, and **Terminal** panes.

## 8. Recommended settings

- In Positron, open Settings (`Ctrl+,`) and:
  - Turn on **auto-save** (`Files: Auto Save` → `afterDelay`).
  - Set **Editor: Rulers** to `80` as a gentle line-length guide.
- In File Explorer, choose *View → Show → File name extensions* and *Hidden items*. Windows hides file extensions by default, which causes a lot of confusion (for example `analysis.qmd.txt`).
- Always open a **project folder** (not individual loose files) in Positron. This is central to good habits; see the [file system guide](02-filesystem-and-projects.md).

## 9. Windows-specific notes

- **Paths.** Windows uses backslashes (`C:\Users\you\projects`), but R and Python both accept forward slashes (`C:/Users/you/projects`). In code, prefer forward slashes or, better, *relative* paths inside your project.
- **Line endings.** Windows and Mac/Linux end lines differently. Git handles this; if you see warnings about "LF will be replaced by CRLF", they are harmless.
- **Antivirus and long paths.** Very deep folder nesting can hit Windows' path-length limit; one more reason to keep project folders shallow and to keep them close to the top of your user directory.
- **Windows on ARM.** Most tools work, but some Python packages lack ARM builds. If a package won't install, tell your mentor which one.

## 10. Verify your setup

Create a throwaway test project and render a Quarto file.

```
> cd C:\Users\$env:USERNAME\projects
> mkdir setup-check
> cd setup-check
> uv init --bare
> uv add jupyter pandas matplotlib
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
> uv run quarto render check.qmd
```

A `check.html` file should appear. Next, confirm R packages load:

```
> Rscript -e "library(tidyverse); library(renv); sessionInfo()"
```

Finally confirm git and GitHub:

```
> git --version
> gh auth status
```

If all of these work, **you're done**. Email your mentor the output of `quarto check`, `rig list`, `uv --version`, and `gh auth status`. Delete the `setup-check` folder when finished.

## Troubleshooting

- **"is not recognized as the name of a cmdlet" (for rig, uv, quarto, git, gh).** Close and reopen the terminal. If it persists, the program didn't install; re-run its `winget install` command and read the output.
- **A `winget` ID is not found.** Package names change. Run `winget search <name>` to find the current ID and tell your mentor so this guide can be updated.
- **"running scripts is disabled on this system".** Don't change security policy on your own; ask your mentor.
- **A package fails to install with compiler errors.** Run `rig system rtools list`; make sure Rtools is installed for your R version. Copy the *first* error, not the last, when asking for help.
- **Keeping things up to date** (do this a few times per semester):
  ```
  > winget upgrade --all
  > rig add release     # when a new R version is released
  ```

## Where to go next

Read [Your computer's file system and how to organize projects](02-filesystem-and-projects.md).
