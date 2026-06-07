# CLAUDE.md — Context for Claude Code

This file gives Claude Code in Cursor the context it needs to assist
with the `msc-thesis` repository. Read this before acting on any request.

---

## What This Repo Is

A master's thesis in Data Science (Statistics and Modern Machine Learning)
at Bocconi University. The thesis presents an empirical research project
in computer vision — specifically an inference pipeline for cross-view
object correspondence in egocentric/exocentric video using foundation models.

The thesis is ~50 pages, written in LaTeX, and compiled via Overleaf
(GitHub sync). Prose is drafted directly into LaTeX chapter files via
Claude Code.

---

## Repo Structure

```
/project-guidelines/
  - scope.md
  - style-guide.md
  - writing-samples.md
  - thesis-outline.md
  - thesis-theory-topics.md
  - technical-specs.md

/chapter-briefs/
  - chapter-brief-template.md
  - [chapter-name]-brief.md      (one per chapter — paper section + expansion directive, filled before drafting)

/assets/
  - /figures/                    (.png / .pdf / .svg — plots, diagrams)
  - /tables/                     (.csv / .xlsx — raw data tables)
  - /data/                       (.json / .csv — experiment results)

/latex/
  - main.tex                     (master document)
  - references.bib               (all citations)
  - /chapters/
  - /style/
      - thesis.sty
      - custom-macros.tex

```

---

## Your Responsibilities in This Repo

### Drafting in LaTeX
Write prose directly into `/latex/chapters/[XX-chapter-name].tex`:
1. Follow all prose style and voice rules from `project-guidelines/style-guide.md`
2. Wrap display equations in `\begin{equation}...\end{equation}` with `\label{eq:name}`
3. Wrap inline math in `$...$`
4. Use `\cite{bibtex_key}` for citations — keys from `thesis-theory-topics.md` only, never invented
5. Use `\begin{figure}...\end{figure}` blocks pulling from `/assets/figures/`
6. Use `\begin{table}...\end{table}` blocks with `\toprule/\midrule/\bottomrule`
7. Add `\input{chapters/XX-chapter-name}` to `main.tex` if not already present

### Fixing grammatical errors
When you encounter a clear grammatical error in prose you are reading or editing —
a typo, subject-verb disagreement, a dropped or doubled word, a broken sentence
left from a manual edit — fix it, and tell the user exactly what you changed.
This applies even outside the section you were asked to work on. Keep the bar at
unambiguous grammatical correctness: do not rewrite for style, tone, or word
choice under this license, and do not alter technical meaning. When a fix is not
purely grammatical (it changes meaning or could be a deliberate choice), flag it
for review instead of editing.

### Managing references.bib
When adding new citations:
- Use the bibtex keys defined in `thesis-theory-topics.md` exactly — do not invent new keys
- Check for duplicates before adding
- Standard fields required: author, title, year, and venue (journal/booktitle/publisher)
- For arXiv papers: use `@misc` with `howpublished = {arXiv preprint arXiv:XXXX.XXXXX}`

### Git Commits
Use this commit format:
```
latex: draft [chapter name]
latex: revise [chapter name] — [brief description]
refs: add [author] bibtex entries
fix: [brief description of fix]
assets: add [figure/table name]
```

One logical change per commit.

### Compilation
When asked to compile:
1. Run pdflatex → bibtex → pdflatex → pdflatex (standard sequence)
2. Report all errors and warnings clearly
3. Fix LaTeX errors autonomously if they are formatting/syntax issues
4. Flag content errors (wrong citation key, missing figure) for human review

---

## LaTeX Conventions

These are non-negotiable — they apply to every chapter file.

**Math notation** (defined in `custom-macros.tex` — use macros, not raw LaTeX):
- Vectors: `\vv{x}` (bold lowercase)
- Matrices: `\mm{A}` (bold uppercase)
- Sets: `\set{X}` (calligraphic)
- Real numbers: `\R`
- Expectation: `\E`
- All other macros: see `custom-macros.tex`

**Equations:**
- Every display equation gets a `\label{eq:descriptive-name}`
- Every equation is referenced in prose before or immediately after it appears
- Component-by-component explanation follows every display equation in prose

**Figures:**
- Every figure is referenced in text with `\cref{fig:name}` before it appears
- Figures live in `/assets/figures/` — never embedded raw in `.tex` files

**Citations:**
- Encode all citations as `\cite{key}`. Use keys from `thesis-theory-topics.md` verbatim — never invent.
- All narrative citation conventions (when to name authors inline, prose embedding format) are governed by `project-guidelines/style-guide.md` § Citations — preserve the approved draft's choices exactly.

**Cross-references:**
- Use `\cref{}` throughout (not `\ref{}`) — requires `cleveref` package
- Label convention: `fig:name`, `tab:name`, `eq:name`, `sec:name`

---

## What Claude Code Should Never Do

- Never deviate from the established prose style; always follow `project-guidelines/style-guide.md` for voice, tone, and citation conventions.
- Never invent bibtex keys — use only keys from `thesis-theory-topics.md`
- Never edit files in `/project-guidelines/` without explicit instruction — these are reference documents
- Never commit directly to main without flagging if changes touch `main.tex` structure
- Never resolve a missing figure by removing the reference — flag it instead
- Never try to compile locally the .tex files; it's not the purpose of this project directory. 

---

## Current Status

Track the current state of each chapter here. Update after each commit.

| Chapter | Directive | LaTeX | Compiled |
|---|---|---|---|
| 01 Introduction | ☐ | ☐ | ☐ |
| 02a Visual Representation | ☑ | ☑ | ☐ |
| 02b Segmentation | ☑ | ☑ | ☐ |
| 02c Language Grounding | ☑ | ☑ | ☐ |
| 02d Foundation Models & Agents | ☑ | ☑ | ☐ |
| 02e Addition of Time | ☑ | ☑ | ☐ |
| 02f Related Works | ☐ | ☐ | ☐ |
| 03 Proposed Pipeline | ☐ | ☐ | ☐ |
| 04 Experiments | ☐ | ☐ | ☐ |
| 05 Conclusions | ☐ | ☐ | ☐ |