# Copilot Instructions for pandoc-action

## Repository purpose

This repository is a **template for automated PDF generation** via GitHub Actions.
Markdown source files in `docs/` are compiled into publication-ready PDFs
using [Pandoc](https://pandoc.org) + XeLaTeX, then published as a GitHub Release.
Each subdirectory under `docs/` becomes its own PDF.

## Repository layout

```
.
├── docs/                          # One subdirectory per document
│   ├── pandoc-guide/              # Chapters of the pandoc-guide document
│   │   ├── 01-introduction.md
│   │   ├── 02-content.md
│   │   ├── 03-conclusion.md
│   │   ├── 04-advanced.md
│   │   └── images/               # Images used by this document
│   │       └── sample-gradient.png
│   └── quick-reference/           # Chapters of the quick-reference document
│       ├── 01-overview.md
│       └── 02-options.md
├── images/                        # Header / footer images used on every page
│   ├── header.png
│   └── footer.png
├── header.tex                     # LaTeX preamble (header/footer setup)
├── references.bib                 # BibTeX bibliography
├── metadata.yaml                  # Pandoc document metadata and LaTeX settings
└── .github/workflows/
    └── release.yml                # CI workflow: build PDFs → tag → release
```

## Key design decisions

- **Multi-document structure**: each subdirectory under `docs/` is compiled into
  a separate PDF named after the directory.  Add a new document by creating a new
  `docs/<name>/` folder with numbered `.md` files — no workflow changes needed.
- **Chapter ordering**: files inside a document folder are passed to Pandoc via
  glob (`docs/<name>/*.md`), so chapters are included in alphabetical/numerical
  order.  Use numeric prefixes (e.g. `01-`, `02-`) to control chapter order.
- **Metadata lives in `metadata.yaml`**: title, author, date, TOC settings, paper
  size, margins, and bibliography reference all reside in this single shared file.
  Do not hard-code LaTeX settings in individual Markdown files.
- **Header/footer images**: controlled via `\newcommand{\headerimage}{...}` and
  `\newcommand{\footerimage}{...}` inside `header.tex`.  Set the value to an empty
  string to disable; never remove the `\newcommand` line entirely.
- **Versioning**: the CI workflow auto-tags every successful build as `v1.<run_number>`.
  The tag is pushed before the release is created.

## How the CI workflow works (`release.yml`)

1. Checks out the repo with full history (`fetch-depth: 0`) so tagging works.
2. Installs `pandoc`, `texlive-xetex`, `texlive-fonts-recommended`, and
   `texlive-fonts-extra` via `apt-get`.
3. Iterates over every subdirectory in `docs/` and runs:
   ```bash
   pandoc --metadata-file=metadata.yaml --include-in-header=header.tex \
     --pdf-engine=xelatex --citeproc <doc_dir>*.md -o <name>.pdf
   ```
4. Computes a version tag `v1.${{ github.run_number }}`.
5. Creates and pushes the git tag.
6. Creates a GitHub Release with all `*.pdf` files as assets using
   `softprops/action-gh-release@v2`.

## Common agent tasks

### Adding a new document
1. Create `docs/<name>/` and populate it with numbered `.md` files.
2. No other changes needed — the workflow glob `docs/*/` picks it up automatically.

### Adding a chapter to an existing document
1. Create `docs/<document-name>/NN-chapter-name.md` (choose a number that sorts where you want it).
2. No other changes needed.

### Changing document metadata
Edit `metadata.yaml`.  Common fields:
- `title`, `author`, `date`
- `toc: true/false`, `toc-depth: <number>`
- `geometry: margin=2.5cm`
- `fontsize: 12pt`, `papersize: a4`

### Replacing header/footer images
Overwrite `images/header.png` and/or `images/footer.png`.  Any image format that
XeLaTeX accepts works (PNG, JPEG, PDF).

### Building a PDF locally
```bash
pandoc --metadata-file=metadata.yaml --include-in-header=header.tex \
  --pdf-engine=xelatex --citeproc \
  docs/pandoc-guide/*.md -o pandoc-guide.pdf
```

### Adding a bibliography
1. Add entries to `references.bib`.
2. Ensure `bibliography: references.bib` is set in `metadata.yaml` (already configured).
3. Cite entries with `[@citekey]` syntax in Markdown; `--citeproc` handles the rest.

## What NOT to do

- Do not commit generated `*.pdf` files — they are build artifacts produced by CI.
- Do not remove `\newcommand{\headerimage}` or `\newcommand{\footerimage}` from
  `header.tex`; set them to empty instead of removing them.
- Do not add raw LaTeX directly to Markdown chapter files; put all LaTeX customisation
  in `header.tex` or `metadata.yaml`.
- Do not place `.md` files directly in `docs/` root — they must be inside a named
  subdirectory to be compiled.
- Do not change the workflow trigger or permissions without understanding that the
  `contents: write` permission is required to push tags and create releases.
