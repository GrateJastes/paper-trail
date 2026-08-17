---
name: fetch-jd
description: Capture a job description from a careers-page URL as greppable markdown (jd.md), going through the ATS backing API when the page is JS-rendered. Use when a JD exists as a web page, or when a fetched careers page comes back as an empty shell.
---

# fetch-jd

Career pages are usually JS-rendered, so a plain fetch returns an empty shell —
and links rot, which is why the JD is recreated as `jd.md` in the application
folder instead of stored as a URL.

## Steps

1. Identify the ATS from the URL and go at the backing API. Known routes are in
   `docs/fetching-jds.md` (Greenhouse, Workday, Lever, Ashby). If the ATS is not
   listed there, check the page's network requests for a JSON API, or fall back
   to rendering the page (browser tooling) and extracting the posting body.

2. Convert the HTML body to markdown:
   `pandoc -f html -t gfm --wrap=none`. Unescape HTML entities first where the
   API returns escaped HTML (Greenhouse does).

3. **Verify nothing was dropped**: compare the `<li>` count in the source HTML
   against the bullet count in the markdown. If they differ, find what fell out
   before saving.

4. Save as `jd.md` with a small header: company, role title, source URL, fetch
   date. If an original file (PDF) was supplied instead, keep it as `jd.pdf` —
   frozen — and still produce `jd.md` beside it.

5. Pull the hard facts (salary band if posted, location/remote policy, contract
   type) into NOTES.md "Role facts".

`jd.md` stays living: if the posting is edited or clarified during the process,
update it and note the change in the process log.
