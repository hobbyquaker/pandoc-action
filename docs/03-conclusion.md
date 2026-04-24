# Conclusion

## Summary

This document has demonstrated a complete, automated documentation workflow
built on two open-source tools:

- **Pandoc** — for converting Markdown source files into a publication-ready PDF.
- **GitHub Actions** — for running the conversion automatically on every push to
  `main`, tagging the commit, and publishing the PDF as a GitHub Release asset.

Along the way, [Chapter 2 — Content Showcase](#content-showcase) showcased the
breadth of Markdown features that Pandoc handles natively: rich text emphasis,
nested lists, pipe tables, fenced code blocks with syntax highlighting, inline
and display LaTeX mathematics, and blockquotes.  It also demonstrated how to
create [clickable cross-references](#cross-references) to any chapter or
sub-chapter — including [custom anchors](#custom-anchors) — that render as live
links inside the PDF.

## Next Steps

The following enhancements are worth exploring as the project grows:

1. **Custom LaTeX templates** — supply a `--template` file to control fonts,
   headers, footers, and cover-page layout precisely.
2. **Table of Contents** — already enabled via `toc: true` in `metadata.yaml`;
   adjust `toc-depth` to control how many heading levels appear.
3. **Bibliography & citations** — add a `references.bib` file and pass
   `--citeproc` to Pandoc for automatic reference formatting.
4. **Multiple output formats** — extend the workflow to produce an HTML site or
   EPUB alongside the PDF, all from the same Markdown sources.
5. **Semantic versioning** — replace the simple `v1.<run_number>` scheme with
   a commit-message-driven semver bump (e.g. using
   [`mathieudutour/github-tag-action`](https://github.com/mathieudutour/github-tag-action)).

With these additions, this repository can serve as the single source of truth
for a professional, multi-format documentation suite — maintained entirely in
plain text and published automatically on every change.
