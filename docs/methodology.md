# The paper-trail methodology

This system was distilled from a real job hunt — one run entirely out of a private
git repo with an AI agent doing the legwork. The tooling is deliberately thin:
markdown, git, pandoc, and a handful of agent skills. What made the difference was
never the tooling. It was a small set of record-keeping disciplines that most job
seekers (and most job-search tools) skip, and that turn out to be exactly the
disciplines that keep an LLM assistant honest and useful over a months-long
process. This document explains them, in the order they matter.

## 1. Records, not documents

A job search produces two kinds of files, and confusing them is the root mistake.

A **document** is something you maintain: it has one current version, and old
versions are obsolete. You have exactly one of these — the general CV variant.
It's always current, it's the only place shared facts (dates, contact line,
headline numbers) live, and it's the base every tailoring starts from.

A **record** is something that happened: it has no current version, only the
version that exists. The CV you sent to a company is a record. The company holds
those exact bytes; they may be on a hiring manager's screen right now. "Fixing" a
record doesn't update anything in the world — it just makes your copy disagree
with theirs. So sent CVs are frozen the moment they're sent: never edited, never
rebuilt (renderer and font versions drift, so a rebuild wouldn't reproduce the
bytes anyway), never retro-fitted with facts that changed later. A sent CV going
stale is not a defect. It's what being a record means.

One maintained document; everything else a frozen snapshot with a date on it.
Every other discipline in this system falls out of that split.

## 2. Git is the audit trail

One folder per application — CV source and sent PDF, the job description, and a
notes file — moved wholesale to an archive tree when the process ends. Two
properties make git the right substrate rather than a spreadsheet or an app:

**The trees are the state.** Which processes are live is answered by `ls
applications/`; what went to which company is answered by the folders' contents.
There is no tracker to keep in sync with reality, because the folder layout *is*
the tracker. Closing a process is `git mv` — history follows the folder, so even
the archive keeps its full provenance.

**History is the tamper-evidence.** Committing the sent PDF pins what was sent
and when. Committing tailoring decisions pins what you claimed and why. Months
later, in a final round, "what exactly did I tell these people in July?" is a
`git log` away — not a memory exercise.

The archive is not a graveyard. Notes files stay living even after archiving —
late feedback still gets written down, and a revived process just moves back.
Freezing applies to sent artifacts; nothing else.

## 3. The truthfulness gate

This is the discipline the whole system exists to enforce, and the one no
job-search tool ships.

An LLM tailoring a CV against a job ad has one instinct: mirror the ad. The ad
says Kubernetes and your platform ran on something adjacent? The draft now says
Kubernetes. Run that loop across a dozen applications and your CV converges on
whatever the market asks for, one plausible inference at a time — and you get to
discover the drift in an interview, under pressure, from someone who knows the
difference.

The gate is a hard rule with a paper trail behind it:

- **No claim enters any CV that isn't grounded in real experience.** However
  plausible the inference, the agent asks first. "Close enough" technologies are
  not the same technology; real work described as running on a platform it didn't
  run on is a false claim wearing a true one's clothes.
- **Every include/exclude decision is recorded** in the application's notes file
  — the verdict *and* the reasoning.
- **Past decisions are binding precedent.** Before adding anything, the agent
  greps every application folder, archived ones included. A claim ruled out in a
  closed process from March stays ruled out in September unless the facts
  changed. The archive isn't just history; it's case law.

The compounding effect is the point. Each application makes the next one more
honest and faster, because the judgment calls accumulate in greppable form
instead of evaporating.

The same gate covers self-presentation more broadly: no self-awarded seniority in
the headline (if your real titles carry no "Senior", the body evidences scope
instead of the headline asserting a grade), and interview story banks built only
from what the record supports.

## 4. Capture the job description

Job ads are the flakiest artifact in the process: careers pages are JS-rendered
(a naive fetch returns an empty shell), postings get edited mid-process, and the
link dies the moment the role closes — which is often precisely when you want to
reread what they had promised.

