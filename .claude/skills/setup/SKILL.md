---
name: setup
description: First-run onboarding for a fresh paper-trail repo — builds the candidate's general CV variant, fills the conventions in CLAUDE.md, and verifies the PDF toolchain. Use when the repo was just created from the template or when CLAUDE.md's Conventions section is still unset.
---

# setup

Onboard this repo for its owner. Everything below feeds the truthfulness gate:
the general variant built here is the ground truth every future tailoring starts
from, so nothing goes into it that the candidate has not confirmed.

## Steps

1. **Privacy check.** Confirm the repo is private (`gh repo view --json visibility`
   if a remote exists; if there is no remote yet, remind the candidate it must be
   a private one). This repo will hold salary data, interview notes, and personal
   contact details — it must never be public or a fork of a public repo.

2. **Toolchain check.** Verify `pandoc`, `weasyprint`, and `pdfinfo` are on PATH.
   Install hints are in `docs/building-pdfs.md` — including the weasyprint-on-
   system-Python gotcha. Don't proceed to step 6 until a trivial test build works.

3. **Gather source material.** Ask the candidate for whatever they already have:
   an existing CV (PDF/DOCX/Markdown), a LinkedIn profile export, or nothing but
   their memory. Read what they provide.

4. **Interview for the gaps.** Walk through their timeline employer by employer:
   exact dates, role titles as they officially were, what they actually did, and
   the numbers behind each claim (cost cut by how much? how many users? how many
   services?). Only record what they state — never round up, never infer a tool
   from a job title, never "improve" a title (no self-awarded seniority: if the
   real title carried no "Senior", the body must evidence scope instead of the
   headline asserting a grade). If a number is fuzzy, record the honest form
   ("20–30%", "40+") or leave it out.

5. **Write the general variant.** Copy `templates/CV_general.md` to the repo root
   as `<Lastname>_<Firstname>_CV_general.md` and fill it with the confirmed
   material, following the template's structure and the bullet discipline from
   CLAUDE.md (verb + claim + number first, detail after the em-dash, one claim per
   bullet). The file name a recruiter saves should carry the candidate's name.

6. **Build and trim.** Build the PDF per `docs/building-pdfs.md` and check
   `pdfinfo` reports exactly the page budget (default 2 A4 pages). Trim content
   with the candidate until it fits — cut, don't shrink fonts.

7. **Fill CLAUDE.md Conventions.** Candidate name, CV file stem, page budget,
   canonical employment timeline (this is the line LinkedIn must match).

8. **Commit** (`feat: general CV variant` or similar, concise conventional
   subject). Leave `templates/CV_general.md` in place — it stays as reference.
