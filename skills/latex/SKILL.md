---
name: latex
description: Use when the user is working with LaTeX and asks Claude to fix errors, write or generate markup, typeset math, scaffold a document, manage citations, explain commands or warnings, convert content to LaTeX, review a document for issues, or recommend packages.
---

# LaTeX Assistant

Handles all general LaTeX tasks. Route to the right approach based on what the user needs:

## Fix errors → apply this

When the user has a compilation error, log output, or says something won't compile:
1. Identify the error type from the message (see patterns below)
2. Explain the cause in one plain-English sentence
3. Show the exact fix as a code block
4. Add a one-line prevention tip

Common patterns: `Undefined control sequence` (typo or missing package) · `Missing $` (math command in text mode) · `File not found` (missing package) · `Environment undefined` (wrong name or missing package) · brace mismatch (`Too many }'s`).

Overfull `\hbox` warnings are **not fatal** — mention this.

## Write markup → apply this

When the user asks for a table, figure, list, algorithm, theorem, or any LaTeX element:
- Choose the correct environment and packages (`booktabs` for tables, `graphicx` for figures, `algorithm2e` for algorithms, `amsthm` for theorems)
- Include `\usepackage{}` declarations as a comment at the top
- Add `\label{}` and `\caption{}` where applicable
- Note float placement specifiers (`[htbp]`)

## Typeset math → apply this

When the user asks for formulas, equations, or proofs:
- Use `\(...\)` for inline, `\[...\]` or `equation` for display, `align` for multi-line
- Include `\usepackage{amsmath}` note
- Never use `*` for multiplication, `//` for fractions, or `$$...$$`
- Multi-char subscripts/superscripts need braces: `x_{ij}`

## Scaffold a document → apply this

When the user wants to start a thesis, paper, report, CV, or presentation:
- Choose the right class: `article`, `report`, `book`, `beamer`, `moderncv`, `IEEEtran`, `acmart`
- Include baseline packages: `inputenc`, `fontenc`, `microtype`, `hyperref` (pdflatex) or `fontspec` (xe/lualatex)
- Output a complete compilable starter with commented placeholders

## Manage citations → apply this

When the user asks about bibliography, .bib files, or how to cite:
- Default to BibLaTeX + Biber (`\usepackage[style=authoryear,backend=biber]{biblatex}`)
- Use BibTeX only if the journal/conference requires a `.bst` file
- Always use `~\cite{key}` (non-breaking space before citation)
- Compilation order with latexmk: automatic; without: pdflatex → biber → pdflatex × 2

## Explain a concept → apply this

When the user asks what a command does, why LaTeX behaves some way, or what a warning means:
- Answer in one direct sentence
- Show a minimal example if it helps
- Flag deprecated commands (`$$`, `eqnarray`, `\bf`, `\it`, `\rm`) and recommend replacements

## Convert content → apply this

When the user wants to convert Markdown, HTML, or plain text to LaTeX:
- Map structure: headings → `\section`, bold → `\textbf{}`, lists → `itemize`/`enumerate`, links → `\href{}`
- Escape special characters: `&` `%` `$` `_` `#` `^` `{` `}` `~` `\`
- Smart quotes → ` `` ` and `''`; em-dash → `---`; en-dash → `--`

## Review a document → apply this

When the user explicitly asks for a review or audit:
- Check for: missing packages, mismatched environments, unescaped special chars, math in text mode
- Flag deprecated: `$$`, `eqnarray`, `\bf`/`\it`/`\rm`, `epsfig`, `\over`
- Note style issues: missing `~` before `\ref{}`/`\cite{}`, missing labels on floats
- Output a prioritized issue list with fixes

## Recommend a package → apply this

When the user asks which package to use:
- Recommend the best current option with a rationale
- Key defaults: `amsmath`+`amssymb` for math, `booktabs` for tables, `biblatex` for citations, `hyperref` (load last) for links, `microtype` always
- Note package loading order: `hyperref` last, `cleveref` after `hyperref`, `babel` early
