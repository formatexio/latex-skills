---
name: latex-review
description: Use when the user explicitly asks Claude to review, audit, check, or assess a LaTeX document or snippet for quality, correctness, or issues — as a deliberate review request rather than a specific error the user is already seeing.
---

# LaTeX Review

When asked to review a LaTeX document:

1. **Scan** the full document across all categories below.
2. **Output** a prioritized list of issues grouped by severity.
3. **For each issue:** name it, quote the problematic line, explain why it is a problem, provide the fix.

Output only real issues found — do not pad with generic advice.

## Review Categories

### Critical — compilation failures or incorrect output

- Missing `\usepackage` for commands that are used
- Mismatched `\begin{X}` / `\end{Y}` environments
- Unescaped special characters in text mode: `&`, `%`, `$`, `_`, `#`, `^`, `~`, `{`, `}`
- Math commands used outside math mode (e.g., `\frac` in text)
- Missing required arguments to commands

### Important — bad practice, deprecated, or fragile

| Found | Recommend instead |
|---|---|
| `$$...$$` for display math | `\[...\]` or `equation` environment |
| `eqnarray` | `align` from `amsmath` |
| `\bf`, `\it`, `\rm` | `\textbf{}`, `\textit{}`, `\textrm{}` |
| `epsfig` | `graphicx` |
| `\over` | `\frac{}{}` |
| `\begin{center}` inside `figure`/`table` | `\centering` (avoids extra vertical space) |
| Hardcoded spacing (`\\[2cm]`) | Proper structural commands |
| `\ref{}` without `~` before it | `Figure~\ref{fig:x}` |
| `\cite{}` without `~` before it | `\cite{key}` preceded by `~` |
| Figures/tables without `\label{}` | Add labels for all numbered floats |

### Style — cosmetic, worth noting

- `Overfull \hbox` warnings — line too long, may need manual hyphenation or rewriting
- Missing `\usepackage{microtype}` — improves typography significantly at no cost
- Inconsistent `\label{}` naming convention across the document
- Unused `\usepackage` declarations
