# pandoc-action

A GitHub Actions workflow that converts Markdown source files into a
publication-ready PDF using [Pandoc](https://pandoc.org) and XeLaTeX, then
publishes the result as a GitHub Release.

## Quick start

1. **Fork or use this repository as a template.**
2. Create a subdirectory under `docs/` for each document you want to produce.
3. Add numbered Markdown chapter files (`01-intro.md`, `02-content.md`, …)
   inside each subdirectory.
4. Place a `metadata.yaml`, `header.tex`, and `references.bib` in each
   document folder and customise them per document.
5. Optionally replace `images/header.png` and `images/footer.png` inside each
   document folder with your own branding images.
6. Push to `main` — GitHub Actions builds all PDFs and publishes them as a
   Release automatically.

## Repository layout

```
.
├── docs/                          # One subdirectory per document
│   ├── pandoc-guide/              # Chapters of the pandoc-guide document
│   │   ├── 01-introduction.md
│   │   ├── 02-content.md
│   │   ├── 03-conclusion.md
│   │   ├── 04-advanced.md
│   │   ├── images/               # Images used by this document
│   │   │   ├── header.png        # Printed at the top of every page
│   │   │   ├── footer.png        # Printed at the bottom of every page
│   │   │   └── sample-gradient.png
│   │   ├── header.tex            # LaTeX preamble for this document
│   │   ├── metadata.yaml         # Pandoc metadata for this document
│   │   └── references.bib        # BibTeX bibliography for this document
│   └── quick-reference/          # Chapters of the quick-reference document
│       ├── 01-overview.md
│       ├── 02-options.md
│       ├── images/               # Images used by this document
│       │   ├── header.png        # Printed at the top of every page
│       │   └── footer.png        # Printed at the bottom of every page
│       ├── header.tex
│       ├── metadata.yaml
│       └── references.bib
└── .github/workflows/
    └── release.yml                # CI workflow: build PDFs → tag → release
```

## Writing content

Add or remove Markdown files inside a document's subfolder under `docs/`.
Chapters are included in **alphabetical order**, so use numeric prefixes
(e.g. `01-`, `02-`) to control chapter order.  The workflow picks them up
automatically via `docs/<name>/*.md` — no changes to the workflow file are needed.

To add a **new document**, create a new `docs/<name>/` subdirectory with its
own `metadata.yaml`, `header.tex`, `references.bib`, and numbered `.md` files.

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

Each document folder contains its own `metadata.yaml`.  All document-level
settings live there:

| Field          | Purpose                                          |
|----------------|--------------------------------------------------|
| `title`        | Document title (appears on cover / in PDF metadata) |
| `author`       | Author name                                      |
| `date`         | Publication date                                 |
| `toc`          | `true` to include a Table of Contents            |
| `toc-depth`    | Number of heading levels shown in the TOC        |
| `numbersections` | `true` to number chapters and sections         |
| `geometry`     | Page margins, e.g. `margin=2.5cm`               |
| `fontsize`     | Base font size, e.g. `11pt` or `12pt`           |
| `mainfont`     | Main body font (XeLaTeX), e.g. `"DejaVu Serif"` |
| `sansfont`     | Sans-serif font used in headings, e.g. `"DejaVu Sans"` |
| `monofont`     | Monospaced font used in code blocks, e.g. `"DejaVu Sans Mono"` |
| `linestretch`  | Line-spacing multiplier, e.g. `1.5` for 1.5× spacing |
| `papersize`    | Paper size, e.g. `a4` or `letter`              |
| `lang`         | Document language (BCP 47 tag), e.g. `en`       |

## Header and footer images

Every page carries a header image at the top and a footer image at the
bottom.  Each document keeps its own copies of these images inside its
`images/` subfolder, which is referenced from that document's `header.tex`:

```latex
\newcommand{\headerimage}{images/header.png}
\newcommand{\footerimage}{images/footer.png}
```

**To replace** the images for a specific document, overwrite
`docs/<name>/images/header.png` and/or `docs/<name>/images/footer.png` with
your own file.  PNG, JPEG, and PDF are all accepted.  The image is scaled to
the full page width while preserving its aspect ratio.

**To disable** a header or footer image for a specific document, edit that
document's `header.tex` and set the command value to empty:

```latex
\renewcommand{\headerimage}{}   % no header image
\renewcommand{\footerimage}{}   % no footer image
```

> **Important:** Do _not_ remove the `\newcommand` lines entirely —
> the `\ifthenelse` guards depend on the commands being defined.

## Running locally

Install [Pandoc](https://pandoc.org/installing.html) and a XeLaTeX
distribution (e.g. TeX Live on Linux/macOS or MiKTeX on Windows), then run
from inside a document folder:

```bash
cd docs/pandoc-guide
pandoc *.md \
  --metadata-file=metadata.yaml \
  --include-in-header=header.tex \
  --pdf-engine=xelatex \
  --citeproc \
  -o ../../pandoc-guide.pdf
```

> On Windows with PowerShell, replace `*.md` with an explicit list of files
> since PowerShell does not expand globs the same way as bash.

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
3. Iterates over every subdirectory in `docs/`, `cd`s into it, and runs
   Pandoc using the folder's own `metadata.yaml` and `header.tex`.
4. Tags the commit as `v1.<run_number>`.
5. Creates a GitHub Release and uploads all `*.pdf` files as release assets.

The workflow requires the `contents: write` permission to push tags and
create releases.  This is set at the workflow level in `release.yml`.

## Common customisations

### Custom fonts, font size, and line spacing

XeLaTeX (the PDF engine used by this project) can use any font installed on the
system.  The CI runner has `texlive-fonts-recommended` and `texlive-fonts-extra`
installed, which include a wide range of fonts.  Set the following keys in
`metadata.yaml`:

```yaml
mainfont: "DejaVu Serif"       # body text
sansfont: "DejaVu Sans"        # headings (if using a sans-serif heading style)
monofont: "DejaVu Sans Mono"   # code blocks
fontsize: 11pt                 # base size: 10pt, 11pt, or 12pt
linestretch: 1.5               # line-spacing multiplier (1 = single, 1.5 = one-and-a-half)
```

> **Font availability:** to use a system font by name the font must be installed
> on the machine running `xelatex`.  On the CI runner the packages
> `texlive-fonts-recommended` and `texlive-fonts-extra` are already installed.
> For fonts outside those packages, add an `apt-get install` step in
> `.github/workflows/release.yml` before the Pandoc build step.

**Common font choices available out of the box**

| Purpose    | Font name examples                                  |
|------------|-----------------------------------------------------|
| Body serif | `"DejaVu Serif"`, `"TeX Gyre Termes"`, `"Latin Modern Roman"` |
| Body sans  | `"DejaVu Sans"`, `"TeX Gyre Heros"`, `"Latin Modern Sans"`    |
| Monospace  | `"DejaVu Sans Mono"`, `"TeX Gyre Cursor"`                     |

**Line spacing values**

| `linestretch` value | Effect                        |
|---------------------|-------------------------------|
| `1`                 | Single spacing (default)      |
| `1.25`              | Slightly open                 |
| `1.5`               | One-and-a-half spacing        |
| `2`                 | Double spacing                |

### Add a bibliography

1. Add entries to the `references.bib` inside the relevant document folder.
2. Ensure `bibliography: references.bib` is set in that document's
   `metadata.yaml` (already configured).
3. The `--citeproc` flag is already passed in the workflow; cite entries
   with `[@citekey]` syntax in Markdown.

### Use a custom LaTeX template

Pass `--template=my-template.tex` to the Pandoc command in the workflow and
commit your template file to the relevant document folder.

### Generate additional output formats

Duplicate the inner Pandoc invocation in `release.yml` with a different
`-o` target (e.g. `-o "../../${name}.html"`) and add the extra file pattern
to the `files:` list of the release step.

### Change the version scheme

Replace the `echo "VERSION=v1.${{ github.run_number }}"` line with any
versioning logic you prefer, such as reading a `VERSION` file or using a
semantic-versioning action.

## License

[MIT](LICENSE) © hobbyquaker
