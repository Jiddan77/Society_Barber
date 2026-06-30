# SEO Audit — Society Barber
**URL:** https://jiddan77.github.io/Society_Barber/  
**Datum:** 2026-06-29  
**Business type:** Lokal tjänsteverksamhet — Barbershop (Åkersberga)

---

## SEO Health Score: 42 / 100

| Kategori | Poäng | Vikt |
|---|---|---|
| Technical SEO | 30/100 | 22% |
| Content Quality | 72/100 | 23% |
| On-Page SEO | 65/100 | 20% |
| Schema / Structured Data | 50/100 | 10% |
| Performance (CWV) | 15/100 | 10% |
| AI Search Readiness | 40/100 | 10% |
| Images | 20/100 | 5% |

---

## 🔴 KRITISKA PROBLEM (fixa omedelbart)

### 1. Template-variabler inte ersatta — bokningslänkar är brutna
Alla "Boka Tid"-knappar innehåller `{{ bookingUrl }}` istället för det riktiga URL:et. Inga besökare kan boka.

**Fix:** Ersätt `{{ bookingUrl }}` med `https://www.bokadirekt.se/places/society-barber-136520`

---

### 2. Bildkällor är UUID-platshållare — alla bilder är brutna
Alla `<img src="...">` använder interna design-UUID:n (t.ex. `66f6b48a-8049-4403-9233-c581ab76f2bc`) som inte fungerar i en webbläsare. Ingen bild visas på sajten.

**Fix:** Ersätt varje UUID med rätt sökväg i `assets/`-mappen.

---

### 3. Canonical URL pekar på fel domän
`<link rel="canonical" href="https://www.societybarber.se/">` — sidan lever på GitHub Pages men canonical säger åt Google att indexera en annan domän. Resultatet: den här sidan behandlas som ett duplikat och indexeras inte.

**Fix:** Ändra canonical till `https://jiddan77.github.io/Society_Barber/` tills en riktig domän kopplas.

---

### 4. Sidans filstorlek 2,1 MB — extrem prestanda-förlust
HTML-filen är 2 149 867 bytes. Google rekommenderar under 100 KB. LCP-värdet är troligen 5–8 sekunder på mobil. Detta skadar rankings direkt.

**Fix:** Fonter inbäddade som UUID-interna refs laddas inte alls — lägg till Google Fonts via stylesheet istället. Separera CSS till extern fil.

---

### 5. Font-face src är UUID:n — fonter laddas inte
Alla `@font-face { src: url("UUID") }` är design-verktygets interna refs och fungerar inte på GitHub. Trots `preconnect` till Google Fonts saknas stylesheet-länken.

**Fix:** Lägg till `<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@500;600;700&family=Jost:wght@300;400;500;600&display=swap">` och ta bort de brutna @font-face-blocken.

---

## 🟠 HÖG PRIORITET (fixa inom 1 vecka)

### 6. OG:image och Twitter:image pekar på societybarber.se — brutna
`og:image` och `twitter:image` refererar till `https://www.societybarber.se/web/exterior-sign.jpg` som inte existerar. Delningar på sociala medier visar ingen bild.

**Fix:** Ändra till `https://jiddan77.github.io/Society_Barber/assets/exterior-sign.png`

---

### 7. Schema.org-bild och URL pekar på fel domän
JSON-LD-schemat har `"url": "https://www.societybarber.se/"` och `"image": "https://www.societybarber.se/web/exterior-sign.jpg"` — båda brutna/felaktiga.

**Fix:** Uppdatera till GitHub Pages-URL och `assets/exterior-sign.png`

---

### 8. Ingen sitemap.xml
`/sitemap.xml` returnerar 404. Google har inget dokument att crawla.

**Fix:** Skapa `sitemap.xml` med sidan och alla ankarsektioner.

---

### 9. Ingen robots.txt
Returnerar 404. Bra praxis att ha en explicit `robots.txt`.

**Fix:** Skapa `robots.txt` med `User-agent: *` och peka på sitemap.

---

### 10. Schema @type är HairSalon — bör vara BarberShop
Google stöder `BarberShop` som specifik typ och ger lokala sökresultat för den. `HairSalon` är för generellt.

**Fix:** Ändra `"@type": "HairSalon"` till `"@type": "BarberShop"`

---

## 🟡 MEDIUM (fixa inom 1 månad)

### 11. `<html lang>` sätts via JavaScript, inte i HTML-attributet
`document.documentElement.lang = "sv"` körs via skript — crawlers som inte kör JS missar språkdeklarationen.

**Fix:** Lägg till `lang="sv"` direkt på `<html>`-taggen.

---

### 12. Ingen favicon
Ingen `<link rel="icon">` — webbläsarflikar visar standardikon.

---

### 13. Ingen AggregateRating i schema
Kundrecensioner i schema ökar CTR (click-through rate) kraftigt i sökresultaten. Stars visas bredvid namnet.

**Möjlighet:** Lägg till `"aggregateRating": {"@type": "AggregateRating", "ratingValue": "5", "reviewCount": "28"}` baserat på bokadirekt-betyg.

---

### 14. Ingen FAQ-schema
"Vad kostar herrklippning i Åkersberga?" är en vanlig sökfråga. FAQ-schema kan ge rich results och ta plats i AI Overviews.

---

### 15. UUID-script i `<head>`
`<script src="ea625112-a0fc-4c20-88e3-e749a884cf63">` — okänd design-verktygsref som genererar ett nätverksfel.

---

## 🟢 STYRKOR (det som redan är bra)

- ✅ Stark `<title>` med lokal geo-keyword: "Barberare i Åkersberga | Society Barber"
- ✅ Bra meta description med adress och CTA
- ✅ Komplett JSON-LD med adress, koordinater, prislista, personal, öppettider
- ✅ Geo-meta tags (geo.region, geo.placename, ICBM)
- ✅ OG-tags och Twitter Card finns (men pekar på fel URL)
- ✅ Alt-text på alla bilder (när de väl laddas)
- ✅ `robots: index, follow, max-image-preview:large`
- ✅ `font-display: swap` deklarerat
- ✅ Tydlig H1/H2/H3-hierarki
- ✅ Bokadirekt-länk i sameAs
- ✅ HTTPS (GitHub Pages)
