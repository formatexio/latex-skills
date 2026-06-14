---
name: latex-packages
description: Use when the user asks which LaTeX package to use for a specific task, how to accomplish something in LaTeX that they don't know the package for, or whether a package exists for a given purpose.
---

# LaTeX Packages

When the user asks about packages:

1. **Identify** the specific need.
2. **Recommend** the best current option first with a one-line rationale.
3. **List alternatives** with trade-offs if the choice is non-obvious.
4. **Show a minimal working example** of the recommended package.

## Quick Reference by Domain

| Need | Recommended | Alternatives / Notes |
|---|---|---|
| Better typography | `microtype` | Load always — free win |
| Math environments | `amsmath` | Essential for any math |
| Math symbols | `amssymb` | Pairs with amsmath |
| Physics notation | `physics` | `siunitx` for SI units specifically |
| Graphics / images | `graphicx` | Standard |
| Sub-figures | `subcaption` | `subfig` (older, avoid in new docs) |
| Professional tables | `booktabs` + `array` | `tabularray` for complex modern tables |
| Multi-page tables | `longtable` | — |
| Code listings (no deps) | `listings` | `minted` for syntax highlighting (needs Python) |
| Algorithms | `algorithm2e` | `algorithmicx` + `algpseudocode` |
| Hyperlinks + PDF metadata | `hyperref` | Always load last |
| Smart cross-references | `cleveref` | Load after `hyperref` |
| Bibliography | `biblatex` | `natbib` for BibTeX-only journals |
| List customization | `enumitem` | — |
| Multi-column layout | `multicol` | — |
| Colors | `xcolor` | — |
| Frames and callout boxes | `tcolorbox` | `mdframed` (simpler) |
| TikZ / diagrams | `tikz` | `pgfplots` for data visualization |
| Page layout / margins | `geometry` | — |
| Headers and footers | `fancyhdr` | `scrlayer-scrpage` (KOMA-Script) |
| Font selection (xe/lualatex) | `fontspec` | — |
| Font selection (pdflatex) | `lmodern` | `lmodern` or specific font packages |
| SI units | `siunitx` | — |
| Glossaries | `glossaries-extra` | `glossaries` (simpler subset) |
| Index | `imakeidx` | `makeidx` (basic) |
| Watermarks | `draftwatermark` | — |
| Line numbers | `lineno` | — |
| Landscape pages | `pdflscape` | `lscape` (no PDF rotation) |

## Package Loading Order Rules

- `hyperref` loads **last** (or second-to-last before `cleveref`)
- `cleveref` loads **after** `hyperref`
- `babel` / `polyglossia` loads **early** (right after document class)
- `fontspec` loads **before** other font packages (xelatex/lualatex only)
- `inputenc` and `fontenc` are **pdflatex only** — omit with xelatex/lualatex
