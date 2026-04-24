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
