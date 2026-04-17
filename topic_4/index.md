---
title: Topic 4 - LaTeX
layout: default
nav_order: 4
---

# Topic 4: LaTeX

## Overview

So far we have downloaded data and versioned our code. Now we need a way to write up the results with **LaTeX**.

Some of you may have never used LaTeX. That is fine: we start from zero.

- **0.** Install: TeX distribution and the VS Code extension.
- **1. Sample 1:** a first LaTeX document (`sample1_first.tex`). Compile it, see the files it produces, compare to Word, and discuss Overleaf.
- **2. Sample 2:** homework template from a PDF (`sample_problem_set_source.pdf` → `sample_problem_set_converted.tex`). Hand the agent a problem-set PDF and ask for a typeset solutions document.
- **3. Sample 3:** handwritten notes to slides (`sample_notes.pdf` → `sample3_handwritten.tex`). Turn a phone photo of handwritten notes into a beamer deck.
- **4. Sample 4:** empirical analysis. Build a pipeline that downloads yields from FRED, runs the slope regression, and produces figures and tables that `\input` straight into a LaTeX report.
- **5. Sample 5:** adding references. Take Sample 4's output and layer in biblatex, cleveref, and hyperref for a citation-complete, clickable PDF.

All sample sources live in [`topic_4/assets/`]({{ site.baseurl }}/topic_4/assets/), alongside the two input PDFs that samples 2 and 3 are built from, so you can download any of them and compare.

---

## 0. Install LaTeX and the VS Code Extension

Download LaTeX. The distribution is big and may take a while.

### TeX distribution (the compiler)

**macOS**:

```bash
brew install --cask mactex-no-gui
```

