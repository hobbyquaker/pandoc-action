# Essential Pandoc Options

## PDF Engine

```bash
pandoc input.md --pdf-engine=xelatex -o output.pdf
```

Use `xelatex` for full Unicode and font support.

## Metadata

Pass a YAML metadata file as the first argument:

```bash
pandoc metadata.yaml input.md -o output.pdf
```

## Table of Contents

Enable with `toc: true` in your metadata file or with the `--toc` flag:

```bash
pandoc --toc input.md -o output.pdf
```

## Numbered Sections

```bash
pandoc -N input.md -o output.pdf
```

## Custom Geometry

Set page margins in the metadata file:

```yaml
geometry: margin=2.5cm
```

## Typography: Fonts, Size, and Line Spacing

Set all typographic options in the document's `metadata.yaml`:

```yaml
mainfont: "TeX Gyre Pagella"    # body text (any XeLaTeX-accessible font)
sansfont: "TeX Gyre Heros"      # headings / sans-serif elements
monofont: "DejaVu Sans Mono"    # code blocks
fontsize: 11pt                  # 10pt | 11pt | 12pt
linestretch: 1.5                # 1 = single, 1.5 = one-and-a-half, 2 = double
```

Common TeX Live font families available on the CI runner:

| Slot      | Examples                                                          |
|-----------|-------------------------------------------------------------------|
| Serif     | `"Latin Modern Roman"`, `"TeX Gyre Termes"`, `"TeX Gyre Pagella"` |
| Sans      | `"Latin Modern Sans"`, `"TeX Gyre Heros"`, `"DejaVu Sans"`       |
| Mono      | `"Latin Modern Mono"`, `"TeX Gyre Cursor"`, `"DejaVu Sans Mono"` |
