# latex-skills

> AI assistant skills for LaTeX and FormaTeX — for Claude Code, Cursor, Copilot, and more.

Install these skills to get LaTeX and FormaTeX superpowers directly inside your AI coding assistant. Whether you're writing a thesis, generating PDFs programmatically, or building a document automation pipeline — there's a skill for it.

---

## What Are Skills?

Skills are instruction files your AI assistant loads automatically when you're working on a relevant task. No prompting needed — just install once and your assistant knows how to fix LaTeX errors, typeset equations, call the FormaTeX API, and more.

---

## Install

### Choose your AI assistant

#### Claude Code

```bash
# Clone the repo
git clone https://github.com/formatexio/latex-skills.git

# Install for your role (pick one):
cp -r latex-skills/skills/latex ~/.claude/skills/        # Editor / academic users
cp -r latex-skills/skills/formatex ~/.claude/skills/     # Developer / API users
cp -r latex-skills/skills/latex-full ~/.claude/skills/   # Everything

# Or cherry-pick individual skills:
cp -r latex-skills/skills/latex-fix ~/.claude/skills/
cp -r latex-skills/skills/formatex-compile ~/.claude/skills/
```

#### Cursor

Copy skill content into `.cursor/rules/latex.mdc` in your project (or global rules):

```bash
cat latex-skills/skills/latex-full/SKILL.md > .cursor/rules/latex.mdc
```

#### GitHub Copilot

```bash
mkdir -p .github
cat latex-skills/skills/latex-full/SKILL.md > .github/copilot-instructions.md
```

#### Gemini CLI

```bash
cp -r latex-skills/skills/latex-full ~/.agents/skills/
```

#### Windsurf

```bash
cat latex-skills/skills/latex-full/SKILL.md > .windsurfrules
```

---

## Skills

### By user type

| I am... | Install this |
|---|---|
| Student / researcher writing a thesis or paper | `latex` |
| Academic using BibTeX, figures, math | `latex` |
| Developer calling the FormaTeX API | `formatex` |
| Building a PDF generation pipeline | `formatex` |
| Power user who wants everything | `latex-full` |

---

### General LaTeX Skills (9)

| Skill | Triggers when you... | What it does |
|---|---|---|
| `latex-fix` | Have a compilation error or `.log` output | Diagnoses the error, explains the cause, shows the exact fix |
| `latex-write` | Ask to generate a table, figure, algorithm, theorem, or any element | Outputs correct LaTeX with packages, labels, and captions |
| `latex-math` | Ask to typeset a formula, equation, matrix, or proof | Picks the right environment (`align`, `equation`, `cases`...) with correct notation |
| `latex-structure` | Want to start a thesis, paper, report, CV, or presentation | Scaffolds a complete document with the right class, packages, and sections |
| `latex-citations` | Ask about bibliography, BibTeX, `.bib` files, or citation styles | Sets up BibLaTeX/BibTeX, generates entries, explains APA/IEEE/Chicago styles |
| `latex-explain` | Ask what a command or warning means | Plain-English answer with a minimal example, flags deprecated commands |
| `latex-convert` | Want to convert Markdown, HTML, or plain text to LaTeX | Maps every element correctly, escapes special characters |
| `latex-review` | Ask for a document audit or code review | Prioritized issue list: critical errors, bad practices, style issues |
| `latex-packages` | Ask which package to use for something | Recommends the best option with trade-offs and a working example |

### FormaTeX Skills (3)

| Skill | Triggers when you... | What it does |
|---|---|---|
| `formatex-compile` | Want to compile LaTeX via the FormaTeX API | Builds the API call, picks the right engine, handles errors |
| `formatex-template` | Want to use or build a FormaTeX template | Lists templates, compiles with JSON variables, handles batch generation |
| `formatex-api` | Want to integrate FormaTeX into Python, JS, Go, or curl | Shows complete, runnable integration code with auth and error handling |

### Meta Skills — one install, full coverage (3)

| Skill | Covers | Best for |
|---|---|---|
| `latex` | All 9 general LaTeX skills | Students, researchers, academics |
| `formatex` | All 3 FormaTeX skills + error fixing | Developers, API users, automation builders |
| `latex-full` | All 12 skills | Power users, full-stack LaTeX workflows |

---

## FormaTeX API Key

The FormaTeX skills read your API key from the environment:

```bash
export FORMATEX_API_KEY=your_key_here
```

Find your key at [formatex.io](https://formatex.io) under **Settings → API Keys**.

Add it to your shell profile (`.bashrc`, `.zshrc`) to make it permanent.

---

## Quick Examples

**Fix a LaTeX error:**
> "I'm getting `! Undefined control sequence. \l@nohyphenation` — what's wrong?"

**Write markup:**
> "Write me a professional table comparing three algorithms with runtime and accuracy columns."

**Typeset math:**
> "Write the Euler-Lagrange equation with a proper `align` environment."

**Compile via FormaTeX:**
> "Compile this document using my FormaTeX account with XeLaTeX."

**Build an integration:**
> "Show me how to call the FormaTeX API in Python and save the PDF."

**Convert to LaTeX:**
> "Convert this Markdown document to LaTeX."

---

## API Reference

**Base URL:** `https://api.formatex.io/api/v1/`

| Endpoint | Method | Description |
|---|---|---|
| `/compile` | POST | Compile LaTeX to PDF |
| `/compile/smart` | POST | Compile with auto error-fixing |
| `/templates` | GET | List your templates |
| `/templates/user/:id/compile` | POST | Compile a template with variables |

**Request body for `/compile`:**
```json
{
  "latex": "\\documentclass{article}...",
  "engine": "pdflatex",
  "runs": 2,
  "timeout": 30,
  "files": [{ "path": "image.png", "data": "<base64>" }]
}
```

**Engines:** `pdflatex` · `xelatex` · `lualatex` · `latexmk` · `auto`

**Responses:** `200` = PDF binary · `422` = compilation error (JSON with `error`, `log`, `duration`) · `401` = bad key · `429` = rate limited

---

## Contributing

Corrections, new skills, and platform adapters welcome. Open a PR against `main`.

---

## Links

- [FormaTeX](https://formatex.io) — browser editor, REST API, AI assistant, real-time collaboration
- [FormaTeX Blog](https://formatex.io/blog) — deep-dive posts on each skill in this pack
- [API Docs](https://formatex.io/docs/api) — full API reference
