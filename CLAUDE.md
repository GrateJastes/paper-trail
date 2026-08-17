# paper-trail — instructions for Claude

This is a private job-search repo created from the paper-trail template. It is the
candidate's system of record: exactly **one** maintained document — the general CV
variant at the repo root — and one folder per application under
`applications/<date>-<company>/`, frozen as a snapshot the moment the CV is sent.
When a process ends (rejection, withdrawal, gone quiet) the folder is `git mv`'d
unchanged to `archived-applications/<date>-<company>/`. The workflow lives in the
skills under `.claude/skills/`; the reasoning behind the system is in
`docs/methodology.md`.

If the Conventions section at the bottom of this file is still unset, the repo has
not been onboarded — run the `setup` skill before anything else.

## Hard rules

- **Truthfulness gate**: never add a skill, tool, or claim to any CV or letter that
  is not grounded in the candidate's real experience — ask them first, however
  plausible the inference looks. Presenting real work as running on a platform or
  stack it does not run on is the same violation. Past include/exclude calls and
  their reasoning are recorded per application in `*/NOTES.md` —
  `grep -ri "<term>" applications/ archived-applications/` before assuming a claim
  is fair game. **Always grep both trees**: a closed process is still binding
  precedent.
- **Never edit, rebuild, or overwrite a sent CV.** A sent PDF is the exact artifact
  the company holds; a rebuild will not reproduce it byte-for-byte (renderer and
  font versions drift). A CV becomes frozen the moment it is sent. Until it is
  sent, edit freely.
- `NOTES.md` and `jd.md` in an application folder are **living** — keep updating
  them as the process advances (call dates, outcomes, prep). Freezing applies to
  the CV artifacts only. Archiving does not freeze them either: late feedback
  after a rejection still gets written down, and a revived process moves the
  folder back to `applications/`.
- **Start every tailoring from the general variant**, never from a past
  application — archived shared facts are stale by design. Lifting phrasing is the
  intended reuse path: `grep -r "<term>" applications/ archived-applications/` and
  copy by hand.
- When a JD exists only as a web page, parse and recreate it as `jd.md` rather
  than storing a link — career pages are JS-rendered and links rot. Routes and the
  verification step are in `docs/fetching-jds.md`.
- After any CV `.md` edit, rebuild the PDF (`docs/building-pdfs.md`) and verify
  `pdfinfo` reports exactly the page budget from Conventions. Trim content,
  don't shrink fonts or margins.
- Shared facts (dates, contact line, languages, headline numbers) are maintained
  in the general variant only. Sent applications are records — they go stale by
  design, and that is fine. Do not retro-fix them.
- Bullets are front-loaded: verb + claim + number in the first line, detail after
  the em-dash. Keep single-claim bullets in the general variant.
- When facts in the general variant change, remind the candidate to sync their
  LinkedIn profile (headline matches the general variant's headline).

## Git

- Commit subjects: concise conventional style (`chore:`, `docs:`, `feat:`
  sparingly), no body.
- One logical change per commit. The `applications/` and `archived-applications/`
  trees are the primary record of which version went to which company; history is
  the audit trail behind it. Move a closed folder with `git mv` so that history
  follows it.
- This repo must stay **private**. Never add a public remote, never publish its
  contents, and warn the candidate if the remote's visibility is unknown.

## Conventions — filled in by the setup skill

- Candidate: _not set — run setup_
- CV file stem: _not set_ (general variant: `<stem>_general.md` at the repo root;
  per-application copy: `<stem>.md` — the file name a recruiter saves should carry
  the candidate's name)
- Page budget: 2 A4 pages
- Canonical employment timeline: _not set_