**Windows**: install [MiKTeX](https://miktex.org/download) or [TeX Live](https://www.tug.org/texlive/).

**If you're short on time or disk**: [TinyTeX](https://yihui.org/tinytex/) is a lightweight alternative that fetches missing packages on demand. It isn't on Homebrew; install it with:

```bash
curl -sL "https://yihui.org/tinytex/install-bin-unixoid.sh" | sh
```

### VS Code extension

1. Open Extensions (`Cmd+Shift+X` / `Ctrl+Shift+X`).
2. Search "LaTeX Workshop" (by James Yu).
3. Click Install.

---

## 1. Sample 1: A First LaTeX Document

Create `sample1_first.tex`:

```latex
\documentclass{article}

\usepackage{amsmath, amssymb}

% Macro: shorthand for the risk-neutral measure.
\newcommand{\Q}{\mathbb{Q}}

\title{My First LaTeX Document}
\author{Your Name}
\date{\today}

\begin{document}
\maketitle

\section{Introduction}
This document demonstrates the basic LaTeX workflow. In later sections of the course we will produce data and figures from a data pipeline, as well as adding references from a \verb|.bib| file.

\section{Math: Inline and Displayed}
Hello, world. The Pythagorean theorem says
\begin{equation}
  a^2 + b^2 = c^2.
\end{equation}
We can also write math inline: a right triangle with legs $a = 3$ and $b = 4$ has hypotenuse $c = 5$.

\section{A Sliver of Asset Pricing}
Under the risk-neutral measure $\Q$, the price of a payoff $X_T$ at date $T$ is
\begin{equation}
  P_0 = E^{\Q}\left[ e^{-\int_0^T r_s\,ds} \, X_T \right].
\end{equation}
The macro \verb|\Q| expands to $\Q$ --- every reference to the risk-neutral measure uses the same symbol, and if we ever change notation we change it in one place.
\end{document}
```

LaTeX is a markup language: a `.tex` source file is compiled into a typeset `.pdf` (or DVI, PostScript, HTML). Every document has a **preamble** (everything before `\begin{document}`: document class, packages, macros, title) and a **body** inside `\begin{document} ... \end{document}`. Commands start with a backslash (`\section{Introduction}`, `\title{...}`), and content blocks use **environments** delimited by `\begin{name}` and `\end{name}` (for example `equation`, `itemize`, `figure`, and `document` itself).

Two things to notice:

- **Inline vs. displayed math**. `$...$` sets an equation inline with the prose; `\begin{equation}...\end{equation}` puts it on its own numbered line. Use inline for short asides, display for anything you want to refer back to.
- **The `\newcommand{\Q}{\mathbb{Q}}` macro**. Defined once in the preamble, used anywhere in the document. This matters more than it looks: serious LaTeX documents define macros for every recurring symbol (`\Q`, `\P`, `\Ex`, `\Var`, `\Cov`, `\R`, ...) so that notation stays consistent and is trivially changed. Agents are happy to add more as you need them.

**Compiling LaTeX files into PDF.** In VS Code, LaTeX Workshop auto-compiles on save (watch the bottom-left status bar); the preview opens in a side pane with `Ctrl+Alt+V` / `Cmd+Option+V`. On the command line, `pdflatex sample1_first.tex` runs one pass. For anything non-trivial, prefer:

```bash
latexmk -pdf sample1_first.tex
```

`latexmk` re-runs `pdflatex` (and other commands) as many times as required to resolve cross-references, bibliographies, and tables of contents, and only rebuilds when something actually changed. It's what you want as soon as a document has dependencies like `\input{...}` fragments or a `.bib` file.

### What files get produced?

After one compile, the directory looks like:

```text
.
├── sample1_first.tex        # your source
├── sample1_first.pdf        # final output
├── sample1_first.aux        # cross-reference data
├── sample1_first.log        # full compiler log (debug here)
├── sample1_first.synctex.gz # source ↔ PDF position mapping
└── sample1_first.out        # hyperref bookmark cache
```

| File          | What it is                                         | When to look at it                    |
| ------------- | -------------------------------------------------- | ------------------------------------- |
| `.pdf`        | the rendered document                              | always                                |
| `.log`        | verbose compiler output                            | when compilation fails                |
| `.aux`        | labels, refs, citations recorded for the next pass | rarely                                |
| `.synctex.gz` | lets you Cmd-click between `.tex` and `.pdf`       | rarely (but it's why Cmd-click works) |
| `.out`        | bookmarks for PDF viewers                          | never                                 |

Only `sample1_first.tex` is your input. Everything else is regenerated on each compile — add `*.aux`, `*.log`, `*.synctex.gz`, `*.out` to `.gitignore` and only commit `.tex` and final `.pdf`.

### Why not just use Word?

|                             | LaTeX                                   | Word                          |
| --------------------------- | --------------------------------------- | ----------------------------- |
| Math typesetting            | Publication-grade, effortless           | Clunky, not camera-ready      |
| Version control             | Plain text → git-friendly diffs         | Binary files → git-hostile    |
| Cross-references, citations | Automatic via `\ref`, `\cite` + BibTeX  | Manual, breaks on renumbering |
| Tables/figures from code    | `\input{table.tex}`, `\includegraphics` | Copy-paste, manual re-export  |
| Template consistency        | One `.sty` file rules them all          | Per-doc formatting drift      |
| Learning curve              | Steep                                   | Shallow                       |
| Collaboration               | Git or Overleaf                         | Tracked changes               |

For economics and finance the cost-benefit is settled: **LaTeX**. Journals expect it, referees expect it, and critically figures and tables generated by Python slot in as `\input{}` fragments that stay in sync with the underlying data every time you re-run the pipeline. No copy-paste.

### Overleaf

[Overleaf](https://www.overleaf.com) is a browser-based LaTeX editor with a shared cloud compiler. Worth knowing about.

**When Overleaf shines**:

- Collaborating with a co-author who does not want to install anything.
- Quick one-off documents (letters, short notes).

**When it gets in the way**:

- **Git integration** is awkward. A paid tier is required for native git access, and even then multi-file workflows lag behind local git.
- **Multi-output projects** (a paper that shares figures with a slide deck and an appendix) are harder, because Overleaf hides the compile chain.
- **Debugging** is harder when the build steps are abstracted away. Creates a weaker mental model of what LaTeX actually does.
- **Automated workflow** can be harder with Overleaf.

---

## 2. Sample 2: Homework Template from a PDF

The agent can draft a typeset problem-set document for us, complete with solution boxes where we can fill in our answers. The key point: **we feed it the PDF, not any source `.tex`.** That mirrors the realistic workflow — you usually only have the problem-set PDF — and shows off multimodal input.

### Get the problem set

Download the source PDF: [sample_problem_set_source.pdf]({{ site.baseurl }}/topic_4/assets/sample2_homework/sample_problem_set_source.pdf). It's a four-problem math and statistics review (derivatives, Gordon growth partial derivatives, expectation/volatility, normal distribution).

Drop it into a fresh project directory.

### Prompt the agent

```text
I've placed sample_problem_set_source.pdf in this directory. Please
create sample_problem_set_converted.tex that reproduces the problem set
and fills in worked solutions. Requirements:

- Use the article documentclass.
- For each problem, reproduce the problem statement verbatim.
- After each question part (a), (b), ... put a tcolorbox with a bold
  "Solution:" lead-in (matching the boxes in the original PDF) and fill
  it with a worked solution.
- Use amsmath for equations.
- Make sure it compiles standalone (include a minimal preamble).
```

### What to expect

What the agent produced on the first pass was close but not pixel-perfect: you will need to modify if you want it to match very closely.

## 3. Sample 3: Handwritten Notes → Beamer Slides

The same PDF-input trick works for slides. Take a phone photo of handwritten lecture notes, drag it into the chat, and ask for a beamer deck.

### The input

Here is a scan of handwritten notes on bond excess returns and the expectations hypothesis: [sample_notes.pdf]({{ site.baseurl }}/topic\*4/assets/sample3_handwritten/sample_notes.pdf). Two pages of marker-on-paper math.

### Prompt the agent

```text
Attached is a scan of handwritten notes on bond excess returns and the
expectations hypothesis. Turn it into a beamer deck named
sample3_handwritten.tex. One frame per main point. Use the Madrid theme.
Include the key derivations as numbered equation blocks. Please use a
blue tcolorbox to highlight the main result.
```

### What to expect

The agent produced a five-frame beamer deck: title, notation, EH statement, slope-regression form, and the "why this matters" wrap-up.

Same iteration loop: the agent won't nail the handwriting perfectly.

---

## 4. Sample 4: Empirical Analysis

In this section, we are going to build a pipeline to create and present empirical results about the empirical violations of the expectations hypothesis for interest rates.

### Setup prompt

Start the agent with a general-flavor ask before locking in a spec.

```text
I want to do an empirical analysis now based on the slides in
sample3_handwritten.tex. How should I organize the directory for
secrets, data, code, figures and tables, latex? What data is
available in FRED to do this? What kind of output should I generate
in terms of plots and tables? What kind of empirical issues should
I consider in the analysis?
```

Read the agent's answer before committing. If the directory layout, data source, plots/tables, or empirical issues are off, push back before moving on to the detailed spec.

### Detailed spec

Once the setup answer looks right, lock in the implementing the spec (can also refine with plan mode).

```text
Thanks. I want to do an empirical analysis based on the slides in
sample3_handwritten.tex. Please implement this in one pass inside the
sample4_empirical/ directory, which already contains a .env file with
FRED_API_KEY. Do not create a .env.example.

1. DIRECTORY STRUCTURE

       sample4_empirical/
       ├── .env                   # already present
       ├── .gitignore             # .env, data/*.csv, output/*, latex build artifacts
       ├── requirements.txt
       ├── README.md
       ├── data/                  # raw downloaded data
       ├── code/                  # Python scripts
       ├── output/                # figures and table bodies
       └── latex/
           └── report.tex

2. DATA (code/download_yields.py)

   GSW fitted zero-coupon Treasury yields from FRED, 1- through 10-year
   zeros. Read FRED_API_KEY from .env. Range: 1985-01-31 to 2025-12-31,
   end-of-month observations. Write data/yields.csv with a date index
   and columns y1, y2, ..., y10.

3. ANALYSIS (code/*.py)

   Save figures as PDF (vector) so they stay crisp in the report.
   Use descriptive filenames: figures as figure_{kind}_{what}.pdf, table
   bodies as table_body_{what}.tex.

   i.   summary_stats.py — mean, sd, min, max, N per series.
        -> output/table_body_summary_stats.tex (booktabs).

   ii.  plot_yields.py — two figures:
          - output/figure_time_series_yields.pdf: y1 and y10 on one axes.
          - output/figure_time_series_slope.pdf:  y10 - y1.

   iii. plot_scatter.py — output/figure_scatter_slope_vs_yieldchange.pdf.
        For n = 10: scatter of slope (y^n_t - y^1_t) on x vs. change in
        yield y^n_{t+1} - y^n_t on y (ignore maturity roll-down in this
        plot). Overlay OLS fit line.

   iv.  run_regressions.py — for each n in {2,...,10}, run

            y^{n-1}_{t+1} - y^n_t
              = alpha_0 + alpha_1 * (y^n_t - y^1_t)/(n-1) + e_{t+1}

        Produce two booktabs LaTeX table bodies:
          - output/table_body_regression_ols.tex: plain OLS SEs.
          - output/table_body_regression_newey_west.tex: HAC, maxlags=5.
        Columns: n (years), alpha_1, SE, t-stat, R^2, N. Add *, **,
        *** on alpha_1 at |t| > 1.96 / 2.58 / 3.29.

4. WRITEUP (latex/report.tex)

   - Title, author, \today.
   - Section 1 — Setup: reproduce the notation and derivation from
     sample3_handwritten.tex. Define y^n_t, P^n_t, r^n_{t+1},
     xr^n_{t+1}; state the EH restriction; derive the slope regression
     and note EH predicts alpha_1 = 1 for all n.
   - Section 2 — Data: pull in the summary-stats table body and show
     the two yield/slope figures.
   - Section 3 — Relationship: show the scatter figure with a
     one-paragraph read of what it shows.
   - Section 4 — Regressions: pull in both regression table bodies.
     Paragraph on: are the alpha_1 estimates consistent with EH? Does
     OLS vs. Newey-West change the conclusion?
   - Build: `cd latex && latexmk -pdf report.tex`.

5. README.md — three commands: install deps, run the pipeline in order,
   build the PDF.
```

---

## 5. Sample 5: Adding References

Once the empirical draft is in place, a natural polish step is adding citations. The prompt is to copy Sample 4 and ask for references plus two packages that make references better.

### Prompt the agent

```text
Copy the sample4_empirical project into sample5_references. Upgrade
the LaTeX writeup with:

1. References to the following papers, cited in the setup section
   where the slope regression is introduced:
     - Campbell and Shiller (1991), "Yield Spreads and Interest Rate
       Movements: A Bird's Eye View", Review of Economic Studies.
     - Dai and Singleton (2002), "Expectation Puzzles, Time-Varying
       Risk Premia, and Affine Models of the Term Structure",
       Journal of Financial Economics.

2. cleveref for smart cross-references. Write \cref{tab:...},
   \cref{fig:...}, \cref{sec:...} and let the package insert
   "Table", "Figure", "Section" automatically (including plurals
   and ranges).

3. hyperref so citations, refs, URLs, and TOC entries become
   clickable in the PDF. Load it last among the packages.
```

### Add-ons

Hand the agent a follow-up prompt with any of these polish items:

- **`refs.bib` as a single source of truth.** Ask the agent to create a standard `refs.bib` file and route all citations through it (`biblatex` + `\cite{key}` + `\printbibliography`). This is the right setup as soon as you have more than one or two references, and the agent can migrate the inline citations for you automatically.
- **microtype** — typography polish (character protrusion, font expansion). One line in the preamble.
- **tikz** — draw diagrams inside LaTeX (commutative diagrams, timelines, flowcharts).
- **pgfplots** — plots that live in the LaTeX source, no external PNG/PDF round-trip.
- **Splitting across files** — `\input` or `\include` section by section once the document grows past ~20 pages.
- **Custom `.sty` file** — factor recurring preamble (packages, macros, formatting) into a reusable style file.

---

## 6. Other Useful LaTeX Things

What we've covered is enough for a first document and a reproducible report. A few more tools are worth knowing about as you go deeper.

### Splitting a document across files

For anything longer than ~20 pages, keep the main file short and `\input` or `\include` the sections:

```latex
% main.tex
\documentclass{article}
...
\begin{document}
\input{sections/intro}
\input{sections/data}
\input{sections/results}
\input{sections/conclusion}
\end{document}
```

- `\input{file}` — textual inclusion, as if pasted in place.
- `\include{file}` — similar, but forces a page break and supports `\includeonly{}` for compiling one section at a time (faster iteration on long documents).
- `\import{path}{file}` (from the `import` package) — like `\input` but relative paths inside the imported file resolve correctly, which matters once you nest subdirectories.

### `latexmk` for builds

`latexmk` handles the annoying details of running `pdflatex` → `biber`/`bibtex` → `pdflatex` → `pdflatex` in the right order. One command:

```bash
latexmk -pdf report.tex
latexmk -pdf -pvc report.tex    # watch mode: recompiles on save
latexmk -C                      # clean all build products
```

A `.latexmkrc` file at the project root locks in options so collaborators compile the same way.

### `biblatex` + Zotero for references

Keep references in a `.bib` file, cite them by key:

```latex
\usepackage[style=authoryear]{biblatex}
\addbibliography{refs.bib}
...
As \textcite{campbell_shiller_1991} showed, $\phi_n$ is negative.
...
\printbibliography
```

Zotero + the Better BibTeX plugin auto-generates stable citekeys and keeps `refs.bib` in sync with your Zotero library. Never hand-type a bibliography again.

### `cleveref` for smart cross-references

Instead of "see Table~\ref{tab:cs}", just write `\cref{tab:cs}` and get "see Table 3" automatically — the package knows it's a table, a figure, a section, etc.

### `tikz` for figures

TikZ draws diagrams inside LaTeX itself (no external image). Great for commutative diagrams, decision trees, timelines, flowcharts. The learning curve is real, but agents handle TikZ well — describe the diagram in prose and let the agent draft the code.

```latex
\usepackage{tikz}
\begin{tikzpicture}
  \draw[->] (0,0) -- (3,0) node[right] {time};
  \draw[->] (0,0) -- (0,2) node[above] {yield};
\end{tikzpicture}
```

### `pgfplots` for plots

If you want a plot to live inside the LaTeX source (no matplotlib round-trip), `pgfplots` reads a `.csv` or `.dat` file and renders the plot at compile time. Great when the figure is tiny or when you want fonts and line widths to match the body text exactly.

### `hyperref` for clickable PDFs

```latex
\usepackage{hyperref}
```

Turns every `\ref`, `\cite`, `\url`, and TOC entry into a clickable link inside the PDF. Always load it --- and load it **last** among your packages, as it rewrites many internals.

### `microtype` for typography polish

```latex
\usepackage{microtype}
```

One line. Enables character protrusion, font expansion, and better line-breaking. The difference is subtle but consistent; papers look more professional.

### Custom style files (`.sty`)

Once you have a set of packages, macros, and formatting tweaks you reuse across papers, factor them into your own `mystyle.sty` and load it with `\usepackage{mystyle}`. Example in this repo: `USC.FBE.633.26.01/_latex_common/fbeDocumentStylePS1.sty`, which centralizes the problem-set formatting for this course.

---

## Summary

- LaTeX + VS Code + LaTeX Workshop is the setup. Everything else (hello document, compile, file structure) is practice.
- Feed the agent PDFs; accept that the output needs iteration; polish is cheap.
- The empirical payoff is a pipeline where Python produces `.tex` fragments and `.png` figures, and `report.tex` pulls them together into a publication-ready PDF.
- As projects grow: `\input` to split, `latexmk` to build, `biblatex` + Zotero for refs, `cleveref` for cross-refs, TikZ/pgfplots for figures, `microtype` + a personal `.sty` for polish.

**Next** ([Topic 5]({{ site.baseurl }}/topic_5)): sandboxing — how to let the agent rip through workflows like this one without the risk of it clobbering your working directory.
