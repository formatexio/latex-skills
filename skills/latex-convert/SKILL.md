---
name: latex-convert
description: Use when the user asks to convert existing content into LaTeX — including Markdown, HTML, plain text, Word-style formatting, or any other markup or prose that needs to be transformed into a compilable LaTeX document or snippet.
---

# LaTeX Convert

When converting content to LaTeX:

1. **Assess** the input: identify all structural elements (headings, lists, tables, emphasis, links, code, images, math).
2. **Map** each element to its LaTeX equivalent (see table below).
3. **Output** clean, idiomatic LaTeX — not a mechanical character substitution.
4. **Include** required `\usepackage{}` declarations as a block at the top of the output.
5. **Flag** any elements with no LaTeX equivalent (e.g., embedded video, interactive content).

## Element Mapping

| Source | LaTeX |
|---|---|
| `# Heading 1` | `\section{Heading 1}` |
| `## Heading 2` | `\subsection{...}` |
| `### Heading 3` | `\subsubsection{...}` |
| `**bold**` | `\textbf{...}` |
| `*italic*` | `\textit{...}` |
| `- list item` | `\begin{itemize}\item...\end{itemize}` |
| `1. ordered item` | `\begin{enumerate}\item...\end{enumerate}` |
| `[text](url)` | `\href{url}{text}` — requires `hyperref` |
| `` `inline code` `` | `\texttt{...}` |
| Code block | `\begin{lstlisting}...\end{lstlisting}` — requires `listings` |
| `> blockquote` | `\begin{quote}...\end{quote}` |
| `---` (horizontal rule) | `\noindent\rule{\linewidth}{0.4pt}` |
| Table | `tabular` environment with `booktabs` |
| `![alt](image.png)` | `\includegraphics{image}` — requires `graphicx` |
| Inline math `$x^2$` | `\(x^2\)` |
| Display math `$$...$$` | `\[...\]` or `equation` environment |

## Special Character Escaping

These characters must be escaped in LaTeX text mode:

| Character | LaTeX |
|---|---|
| `&` | `\&` |
| `%` | `\%` |
| `$` | `\$` |
| `_` | `\_` |
| `#` | `\#` |
| `^` | `\^{}` |
| `{` | `\{` |
| `}` | `\}` |
| `~` | `\textasciitilde{}` |
| `\` | `\textbackslash{}` |

## Typography Notes

- Smart quotes (`"` `"`) → use ` `` ` and `''` in LaTeX
- Em-dash (`—`) → `---`, en-dash (`–`) → `--`
- URLs should be inside `\url{}` or `\href{}{}` with `hyperref` loaded
