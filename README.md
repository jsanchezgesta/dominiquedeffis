# dominiquedeffis.fr

Static one-page site for Dominique Deffis, cooking classes in Paris.

Open `index.html` in a browser. That's the whole site.

## Status

| Step | State |
|---|---|
| Design and copy | Built, content still placeholder |
| Bilingual FR/EN | Working |
| Calendar from Google Sheet | Wired, `SHEET_CSV_URL` not yet set |
| Photography | Not started — placeholders in place |
| Domain `dominiquedeffis.fr` | Not registered |
| Hosting | Not deployed |
| Email at the domain | Not set up |

## Local preview

No server needed for most things:

```
open index.html
```

The calendar fetch needs a real origin, so if you're testing with a live sheet:

```
python3 -m http.server 8000
```

then visit `http://localhost:8000`.

## Deploying

See `docs/deploy.md`. Short version: push this repo to GitHub, enable Pages, point
the domain at it.

## The calendar

Dominique maintains a Google Sheet. See `docs/calendrier.csv` for the exact column
layout and `docs/deploy.md` for how to publish it and wire it up.

## Context

`CLAUDE.md` holds the design system, the bilingual mechanism, what content is real
versus invented, and the constraints on this project. Read it before changing
anything.
