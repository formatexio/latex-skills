# FormaTeX Skills

Claude Code skills for LaTeX and FormaTeX workflows.

Install them into your `.claude/skills/` directory. Claude will automatically use them when you're working on LaTeX documents or integrating the FormaTeX API.

---

## Install

### Recommended: pick your role

**Editor / academic user** (thesis, papers, documents):
```bash
cp -r skills/latex ~/.claude/skills/
```

**Developer / API user** (PDF generation, integrations, automation):
```bash
cp -r skills/formatex ~/.claude/skills/
```

**I want everything:**
```bash
cp -r skills/latex-full ~/.claude/skills/
```

### Cherry-pick individual skills

```bash
# Copy any skill you need
cp -r skills/latex-fix ~/.claude/skills/
cp -r skills/formatex-compile ~/.claude/skills/
```

---

## Skills

### General LaTeX

| Skill | What it does |
|---|---|
| `latex-fix` | Diagnoses and fixes LaTeX compilation errors |
| `latex-write` | Generates LaTeX markup for tables, figures, algorithms, and more |
| `latex-math` | Typesets equations, formulas, and proofs |
| `latex-structure` | Scaffolds thesis, paper, report, CV, or Beamer documents |
| `latex-citations` | Sets up BibLaTeX/BibTeX, generates .bib entries, manages citation styles |
| `latex-explain` | Explains LaTeX commands, warnings, and concepts in plain English |
| `latex-convert` | Converts Markdown, HTML, or plain text to LaTeX |
| `latex-review` | Audits a LaTeX document for errors, bad practices, and style issues |
| `latex-packages` | Recommends the right package for any task with usage examples |

### FormaTeX

| Skill | What it does |
|---|---|
| `formatex-compile` | Compiles LaTeX to PDF via the FormaTeX API |
| `formatex-template` | Creates and renders FormaTeX templates with JSON variables |
| `formatex-api` | Shows FormaTeX API integration in Python, JavaScript, or curl |

### Meta Skills (one install, full coverage)

| Skill | Covers |
|---|---|
| `latex` | All 9 general LaTeX skills — for editor/academic users |
| `formatex` | All 3 FormaTeX skills + error fixing — for developer/API users |
| `latex-full` | Everything — for power users |

---

## FormaTeX API Key

The FormaTeX skills read your API key from the environment:

```bash
export FORMATEX_API_KEY=your_key_here
```

Find your key at [formatex.io](https://formatex.io) under **Settings → API Keys**.

---

## Learn More

- [FormaTeX](https://formatex.io) — browser editor, REST API, AI assistant, real-time collaboration
- [API docs](https://api.formatex.io/api/v1/) — full API reference
- [Blog](https://formatex.io/blog) — deep-dive posts for each skill in this pack
