---
name: latex-math
description: Use when the user asks to write, typeset, or generate a mathematical formula, equation, proof, matrix, sum, integral, or any mathematical expression in LaTeX — including when they describe math in plain English or pseudo-notation.
---

# LaTeX Math

When asked to typeset math:

1. **Choose** the right environment based on context (see table below).
2. **Use** proper LaTeX math commands — never plain-text approximations.
3. **Include** `\usepackage{amsmath}` in the package note (assumed for most environments).
4. **Output** compilable LaTeX with correct syntax.

## Environment Reference

| Need | Environment | Notes |
|---|---|---|
| Inline math | `\(...\)` | Preferred over `$...$` |
| Numbered equation | `equation` | Single equation with number |
| Unnumbered equation | `equation*` | Requires `amsmath` |
| Aligned multi-line | `align` / `align*` | Use `&` as the alignment point |
| Piecewise / cases | `cases` | Inside `equation` environment |
| Matrix (round brackets) | `pmatrix` | Requires `amsmath` |
| Matrix (square brackets) | `bmatrix` | Requires `amsmath` |
| Matrix (pipe / determinant) | `vmatrix` | Requires `amsmath` |
| Centered, no alignment | `gather` / `gather*` | Multiple equations centered |
| Fine-grained alignment | `alignat{n}` | n pairs of columns |

## Common Pitfalls

- Never use `*` for multiplication in display math — use `\cdot` or `\times`
- Never use `//` for fractions — use `\frac{numerator}{denominator}`
- Subscripts/superscripts with more than one character need braces: `x_{ij}` not `x_ij`
- Use `\left(` and `\right)` for auto-sizing delimiters around tall content
- Text inside math environments: use `\text{...}` (requires `amsmath`) — plain letters render as variables
- Never use `$$...$$` for display math — use `\[...\]` or `equation` environment

## Useful Packages

- `amssymb` — extra symbols: `\mathbb{R}`, `\mathcal{L}`, `\leq`, `\geq`, `\neq`, `\infty`
- `mathtools` — extends amsmath: `\coloneqq`, bracketing tools
- `physics` — clean notation for derivatives (`\dv`), bras/kets, norms
- `siunitx` — typeset units correctly: `\SI{9.8}{\meter\per\second\squared}`
