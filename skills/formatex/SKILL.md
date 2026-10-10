---
name: formatex
description: Use when the user mentions FormaTeX, wants to compile LaTeX via the FormaTeX API, integrate FormaTeX into an application, create a FormaTeX template, render a template with JSON data, or encounters a LaTeX compilation error while using the FormaTeX platform.
---

# FormaTeX Assistant

Handles all FormaTeX platform tasks plus LaTeX error fixing. Route based on what the user needs:

## Compile via API → apply this

**Check API key first:**
```bash
echo $FORMATEX_API_KEY
```
If empty: ask the user to `export FORMATEX_API_KEY=your_key_here` (find it at formatex.io → Settings → API Keys).

**Compile:**
```bash
curl -X POST https://api.formatex.io/api/v1/compile \
  -H "X-API-Key: $FORMATEX_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"latex": "<LaTeX source>", "engine": "pdflatex"}' \
  -o output.pdf
```

**Engine selection:** `pdflatex` (default) · `xelatex` (system fonts, Unicode) · `lualatex` (Lua) · `latexmk` (auto multi-pass) · `auto` (FormaTeX decides).

Check the document for `\usepackage{fontspec}` or non-Latin scripts — if present, use `xelatex`. When unsure, use `auto`.

**Success (HTTP 200):** response body is binary PDF.
**Failure (HTTP 422):** `{ "success": false, "error": "...", "log": "...", "duration": ms }` — apply error-fix flow below.

## Fix a compilation error → apply this

When the API returns 422 or the LaTeX itself won't compile:
1. Read the `error` field or the first `! Error` line in the log
2. Explain the cause in one plain-English sentence
3. Show the exact fix as a code block
4. Re-run the compilation after fixing

Common patterns: `Undefined control sequence` (typo or missing package) · `Missing $` (math in text mode) · `Environment undefined` (wrong name or missing package) · brace mismatch (`Too many }'s`).

## Use a template → apply this

**List templates:**
```bash
curl https://api.formatex.io/api/v1/templates \
  -H "X-API-Key: $FORMATEX_API_KEY"
```

**Compile a template with variables:**
```bash
curl -X POST "https://api.formatex.io/api/v1/templates/user/{template_id}/compile" \
  -H "X-API-Key: $FORMATEX_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "variables": { "recipient_name": "Alice", "date": "2026-06-14" },
    "engine": "pdflatex"
  }' \
  -o output.pdf
```

Templates use `<<variable_name>>` placeholders. Create and manage templates at formatex.io or via the dashboard.

## Integrate in an application → apply this

**Python:**
```python
import os, httpx

def compile_latex(source: str, engine: str = "pdflatex") -> bytes:
    res = httpx.post(
        "https://api.formatex.io/api/v1/compile",
        headers={"X-API-Key": os.environ["FORMATEX_API_KEY"]},
        json={"latex": source, "engine": engine},
        timeout=60,
    )
    if res.status_code == 422:
        raise ValueError(res.json().get("error", "Compilation failed"))
    res.raise_for_status()
    return res.content  # PDF bytes
```

**JavaScript:**
```javascript
async function compileLaTeX(source, engine = "pdflatex") {
  const res = await fetch("https://api.formatex.io/api/v1/compile", {
    method: "POST",
    headers: {
      "X-API-Key": process.env.FORMATEX_API_KEY,
      "Content-Type": "application/json",
    },
    body: JSON.stringify({ latex: source, engine }),
  });
  if (res.status === 422) throw new Error((await res.json()).error);
  if (!res.ok) throw new Error(`HTTP ${res.status}`);
  return Buffer.from(await res.arrayBuffer());
}
```

**HTTP status reference:** 200 = PDF · 401 = missing key · 403 = invalid key or plan limit · 422 = LaTeX error · 429 = rate limit (check `Retry-After`) · 5xx = retry with backoff.
