---
name: printable
description: "Turn a study file (slideshow, doc, PDF) into a plain printable document for a black and white laser printer: headers, bullets, tables, regular fonts, no backgrounds. Never sends anything to the printer. Use when the user runs /printable or /printable custom."
model: sonnet
---

# Printable

Input: a folder (a path, or a name under the `STUDY_ROOT` environment
variable), plus the optional `custom` parameter. The result is always a
`<name> - printable.pdf` next to the chosen file. Never run `lp` or send
anything to the printer: Victor prints the PDF himself.

## Format

A slideshow becomes a regular document, not a picture of the slides.
Write it yourself: do not convert the slides with Ghostscript, image
filters or any converter.

- Keep all the content, in the original order. Do not summarize.
- Slide titles become headers (`h2` per section, `h3` per slide). Bullets
  become lists, grids become tables, quotes become blockquotes, code stays
  in a code block.
- Describe diagrams and charts as lists or tables with their data.
- Drop slides with no content (cover art, breaks, thanks, legal footer).
- Serif body at 11 pt, black on white, A4, 20 mm margins. Light gray
  fills for table headers and code only.
- Reading aids: headers in a sans-serif font, so they stand apart from the
  body. A thick gray bar on the left of each section header and a thin one
  on each subsection header, to find the block at a glance. The key idea of
  each bullet in bold, as a short label or the first phrase.
- Page number at the bottom right of every page, with a CSS `@page`
  `@bottom-right` box and `counter(page)`.
- Single-sided: Victor prints on used paper.

## With `custom`

Ask before building, each as a short multiple-choice question: font size,
margins, and whether to include speaker notes. Then build with those
answers.

## Steps

1. Resolve the folder. If it is a name, look for it under `STUDY_ROOT`;
   if the variable is empty, ask for the path and stop. List the files in
   the folder, leaving out summaries and printables already built, and ask
   which one to build (multiple choice).
2. Extract the text of the file (`pypdf` for PDF). Look at the pages that
   have no text, since they hold diagrams or charts.
3. Write the document as HTML in the scratchpad, following the format.
4. Render it to `<name> - printable.pdf` in the file's folder with Chrome
   headless (`--print-to-pdf --no-pdf-header-footer`) and give its path.
5. Open the finished PDF with `open "<path>"` so it shows on screen.

The style rules are expected to change over time: edit this file to adjust
them.
