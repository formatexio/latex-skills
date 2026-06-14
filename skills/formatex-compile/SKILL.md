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
> "Please set your FormaTeX API key:
> `export FORMATEX_API_KEY=your_key_here`
> Find it at formatex.io → Settings → API Keys."

Never hardcode the key.

## Step 2 — Choose the Right Engine

| Use case | Engine |
|---|---|
| Standard documents | `pdflatex` (default) |
| System fonts, Unicode, Arabic, CJK | `xelatex` |
| Lua scripting, advanced font features | `lualatex` |
| Auto multi-pass (BibTeX, makeindex) | `latexmk` |
| Let FormaTeX decide | `auto` |

Check the document for `\usepackage{fontspec}` or non-Latin scripts — if present, use `xelatex`. When unsure, use `auto`.

## Step 3 — Compile

```bash
curl -X POST https://api.formatex.io/api/v1/compile \
  -H "Authorization: Bearer $FORMATEX_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "latex": "<LaTeX source>",
    "engine": "pdflatex"
  }' \
  -o output.pdf
```

On success (HTTP 200): response body is the binary PDF — saved to `output.pdf`.

## Error Handling

On compilation failure (HTTP 422), the API returns JSON:
```json
{
  "success": false,
  "error": "! Undefined control sequence.",
  "log": "<full TeX log>",
  "duration": 1243
}
```

Read the `error` field, apply `latex-fix` logic to diagnose and fix, then recompile.

## Optional Fields

```json
{
  "latex": "...",
  "engine": "pdflatex",
  "runs": 2,
  "timeout": 30,
  "files": [
    { "path": "image.png", "data": "<base64>" }
  ]
}
```

- `runs` — number of compilation passes (default: auto)
- `timeout` — seconds before giving up (default: 30)
- `files` — additional files (images, `.bib`, `.cls`, etc.) as base64
