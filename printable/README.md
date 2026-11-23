# printable

`/printable` takes a file, or a folder to browse one level at a time until
you pick a file, and turns it into a plain document: headers, bullets,
tables and regular fonts, black on white, single-sided. Sans-serif headers,
side bars per section and bold key ideas make it easier to scan. Every page
is numbered at the bottom right. A slideshow becomes text, not a picture of
slides, and an image goes on one page in grayscale. With `/printable custom`
it asks first about font size, margins and speaker notes.

It writes only a `<name> - printable.pdf` next to the file, opens it when
finished and never sends anything to a printer. It renders with Chrome
headless. Instructions: [SKILL.md](SKILL.md).
