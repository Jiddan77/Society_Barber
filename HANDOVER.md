# Society Barber — Handover-dokument
**Datum:** 2026-06-29  
**Projekt:** Barbershop-hemsida för Society Barber, Åkersberga

---

## Till Betty — vad händer just nu (ingen teknik)

Hej Betty! Här är en enkel förklaring av vad vi håller på med.

**Du har idag en adress på internet:** `societybarber.se`  
Den adressen pekar just nu på en tom, ful standardsida som inte representerar salongen alls.

**Vi har byggt en helt ny, snygg hemsida** åt dig med bilder, priser, öppettider och en bokningsknapp. Den finns redan live och fungerar — men på en tillfällig adress just nu.

**Det enda som återstår** är att byta vart `societybarber.se` pekar — ungefär som att byta adressskylt på en dörr. Det är ett tekniskt steg som Joel på Internet.se gör åt oss. Vi har skickat instruktioner till honom.

**Vad behöver du göra?** Ingenting just nu. Vi hör av oss när den nya sidan är live på `societybarber.se`. Det brukar ta 1–24 timmar efter att Joel gjort ändringen.

**Vad händer med den gamla sidan?** Den försvinner och ersätts helt av den nya.

---

## Pågående just nu — DNS-byte (2026-06-29)

Alexander har kontaktat Joel Lauronen på Internet.se med följande DNS-poster för att peka `societybarber.se` till den nya hemsidan på GitHub Pages:

**A-poster (societybarber.se):**
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

**När Joel bekräftar:** kontakta Claude för att uppdatera canonical-URL, sitemap och schema från `jiddan77.github.io/Society_Barber/` till `societybarber.se`. Det är ett snabbt jobb på ~5 minuter.

**Joels kontaktuppgifter:**
- E-post: jol@internet.se
- Telefon: 031 723 22 21
- Företag: Internet.se Svenska AB

---

## Översikt

Society Barbers hemsida är publicerad på GitHub Pages och SEO-optimerad. Sidan är byggd med ett proprietärt designverktyg (Claude Design) som buntar ihop allt innehåll i en enda `index.html`-fil på 2.1 MB.

**Live URL:** https://jiddan77.github.io/Society_Barber/  
**GitHub-repo:** https://github.com/Jiddan77/Society_Barber  
**Lokal kopia:** `/Users/alexander.jidenius/Downloads/Society_Barber/`

---

## Vad som är gjort

### Teknisk SEO
- `lang="sv"` på `<html>`-taggen
- Viewport-tagg i outer `<head>` (var tidigare bara i JS-templaten)
- Favicon + Apple touch icon (`assets/logo-sign.png`)
- Google Fonts stylesheet för Cormorant Garamond + Jost (fonter laddades inte korrekt via designverktygets UUID-system på GitHub Pages)
- `theme-color: #1a1a18` för mobila webbläsare

### Meta & Canonical
- Statiska SEO-metataggar i `<head>` för crawlers som inte kör JavaScript: canonical, meta description, Open Graph, Twitter Card
- Canonical URL satt till `https://jiddan77.github.io/Society_Barber/`
- `og:image` och `twitter:image` fixade (pekade på en domän som inte finns)
- Titel förkortad till under 580px (Google-gränsen)

### Strukturerad data (Schema.org)
- `@type` ändrad från `HairSalon` till `BarberShop`
- Schema URL och bild uppdaterade till GitHub Pages-URL
- `ReserveAction`/`BookAction` tillagd — Google kan visa "Boka"-knapp direkt i sökresultaten
- `FAQPage`-schema med 5 vanliga frågor (pris, adress, bokning, öppettider, tjänster)

### Statiskt innehåll för crawlers
Hemsidan renderas via JavaScript, vilket gör att crawlers utan JS inte ser något innehåll. Lösning: en `<div>` med H1, fyra H2-rubriker, navigeringslänkar och text läggs in direkt i `<body>` och döljs omedelbart av ett inline-script för besökare. Crawlers läser det, besökare ser det aldrig.

### Nya filer
| Fil | Syfte |
|---|---|
| `sitemap.xml` | Hjälper Google hitta och indexera alla sektioner |
| `robots.txt` | Tillåter alla crawlers, pekar på sitemap |
| `llms.txt` | Strukturerad faktasida för AI-sökmotorer (ChatGPT, Perplexity, Bing) |
| `SEO-TODO.md` | Prioriterad lista med kvarvarande SEO-åtgärder |
| `HANDOVER.md` | Det här dokumentet |

---

## Kvarvarande att-göra

### 🔴 Kritiskt — gör snarast

#### 1. Skapa Google Business Profile
Det viktigaste som saknas. Utan detta syns inte salongen i Google Maps när folk söker "barberare Åkersberga".

**Steg:**
1. Gå till https://business.google.com
2. Skapa profil: **Society Barber**, Rallarvägen 43, 184 40 Åkersberga
3. Välj kategori: **Barbershop**
4. Verifiera ägarskap (postkort, telefon eller e-post)
5. Fyll i öppettider, foton (bilderna finns i mappen `assets/`) och bokningslänk:  
   `https://www.bokadirekt.se/places/society-barber-136520`

