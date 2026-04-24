# Content Showcase

This chapter illustrates a variety of Markdown features that Pandoc can render
into a high-quality PDF.

## Emphasis

Plain text can be made **bold**, *italic*, or ***bold and italic***.
You can also use `inline code` for short snippets.

## Lists

### Unordered List

- Pandoc supports GitHub-Flavored Markdown
- Tables, footnotes, and math are built-in extensions
- Multiple input formats can be mixed in a pipeline

### Ordered List

1. Install Pandoc and a LaTeX engine
2. Write your content in Markdown
3. Run `pandoc` to produce a PDF
4. Publish the result as a GitHub Release

## Feature Table

| Feature          | Supported | Notes                          |
|------------------|:---------:|--------------------------------|
| Bold / Italic    | ✓         | Standard CommonMark            |
| Tables           | ✓         | Pipe-table extension           |
| Code blocks      | ✓         | Syntax highlighting available  |
| Math (LaTeX)     | ✓         | Via `--pdf-engine=xelatex`     |
| Footnotes        | ✓         | `pandoc_footnotes` extension   |
| Bibliography     | ✓         | Via `--citeproc`               |

## Code Block

The following Python snippet computes the Fibonacci sequence:

```python
def fibonacci(n: int) -> list[int]:
    """Return the first *n* Fibonacci numbers."""
    sequence = [0, 1]
    for _ in range(2, n):
        sequence.append(sequence[-1] + sequence[-2])
    return sequence[:n]

print(fibonacci(10))
# [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

## Mathematics

Inline math is enclosed in single dollar signs: $E = mc^2$.

Display math uses double dollar signs and is typeset on its own line:

$$
\int_{-\infty}^{\infty} e^{-x^2}\, dx = \sqrt{\pi}
$$

The quadratic formula is another classic example:

$$
x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}
$$

## Blockquote

> *"The art of writing is the art of discovering what you believe."*
> — Gustave Flaubert

## Cross-References {#cross-references}

Pandoc automatically assigns every heading an identifier derived from its text.
The identifier is built by converting the heading to lower-case, replacing
spaces with hyphens, and stripping most punctuation — the same rules used by
GitHub Flavored Markdown.

### Automatic heading identifiers

| Heading text | Generated identifier |
|---|---|
| `# Introduction` | `#introduction` |
| `# Content Showcase` | `#content-showcase` |
| `## What is Pandoc?` | `#what-is-pandoc` |
| `## Feature Table` | `#feature-table` |
| `# Conclusion` | `#conclusion` |

Use the identifier as the target of a standard Markdown link to create a
clickable in-document reference:

```markdown
See the [Introduction](#introduction) for background.
See [Feature Table](#feature-table) for a full list of supported features.
Jump straight to the [Conclusion](#conclusion).
```

These render as live, clickable links inside the PDF (rendered with the colour
defined by `linkcolor` in `metadata.yaml`):

- See the [Introduction](#introduction) for background.
- See [Feature Table](#feature-table) for a full list of supported features.
- Jump straight to the [Conclusion](#conclusion).

### Custom anchors {#custom-anchors}

When a heading's auto-generated ID would be awkward, attach an explicit
`{#my-id}` attribute directly after the heading text:

```markdown
## Cross-References {#cross-references}

### Custom anchors {#custom-anchors}
```

You can then link to that anchor from anywhere in the document:

```markdown
Read more about [custom anchors](#custom-anchors).
```

This section itself uses a custom anchor, so the following link works even
though the section title contains mixed case:
[Back to Cross-References](#cross-references).

### Sub-chapter links

Sub-sections are referenced exactly like top-level chapters.  For example, the
[Why Automate Document Generation?](#why-automate-document-generation) section
in the Introduction and the [Mathematics](#mathematics) sub-section earlier in
this chapter are both reachable by clicking the links inline.

### Enabling coloured links in `metadata.yaml`

By default the links are black and indistinguishable from surrounding text.
Add the following keys to `metadata.yaml` to colour them:

```yaml
colorlinks: true
linkcolor: NavyBlue   # internal cross-references
urlcolor:  NavyBlue   # external URLs
citecolor: NavyBlue   # bibliography citations
```

`linkcolor`, `urlcolor`, and `citecolor` accept any colour name recognised by
the LaTeX `xcolor` package (e.g. `blue`, `red`, `teal`, `NavyBlue`).
