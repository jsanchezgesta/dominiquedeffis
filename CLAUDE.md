# dominiquedeffis.fr

Marketing site for Dominique Deffis, who teaches high-end cooking classes in Paris.
Built and maintained by her nephew Javi (Barcelona). She is not technical and will
not edit code — the only thing she maintains herself is a Google Sheet holding the
class calendar.

## What this is, technically

One hand-written HTML file. No build step, no framework, no package.json, no
dependencies beyond two Google Fonts. Deployed as a static file.

**Do not introduce a build step, a framework, or a bundler.** If a change seems to
need one, say so and stop rather than adding it. The whole value of this setup is
that the site is a single file anyone can open, read and upload.

- `index.html` — the entire site
- `CNAME` — custom domain for GitHub Pages
- `assets/images/` — photography (see below; currently empty)
- `docs/` — deployment notes and the calendar sheet template

## Audience and positioning

Two audiences, roughly equal weight:

1. Wealthy Parisians who cook seriously and want technique they don't have
2. Foreign visitors to Paris looking for something better than a tourist cooking class

Consequences that should survive any redesign:
- The site is bilingual FR/EN. Every visible string has both.
- Prices are high and are stated plainly, not hidden behind "contact us".
- The tone is confident and unsentimental. No "passion for food", no "culinary
  journey", no exclamation marks. She is good at this and the copy assumes it.
- Booking is by email, confirmed by hand. This is deliberate — it suits the price
  point. Do not add a checkout flow unless asked.

## Design system

Established in the current build; keep it unless explicitly asked to change direction.

**Colour** (CSS custom properties on `:root`, with a dark-mode block):
- `--prune: #3A222B` — dominant dark, used for the booking section and buttons
- `--oyster: #E8E4DB` / `--oyster-deep: #DAD4C8` — section grounds
- `--paper: #F4F1EB` — page background
- `--brass: #9A7B45` — the only accent; used sparingly for kickers and rules
- `--ink: #241B20` — body text

The palette is the Haussmann dining room, not gold-on-black luxury cliché.
Restraint is the point — if an accent colour is doing decorative work rather than
carrying information, remove it.

**Type**
- Display: Bodoni Moda (Didone; Paris by origin). Italics used for emphasis in the
  h1 and section headings.
- Everything else: Jost, weight 300.
- Avoid all-caps labels except the wordmark. Avoid letterspaced eyebrow labels
  beyond the existing `.kicker`.

**Layout**
- Hero is centred; everything below is left-aligned within a max 1140px wrap.
- The photo mosaic is the hero's real payload. The design is roughly 70%
  photography by weight — it only works once real images are in.

## The bilingual mechanism

Every translatable element carries a `data-en` attribute holding the English. The
French lives in the element's own HTML. On load, a script copies the French into
`data-fr`, then swaps `innerHTML` between the two attributes when the FR/EN toggle
is clicked.

**When adding any new visible text, add `data-en` to it.** Text without `data-en`
will stay French when a visitor switches to English.

Calendar rows injected from the sheet set both `data-fr` and `data-en` explicitly at
creation, so they participate in the toggle like static content.

## The calendar

Read from a published Google Sheet at page load. `SHEET_CSV_URL` at the top of the
script holds the CSV link; when it is empty, the hardcoded rows in the HTML are used
instead and a note appears telling visitors to write for current availability.

Sheet columns, in order, with a header row:

| date | plat_fr | plat_en | places | reservees |

- `date` must be `YYYY-MM-DD`. The page formats it for each language.
- `places` is total seats, `reservees` is seats booked. The page computes what's
  left and prints "Complet" / "Full" at zero.
- Rows with a date before today are hidden automatically. She never deletes rows.
- Bad rows are skipped rather than breaking the page. If nothing renders, the
  fallback note shows.

She updates `reservees` by hand as bookings come in. This is the known weak point
of the design and an accepted tradeoff.

## Photography

`assets/images/` holds real photos now, sourced from Dominique's own Google Drive
and Instagram (confirmed as hers, not stock). The homepage mosaic (7 shots) and the
about-section portrait are all real. Alt text is bilingual via `data-alt-en` /
`data-alt-fr` on the `<img>` (the toggle script handles this — see the script's
`register()`/`applyLang()`).

Do not add stock photography (Unsplash, Wikimedia Commons, etc.) back in. If a new
section needs a photo and no real one exists yet, use the honest placeholder
treatment instead (dashed border + bracketed text, as used in the reviews section)
rather than a generic stand-in.

## Content status — what is real and what is invented

**Confirmed real:**
- Her name, Paris, high price point, elaborate dishes, the two audiences above
- Phone (+33 6 31 34 40 04) and address (Le Havre & Paris)
- All dish names and photos on the repertoire page (`recipes.html`) — sourced from
  her own Drive folders (Salée / Sucrée / Table), not invented
- Calendar dates — live from her Google Sheet (see "The calendar" above)
- Her portrait and the homepage mosaic photos

**Still invented** and awaiting her real answers:
- Prices (450 € / 650 € / 3 200 €)
- Email address (bonjour@dominiquedeffis.com)
- Her biography and years of experience (the "Qui vous reçoit" prose)
- The dish *descriptions* on the repertoire page — the dishes and photos are real,
  but the technique-focused sentences under each one are written copy, not her words
- Guest reviews — the "Ce qu'on en dit" section is still bracketed placeholder,
  waiting on real testimonials

Do not present invented content to Dominique as though it were drafted from her
information. Flag it.

## Working agreements

- Keep the file readable. CSS stays in the `<style>` block, organised by section
  with the existing comment headers.
- Test dark mode when touching colour. The dark block mirrors every token.
- Keep it responsive to ~380px and keep visible focus styles.
- Respect `prefers-reduced-motion`; there is deliberately almost no motion.
- Before adding a section, ask whether it earns its place. The restraint is the
  design.
