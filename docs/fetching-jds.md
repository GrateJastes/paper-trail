# Fetching JDs from careers pages

Career pages are usually JS-rendered — a plain HTTP fetch returns an empty shell.
The reliable path is the ATS's backing API, which returns the posting as JSON.

## Known routes

**Greenhouse** — URL contains `?gh_jid=<id>` or lives under `boards.greenhouse.io/<board>`:

```
https://boards-api.greenhouse.io/v1/boards/<board>/jobs/<id>
```

JSON with an HTML `content` field. The HTML is escaped — unescape it before
converting.

**Workday** — URL matches `*.myworkdayjobs.com/<site>/job/<path>`:

```
https://<host>/wday/cxs/<tenant>/<site>/job/<path>
```

JSON with `jobPostingInfo.jobDescription`.

**Lever** — URL matches `jobs.lever.co/<company>/<id>`:

```
https://api.lever.co/v0/postings/<company>/<id>
```

JSON with a `description` field (HTML) and structured `lists` for the bullet
sections.

**Ashby** — URL matches `jobs.ashbyhq.com/<org>/<id>`:

```
https://api.ashbyhq.com/posting-api/job-board/<org>
```

Returns the org's whole board; find the posting by `id` and use its
`descriptionHtml`.

Anything else: open the posting with browser dev tools (or an agent-driven
browser) and watch the network tab — most ATS frontends fetch the posting as JSON
from somewhere. Failing that, render the page and extract the posting body.

## Converting

```sh
pandoc -f html -t gfm --wrap=none
```

## Verifying — do not skip

Compare the `<li>` count in the source HTML against the bullet count in the
produced markdown:

```sh
grep -o '<li' posting.html | wc -l
grep -c '^\s*[-*]' jd.md
```

If they disagree, something was silently dropped — usually a nested list or a
section behind a "show more". Find it before saving. A requirement you never saw
is the worst way to lose an interview.

## Saving

`jd.md` starts with a small header — company, role title as posted, source URL,
fetch date — then the posting body. If the JD was supplied as a file, keep the
original as `jd.pdf` (frozen) and produce `jd.md` beside it anyway: markdown is
what greps, diffs, and feeds the agent.
