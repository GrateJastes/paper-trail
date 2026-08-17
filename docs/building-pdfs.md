# Building CV PDFs

Markdown → HTML → PDF, via pandoc and weasyprint, styled by the single shared
`cv_style.css` at the repo root. Every variant at any depth uses that one file.

## Install

- **pandoc**: `brew install pandoc` or `apt install pandoc`.
- **weasyprint**: `pipx install weasyprint --python /usr/bin/python3` — build its
  venv on **system** Python. On Homebrew Python the loader fails to find the
  system GTK libraries (`libgobject-2.0-0`).
- **pdfinfo** (page-count check): part of `poppler-utils` (`apt install
  poppler-utils` / `brew install poppler`).

## Build

Run from the repo root so the relative `--css` path resolves:

```sh
pandoc applications/<date>-<company>/<stem>.md -f gfm -t html5 -s --css cv_style.css \
  --pdf-engine=weasyprint \
  --metadata pagetitle="<Candidate Name> – CV" \
  -o applications/<date>-<company>/<stem>.pdf
```

Swap both paths for `<stem>_general.md` / `.pdf` to build the general variant.
If `weasyprint` isn't on PATH, point at it explicitly:
`--pdf-engine="$HOME/.local/bin/weasyprint"`.

## Verify

```sh
pdfinfo <file>.pdf | grep Pages
```

The count must equal the page budget in CLAUDE.md's Conventions — exactly. Over:
trim content, never fonts or margins. Noticeably under: the CV reads thin;
rebalance.

## Two facts about the artifacts

- Committed sent PDFs are the exact bytes the company received. A later rebuild
  will **not** reproduce them — weasyprint and font versions drift — so never
  rebuild a sent CV. There is nothing to fix; it's a record.
- The PDF is built from HTML, so the markdown can use the small set of HTML
  blocks the stylesheet knows: `.headline`, `.contact`, `.job` (with `.role`,
  `.company`, `.sep`, `.dates`), `.tagline`, and `table.skills`. See
  `templates/CV_general.md` for all of them in use.
