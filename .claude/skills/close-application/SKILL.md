---
name: close-application
description: Close an application — record the outcome and lessons in NOTES.md, then git mv the folder to archived-applications/ so history follows it. Use on a rejection, a withdrawal, an accepted offer elsewhere, or a process gone quiet for good.
---

# close-application

Archiving is a **move, not a freeze**. The CV artifacts were already frozen at
send time; NOTES.md stays living forever — late feedback after a rejection still
gets written down, and a revived process simply moves back.

## Steps

1. **Record the outcome first, while the reasoning is fresh** — in NOTES.md
   under "Outcome": what ended the process, on what date, in whose words. That
   note is *why* the folder moves; the move without the note is a record with the
   ending torn out.

2. **Write the Lessons section** with the candidate: what this process taught
   them about the CV (a bullet that never landed?), the interviewing, or the
   targeting. One honest paragraph beats a retrospective template.

3. Commit the NOTES.md update (`docs: record <company> outcome`).

4. `git mv applications/<folder> archived-applications/<folder>` — unchanged,
   whole. Commit the move separately (`chore: archive <company>`), so the history
   of the move is itself clean.

5. If the outcome carries a fact that belongs in the general variant's future
   (e.g. a repeated signal that a bullet overclaims or a skill keeps being asked
   for), raise it — don't silently edit the general variant.

## Reviving

The process comes back? `git mv` the folder back to `applications/`, note the
revival in the process log, and carry on. Nothing else changes — that's the point
of move-not-freeze.
