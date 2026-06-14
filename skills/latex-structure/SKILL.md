---
name: latex-structure
description: Use when the user asks to set up, scaffold, or start a new LaTeX document — including a thesis, research paper, conference paper, report, CV, resume, cover letter, or Beamer presentation — or asks for a document template or boilerplate.
---

# LaTeX Document Structure

When asked to scaffold a document:

1. **Identify** the document type and any constraints (institution, journal/conference, citation style, language, engine).
2. **Choose** the right document class (see table below).
3. **Output** a complete, compilable starter document with essential packages and commented section placeholders.

## Document Classes

| Type | Class | Notes |
|---|---|---|
| Article / short paper | `article` | Most common for papers |
| Conference (IEEE) | `IEEEtran` | `\documentclass[conference]{IEEEtran}` |
| Conference (ACM) | `acmart` | `\documentclass[sigconf]{acmart}` |
| Thesis / long report | `report` | Supports `\chapter` |
| Book | `book` | `\frontmatter` / `\mainmatter` / `\backmatter` |
| Styled CV / résumé | `moderncv` | Themed, good defaults |
| Presentation | `beamer` | Slides; use `\usetheme{...}` |
| Letter | `scrlttr2` | KOMA-Script letter class |

## Essential Package Baseline

Always include unless the user specifies otherwise:

```latex
% For pdflatex only:
\usepackage[utf8]{inputenc}
\usepackage[T1]{fontenc}

% For xelatex / lualatex: replace above with:
% \usepackage{fontspec}

\usepackage{microtype}   % Better typography
\usepackage{hyperref}    % Clickable links and refs (load last)
```

## Thesis-Specific Notes

- Ask about institution requirements before assuming a class — many universities provide their own `.cls`
- Standard structure: `\tableofcontents`, `\listoffigures`, `\listoftables` after `\maketitle`
- Use `\frontmatter` / `\mainmatter` / `\backmatter` with the `book` class for proper page numbering
- For multilingual documents, add `\usepackage[french,english]{babel}` (or `polyglossia` for xe/lualatex)
