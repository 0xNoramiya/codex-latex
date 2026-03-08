![Codex LaTeX Banner](codex-latex/assets/banner-large.svg)

# Codex LaTeX

Professional Codex skill for research-grade LaTeX authoring, editing, and diagnostics.

`codex-latex` gives teams a deterministic workflow to:
- scaffold publication-ready `.tex` projects,
- keep citations and cross-references consistent,
- lint for common structural issues,
- compile with reproducible logs for faster debugging.

## Why This Skill

Academic writing often fails at the same points: broken refs, drifting section structure, and fragile formatting edits.
This skill packages reusable templates, scripts, and reference guides so Codex can handle paper workflows consistently instead of ad-hoc.

## Key Features

- Research-focused LaTeX templates:
  - `article` (general manuscripts / preprints)
  - `ieeetran` (IEEE-style submissions)
  - `acmart` (ACM-style submissions)
- Deterministic utility scripts:
  - project scaffolding
  - compile pipeline (`latexmk`)
  - lint checks (`chktex` + duplicate-label checks)
  - missing citation/label diagnostics
- Progressive references for writing quality:
  - paper structure
  - citation practices
  - figure/table conventions
  - template selection matrix
- Shareable sample manuscripts generated with the skill.

## Repository Layout

```text
.
├── LICENSE
├── README.md
└── codex-latex/
    ├── SKILL.md
    ├── agents/openai.yaml
    ├── assets/
    │   ├── banner-large.svg
    │   ├── banner-small.svg
    │   ├── templates/
    │   │   ├── article/
    │   │   ├── ieeetran/
    │   │   └── acmart/
    │   ├── snippets/
    │   └── samples/
    ├── references/
    └── scripts/
```

## Requirements

Minimum:
- `bash`
- `rg` (ripgrep)

Recommended:
- TeX distribution with `latexmk` (for compile checks)
- `chktex` (for deeper linting)

Notes:
- `lint.sh` still runs if `chktex` is missing.
- `compile.sh` requires `latexmk` and will fail fast if it is unavailable.

## Install in Codex

### Option A: Manual install

```bash
git clone https://github.com/0xNoramiya/codex-latex.git
cd codex-latex

CODEX_HOME="${CODEX_HOME:-$HOME/.codex}"
mkdir -p "$CODEX_HOME/skills"
cp -R codex-latex "$CODEX_HOME/skills/codex-latex"
```

After this, the skill folder should exist at:

```text
$CODEX_HOME/skills/codex-latex/
```

### Option B: Use directly from this repo

If you only want local script usage, run scripts from the repository root:

```bash
./codex-latex/scripts/new_paper.sh /tmp/my-paper --template article --title "My Paper" --author "A. Researcher"
```

## Use in Codex

Invoke explicitly with `$codex-latex` for best determinism.

Examples:

```text
$codex-latex Create a new IEEE paper scaffold titled "Adaptive Routing for Agentic Systems" with placeholder sections.
```

```text
$codex-latex Refactor my existing LaTeX manuscript so figures/tables have stable labels and all citations resolve.
```

```text
$codex-latex Diagnose why this project fails to compile and patch only the minimal affected lines.
```

## Script Usage

### 1) Create a new paper

```bash
./codex-latex/scripts/new_paper.sh /tmp/paper --template article --title "Demo Paper" --author "Demo Team"
```

Supported templates:
- `article`
- `ieeetran`
- `acmart`

### 2) Compile with deterministic logs

```bash
./codex-latex/scripts/compile.sh /tmp/paper --main main.tex --engine pdflatex
```

Supported engines:
- `pdflatex`
- `xelatex`
- `lualatex`

### 3) Lint for style/structure issues

```bash
./codex-latex/scripts/lint.sh /tmp/paper --main main.tex
```

Checks include:
- `chktex` (if installed)
- duplicate `\label{...}` keys
- `TODO` / `FIXME` markers

### 4) Detect missing references and citations

```bash
./codex-latex/scripts/fixrefs.sh /tmp/paper
```

Checks include:
- unresolved `\ref`, `\eqref`, `\autoref`, `\cref`, `\Cref`, `\pageref`
- missing `.bib` keys referenced via `\cite...{}`

## Sample Results

Generated sample documents are included here:
- `codex-latex/assets/samples/article-sample/main.tex`
- `codex-latex/assets/samples/ieee-sample/main.tex`
- `codex-latex/assets/samples/acm-sample/main.tex`

Each sample includes:
- realistic section flow,
- valid citation usage,
- valid cross-references,
- clean pass through `fixrefs.sh` and `lint.sh`.

### Example success output

```text
Created LaTeX project: /tmp/tmp.vCiS5csHah/demo
Template: article
No missing label/citation keys found.
chktex not found. Skipping chktex pass.
Checking duplicate labels...
Checking TODO/FIXME markers...
Lint checks passed.
```

### Example diagnostic output (intentional broken refs)

```text
Missing label definitions for these references:
fig:notfound
Missing bibliography entries for these citation keys:
missingkey
```

## Authoring Conventions Used by the Skill

- Preserve existing class/package choices unless migration is explicitly requested.
- Keep bibliography keys stable (recommended: `lastnameYYYYtopic`).
- Ensure every figure/table has caption + label + in-text mention.
- Prefer minimal, local patches over large rewrites.

## Troubleshooting

### `latexmk is required but not installed`

Install a TeX distribution with `latexmk` and retry:
- TeX Live (Linux)
- MacTeX (macOS)

### Missing class/package errors for `ieeetran` or `acmart`

Your TeX installation may not include all publisher classes. Install/update the relevant LaTeX packages and rerun `compile.sh`.

### `chktex not found`

This is non-blocking. Lint still checks duplicate labels and TODO/FIXME markers.
Install `chktex` if you want deeper style diagnostics.

## License

MIT License. See [LICENSE](LICENSE).
