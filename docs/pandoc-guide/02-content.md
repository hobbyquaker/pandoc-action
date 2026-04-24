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
