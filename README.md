# paper-trail

**A job hunt you can grep.**

paper-trail turns a private git repo + [Claude Code](https://claude.com/claude-code)
into the system of record for your job search. One maintained CV; every
application a dated, frozen snapshot in its own folder; an agent that does the
legwork — JD parsing, tailoring, PDF builds, interview prep, process tracking —
under hard rules that keep it honest.

No app, no dashboard, no auto-apply. Markdown, git, pandoc, and discipline.

> **⚠️ Use the template — never fork.** Forks of public repos are public,
> permanently, and this repo will hold salary data, interview notes, and your
> personal details. Click **"Use this template"** (or
> `gh repo create my-jobhunt --template <this-repo> --private`) to get a
> detached **private** copy. Keep it private forever.

## Why this exists

An LLM is genuinely good at job-search legwork — and has one dangerous instinct:
mirroring the job ad. Left alone, it will nudge your CV toward whatever each
posting asks for, one plausible inference at a time, until you get to defend a
claim you never made in an interview you can't leave.

paper-trail's answer is record-keeping discipline, enforced by the repo's
`CLAUDE.md`:

- **The truthfulness gate.** No claim enters a CV without grounding in your real
  experience — the agent asks instead of inferring. Every include/exclude
  decision is recorded with its reasoning, and past decisions are **binding
  precedent**: the agent greps all previous applications (archived ones included)
  before ruling on a claim. Each application makes the next one more honest.
- **Records vs. documents.** The general CV variant is the one maintained
  document. A sent CV is a frozen record — never edited, never rebuilt, exactly
  the bytes the company holds.
- **Git as the audit trail.** The folder trees are the tracker; closing a process
  is `git mv`, so history follows the folder. "What exactly did I send them in
  July?" is a `git log` away.

The full reasoning: [docs/methodology.md](docs/methodology.md).

## Quickstart

Requirements: [Claude Code](https://claude.com/claude-code), `git`, `pandoc`,
`weasyprint`, `pdfinfo` (install notes in
[docs/building-pdfs.md](docs/building-pdfs.md)).

1. **Use this template** → create a **private** repo → clone it.
2. `claude` in the repo, then run the **setup** skill. It checks the toolchain,
   ingests your existing CV or interviews you from scratch, builds your general
   variant, and fills the conventions in `CLAUDE.md`.
3. Found a role? Run **new-application** and go.

## The workflow

| Skill | What it does |
|---|---|
| `setup` | One-time onboarding: toolchain check, your general CV variant, conventions |
| `new-application` | Opens `applications/<date>-<company>/` with a fresh copy of the general variant |
| `fetch-jd` | Captures the job ad as greppable `jd.md` via the ATS API (Greenhouse, Workday, Lever, Ashby), with a nothing-was-dropped check |
| `tailor-cv` | Headline swap, promote 2–3 matching bullets, cut the rest — under the truthfulness gate, decisions recorded in `NOTES.md` |
| `build-pdf` | pandoc + weasyprint build, verified against an exact page budget |
| `interview-prep` | Per-round prep doc: story bank, gaps named first, questions as themes to riff on — fuel, not a script |
| `status` | Where every live process stands, which follow-ups are due (read-only) |
| `close-application` | Outcome + lessons written first, then `git mv` to the archive |

A process's whole life in one folder:

```
applications/2026-08-14-example-corp/
├── Doe_Jane_CV.md        # tailored source — frozen once sent
├── Doe_Jane_CV.pdf       # the exact bytes the company received
├── jd.md                 # the ad, captured as markdown (links rot)
├── NOTES.md              # living: process log, decisions, debriefs, outcome
└── prep/                 # per-round interview prep docs
```

Rejected, withdrawn, or gone quiet → the folder moves unchanged to
`archived-applications/`, where it keeps working for you: its recorded decisions
stay binding precedent, and its `NOTES.md` stays open for late feedback.

## Repo layout

| Path | Purpose |
|---|---|
| `<You>_CV_general.md` | The one maintained document — every tailoring starts here (created by `setup`) |
| `applications/` | One folder per live application |
| `archived-applications/` | Closed processes, full record intact |
| `cv_style.css` | The single shared stylesheet — 2-page A4 layout |
| `templates/` | Skeletons for the general CV and `NOTES.md` |
| `.claude/skills/` | The eight workflow skills |
| `docs/` | [Methodology](docs/methodology.md) · [Fetching JDs](docs/fetching-jds.md) · [Building PDFs](docs/building-pdfs.md) |
| `CLAUDE.md` | The hard rules — always in the agent's context |

## How it compares

Bigger, feature-rich frameworks exist and are good at what they do:
[career-ops](https://github.com/santifer/career-ops) (multi-CLI, portal scanning
across 100+ companies, scoring rubrics, TUI dashboard) and
[ai-job-search](https://github.com/MadsLorentzen/ai-job-search) (portal scrapers,
LaTeX pipeline, Gmail sync). If you want volume tooling — scanning, scoring,
auto-fill — use them.

paper-trail is the small, strict one: zero toolchain beyond pandoc, git itself as
the tracker, and the truthfulness gate + precedent record as the core feature.
It optimizes ten careful applications, not two hundred cheap ones.

## FAQ

**Why no auto-apply?** Because the point is the opposite of volume. Every
artifact here is something you reviewed and stand behind — that's what the audit
trail is *of*.

**Why markdown + pandoc, not LaTeX or a builder app?** The CV must be diffable,
greppable, and cheap for an agent to edit under rules. One CSS file gives a clean
2-page A4 layout; anything more is maintenance.

**Does it work with other agent CLIs?** The skills are plain markdown
(`SKILL.md`), and the rules live in `CLAUDE.md` — porting to another agent's
convention is mostly renaming. Built and tested with Claude Code.

**Can I change the page budget / language / style?** Yes — budget lives in
`CLAUDE.md`'s Conventions, style in `cv_style.css`. The discipline is the
product; the defaults are just defaults.

## License

[MIT](LICENSE)
