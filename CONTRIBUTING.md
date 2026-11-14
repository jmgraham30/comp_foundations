# Contributing and maintaining

## Previewing the site locally

Install Quarto, then from the repository root:

```
quarto preview
```

## Adding a tutorial

1. Create `tutorials/NN-topic-name.qmd` (zero-padded number, lowercase, hyphens).
2. Include learning goals, worked examples, exercises, and a short "check your understanding" section.
3. Link it from the topic table in `README.md`, and add it to the sidebar in `_quarto.yml` if desired.
4. If the tutorial needs packages, record them with `uv add` / `renv::snapshot()` in a project-level environment.

## Reporting problems

Open an issue at <https://github.com/jmgraham30/comp_foundations/issues> with your operating system, the step, and the exact error message.

## Keeping setup guides current

Each setup guide has a "last reviewed" date. When you verify the instructions on a new OS version or tool release, update it.
