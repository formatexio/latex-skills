---
name: formatex-api
description: Use when the user wants to integrate the FormaTeX API into their application, asks how to use the FormaTeX API in a specific programming language (Python, JavaScript, TypeScript, Go, curl), or asks about authentication, PDF generation, or error handling with the FormaTeX API.
---

# FormaTeX API Integration

**Base URL:** `https://api.formatex.io/api/v1/`
**Auth:** `X-API-Key: $FORMATEX_API_KEY` on every request (not `Authorization: Bearer`, which the REST API rejects with `401`).

Never hardcode API keys — always read from environment variables.

## Python

```python
import os
import httpx

def compile_latex(source: str, engine: str = "pdflatex") -> bytes:
    response = httpx.post(
        "https://api.formatex.io/api/v1/compile",
        headers={"X-API-Key": os.environ["FORMATEX_API_KEY"]},
        json={"latex": source, "engine": engine},
        timeout=60,
    )
    if response.status_code == 422:
        raise ValueError(response.json().get("error", "Compilation failed"))
    response.raise_for_status()
    return response.content  # PDF bytes

pdf = compile_latex(r"\documentclass{article}\begin{document}Hello\end{document}")
with open("output.pdf", "wb") as f:
    f.write(pdf)
```

## JavaScript / Node.js

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
  if (res.status === 422) {
    const err = await res.json();
    throw new Error(err.error ?? "Compilation failed");
  }
  if (!res.ok) throw new Error(`HTTP ${res.status}`);
  return Buffer.from(await res.arrayBuffer()); // PDF buffer
}
```

## curl

```bash
curl -X POST https://api.formatex.io/api/v1/compile \
  -H "X-API-Key: $FORMATEX_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"latex": "\\documentclass{article}\\begin{document}Hello\\end{document}", "engine": "pdflatex"}' \
  -o output.pdf
```

## Request Body

```json
{
  "latex": "<LaTeX source>",
  "engine": "pdflatex",
  "runs": 2,
  "timeout": 30,
  "files": [
    { "path": "image.png", "data": "<base64-encoded bytes>" }
  ]
}
```

`latex` is required. All other fields are optional.

## Available Engines

| Engine | When to use |
|---|---|
| `pdflatex` | Default — standard documents |
| `xelatex` | System fonts, Unicode, non-Latin scripts |
| `lualatex` | Lua scripting, advanced font features |
| `latexmk` | Auto multi-pass (BibTeX, makeindex) |
| `auto` | Let FormaTeX detect the right engine |

## HTTP Response Reference

| Status | Meaning | Body |
|---|---|---|
| `200` | Success | Binary PDF |
| `401` | Missing API key (send `X-API-Key`) | `{ "error": "missing API key" }` |
| `403` | Invalid API key, or a plan limit or engine not allowed | `{ "error": "..." }` |
| `422` | Compilation failed | `{ "success": false, "error": "...", "log": "...", "duration": ms }` |
| `429` | Rate limit exceeded | `{ "error": "..." }` + `Retry-After` header |
| `5xx` | Server error | Retry with exponential backoff |
