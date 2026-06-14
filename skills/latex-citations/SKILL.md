---
name: latex-citations
description: Use when the user asks about bibliography, citations, references, BibTeX, BibLaTeX, natbib, .bib files, citation styles (APA, IEEE, Chicago, Vancouver), or asks how to cite a paper, book, website, or any source in LaTeX.
---

# LaTeX Citations

## Recommended: BibLaTeX + Biber

Use BibLaTeX unless the user has a specific journal/conference requirement for BibTeX.

```latex
\usepackage[style=authoryear, backend=biber]{biblatex}
\addbibresource{references.bib}

% At the end of the document:
\printbibliography
```

Common styles: `authoryear` (APA-like), `numeric` (IEEE-like), `alphabetic`, `chicago-authordate`.

## When to Use BibTeX / natbib Instead

- Journal provides a `.bst` file → use BibTeX with `\bibliographystyle{...}`
- Existing document already uses `\bibliographystyle{}` → preserve it
- Conference template requires it → follow the template

## .bib Entry Templates

**Article:**
```bibtex
@article{key2024,
  author  = {Last, First and Last2, First2},
  title   = {Title of the Article},
  journal = {Journal Name},
  year    = {2024},
  volume  = {10},
  number  = {2},
  pages   = {100--110},
  doi     = {10.xxxx/xxxxx}
}
```

**Book:**
```bibtex
@book{author2023,
  author    = {Last, First},
  title     = {Book Title},
  publisher = {Publisher},
  year      = {2023},
  address   = {City}
}
```

**Website / online resource:**
```bibtex
@online{site2024,
  author  = {Last, First},
  title   = {Page Title},
  url     = {https://example.com},
  urldate = {2024-06-14}
}
```

## In-Document Citation Commands

| Command | Output | Backend |
|---|---|---|
| `\cite{key}` | [1] or (Author, 2024) | both |
| `\parencite{key}` | (Author, 2024) | BibLaTeX |
| `\textcite{key}` | Author (2024) | BibLaTeX |
| `\citet{key}` | Author (2024) | natbib |
| `\citep{key}` | (Author, 2024) | natbib |

Always use `~\cite{key}` (non-breaking space before `\cite`) to prevent line breaks before citation numbers.

## Compilation Order

- **BibTeX:** `pdflatex` → `bibtex` → `pdflatex` → `pdflatex`
- **BibLaTeX/Biber:** `pdflatex` → `biber` → `pdflatex` → `pdflatex`
- **With `latexmk`:** automatic — just run `latexmk -pdf main.tex`
