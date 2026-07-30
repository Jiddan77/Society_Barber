# Reviews Badge Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Show a static "Boka Direkt 5.0 (174) / Google 5.0 (23)" star-rating badge in the hero and Kontakt areas of societybarber.se, and add `aggregateRating` (sourced from Boka Direkt) to the site's existing BarberShop JSON-LD schema.

**Architecture:** `index.html` is a single static file with two independent copies of the page content: (1) a plain-HTML `#__seo_static` `<div>` shown to non-JS crawlers then hidden by an inline `<script>`, and (2) a `<script type="__bundler/template">` block whose text content is a JSON-string-encoded copy of the *real* document (heading, sections, and its own JSON-LD schema) that a bundler script unpacks into the live DOM for real visitors. Both copies must be edited so the badge is visible to (a) simple crawlers/no-JS clients and (b) actual browser visitors. There is only **one** JSON-LD schema block with business data (`"@type": "BarberShop"`), and it lives inside the escaped template — the top-level real `<script type="application/ld+json">` block is a separate `FAQPage` schema and is not touched.

**Tech Stack:** Plain HTML/CSS, no build step, no framework. Edits to the escaped template block are done with a Python `str.replace` script (not manual find/replace) because the block is one 40KB+ line of JSON-escaped HTML — an assert-based script fails loudly if an anchor doesn't match exactly once, instead of silently corrupting the file.

## Global Constraints

