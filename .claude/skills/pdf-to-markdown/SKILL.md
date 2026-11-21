---
name: pdf-to-markdown
description: Use whenever a task involves reading, analyzing, summarizing, extracting data from, or clipping a PDF file (any .pdf path or attachment). Never read or parse PDF bytes directly. Always convert the PDF to Markdown first using MarkItDown, then work from the resulting .md file. Triggers on "PDF", "read this PDF", "summarize the PDF", "import this PDF into my vault", or any file path ending in .pdf.
---

# PDF to Markdown

Never read a PDF directly (no PDF byte parsing, no PDF-aware Read calls, no
opening it in a PDF viewer tool). Always convert it to Markdown first with
MarkItDown, then read and analyze the generated `.md` file instead.

## 1. Make sure MarkItDown is available

Check first, since it may already be installed:

```bash
python3 -c "import markitdown" 2>/dev/null && echo OK || echo MISSING
```

If `MISSING`, install it:

```bash
git clone https://github.com/microsoft/markitdown.git /tmp/markitdown
cd /tmp/markitdown
pip install -e 'packages/markitdown[all]'
```

(If `markitdown` is already on `PATH` or importable, skip cloning/installing.)

## 2. Convert the PDF to Markdown

Convert into a sibling `.md` file (don't overwrite the source PDF):

```bash
markitdown "path/to/file.pdf" -o "path/to/file.md"
```

For a PDF inside this vault, prefer writing the output next to where a
converted note belongs (e.g. `Clippings/` for imported reference material),
using the PDF's title/filename to name the note per this vault's existing
naming conventions.

## 3. Analyze the Markdown, not the PDF

Once `file.md` exists, use it for every downstream step — reading, summarizing,
extracting sections, or turning it into an Obsidian note. Do not fall back to
reading the original PDF even if the Markdown conversion looks imperfect;
instead fix up the Markdown output directly.

## Notes

- If MarkItDown reports the PDF is scanned/image-only and produces little or
  no text, say so explicitly rather than silently falling back to reading the
  PDF directly.
- Clean up any temporary clone (`/tmp/markitdown`) once installed; it only
  needs to be cloned once per environment.
