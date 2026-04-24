# pandoc-action

A GitHub Actions workflow that converts Markdown source files into a
publication-ready PDF using [Pandoc](https://pandoc.org) and XeLaTeX, then
publishes the result as a GitHub Release.

## Repository layout

```
.
├── docs/                    # Markdown source chapters
│   ├── 01-introduction.md
│   ├── 02-content.md
│   └── 03-conclusion.md
├── images/                  # Header / footer images used on every page
│   ├── header.png           # Printed at the top of every content page
│   └── footer.png           # Printed at the bottom of every content page
├── metadata.yaml            # Pandoc document metadata and LaTeX settings
└── .github/workflows/
    └── release.yml          # CI workflow: build PDF → tag → release
```

## Header and footer images

Every content page (including chapter-opener pages) carries a header image
at the top and a footer image at the bottom.  The images are configured in
`metadata.yaml`:

```yaml
header-image: "images/header.png"
footer-image: "images/footer.png"
```

To **replace** the images, simply overwrite `images/header.png` and/or
`images/footer.png` with your own PNG (or any format that XeLaTeX supports —
PNG, JPEG, PDF).  The image is scaled to the full text-block width
(`\textwidth`) while keeping its aspect ratio, so any size works.

To **disable** a header or footer image, set the corresponding
`\newcommand` value to empty inside the `header-includes` block in
`metadata.yaml`.  The `\ifthenelse` guard will then skip the
`\includegraphics` call entirely:

```latex
\renewcommand{\headerimage}{}   % no header image
\renewcommand{\footerimage}{}   % no footer image
```

Do **not** remove the `\newcommand` lines entirely — the `\ifthenelse`
checks rely on the commands being defined.

The feature is implemented via the `fancyhdr` LaTeX package, which is
included in the standard `texlive-latex-extra` / `texlive-fonts-recommended`
distribution installed by the workflow.

## Running locally

```bash
pandoc metadata.yaml \
  docs/01-introduction.md \
  docs/02-content.md \
  docs/03-conclusion.md \
  --pdf-engine=xelatex \
  -o document.pdf
```

## CI workflow

The workflow (`.github/workflows/release.yml`) triggers on every push to
`main` and on manual dispatch.  It:

1. Installs Pandoc and a full XeLaTeX stack.
2. Builds `document.pdf` from the Markdown sources, applying the header and
   footer images defined in `metadata.yaml`.
3. Tags the commit as `v1.<run_number>`.
4. Creates a GitHub Release and uploads `document.pdf` as an asset.
