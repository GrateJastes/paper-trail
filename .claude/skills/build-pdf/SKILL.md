---
name: build-pdf
description: Build a CV variant's PDF with pandoc + weasyprint and verify it hits the page budget exactly. Use after any CV markdown edit, or when the candidate asks for a rebuilt or checked PDF.
---

# build-pdf

Full toolchain reference (install, gotchas): `docs/building-pdfs.md`.

## Steps

1. Never rebuild a **sent** CV — check the process log in the folder's NOTES.md
   first. Sent PDFs are frozen; this skill is for variants still being edited.

2. From the **repo root** (the relative `--css` path must resolve):

   ```sh
   pandoc <path>/<variant>.md -f gfm -t html5 -s --css cv_style.css \
     --pdf-engine=weasyprint \
     --metadata pagetitle="<Candidate Name> – CV" \
     -o <path>/<variant>.pdf
   ```

3. Verify the page count matches the budget in CLAUDE.md Conventions, exactly:

   ```sh
   pdfinfo <path>/<variant>.pdf | grep Pages
   ```

4. Over budget → **trim content**, with the candidate's input on what goes: cut
   the lowest-value bullets or sections whole. Never shrink fonts, margins, or
   line height to make room — the layout is part of the document's credibility.
   Under budget is also a miss when the budget is the point: a 1.2-page CV reads
   as thin; rebalance.

5. Rebuild and re-check after every trim until the count is exact.
