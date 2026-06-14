---
name: formatex-template
description: Use when the user wants to create a FormaTeX template, save a LaTeX document as a reusable template, add variable placeholders to a document using double-brace syntax, or render a template by passing JSON data to the FormaTeX API.
---

# FormaTeX Templates

FormaTeX templates are LaTeX documents with `{{variable_name}}` placeholders. Submit JSON data to render a filled PDF — no LaTeX editing required per document.

## Step 1 — Add Placeholders

Replace dynamic values with `{{variable_name}}` in the LaTeX source:

```latex
\documentclass{article}
\begin{document}

Dear {{recipient_name}},

Your certificate for \textbf{{{course_title}}} is awarded on {{date}}.

\end{document}
```

Variable names: lowercase letters, numbers, and underscores only. No spaces.

## Step 2 — Create the Template

```bash
curl -X POST https://api.formatex.io/api/v1/templates \
  -H "Authorization: Bearer $FORMATEX_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Certificate",
    "content": "<escaped LaTeX with {{placeholders}}>",
    "engine": "pdflatex"
  }'
```

Response: `{ "id": "tpl_xxx", "name": "Certificate", ... }`

Save the `id` — you need it to render.

## Step 3 — Render the Template

```bash
curl -X POST https://api.formatex.io/api/v1/templates/tpl_xxx/render \
  -H "Authorization: Bearer $FORMATEX_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "variables": {
      "recipient_name": "Alice Martin",
      "course_title": "Introduction to LaTeX",
      "date": "June 14, 2026"
    }
  }' \
  -o certificate.pdf
```

## API Key

If `FORMATEX_API_KEY` is not set, ask the user to export it before proceeding:
```bash
export FORMATEX_API_KEY=your_key_here
```

## Use Case: Batch Generation

For bulk PDF generation (certificates, invoices, reports), loop over a list of variable sets and render once per record. Each render call is independent.
