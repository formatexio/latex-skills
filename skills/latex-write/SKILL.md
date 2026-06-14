---
name: latex-write
description: Use when the user asks Claude to write, generate, or produce LaTeX markup for a specific element — such as a table, figure, list, algorithm, code listing, theorem, custom environment, or any LaTeX structure described in plain text.
---

# LaTeX Write

When asked to generate LaTeX for a specific element:

1. **Identify** the element type and constraints (column count, caption, label, style).
2. **Choose** the correct environment and packages.
3. **Output** clean, compilable LaTeX with:
   - Required `\usepackage{}` declarations listed as a comment block at the top
   - Proper labels (`\label{fig:name}`, `\label{tab:name}`) when the element will be referenced
   - `\caption{}` where applicable
4. **Note** common variations the user might need.

## Common Elements & Packages

| Element | Package | Environment |
|---|---|---|
| Professional table | `booktabs` | `tabular` with `\toprule`, `\midrule`, `\bottomrule` |
| Multi-page table | `longtable` | `longtable` |
| Figure | `graphicx` | `figure` + `\includegraphics` |
| Sub-figures | `subcaption` | `subfigure` inside `figure` |
| Algorithm | `algorithm2e` | `algorithm` |
| Code listing (no deps) | `listings` | `lstlisting` |
| Code listing (highlighted) | `minted` | `minted` (requires Python + Pygments) |
| Theorem / Proof | `amsthm` | `theorem`, `proof` |
| Custom list | `enumitem` | `itemize` or `enumerate` with options |
| Colored box | `tcolorbox` | `tcolorbox` |

## Float Placement Specifiers

Always explain placement when outputting floats (`figure`, `table`):
- `[h]` — here if it fits, `[t]` — top of page, `[b]` — bottom, `[p]` — float page
- `[H]` — force here (requires `float` package) — use sparingly, breaks float algorithm
- Recommended default: `[htbp]`
