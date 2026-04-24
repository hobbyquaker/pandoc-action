# pandoc-action

A GitHub Actions workflow that converts Markdown source files into a
publication-ready PDF using [Pandoc](https://pandoc.org) and XeLaTeX, then
publishes the result as a GitHub Release.

## Quick start

1. **Fork or use this repository as a template.**
2. Replace the Markdown files in `docs/` with your own content.
3. Update the title, author, and date in `metadata.yaml`.
4. Optionally replace `images/header.png` and `images/footer.png` with your
   own branding images.
5. Push to `main` — GitHub Actions builds the PDF and publishes it as a
   Release automatically.

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

## Writing content

Add or remove Markdown files in `docs/`.  Chapters are included in
**alphabetical order**, so use numeric prefixes (e.g. `01-`, `02-`) to
control the chapter order.  The workflow picks them up automatically via
`docs/*.md` — no changes to the workflow file are needed.

### Supported Markdown features

Pandoc's extended Markdown supports everything you need for a polished
technical document:

| Feature             | Example syntax                              |
|---------------------|---------------------------------------------|
| Bold / Italic       | `**bold**`, `*italic*`                      |
| Inline code         | `` `code` ``                                |
| Fenced code blocks  | ```` ```python … ``` ```` (with highlighting)|
| Tables              | Pipe-table syntax                           |
| Inline math         | `$E = mc^2$`                                |
| Display math        | `$$\int … dx$$`                             |
| Footnotes           | `text[^1]` / `[^1]: note`                  |
| Blockquotes         | `> quoted text`                             |
| Images              | `![alt](path/to/image.png)`                 |

## Document metadata (`metadata.yaml`)

All document-level settings live in `metadata.yaml`:

| Field          | Purpose                                          |
|----------------|--------------------------------------------------|
| `title`        | Document title (appears on cover / in PDF metadata) |
| `author`       | Author name                                      |
| `date`         | Publication date                                 |
| `toc`          | `true` to include a Table of Contents            |
| `toc-depth`    | Number of heading levels shown in the TOC        |
| `numbersections` | `true` to number chapters and sections         |
| `geometry`     | Page margins, e.g. `margin=2.5cm`               |
| `fontsize`     | Base font size, e.g. `12pt`                     |
| `papersize`    | Paper size, e.g. `a4` or `letter`              |
| `lang`         | Document language (BCP 47 tag), e.g. `en`       |

## Header and footer images

Every page carries a header image at the top and a footer image at the
bottom.  They are configured via LaTeX commands inside the `header-includes`
block of `metadata.yaml`:

```latex
\newcommand{\headerimage}{images/header.png}
\newcommand{\footerimage}{images/footer.png}
```

**To replace** the images, overwrite `images/header.png` and/or
`images/footer.png` with your own file.  PNG, JPEG, and PDF are all
accepted.  The image is scaled to the full text-block width
(`\textwidth`) while preserving its aspect ratio.

**To disable** a header or footer image, set the command value to empty:

```latex
\renewcommand{\headerimage}{}   % no header image
\renewcommand{\footerimage}{}   % no footer image
```

> **Important:** Do _not_ remove the `\newcommand` lines entirely —
> the `\ifthenelse` guards depend on the commands being defined.

## Running locally

Install [Pandoc](https://pandoc.org/installing.html) and a XeLaTeX
distribution (e.g. TeX Live on Linux/macOS or MiKTeX on Windows), then run:

```bash
pandoc docs/*.md \
  --metadata-file=metadata.yaml \
  --pdf-engine=xelatex \
  -o document.pdf
```

> On Windows with PowerShell, replace `docs/*.md` with an explicit list
> of files since PowerShell does not expand globs the same way as bash.

## Clickable cross-references

Pandoc automatically assigns every heading an identifier built from its text
(lower-case, spaces → hyphens, punctuation stripped).  Use standard Markdown
link syntax to create a clickable in-document reference:

```markdown
See the [Introduction](#introduction) for background.
Jump to [Feature Table](#feature-table).
```

When you need a stable or shorter ID, attach an explicit anchor to the heading:

```markdown
## Cross-References {#cross-references}
```

Then link to it from anywhere:

```markdown
Read the [cross-reference guide](#cross-references).
```

To make internal links visible as coloured text in the PDF, add the following
keys to `metadata.yaml`:

```yaml
colorlinks: true
linkcolor: NavyBlue   # internal cross-references
urlcolor:  NavyBlue   # external URLs
citecolor: NavyBlue   # bibliography citations
```

See the [Cross-References section of `docs/pandoc-guide/02-content.md`](docs/pandoc-guide/02-content.md)
for a live, rendered example of every technique.

## CI workflow

The workflow (`.github/workflows/release.yml`) triggers on every push to
`main` and on manual dispatch (`workflow_dispatch`).  It:

1. Checks out the repository with full history so the tag push succeeds.
2. Installs Pandoc and a full XeLaTeX stack (`texlive-xetex`,
   `texlive-fonts-recommended`, `texlive-fonts-extra`).
3. Builds `document.pdf` from all Markdown files in `docs/`.
4. Tags the commit as `v1.<run_number>`.
5. Creates a GitHub Release and uploads `document.pdf` as a release asset.

The workflow requires the `contents: write` permission to push tags and
create releases.  This is set at the workflow level in `release.yml`.

## Common customisations

### Add a bibliography

1. Create a `references.bib` file in the repo root.
2. Add `bibliography: references.bib` to `metadata.yaml`.
3. Add `--citeproc` to the Pandoc command in `.github/workflows/release.yml`.

### Use a custom LaTeX template

Pass `--template=my-template.tex` to the Pandoc command and commit your
template file to the repository.

### Generate additional output formats

Duplicate the Pandoc step in `release.yml` with a different `-o` target
(e.g. `-o document.html` or `-o document.epub`) and add the extra file to
the `files:` list of the release step.

### Change the version scheme

Replace the `echo "VERSION=v1.${{ github.run_number }}"` line with any
versioning logic you prefer, such as reading a `VERSION` file or using a
semantic-versioning action.

## License

[MIT](LICENSE) © hobbyquaker
