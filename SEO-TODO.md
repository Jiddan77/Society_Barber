# Society Barber — SEO Action List

These tasks require manual steps and cannot be done in the code.

---

## 🔴 Critical (do immediately)

### 1. Create Google Business Profile
**Why:** The salon must appear in the "Local 3-pack" on Google Maps when people search "barber Åkersberga". This is the single most important local SEO action.
**Steps:**
1. Go to https://business.google.com
2. Create profile with exact NAP: Society Barber, Rallarvägen 43, 184 40 Åkersberga
3. Select category: "Barbershop"
4. Verify with postcard or phone
5. Add opening hours, photos (use images in `assets/`), and booking link: https://www.bokadirekt.se/places/society-barber-136520

### 2. Submit sitemap to Google Search Console
**Why:** Speeds up indexing — without this it can take weeks before Google finds the site.
**Steps:**
1. Go to https://search.google.com/search-console
2. Add property: `https://societybarber.se/`
3. Verify ownership (HTML tag or DNS)
4. Go to Sitemaps → submit: `https://societybarber.se/sitemap.xml`

---

## 🟠 High priority (within 1 week)

### 3. Add social profiles to schema
**Why:** `sameAs` in the JSON-LD schema links the salon to external profiles, which strengthens Google's trust.
**Steps:** Send Alexander your Instagram/Facebook URLs and the schema will be updated automatically.
- [ ] Instagram URL (e.g. `https://www.instagram.com/societybarber/`)
- [ ] Facebook URL
- [ ] TikTok URL (if applicable)

---

## 🟡 Medium (within 1 month)

### 4. Build backlinks (external links to the site)
**Why:** The site has 0 backlinks from other domains. Google treats backlinks as votes of confidence — without them it's hard to rank against competitors.
**Actions:**
- [ ] Ask Bokadirekt to add a link to the website on your profile
- [ ] Register on Reco.se, Hitta.se, Eniro.se (see item 7) — these automatically provide a backlink
- [ ] If you have Instagram/TikTok — add the website URL to the bio
- [ ] If local newspapers (e.g. Österåker Lokalnytt) write about the salon — ask them to link

### 5. Reduce file size (2.1 MB HTML)
**Why:** Google recommends under 100 KB. 2.1 MB causes slow loading on mobile which affects ranking (Core Web Vitals).
**Cause:** The design tool bundles all images and fonts as base64 inside the HTML file.
**Solution:** Export the site using a tool that supports external files, or rebuild with a standard web framework (Next.js, Astro, etc.).
**Cannot be fixed without redesigning the site.**

### 6. Build reviews on Bokadirekt
**Why:** AggregateRating with real reviews shows gold stars in search results — dramatic CTR increase.
**Steps:**
1. Ask customers to leave reviews on Bokadirekt after each visit
2. When you have at least 5 reviews → notify Alexander → AggregateRating will be added to the schema

### 7. Register on local directories (citation building)
**Why:** Consistent NAP data (name, address, phone) on multiple sites strengthens local ranking.
**Sites to register on:**
- [ ] Reco.se
- [ ] Hitta.se
- [ ] Eniro.se
- [ ] Yelp
- [ ] Apple Maps (https://mapsconnect.apple.com)

### 8. Add Instagram feed or social proof to the site
**Why:** Fresh content and social proof increases conversion and signals activity to Google.

---

## ✅ Done

- [x] `lang="sv"` on HTML tag
- [x] Static SEO meta tags in `<head>` for non-JS crawlers
- [x] Canonical URL → `https://societybarber.se/`
- [x] `og:image` and `twitter:image` fixed
- [x] Schema `@type` changed to `BarberShop`
- [x] `BookAction`/`ReserveAction` in schema (direct booking button from Google)
- [x] FAQ schema (5 common questions)
- [x] `sitemap.xml` → `https://societybarber.se/sitemap.xml`
- [x] `robots.txt`
- [x] Favicon + Apple touch icon
- [x] Google Fonts stylesheet (Cormorant Garamond + Jost)
- [x] `llms.txt` for AI search engines (ChatGPT, Perplexity, Bing)
- [x] `theme-color` for mobile browsers
- [x] Viewport tag in outer `<head>`
- [x] Static H1, H2 headings and internal links for non-JS crawlers
- [x] Title shortened to under 580px (Google's limit)
- [x] Custom domain `societybarber.se` connected to GitHub Pages
- [x] HTTPS certificate active and enforced (Let's Encrypt via GitHub)
- [x] `404.html` — unknown URLs like `/kontakt/` automatically redirect to the correct section
- [x] www/non-www duplication resolved (GitHub Pages handles automatically with custom domain)
