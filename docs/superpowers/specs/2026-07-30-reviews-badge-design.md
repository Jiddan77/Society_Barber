# Reviews badge — design spec

**Date:** 2026-07-30
**Status:** Approved

## Goal

Show visitors that Society Barber is well-reviewed on Boka Direkt and Google, as a simple star-rating badge — no review text/quotes, no live API calls. Consistent with the site's current architecture: static HTML in the repo root, served by GitHub Pages, no build step, no CMS ([[project_society_barber_deploy]]).

## Current numbers (as of 2026-07-30)

- **Boka Direkt:** 5.0 ★, 174 reviews — https://www.bokadirekt.se/places/society-barber-136520
- **Google:** 5.0 ★, 23 reviews — Google Business Profile / Maps listing

These are typed in as plain static text. They are not fetched live from any API.

## Approach

Fully static, manually-maintained numbers baked directly into `index.html`. Rejected alternatives: live Google Places API fetch (adds an API key, billing, and a runtime dependency for data that barely changes week to week — inconsistent with the site's "keep it simple, static" direction); a local helper script to semi-automate updates (unnecessary extra tooling for a number that changes a few times a year).

## Placement & copy

1. **Hero section** (directly under the `<h1>`): a compact one-line badge —
   `★ 5.0 Boka Direkt (174) · ★ 5.0 Google (23)`
   Each rating links out to the respective public review page.

2. **Kontakt & Hitta oss section**: a fuller sentence —
   "Betygsatt 5.0 av 5 på Boka Direkt (174 recensioner) och 5.0 av 5 på Google (23 recensioner)."
   Same outbound links.

No review quotes/testimonials are shown — rating + count only, per user decision.

## Structured data (schema.org)

Add `aggregateRating` to the existing `LocalBusiness` JSON-LD block, sourced **only from Boka Direkt**:

```json
"aggregateRating": {
  "@type": "AggregateRating",
  "ratingValue": "5.0",
  "reviewCount": "174"
}
```

Google's numbers are **not** added to structured data. Rationale: the existing schema already has `sameAs`/`ReserveAction` pointing at the Boka Direkt listing, making it a verifiable, attributable source for `aggregateRating`. Attaching Google's review numbers to the site's own schema isn't standard practice and risks conflicting with Google's structured-data guidelines (ratings must be sourced from an identifiable, verifiable review platform — not self-aggregated across an unrelated third party). Google's rating stays visible to human visitors only, as plain text in the two placements above.

## Maintenance

All four values (hero line, Kontakt line, `ratingValue`, `reviewCount`) live directly in `index.html`. When the numbers change, edit these four spots by hand and push to `main` — GitHub Pages picks it up automatically (per the `.nojekyll` static-serve setup already in place). A note is added to `SEO-TODO.md` pointing at exactly which lines to update, so this isn't forgotten.

## Out of scope

- No review text/quotes/testimonial carousel.
- No live API integration (Google Places or otherwise).
- No changes to Boka Direkt or Google listings themselves.
- No CMS/build-step involvement — this stays consistent with the current static-only architecture.
