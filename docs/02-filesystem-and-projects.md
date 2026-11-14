# Your Computer's File System and How to Organize Projects

[← Back to README](../README.md)

Most "coding problems" for beginners are really *file* problems: a script can't find its data, a result got saved somewhere unexpected, or three slightly different copies of the same file exist in three folders. This guide gives you the mental model and habits to avoid those problems. It is worth reading slowly.

## 1. How a file system works

Your computer stores files in a **tree of folders** (also called *directories*). Every file has exactly one location in the tree, described by its **path**.

```
/Users/jane/projects/eeg-study/data/raw/subject01.csv      (macOS)
C:/Users/jane/projects/eeg-study/data/raw/subject01.csv    (Windows)
```

Key ideas:

- **Root.** The top of the tree (`/` on Mac; a drive letter such as `C:` on Windows).
- **Home directory.** Your personal area (`/Users/jane` on Mac, `C:\Users\jane` on Windows). Abbreviated `~` in the terminal on Mac. Your own files should live here.
- **Absolute path.** A path starting from the root. It works from anywhere, but it breaks the moment the project moves to another computer.
- **Relative path.** A path starting from the *current working directory*, such as `data/raw/subject01.csv`. It keeps working when the whole project folder is moved or shared. **Prefer relative paths.**
- **Working directory.** The folder a program considers "here". If your code can't find a file, the working directory is the first thing to check (`getwd()` in R; `import os; os.getcwd()` in Python; `pwd` in the terminal).
- **Extensions.** `.csv`, `.R`, `.py`, `.qmd` tell programs what kind of file it is. Make sure your computer shows them (see your setup guide).

### Terminal navigation cheat sheet

| Task | macOS (Terminal) | Windows (PowerShell) |
|---|---|---|
| Where am I? | `pwd` | `pwd` |
| List files | `ls -la` | `ls` |
| Change folder | `cd folder` | `cd folder` |
| Up one level | `cd ..` | `cd ..` |
| Go home | `cd ~` | `cd ~` |
| Make a folder | `mkdir name` | `mkdir name` |
| Copy / move | `cp`, `mv` | `cp`, `mv` |

## 2. The one-project, one-folder rule

**Everything that belongs to a project lives inside one folder** (and its subfolders): data, code, documents, figures, and notes. That folder is also the git repository and the folder you open in Positron or RStudio.

Bad pattern (this is the most common one):

```
Desktop/analysis.R
Downloads/data (1).csv
Documents/homework/final_FINAL_v2.docx
Documents/stuff/plot.png
```

Good pattern:

```
projects/
└── eeg-study/
    ├── README.md
    ├── eeg-study.Rproj        (or pyproject.toml)
    ├── data/
    │   ├── raw/               # original data: never edited
    │   └── processed/         # cleaned data produced by code
    ├── R/  or  src/           # reusable functions
    ├── analysis/              # Quarto documents and scripts
    ├── output/
    │   ├── figures/
    │   └── tables/
    └── docs/                  # reference materials, notes
```

Principles behind this layout:

1. **Raw data are read-only.** Never edit the original files. Write code that reads `data/raw/` and writes cleaned data to `data/processed/`. Then anyone (including future you) can rerun the whole thing.
2. **Generated things are disposable.** Anything in `processed/` or `output/` can be recreated by running the code. If you can't recreate it, something is missing from your code.
3. **Don't nest too deeply.** Two or three levels below the project root is usually enough. Deep nesting makes paths long and hard to navigate.
4. **Don't scatter.** If you find yourself saying "the data are in my Downloads folder", move them into the project.

## 3. Where should projects live?

- Make **one parent folder for all projects**, such as `~/projects` (Mac) or `C:\Users\<you>\projects` (Windows).
- **Do not put projects in cloud-synced folders** (OneDrive, iCloud Drive, Dropbox, Google Drive). Sync tools rewrite and lock files in the background, which corrupts git repositories and package environments. Git and GitHub are your backup and sync system.
- **Don't use the Desktop or Downloads** for project work. They fill up with clutter and are often synced.
- Keep your **course folders** (`comp_foundations`) and **your own project folders** all as children of the same parent.

