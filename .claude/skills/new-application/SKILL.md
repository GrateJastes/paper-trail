---
name: new-application
description: Open a new application folder — copies the general CV variant in, starts NOTES.md from the template, and captures the JD. Use when the candidate wants to apply to a role or asks to start a new application.
---

# new-application

## Steps

1. Create `applications/$(date +%F)-<company-slug>/` (lowercase, hyphenated
   company name). The folder date convention is **the date the CV is sent** — if
   the send ends up on a later day, `git mv` the folder to the send date before
   committing the sent artifact. Process start, if different, belongs in NOTES.md.

2. Copy the general variant in as `<stem>.md` (stem from CLAUDE.md Conventions —
   the per-application copy drops the `_general` suffix). Never start from a past
   application's CV.

3. Create `NOTES.md` from `templates/NOTES.md` with the title line filled
   (`# <Company> — <Role title as posted>`).

4. Capture the JD: if it's a URL, run the `fetch-jd` skill; if it's a file, save
   the original as `jd.pdf` and convert it to `jd.md` as well. Record salary band,
   location, and other hard facts from it under "Role facts" in NOTES.md.

5. Commit the opened folder (`docs: open <company> application`).

Then hand over to `tailor-cv`.
