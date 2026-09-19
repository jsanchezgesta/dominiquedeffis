# Deployment

Six steps. Roughly an hour, not counting photography.

---

## 1. Register the domain

`dominiquedeffis.fr` at OVH or Porkbun. Around 8–10 € the first year.

`.fr` requires an EU-resident registrant. Barcelona qualifies.

Register it in Dominique's name if she's willing to handle the confirmation email —
it avoids a transfer later. Otherwise yours, and move it when convenient.

---

## 2. Push to GitHub

Create a GitHub account, then a new repository. Public is fine and required for
Pages on a free account.

Either upload through the web interface (Add file → Upload files → drag this whole
folder), or from the command line:

```
git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/dominiquedeffis.git
git push -u origin main
```

---

## 3. Enable GitHub Pages

Repository → Settings → Pages.

- Source: Deploy from a branch
- Branch: `main`, folder: `/ (root)`
- Save

A minute later the site is live at `YOUR-USERNAME.github.io/dominiquedeffis`.

Then on the same screen, under Custom domain, enter `dominiquedeffis.fr` and save.
The `CNAME` file in this repo already contains that domain, so this should match.

Tick "Enforce HTTPS" once it becomes available — it can take up to 24 hours while
the certificate is issued.

---

## 4. Point the domain at GitHub

At your registrar's DNS settings, for the apex domain `dominiquedeffis.fr`, create
four A records — all four, one per IP:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

And for IPv6, four AAAA records:

```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

Then one CNAME record so the www version works:

```
Name: www    →    YOUR-USERNAME.github.io
```

Propagation takes ten minutes to a few hours. Verify these IPs against GitHub's
current documentation before entering them — they have changed in the past.

---

## 5. The calendar sheet

Create a Google Sheet with exactly these columns, header row included. See
`calendrier.csv` in this folder for a copy-paste starting point.

| date | plat_fr | plat_en | places | reservees |
|---|---|---|---|---|
| 2026-10-08 | Pâté en croûte de gibier | Game pâté en croûte | 6 | 6 |

Rules for Dominique:
- Dates always `YYYY-MM-DD`. The site writes them out properly in both languages.
- Both the French and English dish name are needed.
- `reservees` is updated by hand as bookings come in.
- Never delete a past class. The site hides it automatically.
- Nothing private in this sheet — it's publicly readable. Customer names and
  contact details go in a separate file.

Publish it: File → Share → Publish to web → select the tab → CSV → Publish.

Copy the URL it gives you and paste it into `index.html`, near the top of the
`<script>` block:

```js
var SHEET_CSV_URL = "https://docs.google.com/spreadsheets/d/e/...&output=csv";
```

Commit and push. Google caches the published CSV for a few minutes, so edits take
that long to appear.

If the fetch ever fails — sheet unpublished, tab renamed, Google down — the site
falls back to the dates written into the HTML and shows a line asking visitors to
write for current availability. It does not break.

---

## 6. Email at the domain

Optional, do it last.

Simplest: a forward from `bonjour@dominiquedeffis.fr` to whatever inbox she already
uses. OVH includes this free. Replies will come from her personal address, which
may or may not bother her.

Proper mailbox: Zoho Mail has a free tier for one domain. More setup — MX records,
SPF, DKIM — but she sends and receives as the domain.

Whichever you choose, update the two `mailto:` links and the footer in `index.html`.

---

## Iterating after launch

Change `index.html`, commit, push. Pages redeploys in under a minute. Every version
is in the git history, so anything can be rolled back.
