---
name: formatex-api
description: Use when the user wants to integrate the FormaTeX API into their application, asks how to use the FormaTeX API in a specific programming language (Python, JavaScript, TypeScript, Go, curl), or asks about authentication, PDF generation, or error handling with the FormaTeX API.
---

# FormaTeX API Integration

**Base URL:** `https://api.formatex.io/api/v1/`
**Auth:** `Authorization: Bearer $FORMATEX_API_KEY` on every request.

Never hardcode API keys — always read from environment variables.

## Python

```python
import os
import httpx

def compile_latex(source: str, engine: str = "pdflatex") -> bytes:
    response = httpx.post(
        "https://api.formatex.io/api/v1/compile",
        headers={"Authorization": f"Bearer {os.environ['FORMATEX_API_KEY']}"},
        json={"content": source, "engine": engine},
        timeout=60,
    )
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
      "Authorization": `Bearer ${process.env.FORMATEX_API_KEY}`,
      "Content-Type": "application/json",
    },
    body: JSON.stringify({ content: source, engine }),
  });
  if (!res.ok) {
    const err = await res.json();
    throw new Error(err.error ?? "Compilation failed");
  }
  return Buffer.from(await res.arrayBuffer()); // PDF buffer
}
```

## curl

```bash
curl -X POST https://api.formatex.io/api/v1/compile \
  -H "Authorization: Bearer $FORMATEX_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"content": "\\documentclass{article}\\begin{document}Hello\\end{document}", "engine": "pdflatex"}' \
  -o output.pdf
```

## Available Engines

| Engine | When to use |
|---|---|
| `pdflatex` | Default — standard documents |
| `xelatex` | System fonts, Unicode, non-Latin scripts |
| `lualatex` | Lua scripting, advanced font features |
| `latexmk` | Auto multi-pass (BibTeX, makeindex) |

## Error Handling Reference

| HTTP status | Meaning |
|---|---|
| `200` + PDF body | Success |
| `200` + JSON with `error` field | Compilation failed — read `error` for the TeX log |
| `401` | Invalid or missing API key |
| `422` | Malformed request body |
| `429` | Rate limit exceeded — back off and retry |
| `5xx` | Server error — retry with exponential backoff |
