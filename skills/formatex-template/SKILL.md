---
name: formatex-template
description: Use when the user wants to create a FormaTeX template, save a LaTeX document as a reusable template, add variable placeholders to a document using `<<name>>` syntax, or render a template by passing JSON data to the FormaTeX API.
---

# FormaTeX Templates

FormaTeX templates are LaTeX documents with `<<variable_name>>` placeholders. Submit JSON variables to compile a filled PDF — no LaTeX editing required per document.

## Step 1 — Add Placeholders

Replace dynamic values with `<<variable_name>>` in the LaTeX source:

```latex
\documentclass{article}
\begin{document}

Dear <<recipient_name>>,

Your certificate for \textbf{<<course_title>>} is awarded on <<date>>.

\end{document}
```

Variable names: letters, digits and underscores, starting with a letter or underscore. No spaces. They are case sensitive.

## Step 2 — Create the Template

Create the template with the API key (Pro plan or above). `name` and `source` are required; `engine` defaults to `pdflatex` and `category` to `Professional`:

```bash
curl -X POST https://api.formatex.io/api/v1/templates \
  -H "X-API-Key: $FORMATEX_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Course certificate",
    "source": "\\documentclass{article}\n\\begin{document}\nDear <<recipient_name>>, your certificate for <<course_title>> is awarded on <<date>>.\n\\end{document}",
    "schema": [
      { "key": "recipient_name", "type": "string", "required": true, "description": "Who receives it" },
      { "key": "course_title", "type": "string", "required": true },
      { "key": "date", "type": "date", "required": false, "default": "today" }
    ]
  }'
```

Response (HTTP 201): `{ "template": { "id": "uuid", "name": "...", ... } }`. Each `schema` entry has `key`, `type`, `required`, `default` and `description`.

A template can also be created in the dashboard at formatex.io, or from an AI assistant through the FormaTeX MCP server (`create_template`, `update_template`, `get_template`). To find a template's UUID later:

```bash
curl https://api.formatex.io/api/v1/templates \
  -H "X-API-Key: $FORMATEX_API_KEY"
```

Response: `{ "templates": [{ "id": "uuid", "name": "...", "engine": "..." }, ...] }`

Change or remove it with `PUT` and `DELETE` on `/api/v1/templates/user/{template_id}`. `PUT` changes only the fields you send.

## Step 3 — Compile the Template

```bash
curl -X POST "https://api.formatex.io/api/v1/templates/user/{template_id}/compile" \
  -H "X-API-Key: $FORMATEX_API_KEY" \
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

## Rules

- **Plan:** templates need the Pro plan or above. On Free, `GET /api/v1/templates` returns an empty list with an upgrade message.
- **Credentials:** an API key can list, create, read, update, delete and compile templates, and manage their assets. Nothing needs a dashboard login or a session token. Template writes are limited to 60 per minute per account.
- **Limit:** each plan has a maximum number of templates. Creating one past it returns `403` with `template limit reached`.
- **Assets:** images and other files for a template are managed under `/api/v1/templates/user/{template_id}/assets`, not through the MCP tools.
- **Values are strings:** send every variable as a JSON string, numbers included (`"amount": "129.90"`).
- **Values are LaTeX:** they are inserted as written, with no escaping. Escape `& % $ # _ { } ~ ^ \` in user supplied text before sending it.
- **Types:** `string`, `number` (must parse as a number) and `date` (`YYYY-MM-DD` or `today`). A missing required variable, or a value of the wrong type, returns `400` naming the variable.
- A placeholder with no value and no default is left in the PDF as written.

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
