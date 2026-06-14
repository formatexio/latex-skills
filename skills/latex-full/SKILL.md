---
name: latex-full
description: Use when the user is working with LaTeX or FormaTeX for any purpose — fixing errors, writing markup, typesetting math, scaffolding documents, managing citations, explaining commands, converting content, reviewing documents, recommending packages, compiling via the FormaTeX API, creating templates, or building integrations.
---

# LaTeX + FormaTeX Full Assistant

Handles all LaTeX and FormaTeX tasks. Route to the right approach:

## Fix errors → apply this

Error in log or compilation failure:
1. Find the first `! Error` line — identify type (see patterns)
2. Explain cause in one plain-English sentence
3. Show exact fix as a code block + prevention tip

Patterns: `Undefined control sequence` (typo/missing package) · `Missing $` (math in text mode) · `File not found` (missing package) · `Environment undefined` · brace mismatch (`Too many }'s`).
`Overfull \hbox` is a warning, not an error — mention it won't block compilation.

## Write LaTeX markup → apply this

For tables, figures, algorithms, theorems, lists, or any element:
- Pick correct environment: `tabular`+`booktabs` (tables) · `figure`+`graphicx` (images) · `algorithm2e` (algorithms) · `amsthm` (theorems)
- Include `\usepackage{}` declarations, `\label{}`, `\caption{}`, float specifiers `[htbp]`

## Typeset math → apply this

- Inline: `\(...\)` · Display: `\[...\]` or `equation` · Multi-line: `align`/`align*`
- Require `amsmath`. Never use `$$`, `*` for multiply, `//` for fractions
- Multi-char sub/superscripts need braces: `x_{ij}` not `x_ij`

## Scaffold a document → apply this

- Classes: `article` (papers) · `report`/`book` (thesis) · `beamer` (slides) · `moderncv` (CV) · `IEEEtran`/`acmart` (conference)
- Base packages (pdflatex): `inputenc`, `fontenc`, `microtype`, `hyperref` (last)
- For xe/lualatex: replace inputenc/fontenc with `fontspec`

## Citations → apply this

- Default: `\usepackage[style=authoryear,backend=biber]{biblatex}` + `\addbibresource{refs.bib}`
- BibTeX only when journal requires a `.bst` file
- Always use `~\cite{key}` (non-breaking space). `latexmk` handles multi-pass automatically.

## Explain a concept → apply this

- Answer in one sentence · Show minimal example if helpful
- Flag deprecated: `$$`, `eqnarray`, `\bf`/`\it`/`\rm` → use `\textbf{}`, `\textit{}`, `\textrm{}`

## Convert content to LaTeX → apply this

- Headings → `\section`/`\subsection` · Bold → `\textbf{}` · Lists → `itemize`/`enumerate` · Links → `\href{}`
- Escape: `& % $ _ # ^ { } ~ \`
- Smart quotes → ` `` ` and `''` · `—` → `---` · `–` → `--`

## Review a document → apply this

Explicit audit request only — output a prioritized issue list:
- **Critical:** missing packages, mismatched environments, unescaped chars, math in text mode
- **Important:** `$$`, `eqnarray`, `\bf`/`\it`, `\over`, missing `~` before `\ref{}`/`\cite{}`
- **Style:** Overfull hboxes, missing `microtype`, inconsistent labels

## Recommend a package → apply this

- Math: `amsmath`+`amssymb` · Tables: `booktabs` · Citations: `biblatex` · Links: `hyperref` (last) · Typography: `microtype`
- Loading order: `babel` early → content packages → `hyperref` last → `cleveref` after `hyperref`

## Compile via FormaTeX API → apply this

Check key: `echo $FORMATEX_API_KEY` — if empty, ask user to export it.

```bash
curl -X POST https://api.formatex.io/api/v1/compile \
  -H "Authorization: Bearer $FORMATEX_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"latex": "<LaTeX source>", "engine": "pdflatex"}' \
  -o output.pdf
```

Engines: `pdflatex` (default) · `xelatex` (fonts/Unicode) · `lualatex` (Lua) · `latexmk` (auto) · `auto` (FormaTeX decides).

Success (HTTP 200) = PDF binary. Failure (HTTP 422) = `{ "success": false, "error": "...", "log": "...", "duration": ms }`.

## Use a FormaTeX template → apply this

Templates use `{{variable_name}}` placeholders. List templates:
```bash
curl https://api.formatex.io/api/v1/templates -H "Authorization: Bearer $FORMATEX_API_KEY"
```

Compile a template:
```bash
curl -X POST "https://api.formatex.io/api/v1/templates/user/{id}/compile" \
  -H "Authorization: Bearer $FORMATEX_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"variables": {"key": "value"}, "engine": "pdflatex"}' \
  -o output.pdf
```

## Integrate FormaTeX in an app → apply this

```python
# Python
import os, httpx
res = httpx.post("https://api.formatex.io/api/v1/compile",
  headers={"Authorization": f"Bearer {os.environ['FORMATEX_API_KEY']}"},
  json={"latex": source, "engine": "pdflatex"}, timeout=60)
if res.status_code == 422: raise ValueError(res.json()["error"])
res.raise_for_status()
pdf_bytes = res.content
```

```javascript
// JavaScript
const res = await fetch("https://api.formatex.io/api/v1/compile", {
  method: "POST",
  headers: { "Authorization": `Bearer ${process.env.FORMATEX_API_KEY}`, "Content-Type": "application/json" },
  body: JSON.stringify({ latex: source, engine: "pdflatex" }),
});
if (res.status === 422) throw new Error((await res.json()).error);
```

HTTP: 200 = PDF · 401 = bad key · 403 = plan limit · 422 = LaTeX error · 429 = rate limit · 5xx = retry.
