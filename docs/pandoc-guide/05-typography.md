# Typography

XeLaTeX gives you full control over the visual appearance of your document.
This chapter explains how to choose custom fonts, adjust the base font size,
and control line spacing — all through entries in `metadata.yaml`.

## Custom Fonts

XeLaTeX can use any font that is installed on the host system.  Pandoc's
default LaTeX template exposes three font slots:

| `metadata.yaml` key | Applies to                                     |
|---------------------|------------------------------------------------|
| `mainfont`          | Body text (paragraphs, captions, …)            |
| `sansfont`          | Headings when a sans-serif heading style is used |
| `monofont`          | Inline code and fenced code blocks             |

Set them in `metadata.yaml` by name exactly as the font appears in the system:

```yaml
mainfont: "DejaVu Serif"
sansfont: "DejaVu Sans"
monofont: "DejaVu Sans Mono"
```

The CI runner has `texlive-fonts-recommended` and `texlive-fonts-extra`
installed, which provide a large collection of fonts.  A selection of
commonly used families is shown below.

### Serif body fonts

| Font name              | Character                              |
|------------------------|----------------------------------------|
| `"Latin Modern Roman"` | Default LaTeX look, very clean         |
| `"TeX Gyre Termes"`    | Times New Roman-compatible             |
| `"TeX Gyre Pagella"`   | Palatino-compatible, elegant           |
| `"TeX Gyre Schola"`    | Century Schoolbook-compatible          |
| `"DejaVu Serif"`       | Humanist serif, good Unicode coverage  |

### Sans-serif body fonts

| Font name            | Character                              |
|----------------------|----------------------------------------|
| `"Latin Modern Sans"` | Default LaTeX sans                    |
| `"TeX Gyre Heros"`   | Helvetica-compatible                   |
| `"TeX Gyre Adventor"` | Avant Garde-compatible                |
| `"DejaVu Sans"`      | Humanist sans, good Unicode coverage   |

### Monospaced fonts (code blocks)

| Font name              | Character                              |
|------------------------|----------------------------------------|
| `"Latin Modern Mono"`  | Classic LaTeX typewriter look          |
| `"TeX Gyre Cursor"`    | Courier-compatible                     |
| `"DejaVu Sans Mono"`   | Monospaced, great readability            |

### Adding a font not in TeX Live

If you need a font that is not bundled with TeX Live, add an installation step
to `.github/workflows/release.yml` before the Pandoc build step:

```yaml
- name: Install extra fonts
  run: sudo apt-get install -y fonts-liberation
```

After installation the font is available to XeLaTeX by its family name (e.g.
`"Liberation Serif"`).  Alternatively, commit the font files to the repository
and reference them by relative path in `metadata.yaml`:

```yaml
mainfont: "fonts/MyFont-Regular.otf"
```

## Font Size

The `fontsize` key sets the base font size for the entire document.  XeLaTeX's
standard document classes support three values natively; other sizes require
the `extsizes` package (not needed here — stick to the three listed values for
reliable results):

```yaml
fontsize: 10pt   # compact; good for reference documents
fontsize: 11pt   # slightly larger; comfortable for most documents
fontsize: 12pt   # large; best for accessibility or formal reports
```

All relative sizes in the document (section headings, captions, footnotes, …)
scale automatically with the chosen `fontsize`.

## Line Spacing

The `linestretch` key accepts a decimal multiplier applied to the normal
(single-spaced) baseline distance:

```yaml
linestretch: 1     # single spacing — default
linestretch: 1.25  # slightly open; easier to read on screen
linestretch: 1.5   # one-and-a-half spacing; common for drafts
linestretch: 2     # double spacing; required by some submission guidelines
```

> **Note:** `linestretch` affects paragraph text only.  Code blocks and
> footnotes use their own spacing and are not stretched.

## Complete Typography Example

The following `metadata.yaml` snippet combines all three settings into a
readable, modern-looking document:

```yaml
mainfont: "TeX Gyre Pagella"
sansfont: "TeX Gyre Heros"
monofont: "DejaVu Sans Mono"
fontsize: 11pt
linestretch: 1.25
```

This produces Palatino-style body text, a Helvetica-style heading font, a
clear monospaced code font, 11 pt body size, and slightly open line spacing —
a combination that is easy to read both in print and on screen.
