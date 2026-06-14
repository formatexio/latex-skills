---
name: formatex-template
description: Use when the user wants to create a FormaTeX template, save a LaTeX document as a reusable template, add variable placeholders to a document using double-brace syntax, or render a template by passing JSON data to the FormaTeX API.
---

# FormaTeX Templates

FormaTeX templates are LaTeX documents with `{{variable_name}}` placeholders. Submit JSON variables to compile a filled PDF — no LaTeX editing required per document.

## Step 1 — Add Placeholders

Replace dynamic values with `{{variable_name}}` in the LaTeX source:

```latex
\documentclass{article}
\begin{document}

Dear {{recipient_name}},

Your certificate for \textbf{{{course_title}}} is awarded on {{date}}.

\end{document}
```

Variable names: lowercase letters, numbers, underscores. No spaces.

## Step 2 — Create the Template

Templates are created via the dashboard at formatex.io, or via the user API (JWT auth). Once saved, each template has a UUID — find it in the dashboard or via:

```bash
curl https://api.formatex.io/api/v1/templates \
  -H "Authorization: Bearer $FORMATEX_API_KEY"
```

Response: `{ "templates": [{ "id": "uuid", "name": "...", "engine": "..." }, ...] }`

## Step 3 — Compile the Template

```bash
curl -X POST "https://api.formatex.io/api/v1/templates/user/{template_id}/compile" \
  -H "Authorization: Bearer $FORMATEX_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "variables": {
      "recipient_name": "Alice Martin",
      "course_title": "Introduction to LaTeX",
      "date": "June 14, 2026"
    },
    "engine": "pdflatex"
  }' \
  -o output.pdf
```

Replace `{template_id}` with the UUID from Step 2.

On success (HTTP 200): response body is the binary PDF.
On failure (HTTP 422): `{ "success": false, "error": "...", "log": "...", "duration": ... }`

## Optional Fields

```json
{
  "variables": { "key": "value" },
  "engine": "pdflatex",
  "runs": 2,
  "timeout": 30,
  "files": [{ "path": "logo.png", "data": "<base64>" }]
}
```

## API Key

If `FORMATEX_API_KEY` is not set, ask the user to export it before proceeding.

## Use Case: Batch Generation

For bulk PDFs (certificates, invoices, reports), loop over records and POST to the compile endpoint once per record. Each call is independent and stateless.
