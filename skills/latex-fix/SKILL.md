---
name: latex-fix
description: Use when the user has a LaTeX compilation error, shares a .log file, says their document won't compile, or shows error output containing messages like "undefined control sequence", "missing $ inserted", "File not found", "environment undefined", or any TeX/LaTeX error or warning output.
---

# LaTeX Error Fix

When the user shares a LaTeX error or log output:

1. **Identify** the error type from the message or log.
2. **Explain** the cause in one plain-English sentence — no jargon.
3. **Show the fix** as a code block with the exact corrected lines. Always show code, not just a description of what to change.
4. **Add a prevention tip** (one line) on how to avoid this class of error.

## Common Error Patterns

| Error message | Cause | Fix direction |
|---|---|---|
| `Undefined control sequence` | Typo in command name, or missing `\usepackage` | Fix the typo or add the package |
| `Missing $ inserted` | Math command used outside math mode | Wrap in `$...$` or move inside math environment |
| `File not found: X.sty` | Package not installed or misspelled | Check package name; on FormaTeX, unsupported packages cause this |
| `Environment X undefined` | Wrong environment name or missing package | Fix name or add the package that defines it |
| `Too many }'s` / `Missing {` | Brace mismatch | Scan near the reported line number for unbalanced braces |
| `\begin{X} ended by \end{Y}` | Mismatched environments | Ensure every `\begin` has a matching `\end` |

## Warnings vs Errors

Warn the user these are **not fatal** — they won't stop compilation:
- `Overfull \hbox (Xpt too wide)` — line too long, cosmetic issue
- `Font shape undefined, using X instead` — font fallback, usually fine
- `Package X: Warning ...` — informational unless it says "Error"

## Engine-Specific Notes

- `pdflatex`: Does not support system fonts or OpenType. Use `fontenc`/`inputenc`.
- `xelatex`/`lualatex`: Supports system fonts and Unicode natively. Do **not** use `inputenc`.
- `latexmk`: Auto-selects the right engine; errors reported come from the underlying engine.
