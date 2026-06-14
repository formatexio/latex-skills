---
name: formatex
description: Use when the user mentions FormaTeX, wants to compile LaTeX via the FormaTeX API, integrate FormaTeX into an application, create a FormaTeX template, render a template with JSON data, or encounters a LaTeX compilation error while using the FormaTeX platform.
---

# FormaTeX Assistant

Handles all FormaTeX platform tasks plus LaTeX error fixing. Route based on what the user needs:

## Compile via API → apply this

When the user wants to compile LaTeX using FormaTeX:

**Check API key first:**
```bash
echo $FORMATEX_API_KEY
```
If empty: ask the user to `export FORMATEX_API_KEY=your_key_here` (find it at formatex.io → Settings → API Keys).

**Compile:**
```bash
curl -X POST https://api.formatex.io/api/v1/compile \
  -H "Authorization: Bearer $FORMATEX_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"content": "<LaTeX source>", "engine": "pdflatex"}' \
  -o output.pdf
```

**Engine selection:** `pdflatex` (default) · `xelatex` (system fonts, Unicode) · `lualatex` (Lua scripting) · `latexmk` (auto multi-pass).

Check the document for `\usepackage{fontspec}` or non-Latin scripts — if present, use `xelatex`.

On error: the API returns JSON with an `error` field containing the TeX log — apply the error-fix flow below.

## Fix a compilation error → apply this

When the API returns an error or the LaTeX itself won't compile:
1. Read the first `! Error` line in the log
2. Explain the cause in one plain-English sentence
3. Show the exact fix as a code block
4. Re-run the compilation after fixing

Common patterns: `Undefined control sequence` (typo or missing package) · `Missing $` (math in text mode) · `Environment undefined` (wrong name or missing package).

## Create a template → apply this

When the user wants a reusable FormaTeX template:

1. Replace dynamic values with `{{variable_name}}` placeholders (lowercase, underscores, no spaces)
2. Create the template:
```bash
curl -X POST https://api.formatex.io/api/v1/templates \
  -H "Authorization: Bearer $FORMATEX_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"name": "Template Name", "content": "<LaTeX with {{placeholders}}>", "engine": "pdflatex"}'
```
3. Save the returned `id`.

**Render it:**
```bash
curl -X POST https://api.formatex.io/api/v1/templates/{id}/render \
  -H "Authorization: Bearer $FORMATEX_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"variables": {"variable_name": "value"}}' \
  -o output.pdf
```

## Integrate in an application → apply this

When the user wants to use FormaTeX in their code:

**Python:**
```python
import os, httpx

def compile_latex(source: str, engine: str = "pdflatex") -> bytes:
    res = httpx.post(
        "https://api.formatex.io/api/v1/compile",
        headers={"Authorization": f"Bearer {os.environ['FORMATEX_API_KEY']}"},
        json={"content": source, "engine": engine},
        timeout=60,
    )
    res.raise_for_status()
    return res.content
```

**JavaScript:**
```javascript
async function compileLaTeX(source, engine = "pdflatex") {
  const res = await fetch("https://api.formatex.io/api/v1/compile", {
    method: "POST",
    headers: {
      "Authorization": `Bearer ${process.env.FORMATEX_API_KEY}`,
      "Content-Type": "application/json",
    },
    body: JSON.stringify({ content: source, engine }),
  });
  if (!res.ok) throw new Error((await res.json()).error ?? "Failed");
  return Buffer.from(await res.arrayBuffer());
}
```

**Error handling:** HTTP 401 = bad key · HTTP 422 = bad request · HTTP 200 + `error` field = compilation failed (read the log) · HTTP 429 = rate limited, back off.
