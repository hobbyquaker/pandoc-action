# Copilot Instructions for pandoc-action

## Repository purpose

This repository is a **template for automated PDF generation** via GitHub Actions.
Markdown source files in `docs/` are compiled into a single publication-ready PDF
using [Pandoc](https://pandoc.org) + XeLaTeX, then published as a GitHub Release.

## Repository layout

```
.
├── docs/                    # Markdown source chapters (add/remove freely)
│   ├── 01-introduction.md
│   ├── 02-content.md
│   └── 03-conclusion.md
├── images/                  # Header / footer images used on every page
│   ├── header.png
│   └── footer.png
├── metadata.yaml            # Pandoc document metadata and LaTeX settings
└── .github/workflows/
    └── release.yml          # CI workflow: build PDF → tag → release
```

## Key design decisions

- **Chapter ordering**: files in `docs/` are passed to Pandoc via glob (`docs/*.md`),
  so chapters are included in alphabetical/numerical order.  Use numeric prefixes
  (e.g. `01-`, `02-`) to control chapter order.
- **Metadata lives in `metadata.yaml`**: title, author, date, TOC settings, paper
  size, margins, and the LaTeX preamble for header/footer images all reside in this
  single file.  Do not hard-code LaTeX settings in individual Markdown files.
- **Header/footer images**: controlled via `\newcommand{\headerimage}{...}` and
  `\newcommand{\footerimage}{...}` inside the `header-includes` block of
  `metadata.yaml`.  Set the value to an empty string to disable; never remove the
  `\newcommand` line entirely.
- **Versioning**: the CI workflow auto-tags every successful build as `v1.<run_number>`.
  The tag is pushed before the release is created.

## How the CI workflow works (`release.yml`)

1. Checks out the repo with full history (`fetch-depth: 0`) so tagging works.
2. Installs `pandoc`, `texlive-xetex`, `texlive-fonts-recommended`, and
   `texlive-fonts-extra` via `apt-get`.
3. Runs `pandoc docs/*.md --metadata-file=metadata.yaml --pdf-engine=xelatex -o document.pdf`.
4. Computes a version tag `v1.${{ github.run_number }}`.
5. Creates and pushes the git tag.
6. Creates a GitHub Release with `document.pdf` as an asset using
   `softprops/action-gh-release@v2`.

## Common agent tasks

### Adding a new chapter
1. Create `docs/NN-chapter-name.md` (choose a number that sorts where you want it).
2. No other changes needed — the glob `docs/*.md` picks it up automatically.

### Changing document metadata
Edit `metadata.yaml`.  Common fields:
- `title`, `author`, `date`
- `toc: true/false`, `toc-depth: <number>`
- `geometry: margin=2.5cm`
- `fontsize: 12pt`, `papersize: a4`

### Replacing header/footer images
Overwrite `images/header.png` and/or `images/footer.png`.  Any image format that
XeLaTeX accepts works (PNG, JPEG, PDF).

### Disabling header or footer images
In `metadata.yaml`, change the relevant `\newcommand` to an empty value:
```latex
\renewcommand{\headerimage}{}   % disables header image
\renewcommand{\footerimage}{}   % disables footer image
```

### Building the PDF locally
```bash
pandoc docs/01-introduction.md docs/02-content.md docs/03-conclusion.md \
  --metadata-file=metadata.yaml \
  --pdf-engine=xelatex \
  -o document.pdf
```
Or with glob expansion (bash):
```bash
pandoc docs/*.md --metadata-file=metadata.yaml --pdf-engine=xelatex -o document.pdf
```

### Adding a bibliography
1. Create `references.bib` in the repo root.
2. Add `bibliography: references.bib` to `metadata.yaml`.
3. Append `--citeproc` to the Pandoc command in `release.yml`.

## What NOT to do

- Do not commit the generated `document.pdf` — it is a build artifact produced by CI.
- Do not remove `\newcommand{\headerimage}` or `\newcommand{\footerimage}` from
  `metadata.yaml`; set them to empty instead of removing them.
- Do not add raw LaTeX directly to Markdown chapter files; put all LaTeX customisation
  in the `header-includes` block of `metadata.yaml`.
- Do not change the workflow trigger or permissions without understanding that the
  `contents: write` permission is required to push tags and create releases.
