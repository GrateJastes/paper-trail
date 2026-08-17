---
name: tailor-cv
description: Tailor the application folder's CV copy to its jd.md under the truthfulness gate, recording every include/exclude decision in NOTES.md. Use after new-application, or whenever the candidate wants the CV adjusted for a role before it is sent.
---

# tailor-cv

The tailoring rules exist because an LLM's instinct — flatter the JD, mirror its
keywords — is exactly how false claims get onto a CV. Every step below is bounded
by the truthfulness gate in CLAUDE.md.

## Preconditions

- The folder's `<stem>.md` is a fresh copy of the general variant. If it isn't
  (or you can't tell), stop and re-copy — never tailor on top of a past
  application's CV.
- The CV has not been sent. A sent CV is frozen; a re-application gets a new
  folder.

## Steps

1. **Read `jd.md` and extract what the ad actually asks for** — the must-haves,
   the nice-to-haves, the vocabulary they use for things the candidate has done
   under other names.

2. **Swap the headline toward the job title** — their words, not a self-awarded
   grade. If the role says "Senior" and the candidate's real titles don't, the
   body evidences scope; the headline doesn't assert it.

3. **Promote the 2–3 bullets that match the ad** to the top of their sections;
   cut what the ad doesn't ask for. Promotion and cutting are the whole craft —
   rewriting bullets from scratch is rarely needed and always riskier.

4. **The truthfulness gate, applied.** For any claim, tool, or skill the tailoring
   wants to add that is not already in the general variant:
   - `grep -ri "<term>" applications/ archived-applications/` — has a past
     tailoring already ruled on it? A closed process is still binding precedent.
   - No precedent → **ask the candidate**, however plausible the inference looks.
     Their answer and reasoning become the new precedent.
   - Never present real work as running on a platform or stack it didn't run on —
     "close enough" technologies are not the same technology.

5. **Record the decisions** in NOTES.md while the reasoning is fresh: what was
   promoted and why under "How this CV was tailored"; every include/exclude call
   under "Include / exclude decisions" — the verdict *and* the reasoning, because
   this is the record future tailorings will grep.

6. **Build and verify** via the `build-pdf` skill: exactly the page budget from
   CLAUDE.md Conventions. Trim, don't spill — and don't shrink fonts.

7. **Commit** the tailored source (`docs: tailor CV for <company>`). After the
   candidate sends it, commit the exact sent PDF and add the send date to the
   process log — from that moment the CV artifacts are frozen.
