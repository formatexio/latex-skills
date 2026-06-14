---
name: latex-explain
description: Use when the user asks what a LaTeX command does, why LaTeX behaves a certain way, what the difference between two LaTeX commands or packages is, or asks for an explanation of a LaTeX warning, concept, or term — as a conceptual question rather than a specific compilation error to fix.
---

# LaTeX Explain

When the user asks a conceptual question about LaTeX:

1. **Answer directly** in one clear sentence stating what it does or means.
2. **Show a minimal example** if the concept is better demonstrated than described.
3. **Compare alternatives** where relevant (e.g., deprecated commands vs modern replacements).
4. **Note the context** where it matters ("only works in math mode", "deprecated — use X instead").

Keep answers concise. Offer to go deeper only if the topic warrants it.

## Common Concepts to Know

**Modes:**
- Text mode (default), math mode (`$...$` or `\(...\)`), LR mode (inside boxes)
- Many errors come from using a math-only command in text mode or vice versa

**Spacing commands:**
- `\,` thin space · `\;` thick space · `\quad` 1em · `\qquad` 2em
- `~` non-breaking space — use between words and refs: `Figure~\ref{fig:x}`
- `\hfill` pushes content to fill remaining horizontal space
- `\\` forces a line break; `\newpage` forces a page break

**Deprecated commands (flag these, suggest replacements):**

| Deprecated | Modern replacement |
|---|---|
| `$$...$$` | `\[...\]` or `equation` environment |
| `eqnarray` | `align` from `amsmath` |
| `\rm`, `\it`, `\bf` | `\textrm{}`, `\textit{}`, `\textbf{}` |
| `epsfig` | `graphicx` |
| `\over` | `\frac{}{}` |
| `\centerline{}` | `\begin{center}...\end{center}` or `\centering` |

**Commands vs environments:**
- Commands: `\command[opt]{arg}` — produce output inline or change state
- Environments: `\begin{env}...\end{env}` — wrap a block of content with scoped behavior

**Scope and grouping:**
- `{...}` creates a group — font changes, `\centering`, `\color` etc. are scoped to the group
- This is why `{\bfseries some text}` works without affecting the rest of the paragraph
