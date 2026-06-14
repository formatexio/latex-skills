---
name: formatex-compile
description: Use when the user wants to compile LaTeX using the FormaTeX API, mentions their FormaTeX account, references FORMATEX_API_KEY, says "compile via FormaTeX", "generate PDF using FormaTeX", or "send this to FormaTeX".
---

# FormaTeX Compile

Compile LaTeX to PDF via the FormaTeX REST API.

## Step 1 — API Key

Check for the key in the shell:
```bash
echo $FORMATEX_API_KEY
```

If empty, ask the user:
> "Please set your FormaTeX API key first:
> `export FORMATEX_API_KEY=your_key_here`
> You can find it at formatex.io under Settings → API Keys."

Never hardcode the key.

## Step 2 — Choose the Right Engine

| Use case | Engine |
|---|---|
| Standard documents | `pdflatex` (default) |
| System fonts, Unicode, Arabic, CJK | `xelatex` |
| Lua scripting, advanced font features | `lualatex` |
| Auto-run (BibTeX, makeindex, multiple passes) | `latexmk` |

Check the document for `\usepackage{fontspec}` or non-Latin scripts — if present, use `xelatex`.

## Step 3 — Compile

```bash
curl -X POST https://api.formatex.io/api/v1/compile \
  -H "Authorization: Bearer $FORMATEX_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "<LaTeX source>",
    "engine": "pdflatex"
  }' \
  -o output.pdf
```

On success, the response body is the binary PDF saved to `output.pdf`.

## Error Handling

If compilation fails, the API returns JSON with an `error` field containing the TeX log. Apply `latex-fix` logic to diagnose:
- Read the first `! Error` line in the log
- Identify the line number and error type
- Fix the source and recompile

## Common Options

```json
{
  "content": "...",
  "engine": "pdflatex",
  "options": {
    "timeout": 30
  }
}
```
