# Society Barber — SEO att-göra-lista

Dessa åtgärder kräver manuella steg och kan inte göras i koden.

---

## 🔴 Kritiskt (gör snarast)

### 1. Skapa Google Business Profile
**Varför:** Salongen måste synas i "Local 3-pack" på Google Maps när folk söker "barberare Åkersberga". Det är den enskilt viktigaste lokala SEO-åtgärden.
**Steg:**
1. Gå till https://business.google.com
2. Skapa profil med exakt NAP: Society Barber, Rallarvägen 43, 184 40 Åkersberga
3. Välj kategori: "Barbershop"
4. Verifiera med postkod eller telefon
5. Lägg till öppettider, foton (använd bilderna i `assets/`), och bokningslänk: https://www.bokadirekt.se/places/society-barber-136520

### 2. Skicka in sitemap till Google Search Console
**Varför:** Påskyndar indexering — annars kan det ta veckor innan Google hittar sidan.
**Steg:**
1. Gå till https://search.google.com/search-console
2. Lägg till egendom: `https://jiddan77.github.io/Society_Barber/`
3. Verifiera ägarskap (HTML-tag eller DNS)
4. Gå till Sitemaps → skicka in: `https://jiddan77.github.io/Society_Barber/sitemap.xml`

---

## 🟠 Hög prioritet (inom 1 vecka)

### 3. Koppla egen domän
**Varför:** `societybarber.se` rankar betydligt bättre än `jiddan77.github.io/Society_Barber/`. Sökmotorer premierar egna domäner.
**Steg:**
1. Köp `societybarber.se` hos t.ex. Loopia eller Domainname.se (om ej ägt)
2. I GitHub repo → Settings → Pages → Custom domain → ange `societybarber.se`
3. Hos domänregistratorn, lägg till DNS CNAME: `jiddan77.github.io`
4. Uppdatera canonical URL i `index.html` från `jiddan77.github.io/Society_Barber/` till `https://societybarber.se/`
5. Uppdatera `sitemap.xml` och `llms.txt` med ny URL

### 4. Lägg till sociala profiler i schemat
**Varför:** `sameAs` i JSON-LD-schemat kopplar ihop salongen med externa profiler, vilket stärker Googles förtroende.
**Steg:** Meddela Claude era Instagram/Facebook-URL:er så uppdateras schemat automatiskt.
- [ ] Instagram-URL (t.ex. `https://www.instagram.com/societybarber/`)
- [ ] Facebook-URL
- [ ] TikTok-URL (om tillämpligt)

---

## 🟡 Medium (inom 1 månad)

### 5. Bygg backlinks (externa länkar in till sidan)
**Varför:** Sidan har 0 backlinks från andra domäner. Google ser backlinks som röster — utan dem är det svårt att ranka mot konkurrenter.
**Åtgärder:**
- [ ] Be Bokadirekt lägga till länk till hemsidan i er profil
- [ ] Registrera på Reco.se, Hitta.se, Eniro.se (se punkt 8) — de ger automatiskt en backlink
- [ ] Om ni har ett Instagram/TikTok — lägg till hemsidans URL i bion
- [ ] Om lokaltidningar (t.ex. Österåker Lokalnytt) skriver om salongen — be dem länka

### 6. Åtgärda www/non-www duplicering (301-redirect)
**Varför:** SEO-verktyget flaggar att sidan svarar på både `jiddan77.github.io` och `www.jiddan77.github.io` utan redirect. Google kan behandla dem som duplicerat innehåll.
**Lösning:** Fixas automatiskt när ni kopplar en **egen domän** (se punkt 3). GitHub Pages hanterar www/non-www korrekt med custom domain + HTTPS.
**Kan inte fixas i kod utan custom domain.**

### 7. Minska filstorlek (2.1 MB HTML)
**Varför:** Google rekommenderar under 100 KB. 2.1 MB gör att sidan laddas långsamt på mobil vilket påverkar ranking (Core Web Vitals).
**Orsak:** Designverktygets bundle packar in alla bilder och fonter som base64 i HTML-filen.
**Lösning:** Exportera sidan på nytt i ett verktyg som stöder externa filer, eller bygg om sidan med ett vanligt webbramverk (Next.js, Astro, etc.).
**Kan inte fixas utan att designa om sidan.**

### 8. Bygg recensioner på Bokadirekt
**Varför:** AggregateRating med äkta recensioner ger guldstjärnor i sökresultaten — dramatisk CTR-ökning.
**Steg:**
1. Be kunder lämna recensioner på Bokadirekt efter varje besök
2. När ni har minst 5 recensioner → meddela Claude → lägger till AggregateRating i schemat

### 9. Registrera på lokala kataloger (citationsbygge)
**Varför:** Konsekventa NAP-uppgifter (namn, adress, telefon) på flera sajter stärker lokal rankning.
**Sajter att registrera sig på:**
- [ ] Reco.se
- [ ] Hitta.se
- [ ] Eniro.se
- [ ] Yelp
- [ ] Apple Maps (https://mapsconnect.apple.com)

### 10. Lägg till Instagram-flöde eller socialt bevis på sidan
**Varför:** Färskt innehåll och social proof ökar konvertering och signalerar aktivitet till Google.

---

## ✅ Klart (referens)

- [x] `lang="sv"` på HTML-taggen
- [x] Statiska SEO-metataggar i `<head>` för non-JS-crawlers
- [x] Canonical URL fixad till GitHub Pages
- [x] `og:image` och `twitter:image` fixade
- [x] Schema `@type` ändrad till `BarberShop`
- [x] `BookAction`/`ReserveAction` i schema (direktbokning från Google)
- [x] FAQ-schema (5 vanliga frågor)
- [x] `sitemap.xml`
- [x] `robots.txt`
- [x] Favicon + Apple touch icon
- [x] Google Fonts stylesheet (Cormorant Garamond + Jost)
- [x] `llms.txt` för AI-sökmotorer
- [x] `theme-color` för mobila webbläsare
- [x] Viewport-tagg i outer `<head>`
- [x] Statisk H1, H2-rubriker och interna länkar för non-JS-crawlers
- [x] Titel förkortad till under 580px