## 4. Naming files and folders

Good names are short, descriptive, and boring.

**Rules**

- **No spaces.** Use hyphens or underscores: `eeg-study`, `clean_data.R`. Spaces force quoting and break many tools.
- **Lowercase** by default. `Data.csv` and `data.csv` are different files on some systems and the same on others.
- **Only letters, numbers, `-`, and `_`.** Avoid `& % # ( ) é ñ`, emoji, and quotes.
- **Be consistent.** Pick hyphens or underscores and stick with it.
- **Use ISO dates, year first:** `2027-02-15`, so names sort chronologically.
- **Zero-pad numbers** so they sort correctly: `01_import.R`, `02_clean.R`, `10_model.R`, not `1_`, `2_`, `10_`.
- **Make names say what the thing is**, not how you feel about it. Avoid `final`, `final2`, `newest`, `stuff`, `untitled`. Git tracks versions for you, so you never need `analysis_v3_FINAL.R`.

| Instead of | Use |
|---|---|
| `My Analysis (copy).R` | `01_import-data.R` |
| `Figure 1 - final!!.png` | `fig01_survival-curves.png` |
| `data (1).csv` | `patients_2027-01-20.csv` |
| `HW 3.qmd` | `hw03.qmd` |

A fuller treatment is Jenny Bryan's slides, *Naming Things*, and it's worth a read.

## 5. Always start from a project folder

In **Positron** use *File → Open Folder…* and choose the project folder. In **RStudio** use *File → New Project…* (it creates a `.Rproj` file). Doing this sets the working directory to the project root, so relative paths behave predictably.

Two habits that prevent whole classes of bugs:

- **Never use `setwd()`** in R scripts, and never hard-code `C:/Users/you/...` or `/Users/you/...` in code. Use relative paths or the `here` package (R: `here::here("data", "raw", "file.csv")`), or `pathlib` in Python (`Path("data") / "raw" / "file.csv"`).
- **Don't save your workspace.** Configure RStudio/Positron to never save or restore `.RData`. Your code should be able to rebuild everything. (In R: `usethis::use_blank_slate()`.)

## 6. Every project gets a README

A `README.md` at the project root should answer, in a few lines:

1. What is this project?
2. How do I set up the environment? (e.g., `renv::restore()`, `uv sync`)
3. How do I run it? In what order?
4. Where does the data come from?

Writing it for a stranger is the same as writing it for yourself six months from now.

## 7. What goes into git and what doesn't

**Commit:** code, Quarto documents, small data files that cannot be regenerated, README, lockfiles (`renv.lock`, `uv.lock`), configuration.

**Don't commit** (list these in a `.gitignore` file):

- Large or sensitive data (**never commit human-subjects data or anything with identifiers**)
- Virtual environments (`.venv/`) and R library folders (`renv/library/`)
- Generated output you can rebuild (`*.html` from rendering, `_freeze/` if you wish, caches)
- Operating-system clutter (`.DS_Store`, `Thumbs.db`)
- Passwords, tokens, and API keys

## 8. Backups and good hygiene

- **Commit early and often** with messages that say *why*: "Handle missing values in age column", not "update".
- **Push to GitHub at least at the end of every work session.** Your laptop can be lost, spilled on, or stolen; GitHub copies survive.
- **One-time habit:** keep data you can't regenerate in a second place too (a university-approved storage service).
- **Tidy regularly.** Delete dead scratch files. If you're unsure whether a file matters, it probably belongs in `docs/` or nowhere.

## 9. A quick self-test

1. Where is the working directory when you run your code, and how do you check?
2. Could you zip your project folder, email it to a classmate, and have it run? If not, what's missing or hard-coded?
3. Does any file in the project have a space or `final` in its name?
4. Is there anything in the project you can't regenerate from the raw data and the code?

## Where to go next

- [Python projects and environments](03-python-environments.md)
- [R projects and packages](04-r-packages.md)
