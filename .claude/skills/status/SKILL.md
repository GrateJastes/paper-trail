---
name: status
description: Report where every live application stands by reading each folder's NOTES.md process log. Use when the candidate asks for an overview, what's pending, or which follow-ups are due. Read-only.
---

# status

Read-only: this skill changes nothing, it reports.

## Steps

1. For every folder under `applications/`, read `NOTES.md` — primarily the
   process log (newest entries), Follow-ups, and Role facts.

2. Report per application: company and role, current stage, date of the last
   event, and the next action with its owner (candidate's move vs. waiting on
   them).

3. Flag, without acting:
   - **Follow-ups due** — promised dates that have passed or are imminent.
   - **Gone quiet** — no process-log event in 3+ weeks; suggest a nudge or, if
     the candidate considers it dead, the `close-application` skill. Suggest
     only — the candidate decides what a silence means.
   - **Log gaps** — a folder whose NOTES.md has no process log entries is a
     record with a hole in it; point it out.

4. Keep the output compact: one block or row per application, dates absolute
   (YYYY-MM-DD), no percentages or scores — this is a record, not a funnel
   dashboard.
