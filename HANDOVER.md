# Society Barber — Handover Document
**Date:** 2026-06-29  
**Project:** Barbershop website for Society Barber, Åkersberga, Sweden

---

## Overview

Society Barber's website is published on GitHub Pages and fully SEO-optimized. The site is built with a proprietary design tool (Claude Design) that bundles all content into a single `index.html` file of 2.1 MB.

**Live URL:** https://societybarber.se  
**GitHub repo:** https://github.com/Jiddan77/Society_Barber  
**Local copy:** `/Users/alexander.jidenius/Downloads/Society_Barber/`

---

## What Has Been Done

### Technical SEO
- `lang="sv"` on the `<html>` tag
- Viewport tag in outer `<head>`
- Favicon + Apple touch icon (`assets/logo-sign.png`)
- Google Fonts stylesheet for Cormorant Garamond + Jost
- `theme-color: #1a1a18` for mobile browsers
- `404.html` — unknown URL paths (e.g. `/kontakt/`) redirect automatically to the correct section

### Meta & Canonical
- Static SEO meta tags in `<head>` for crawlers that don't run JavaScript: canonical, meta description, Open Graph, Twitter Card
- Canonical URL set to `https://societybarber.se/`
- `og:image` and `twitter:image` fixed
- Title shortened to under 580px (Google's character limit)

### Structured Data (Schema.org)
- `@type` changed from `HairSalon` to `BarberShop`
- Schema URL and image updated to `https://societybarber.se/`
- `ReserveAction`/`BookAction` added — Google can show a "Book" button directly in search results
- `FAQPage` schema with 5 common questions (price, address, booking, hours, services)

### Static Content for Crawlers
The site renders via JavaScript, meaning crawlers without JS see nothing. Solution: a `<div>` with H1, four H2 headings, navigation links, and text is added directly to `<body>` and immediately hidden by an inline script for real visitors. Crawlers read it; visitors never see it.

### Custom Domain & HTTPS
- `societybarber.se` connected to GitHub Pages via DNS A records + CNAME
- HTTPS certificate issued by Let's Encrypt (valid until September 2026, auto-renews)
- HTTPS enforcement enabled — all `http://` visitors are automatically redirected to `https://`

### New Files
| File | Purpose |
|---|---|
| `sitemap.xml` | Helps Google find and index all sections |
| `robots.txt` | Allows all crawlers, points to sitemap |
| `llms.txt` | Structured fact page for AI search engines (ChatGPT, Perplexity, Bing) |
| `404.html` | Redirects unknown URL paths to correct section |
| `SEO-TODO.md` | Prioritized list of remaining SEO actions |
| `HANDOVER.md` | This document |

---

## DNS Configuration

The domain `societybarber.se` is registered and hosted at Internet.se. DNS records were updated by Joel Lauronen (Internet.se) to point to GitHub Pages.

**A records (societybarber.se → GitHub Pages):**
```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

**CNAME (www.societybarber.se):**
```
www.societybarber.se → jiddan77.github.io
```

**Internet.se contact:**
- Email: jol@internet.se
- Phone: 031 723 22 21
- Company: Internet.se Svenska AB

---

## Remaining To-Do

See `SEO-TODO.md` for the full prioritized list. Quick summary:

| Priority | Action |
|---|---|
| 🔴 Critical | Create Google Business Profile |
| 🔴 Critical | Submit sitemap to Google Search Console |
| 🟠 High | Add social profiles (Instagram, Facebook) to schema |
| 🟡 Medium | Build backlinks (Bokadirekt, Instagram bio, directories) |
| 🟡 Medium | Collect 5+ Bokadirekt reviews → add star rating to schema |
| 🟡 Medium | Register on local directories (Reco, Hitta, Eniro, Yelp, Apple Maps) |

---

## Technical Reference (for developers)

### File Structure
```
Society_Barber/
├── index.html          # Full website (2.1 MB, design tool bundle)
├── 404.html            # Redirect handler for unknown URL paths
├── sitemap.xml         # XML sitemap for Google
├── robots.txt          # Crawler instructions
├── llms.txt            # AI search engines (ChatGPT, Perplexity)
├── SEO-TODO.md         # Technical action list
├── HANDOVER.md         # This document
└── assets/             # 16 PNG images (barbers + exterior)
```

### Key Business Info in Schema
- Address: Rallarvägen 43, 184 40 Åkersberga
- Phone: +46-8-94-10-60
- Email: Info@societybarber.se
- Bokadirekt: https://www.bokadirekt.se/places/society-barber-136520
- Coordinates: 59.4856°N, 18.2763°E

### Updating the Website
Content (texts, prices, opening hours) is embedded in `index.html` around line ~178 as a JSON-encoded HTML template. SEO meta tags and schema are in `<head>` (roughly lines 1–100). Use Claude Code for all changes — it's easy to break the JSON encoding manually.

### 404 URL Redirects
The `404.html` file handles these path mappings:

| URL typed | Redirects to |
|---|---|
| `/kontakt/` | `/#kontakt` |
| `/tjanster/` | `/#tjanster` |
| `/barberare/` | `/#barberare` |
| `/galleri/` | `/#galleri` |
| anything else | homepage |

---

*Generated with Claude Code — contact Alexander Jidenius for repo access.*