- Confirmed current numbers (2026-07-30): **Boka Direkt 5.0 ★, 174 reviews** (https://www.bokadirekt.se/places/society-barber-136520); **Google 5.0 ★, 23 reviews**.
- No review text/quotes/testimonials — rating + count only.
- No live API calls (no Google Places API, no client-side fetch). All values are static text.
- `aggregateRating` is sourced **only** from Boka Direkt (`ratingValue: "5.0"`, `reviewCount: "174"`). Google's numbers are never added to structured data — visible text only.
- Google's outbound link uses the same Google Maps search URL pattern already used elsewhere in the site for the address: `https://www.google.com/maps/search/?api=1&query=Rallarvägen+43,+184+40,+Åkersberga` (there is no dedicated Google Business Profile URL recorded in the project; do not invent one).
- Every edit must be made in **both** copies of the content (`#__seo_static` div and the escaped `__bundler/template` block) unless a task says otherwise.
- File touched throughout: `/Users/alexander.jidenius/Downloads/Society_Barber/index.html`.

---

### Task 1: Add `aggregateRating` to the embedded BarberShop schema

**Files:**
- Modify: `index.html` (the `__bundler/template` script content — the schema fragment currently ending `"itemOffered": { "@type": "Service", "name": "Vaxning" } }\n    ]\n  }\n}\n</script>`, escaped as shown below)

**Interfaces:**
- Consumes: nothing (first task)
- Produces: `index.html` on disk with `aggregateRating` present in the embedded BarberShop JSON-LD. Task 5's verification step checks for the string `"ratingValue\": \"5.0\"` in the file.

- [ ] **Step 1: Write the edit script**

Create a throwaway script at `/tmp/add_aggregate_rating.py`:

```python
import pathlib

path = pathlib.Path("/Users/alexander.jidenius/Downloads/Society_Barber/index.html")
content = path.read_text(encoding="utf-8")

old = (
    '\\"name\\": \\"Vaxning\\" } }\\n    ]\\n  }\\n}\\n<\\u002Fscript>'
)
new = (
    '\\"name\\": \\"Vaxning\\" } }\\n    ]\\n  },\\n  '
    '\\"aggregateRating\\": {\\n    \\"@type\\": \\"AggregateRating\\",\\n    '
    '\\"ratingValue\\": \\"5.0\\",\\n    \\"reviewCount\\": \\"174\\"\\n  }\\n}\\n<\\u002Fscript>'
)

count = content.count(old)
assert count == 1, f"expected exactly 1 match, found {count}"

content = content.replace(old, new, 1)
path.write_text(content, encoding="utf-8")
print("OK: aggregateRating inserted")
```

- [ ] **Step 2: Run it and confirm it prints OK (not an AssertionError)**

Run: `python3 /tmp/add_aggregate_rating.py`
Expected output: `OK: aggregateRating inserted`
If it raises `AssertionError`, STOP — do not proceed. Re-read the current file around `"Vaxning"` (`grep -o 'Vaxning.\{0,200\}' index.html`) and adjust the `old` string to match exactly what's on disk before retrying.

- [ ] **Step 3: Verify the new content is present**

Run: `grep -o 'aggregateRating.\{0,150\}' /Users/alexander.jidenius/Downloads/Society_Barber/index.html`
Expected: prints the new `AggregateRating` object with `ratingValue": "5.0"` and `reviewCount": "174"` (escaped form, e.g. `\"ratingValue\": \"5.0\"`).

- [ ] **Step 4: Commit**

```bash
cd /Users/alexander.jidenius/Downloads/Society_Barber
git add index.html
git commit -m "seo: add aggregateRating (Boka Direkt) to BarberShop schema"
```

---

### Task 2: Add the hero rating badge (both content copies)

**Files:**
- Modify: `index.html` (the `#__seo_static` hero paragraph, plain HTML)
- Modify: `index.html` (the `__bundler/template` hero info-bar row, escaped)

**Interfaces:**
- Consumes: nothing new from Task 1 (independent section of the file)
- Produces: hero badge text visible in both the crawler-fallback markup and the real rendered page. Task 5 verifies both.

- [ ] **Step 1: Add the badge line to the `#__seo_static` hero paragraph**

In `index.html`, find this exact line (around line 103):

```html
  <p>Välkommen till Society Barber på Rallarvägen 43 i Åkersberga. Vi erbjuder herrklippning, fade, skäggtrimning och rakning med kniv av två passionerade barberare.</p>
```

Replace it with:

```html
  <p>Välkommen till Society Barber på Rallarvägen 43 i Åkersberga. Vi erbjuder herrklippning, fade, skäggtrimning och rakning med kniv av två passionerade barberare.</p>
  <p>★ 5.0 <a href="https://www.bokadirekt.se/places/society-barber-136520">Boka Direkt</a> (174 recensioner) · ★ 5.0 <a href="https://www.google.com/maps/search/?api=1&amp;query=Rallarvägen+43,+184+40,+Åkersberga">Google</a> (23 recensioner)</p>
```

- [ ] **Step 2: Add the badge to the real (escaped) hero info-bar**

Create `/tmp/add_hero_badge.py`:

```python
import pathlib

path = pathlib.Path("/Users/alexander.jidenius/Downloads/Society_Barber/index.html")
content = path.read_text(encoding="utf-8")

old = (
    '<span>Drop-in &amp; bokning<\\u002Fspan>\\n      <\\u002Fdiv>\\n    <\\u002Fdiv>\\n  <\\u002Fsection>'
)
new = (
    '<span>Drop-in &amp; bokning<\\u002Fspan>\\n        '
    '<span style=\\"opacity:0.4;\\">|<\\u002Fspan>\\n        '
    '<span><a href=\\"https://www.bokadirekt.se/places/society-barber-136520\\" target=\\"_blank\\" rel=\\"noopener\\" '
    'style=\\"color:inherit; text-decoration:none; border-bottom:1px solid rgba(239,233,220,0.4);\\">'
    '★ 5.0 Boka Direkt (174)<\\u002Fa><\\u002Fspan>\\n        '
    '<span style=\\"opacity:0.4;\\">|<\\u002Fspan>\\n        '
    '<span><a href=\\"https://www.google.com/maps/search/?api=1&amp;query=Rallarvägen+43,+184+40,+Åkersberga\\" '
    'target=\\"_blank\\" rel=\\"noopener\\" '
    'style=\\"color:inherit; text-decoration:none; border-bottom:1px solid rgba(239,233,220,0.4);\\">'
    '★ 5.0 Google (23)<\\u002Fa><\\u002Fspan>\\n      <\\u002Fdiv>\\n    <\\u002Fdiv>\\n  <\\u002Fsection>'
)

count = content.count(old)
assert count == 1, f"expected exactly 1 match, found {count}"

content = content.replace(old, new, 1)
path.write_text(content, encoding="utf-8")
print("OK: hero badge inserted")
```

- [ ] **Step 3: Run it and confirm success**

Run: `python3 /tmp/add_hero_badge.py`
Expected output: `OK: hero badge inserted`
If `AssertionError`: run `grep -o 'Drop-in.\{0,200\}' index.html`, compare byte-for-byte with the `old` string above (the file may have been reformatted since this plan was written), fix `old` to match, retry.

- [ ] **Step 4: Verify**

Run: `grep -c 'Boka Direkt (174)' /Users/alexander.jidenius/Downloads/Society_Barber/index.html`
Expected: `2` (one in `#__seo_static`, one in the escaped template).

- [ ] **Step 5: Commit**

```bash
cd /Users/alexander.jidenius/Downloads/Society_Barber
git add index.html
git commit -m "feat: add Boka Direkt / Google rating badge to hero"
```

---

### Task 3: Add the Kontakt-section rating line (both content copies)

**Files:**
- Modify: `index.html` (the `#__seo_static` Kontakt paragraph, plain HTML)
- Modify: `index.html` (the `__bundler/template` Kontakt grid, escaped — adds a 4th "Recensioner" column alongside the existing "Besök oss" / "Kontakt" / "Öppettider" columns)

**Interfaces:**
- Consumes: nothing new from Tasks 1–2 (independent section of the file)
- Produces: rating sentence visible in both content copies near the contact info. Task 5 verifies both.

- [ ] **Step 1: Add the sentence to the `#__seo_static` Kontakt block**

In `index.html`, find this exact block (around lines 119–122):

```html
  <p>Adress: Rallarvägen 43, 184 40 Åkersberga<br>
  Telefon: <a href="tel:+4689941060">08-94 10 60</a><br>
  E-post: <a href="mailto:Info@societybarber.se">Info@societybarber.se</a></p>
  <p>Öppettider: Mån–Fre 09–18, Lör 10–17, Sön 11–16</p>
```

Replace it with:

```html
  <p>Adress: Rallarvägen 43, 184 40 Åkersberga<br>
  Telefon: <a href="tel:+4689941060">08-94 10 60</a><br>
  E-post: <a href="mailto:Info@societybarber.se">Info@societybarber.se</a></p>
  <p>Öppettider: Mån–Fre 09–18, Lör 10–17, Sön 11–16</p>
  <p>Betygsatt 5.0 av 5 på <a href="https://www.bokadirekt.se/places/society-barber-136520">Boka Direkt</a> (174 recensioner) och 5.0 av 5 på <a href="https://www.google.com/maps/search/?api=1&amp;query=Rallarvägen+43,+184+40,+Åkersberga">Google</a> (23 recensioner).</p>
```

- [ ] **Step 2: Add a "Recensioner" column to the real (escaped) Kontakt grid**

Create `/tmp/add_kontakt_reviews.py`:

```python
import pathlib

path = pathlib.Path("/Users/alexander.jidenius/Downloads/Society_Barber/index.html")
content = path.read_text(encoding="utf-8")

old = (
    '<span style=\\"color:rgba(239,233,220,0.7);\\">11:00–16:00<\\u002Fspan><\\u002Fdiv>\\n            '
    '<\\u002Fdiv>\\n          <\\u002Fdiv>\\n        <\\u002Fdiv>\\n      <\\u002Fdiv>'
)
assert content.count(old) == 1, f"expected exactly 1 match, found {content.count(old)}"

new = (
    '<span style=\\"color:rgba(239,233,220,0.7);\\">11:00–16:00<\\u002Fspan><\\u002Fdiv>\\n            '
    '<\\u002Fdiv>\\n          <\\u002Fdiv>\\n          '
    '<div>\\n            '
    '<h3 style=\\"font-size:12px; letter-spacing:0.2em; text-transform:uppercase; color:rgba(239,233,220,0.5); '
    'margin-bottom:16px; font-weight:500;\\">Recensioner<\\u002Fh3>\\n            '
    '<p style=\\"font-size:15px; line-height:1.7; font-weight:300;\\">Betygsatt 5.0 av 5 på '
    '<a href=\\"https://www.bokadirekt.se/places/society-barber-136520\\" target=\\"_blank\\" rel=\\"noopener\\" '
    'style=\\"color:#EFE9DC; text-decoration:none; border-bottom:1px solid rgba(239,233,220,0.3);\\">Boka Direkt<\\u002Fa> '
    '(174 recensioner) och 5.0 av 5 på '
    '<a href=\\"https://www.google.com/maps/search/?api=1&amp;query=Rallarvägen+43,+184+40,+Åkersberga\\" '
    'target=\\"_blank\\" rel=\\"noopener\\" '
    'style=\\"color:#EFE9DC; text-decoration:none; border-bottom:1px solid rgba(239,233,220,0.3);\\">Google<\\u002Fa> '
    '(23 recensioner).<\\u002Fp>\\n          <\\u002Fdiv>\\n        <\\u002Fdiv>\\n      <\\u002Fdiv>'
)

content = content.replace(old, new, 1)
path.write_text(content, encoding="utf-8")
print("OK: kontakt reviews column inserted")
```

- [ ] **Step 3: Run it and confirm success**

Run: `python3 /tmp/add_kontakt_reviews.py`
Expected output: `OK: kontakt reviews column inserted`
If `AssertionError`: run `grep -o 'Öppettider<\\u002Fh3>.\{0,1100\}' index.html` and re-diff against the `old` string (whitespace/structure may have shifted since this plan was written), fix, retry.

- [ ] **Step 4: Verify**

Run: `grep -c 'Recensioner' /Users/alexander.jidenius/Downloads/Society_Barber/index.html`
Expected: `1` (only the escaped template gets a labeled "Recensioner" column heading; the `#__seo_static` copy is a plain sentence with no heading).

Run: `grep -c 'Betygsatt 5.0 av 5' /Users/alexander.jidenius/Downloads/Society_Barber/index.html`
Expected: `2` (one in `#__seo_static`, one in the escaped template).

- [ ] **Step 5: Commit**

```bash
cd /Users/alexander.jidenius/Downloads/Society_Barber
git add index.html
git commit -m "feat: add Boka Direkt / Google rating sentence to Kontakt section"
```

---

### Task 4: Document the update locations in SEO-TODO.md

**Files:**
- Modify: `SEO-TODO.md` (section 6, "Build reviews on Bokadirekt")

**Interfaces:**
- Consumes: nothing (documentation only)
- Produces: a maintenance note so future updates to the rating numbers don't require re-deriving the four edit locations from scratch.

- [ ] **Step 1: Read the current section 6 content**

Run: `grep -n -A5 '### 6. Build reviews on Bokadirekt' /Users/alexander.jidenius/Downloads/Society_Barber/SEO-TODO.md`

- [ ] **Step 2: Append a maintenance note**

Using the Edit tool (or equivalent), insert this note immediately after the existing section 6 content (do not remove anything already there):

```markdown

**Maintenance (2026-07-30):** Ratings are now live on the site as static text — Boka Direkt 5.0★ (174) and Google 5.0★ (23), shown in the hero bar, the Kontakt section, and (Boka Direkt only) in the BarberShop schema's `aggregateRating`. When these numbers change, update all four spots in `index.html`:
1. `#__seo_static` hero paragraph (plain HTML, ~line 104)
2. Hero info-bar inside the `__bundler/template` script (escaped — search for `Boka Direkt (174)`)
3. `#__seo_static` Kontakt paragraph (plain HTML, ~line 123)
4. "Recensioner" column inside the `__bundler/template` Kontakt grid (escaped — search for `Recensioner`)
5. `aggregateRating.ratingValue` / `reviewCount` in the embedded BarberShop schema (escaped — search for `aggregateRating`) — Boka Direkt numbers only.
```

- [ ] **Step 3: Verify**

Run: `grep -c 'Maintenance (2026-07-30)' /Users/alexander.jidenius/Downloads/Society_Barber/SEO-TODO.md`
Expected: `1`

- [ ] **Step 4: Commit**

```bash
cd /Users/alexander.jidenius/Downloads/Society_Barber
git add SEO-TODO.md
git commit -m "docs: note reviews-badge update locations in SEO-TODO"
```

---

### Task 5: Push and verify on the live site

**Files:** none (deployment verification only)

**Interfaces:**
- Consumes: the three commits from Tasks 1–3 (content) and Task 4 (docs)
- Produces: confirmation that GitHub Pages built successfully and the live site serves the new content

- [ ] **Step 1: Push to main**

```bash
cd /Users/alexander.jidenius/Downloads/Society_Barber
git push origin main
```

- [ ] **Step 2: Wait for the Pages build, then check its status**

Run: `sleep 30 && gh api repos/Jiddan77/Society_Barber/pages/builds/latest`
Expected: `"status":"built"` (not `"errored"`). If errored, run `gh api repos/Jiddan77/Society_Barber/pages/builds/latest -q .error.message` to see why, and fix before continuing — do not leave the build broken.

- [ ] **Step 3: Confirm the live HTML contains the new content**

Run: `curl -s https://societybarber.se/ | grep -o 'Boka Direkt (174)'`
Expected: at least one match.

Run: `curl -s https://societybarber.se/ | grep -o 'aggregateRating'`
Expected: at least one match.

- [ ] **Step 4: Manual visual check**

Open https://societybarber.se/ in a browser (hard-refresh / incognito to bypass cache) and confirm:
- The hero section shows the `★ 5.0 Boka Direkt (174) · ★ 5.0 Google (23)` line under the address/hours bar, with both ratings clickable.
- The Kontakt section shows a "Recensioner" column with the full sentence and working links.

Report back once visually confirmed — this is a static-site content change with no automated UI test, so the browser check is the real acceptance criterion (per project convention: UI changes are confirmed in a live browser, not just by grep).
