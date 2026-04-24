# Introduction

## What is Pandoc?

[Pandoc](https://pandoc.org) is a universal document converter written in Haskell.
It can read and write dozens of markup and document formats, including Markdown,
HTML, LaTeX, EPUB, DOCX, and PDF.
With a single command, Pandoc transforms plain text source files into polished,
publication-ready documents.

## Why Automate Document Generation?

Writing documentation in plain Markdown offers several advantages:

- **Version control** — text files integrate naturally with Git, enabling diffs,
  blame, and branch-based workflows.
- **Collaboration** — authors work in their favourite text editors without
  needing proprietary software.
- **Reproducibility** — a CI/CD pipeline converts the same sources into the
  same output every time, eliminating "works on my machine" formatting issues.
- **Multi-format publishing** — one source, many targets: PDF, HTML, EPUB, and
  more, generated automatically on every push.

By combining Pandoc with GitHub Actions, this project demonstrates how to build
a fully automated documentation pipeline that tags releases and publishes a PDF
artifact without any manual steps.

## Structure of This Document

This document is composed of three chapters:

1. [**Introduction**](#introduction) (this chapter) — motivation and tooling overview.
2. [**Content Showcase**](#content-showcase) — a tour of the Markdown features that Pandoc supports,
   including a dedicated [Cross-References](#cross-references) section.
3. [**Conclusion**](#conclusion) — a summary of what was demonstrated and suggested next steps.