#### 2. Skicka in sitemap till Google Search Console
Påskyndar indexering — utan detta kan det ta veckor innan Google hittar sidan.

**Steg:**
1. Gå till https://search.google.com/search-console
2. Lägg till egendom: `https://jiddan77.github.io/Society_Barber/`
3. Verifiera ägarskap
4. Gå till **Sitemaps** → skicka in: `https://jiddan77.github.io/Society_Barber/sitemap.xml`

---

### 🟠 Hög prioritet — inom 1 vecka

#### 3. Koppla egen domän
`societybarber.se` rankar betydligt bättre än `jiddan77.github.io/Society_Barber/`. En egen domän löser också problemet med www/non-www-duplicering (se punkt 6).

**Steg:**
1. Köp `societybarber.se` hos t.ex. Loopia eller Domainname.se (om ej ägt)
2. I GitHub: **Settings → Pages → Custom domain** → ange `societybarber.se`
3. Hos domänregistratorn: lägg till DNS CNAME-post som pekar på `jiddan77.github.io`
4. Vänta på HTTPS-certifikat (automatiskt via GitHub Pages)
5. **Kontakta Claude** för att uppdatera canonical-URL, sitemap och llms.txt med den nya domänen

#### 4. Lägg till sociala profiler i schemat
Stärker Googles förtroende för verksamheten.

**Steg:** Skicka era profil-URL:er till Claude så uppdateras schemat:
- [ ] Instagram (t.ex. `https://www.instagram.com/societybarber/`)
- [ ] Facebook
- [ ] TikTok (om tillämpligt)

---

### 🟡 Medium — inom 1 månad

#### 5. Bygg backlinks
Sidan har 0 inkommande länkar från andra domäner. Google ser backlinks som förtroenderöster.

**Åtgärder:**
- [ ] Se till att Bokadirekt-profilen har en länk till hemsidan
- [ ] Registrera på kataloger (se punkt 9) — ger automatiskt backlinks
- [ ] Lägg till hemsidans URL i Instagram/TikTok-bio
- [ ] Om lokaltidningar skriver om salongen — be dem länka

#### 6. Åtgärda www/non-www duplicering
SEO-verktyg flaggar att sidan svarar på både `jiddan77.github.io` och `www.jiddan77.github.io`. Fixas automatiskt när egen domän kopplas (punkt 3).

#### 7. Minska filstorlek (2.1 MB)
Google rekommenderar under 100 KB. Stor fil ger långsam laddning på mobil.  
**Orsak:** Designverktyget buntar in alla bilder och fonter i HTML-filen.  
**Lösning:** Exportera med ett verktyg som stöder separata filer, eller bygg om sidan med Next.js/Astro. Kräver omdesign.

#### 8. Bygg recensioner på Bokadirekt
Recensioner med stjärnbetyg syns direkt i sökresultaten och ökar klickfrekvensen markant.

**Steg:**
1. Be kunder lämna recension på Bokadirekt efter varje besök
2. När ni har minst 5 recensioner → kontakta Claude → lägger till `AggregateRating` i schemat

#### 9. Registrera på lokala kataloger
Konsekventa kontaktuppgifter (namn, adress, telefon) på många sajter stärker lokal rankning.

- [ ] Reco.se
- [ ] Hitta.se
- [ ] Eniro.se
- [ ] Yelp
- [ ] Apple Maps: https://mapsconnect.apple.com

#### 10. Lägg till socialt bevis på sidan
Färskt innehåll och kundrecensioner ökar konvertering. Exempel: Instagram-flöde inbäddat på sidan.

---

## Teknisk information (för utvecklare)

### Filstruktur
```
Society_Barber/
├── index.html          # Hela hemsidan (2.1 MB, designverktygets bundle)
├── sitemap.xml         # XML-sitemap för Google
├── robots.txt          # Crawler-instruktioner
├── llms.txt            # AI-sökmotorer (ChatGPT, Perplexity)
├── SEO-TODO.md         # Teknisk att-göra-lista
├── HANDOVER.md         # Det här dokumentet
└── assets/             # 16 PNG-bilder (foton på barberare + exteriör)
```

### Viktig kontaktinfo i schemat
- Adress: Rallarvägen 43, 184 40 Åkersberga
- Telefon: +46-8-94-10-60
- E-post: Info@societybarber.se
- Bokadirekt: https://www.bokadirekt.se/places/society-barber-136520
- Koordinater: 59.4856°N, 18.2763°E

### Om du behöver uppdatera hemsidan
Innehållet (texter, priser, öppettider) finns inbäddade i `index.html` på rad ~178 som ett JSON-kodat HTML-template. SEO-metataggar och schema finns i `<head>` (raderna 1–100 ungefär). Använd Claude Code för alla ändringar — det är lätt att bryta JSON-kodningen manuellt.

---

*Genererat med Claude Code — kontakta Alexander för tillgång till repot.*