So the JD is captured as markdown, in the application folder, at application
time. Not a link, not a screenshot: text that can be grepped ("which of my live
processes asked for on-call?"), diffed when the posting changes, and read by the
agent when tailoring or prepping. Most ATS platforms expose a JSON API behind the
rendered page (routes for Greenhouse, Workday, Lever, and Ashby are in
`fetching-jds.md`), and one verification habit matters: count the bullet points
in the source and in the markdown. Silent truncation is how a requirement you
never saw ends up deciding an interview.

## 5. Living notes beside frozen artifacts

Each application folder carries a notes file that stays editable for the life of
the process — and after it. It holds the process log (one dated line per event),
the facts a screening call added to the JD, the tailoring decisions, follow-ups,
outcome, and lessons.

The discipline that makes it work: **write the reasoning while it's fresh.** The
tailoring rationale is written the day the CV is tailored, because the sent PDF
can never explain itself. The interview debrief is written the hour the call
ends, because that's when you still know what actually got asked, and the next
round's prep is built from exactly that. The outcome is written *before* the
folder moves to the archive — the note is why the move happened, and a move
without the note is a record with the ending torn out.

None of this is journaling for its own sake. Every section has a consumer: the
process log feeds the status overview, the include/exclude section feeds the
truthfulness gate's precedent search, the debrief feeds the next prep, the
lessons feed the next tailoring.

## 6. Interview prep is fuel, not a script

The prep document per round has a fixed anatomy: what this round is actually
deciding; a story bank of 3–5 real stories mapped to the ad's themes, each with
its number; the gaps, named first (a gap you raise yourself, paired with the
nearest real experience, lands better than one they discover); sketches of the
openers every process asks for; and questions to bring.

The stance matters more than the anatomy: everything is material to riff on, not
lines to deliver. Questions asked off the interviewer's own words beat any
prepared list, so the prep holds themes, with enough of them that some survive
being answered mid-conversation. An over-scripted candidate sounds like one.

## 7. Zero toolchain, on purpose

The build chain is pandoc and weasyprint: markdown to a 2-page A4 PDF through one
CSS file. That's the entire stack — no LaTeX, no résumé app, no dashboard, no
browser automation, nothing to update or maintain. Two constraints do carry
weight:

**The page budget is exact.** Two pages, verified with `pdfinfo` after every
edit. Over budget means cutting content — never shrinking fonts or margins, which
reads as exactly what it is. The budget is a forcing function: it's what makes
promoting 2–3 bullets and cutting the rest a real decision instead of an append.

**Bullets are front-loaded.** Verb + claim + number in the first line, detail
after the em-dash, one claim per bullet. Single-claim bullets are what make
tailoring cheap: a bullet is promoted or cut whole, never operated on.

Everything else a job-search product would sell you — portal scanning, auto-fill,
match scores — is deliberately absent. This system optimizes ten careful
applications, not two hundred cheap ones.

## 8. Privacy mechanics

A job-search repo is among the most sensitive things you can version-control:
salary bands, interview assessments in your own blunt words, an employer-legible
record of the fact that you're looking, personal contact details on every CV.

Two mechanical rules follow. **Never fork a public template** — GitHub forks of
public repos are public, permanently; create your copy with "Use this template",
which produces a detached repo that can be private. And **keep the repo private,
forever** — it never becomes portfolio material, because it can't be sanitized:
the sensitive part isn't a config file you can strip, it's the record itself,
which is the whole value.

---

## The short version

- One maintained CV; every application a dated, frozen snapshot in its own folder.
- Git is the tracker and the audit trail; closing a process is `git mv`.
- No claim without grounding; every include/exclude decision recorded; the
  archive is binding precedent — grep it.
- Capture the JD as markdown the day you apply; verify nothing was dropped.
- Write reasoning while it's fresh: tailoring notes at tailoring time, debriefs
  within the hour, outcomes before archiving.
- Prep is riffing fuel, not a script; name your gaps before they do.
- Exact page budget, front-loaded single-claim bullets, trim don't shrink.
- Template, not fork; private, forever.
