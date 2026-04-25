# Advanced Features

This chapter demonstrates three additional Pandoc capabilities: footnotes,
bibliographic citations, and embedded images.  A **List of Figures** is
generated automatically at the beginning of the document (enabled by `lof:
true` in `metadata.yaml`).

## Footnotes

Pandoc supports inline footnotes as well as numbered reference-style footnotes.

Plain text can carry additional detail without cluttering the main flow by
using a footnote.[^first]  A second footnote can appear elsewhere in the same
paragraph.[^second]

[^first]: This is the first footnote.  It can contain *emphasis*, `code`, and
    even multiple paragraphs when indented by four spaces.

[^second]: Footnotes are numbered automatically by Pandoc regardless of the
    label you choose.

Inline footnotes are written directly in the text without a separate
definition:^[This is an inline footnote.  It is convenient for short,
self-contained remarks.]

## Citations and Bibliography

Pandoc integrates with BibTeX through the `--citeproc` processor.  Add a
`bibliography` key to the document metadata and then cite entries with the
`[@citekey]` syntax.

The foundational reference for typesetting with \TeX{} is @knuth1984.
\LaTeX{} builds on that work to provide a higher-level authoring environment
[@lamport1994].  Pandoc itself is described and distributed as an open-source
tool by its author [@pandoc2023].

Multiple references may be grouped in a single citation
[@knuth1984; @lamport1994].

The full bibliography is rendered automatically at the end of the document by
the `--citeproc` processor.

## Images

Pandoc embeds images using standard Markdown image syntax.  When the image
contains a non-empty caption, \LaTeX{} wraps it in a `figure` environment,
which makes it appear in the **List of Figures**.

![A sample gradient image demonstrating colour blending from teal-blue to
  warm-orange, generated programmatically for this
  example.](docs/pandoc-guide/images/sample-gradient.png){width=80%}

Images without a caption are included inline and do not create a figure entry:

![](docs/pandoc-guide/images/sample-gradient.png){width=40%}

# References {.unnumbered}

::: {#refs}
:::
