# PRD — lokalnewww.pl
### Product Requirements Document | Wersja 1.0 | Kwiecień 2026

---

## 1. Informacje ogólne

| Pole | Wartość |
|---|---|
| **Produkt** | Strona internetowa freelancera web-developera |
| **Domena** | lokalnewww.pl |
| **Właściciel** | Freelancer web-developer |
| **Technologia** | HTML5 / CSS3 (Tailwind CSS v3) / Vanilla JS |
| **Typ projektu** | Strona statyczna (bez CMS, bez backendu) |
| **Cel biznesowy** | Pozyskiwanie klientów — małe lokalne firmy bez stron lub ze słabymi stronami |

---

## 2. Cel produktu

Strona ma służyć jako narzędzie sprzedażowe dla freelancera specjalizującego się w tworzeniu stron internetowych dla małych lokalnych firm. Odbiorca to właściciel małego biznesu (sklep, usługi, rzemiosło, gastronomia) — często nieprzekonany do potrzeby posiadania strony lub rozczarowany poprzednimi doświadczeniami.

**Główne cele konwersji:**
1. Kontakt przez formularz lub telefon
2. Zapoznanie się z ofertą i portfolio
3. Budowanie zaufania przez transparentność, styl i profesjonalizm

---

## 3. Technologia i narzędzia

### 3.1 Stack

- **HTML5** — semantyczny markup, zgodny z WCAG 2.1 AA
- **Tailwind CSS v3** — via CDN (Play CDN) lub lokalnie przez npm
- **Vanilla JS (ES6+)** — animacje scroll, hamburger menu, smooth scroll, formularz kontaktowy
- **Google Fonts** — czcionka **Inter** (wagi: 300, 400, 500, 600, 700, 800)
- **Lucide Icons** — lekkie SVG ikony (via CDN lub inline)
- **AOS.js** lub ręczne IntersectionObserver — animacje przy scrollowaniu

### 3.2 Struktura plików

```
lokalnewww.pl/
├── index.html
├── assets/
│   ├── images/
│   │   ├── logopin3D.png          ← logo pin (3D granatowy)
│   │   └── about-photo.jpg        ← zdjęcie właściciela (do dodania)
│   ├── css/
│   │   └── custom.css             ← overrides, animacje, utility classes
│   └── js/
│       └── main.js                ← scroll animations, menu, formularz
├── tailwind.config.js             ← (opcjonalnie, jeśli build lokalnie)
└── PRD_lokalnewww.md
```

---

## 4. Design System

### 4.1 Paleta kolorów

Inspiracja: logo lokalnewww.pl (granatowy pin 3D, jasne tło, żółty akcent ze screenshotów)

```css
:root {
  /* Kolory główne */
  --color-navy:        #1A2540;   /* granat — primary dark */
  --color-navy-mid:    #243155;   /* granat mid — tła sekcji dark */
  --color-blue:        #2563EB;   /* niebieski — akcenty, CTA hover */
  --color-blue-light:  #60A5FA;   /* jasny niebieski — ikony, highlights */

  /* Tła */
  --color-bg-light:    #F8FAFC;   /* sekcje jasne */
  --color-bg-white:    #FFFFFF;   /* hero, karty */
  --color-bg-dark:     #0F172A;   /* sekcje ciemne */
  --color-bg-dark-mid: #1E293B;   /* karty w dark sekcjach */

  /* Akcenty */
  --color-yellow:      #FACC15;   /* żółty — wyróżnienia w hero tekście */
  --color-yellow-dark: #EAB308;   /* hover żółty */

  /* Tekst */
  --color-text-primary:   #0F172A;  /* nagłówki na jasnym tle */
  --color-text-secondary: #475569;  /* body text na jasnym tle */
  --color-text-light:     #F1F5F9;  /* tekst na ciemnym tle */
  --color-text-muted:     #94A3B8;  /* podtytuły na ciemnym tle */

  /* Inne */
  --color-border:      #E2E8F0;
  --color-shadow:      rgba(15, 23, 42, 0.08);
}
```

### 4.2 Typografia

Czcionka: **Inter** (Google Fonts)

```css
/* Skala typograficzna — Tailwind custom config lub CSS vars */
--text-xs:   0.75rem;    /* 12px */
--text-sm:   0.875rem;   /* 14px */
--text-base: 1rem;       /* 16px — body */
--text-lg:   1.125rem;   /* 18px */
--text-xl:   1.25rem;    /* 20px */
--text-2xl:  1.5rem;     /* 24px */
--text-3xl:  1.875rem;   /* 30px */
--text-4xl:  2.25rem;    /* 36px */
--text-5xl:  3rem;       /* 48px — h1 mobile */
--text-6xl:  3.75rem;    /* 60px — h1 desktop */
--text-7xl:  4.5rem;     /* 72px — hero statement */

/* Line heights */
--leading-tight:  1.2;
--leading-normal: 1.6;
--leading-relaxed: 1.75;

/* Letter spacing */
--tracking-tight:  -0.03em;  /* nagłówki display */
--tracking-normal: 0;
--tracking-wide:   0.08em;   /* etykiety, capslock */
```

### 4.3 Spacing & Layout

```css
/* Spacing scale (zgodny z Tailwind) */
--space-section-y:  6rem;    /* 96px — odstęp pionowy sekcji */
--space-section-y-sm: 4rem;  /* 64px — mobile */
--space-container: 1.5rem;   /* 24px — padding boczny na mobile */
--max-width: 1200px;         /* max-width contentu */

/* Border radius */
--radius-sm:  0.375rem;  /* 6px  — przyciski small */
--radius-md:  0.75rem;   /* 12px — karty */
--radius-lg:  1.25rem;   /* 20px — bento duże */
--radius-xl:  2rem;      /* 32px — hero elementy */
--radius-full: 9999px;   /* pill */

/* Shadows */
--shadow-sm:  0 1px 3px rgba(15,23,42,0.06), 0 1px 2px rgba(15,23,42,0.04);
--shadow-md:  0 4px 16px rgba(15,23,42,0.08), 0 2px 6px rgba(15,23,42,0.05);
--shadow-lg:  0 12px 40px rgba(15,23,42,0.12), 0 4px 12px rgba(15,23,42,0.07);
--shadow-blue: 0 8px 32px rgba(37,99,235,0.25);
```

### 4.4 Logo

- **Plik:** `assets/images/logopin3D.png` — wyłącznie grafika pinu
- **Tekst logo** pisany w HTML: `<span>lokalne</span><span class="text-blue-600 font-bold">www</span><span class="text-slate-500">.pl</span>`
- **Czcionka:** Inter 700
- **Nawigacja:** logo + tekst obok (flex, items-center, gap-2)
- **Rozmiar pinu w nav:** 32px × auto

---

## 5. Animacje i interakcje

### 5.1 Animacje scroll (IntersectionObserver)

Klasy CSS dodawane JS-em gdy element wejdzie w viewport:

```
.fade-up     — translateY(30px) → translateY(0) + opacity 0→1, 0.6s ease-out
.fade-in     — opacity 0→1, 0.5s ease-out
.slide-left  — translateX(-40px) → 0 + opacity, 0.6s
.slide-right — translateX(40px) → 0 + opacity, 0.6s
.scale-in    — scale(0.92) → scale(1) + opacity, 0.5s
```

Delay (staggering dla kart w gridzie):

```
.delay-100 { transition-delay: 100ms }
.delay-200 { transition-delay: 200ms }
.delay-300 { transition-delay: 300ms }
.delay-400 { transition-delay: 400ms }
```

### 5.2 Micro-interactions

- **Hover na kartach:** `translateY(-4px)` + shadow-lg, 200ms ease
- **Hover na przyciskach CTA:** scale(1.02) + shadow-blue, 150ms
- **Nawigacja sticky:** po scrollu 80px — `backdrop-blur-md bg-white/90 shadow-sm`
- **Hamburger (mobile):** animowane 3 kreski → X
- **Skewed text w hero:** `transform: skewY(-2deg)` na wyróżnionym bloku z żółtym tłem
- **Pin logo:** subtelny float animation w hero (keyframe translateY -6px loop)

### 5.3 Page load

- Nawigacja: fade-in 0.3s
- Hero headline: staggered reveal — każda linia z delay 100ms
- Hero CTA: fade-up z delay 400ms

---

## 6. Sekcje strony — szczegółowe wymagania

### 6.1 NAWIGACJA (sticky, fixed-top)

**Layout:** `flex justify-between items-center`, max-width container, padding `py-4 px-6`

**Elementy:**
- Lewo: Logo (pin img + tekst HTML)
- Środek (desktop): linki nawigacyjne
- Prawo: CTA button

**Linki:**
- Oferta (scroll do #uslugi)
- O mnie (scroll do #o-mnie)
- Blog (link zewnętrzny lub placeholder)
- Kontakt (scroll do #kontakt)

**CTA button:** `"Bezpłatna konsultacja"` — żółty (`bg-yellow-400 text-slate-900`), rounded-full, font-600

**Mobile:** hamburger menu, drawer z góry lub slide-in z prawej

**Zachowanie:**
- Transparent na szczycie strony (hero jest jasny)
- Po scrollu: `bg-white/95 backdrop-blur shadow-sm` — transition 300ms

---

### 6.2 HERO

**Tło:** białe / bardzo jasne `bg-slate-50`
**Layout:** dwie kolumny (desktop) — tekst lewo, grafika/dekoracja prawo. Mobile: jednokolumnowy.

**Zawartość lewa kolumna:**

```
Eyebrow (label):
"Strony WWW dla lokalnych firm"
— małe, uppercase, letter-spacing wide, kolor blue-600

Headline (H1):
"Twoja firma zasługuje
na [lepszą stronę.]"

gdzie [lepszą stronę.] to element z:
- żółte tło (bg-yellow-400)
- lekki skos: transform skewY(-2deg) lub skewX(-3deg)
- padding 4px 12px
- display inline-block

Subheadline:
"Projektuję i wdrażam strony, które przyciągają klientów —
szybkie, nowoczesne i dopasowane do Twojego biznesu."
— text-lg, text-slate-600, max-width 480px

CTA row:
[Sprawdź ofertę]  →  [Nasze realizacje ↓]
Pierwszy: żółty pill button, duży
Drugi: ghost/outline, strzałka w dół

Social proof (opcjonalnie pod CTA):
"✓ Ponad 30 lokalnych firm zaufało naszej pracy"
— drobny tekst, muted
```

**Prawa kolumna:** 
- Mockup laptopa z przykładową stroną lub abstrakcyjna kompozycja z pinami mapy / siatką linii
- Animowany pin (float keyframe)
- Opcjonalnie: kilka "tagów" floating — "SEO ✓", "Mobile ✓", "Szybka ✓"

---

### 6.3 SEKCJA: CO ZYSKASZ (ciemne tło)

**Tło:** `bg-slate-900` (dark)
**Layout:** nagłówek + grid 3 kolumny (desktop), 1 (mobile)

**Nagłówek:**
```
Label: "Dlaczego warto?"
H2: "Strona WWW to najtańszy
handlowiec jakiego możesz mieć."
Subtext: muted, max-width 560px
```

**Karty (3 sztuki) — ikona + nagłówek + opis:**

| Ikona | Tytuł | Opis |
|---|---|---|
| 🌐 (globe) | Działasz 24/7 | Twoja strona pracuje nawet gdy Ty śpisz — klienci znajdą Cię o każdej porze. |
| 📍 (pin) | Lokalne SEO | Pojawiasz się gdy ktoś szuka Twojej usługi w okolicy — to klienci z konkretnym zamiarem zakupu. |
| 💼 (briefcase) | Profesjonalny wizerunek | Klienci oceniają Cię zanim zadzwonią. Dobra strona robi pierwsze wrażenie zamiast Ciebie. |

Karty: `bg-slate-800 rounded-2xl p-8`, hover: `bg-slate-700`

---

### 6.4 SEKCJA: NASZE USŁUGI — BENTO GRID (jasne tło)

**Tło:** `bg-white` lub `bg-slate-50`
**Layout:** bento grid — asymetryczny, 12-kolumnowy

**Nagłówek:**
```
Label: "Nasze Usługi"
H2: "Tworzymy więcej niż tylko kod."
Subtext: "Kompleksowa obsługa od projektu po pozycjonowanie."
```

**Kafelki bento:**

| # | Nazwa | Rozmiar (desk) | Styl | Ikona |
|---|---|---|---|---|
| 1 | **Strony WWW** | 4 col × 2 row (large) | bg-navy text-white | monitor icon |
| 2 | **Sklepy Online** | 4 col × 1 row | bg-slate-900 text-yellow-400 | shopping-cart |
| 3 | **Pozycjonowanie (SEO)** | 4 col × 1 row | bg-blue-600 text-white | search icon |
| 4 | **Copywriting** | 4 col × 1 row | bg-slate-100 | pen icon |
| 5 | **Branding** | 4 col × 1 row | bg-yellow-400 text-slate-900 | palette icon |
| 6 | **Utrzymanie i opieka** | 4 col × 1 row | bg-slate-200 | shield icon |

**Kafelek #1 (duży):**
- Mockup strony lub zdjęcie laptopa
- Copy: `"Strona, która sprzedaje — nie tylko wygląda."`
- Opis krótki
- Link "Dowiedz się więcej →"

**Każdy kafelek:**
- rounded-2xl, padding 24–32px
- ikona 32–40px (Lucide SVG)
- Nazwa usługi (font-600, text-lg)
- 1–2 zdania opisu
- Hover: skala 1.02 + shadow

---

### 6.5 SEKCJA: JAK WSPÓŁPRACUJEMY (ciemne tło)

**Tło:** `bg-slate-900`
**Layout:** nagłówek + timeline/numerowane kroki (1 kolumna lub 2 kolumny desktop)

**Nagłówek:**
```
Label: "Proces"
H2: "Prosta droga od pomysłu
do gotowej strony."
```

**Kroki (4 etapy):**

| # | Tytuł | Opis |
|---|---|---|
| 01 | Bezpłatna konsultacja | Rozmawiamy o Twoim biznesie, celach i potrzebach. Bez zobowiązań, bez żargonu. |
| 02 | Projekt i wycena | Przygotowuję projekt dopasowany do Twojej branży i budżetu — w ciągu 48h. |
| 03 | Realizacja | Tworzę stronę szybko i terminowo. Na bieżąco informuję o postępach. |
| 04 | Wdrożenie i wsparcie | Uruchamiam stronę i uczę jak z niej korzystać. Jestem dostępny po wdrożeniu. |

**Styl kroków:**
- Duży numer (text-8xl font-900, kolor blue-600/20 — ghost)
- Tytuł nad numerem
- Linia łącząca kroki (tylko desktop, border-dashed)

---

### 6.6 SEKCJA: CO MNIE WYRÓŻNIA (jasne tło)

**Tło:** `bg-slate-50`
**Layout:** nagłówek + 2×2 grid lub 4 karty w rzędzie

**Nagłówek:**
```
H2: "Nie jestem kolejną agencją."
Subtext: "Pracujesz bezpośrednio z osobą, która tworzy Twoją stronę."
```

**4 punkty wyróżnienia:**

| Ikona | Tytuł | Opis |
|---|---|---|
| ⚡ | Szybka realizacja | Pierwsza wersja w 7–14 dni roboczych, nie miesiącach. |
| 💬 | Bezpośredni kontakt | Żadnych pośredników. Piszesz do mnie, nie do call-center. |
| 📍 | Znam lokalny rynek | Specjalizuję się w małych firmach z Twojego regionu. |
| 🔒 | Ceny bez niespodzianek | Stała wycena z góry. Nie płacisz za "dodatkowe godziny". |

---

### 6.7 SEKCJA: O MNIE (ciemne tło)

**Tło:** `bg-navy` (`#1A2540`)
**Layout:** 2 kolumny — zdjęcie lewo (lub prawo), tekst prawo. Mobile: jednokolumnowy.

**Zawartość:**
```
Label: "O mnie"
H2: "Cześć, jestem [Imię] —
buduję strony dla lokalnych biznesów."

Akapit 1:
"Od X lat pomagam małym firmom zaistnieć w internecie.
Wiem, że nie każdy przedsiębiorca potrzebuje rozbudowanej
agencji — potrzebuje kogoś, kto rozumie jego biznes
i po prostu to zrobi."

Akapit 2:
"Pracuję samodzielnie, więc masz pewność, że rozmawiasz
z osobą, która rzeczywiście zajmie się Twoją stroną."

CTA: [Porozmawiajmy →] — żółty button
```

**Zdjęcie:**
- Prostokąt z rounded-2xl
- Opcjonalnie: dekoracyjna ramka/border offset (box z border blue-600 przesunięty o -8px)

---

### 6.8 SEKCJA: KONTAKT (jasne tło)

**Tło:** `bg-white`
**Layout:** 2 kolumny (desktop) — lewo: tekst + dane, prawo: formularz

**Lewa strona:**
```
Label: "Kontakt"
H2: "Zacznijmy od rozmowy."
Subtext: "Pierwsze spotkanie jest bezpłatne i niezobowiązujące.
Opowiedz mi o swoim biznesie — resztą zajmę się ja."

Dane kontaktowe:
📧 kontakt@lokalnewww.pl
📞 +48 XXX XXX XXX
📍 [Miasto], Polska
```

**Formularz (prawa strona):**

```
Pola:
- Imię i nazwisko (required)
- Email (required)
- Telefon (optional)
- Strona WWW (optional, placeholder: "Jeśli już ją masz")
- Wiadomość (textarea, required, placeholder: "Opisz krótko swój biznes...")
- [Wyślij wiadomość] — CTA button, pełna szerokość, żółty
```

Działanie: `mailto:` lub Formspree.io (statyczna strona bez backendu)

**Karty pod formularzem (opcja):**
- "Odpowiadam w ciągu 24h"
- "Pierwsza konsultacja gratis"

---

### 6.9 FOOTER (minimalistyczny, ciemny)

**Tło:** `bg-slate-900`
**Layout:** 3 rzędy

```
Rząd 1:
Logo (pin + tekst) | Linki nawigacyjne | Ikony social (LinkedIn, FB)

Rząd 2 (opcjonalnie):
Krótkie tagline: "Strony WWW dla małych firm | lokalnewww.pl"

Rząd 3:
© 2026 lokalnewww.pl | Polityka prywatności | Realizacja: lokalnewww.pl
```

---

## 7. SEO — wymagania techniczne

### 7.1 On-page SEO

```html
<!-- Meta podstawowe -->
<title>Strony WWW dla lokalnych firm | lokalnewww.pl</title>
<meta name="description" content="Tworzę profesjonalne strony internetowe dla małych lokalnych firm. Szybko, przystępnie, z pozycjonowaniem. Bezpłatna konsultacja!">
<meta name="keywords" content="strony www dla firm, tworzenie stron internetowych, strona www cena, web developer [miasto]">
<link rel="canonical" href="https://lokalnewww.pl/">

<!-- Open Graph -->
<meta property="og:title" content="Strony WWW dla lokalnych firm | lokalnewww.pl">
<meta property="og:description" content="...">
<meta property="og:image" content="https://lokalnewww.pl/assets/images/og-image.jpg">
<meta property="og:url" content="https://lokalnewww.pl">
<meta property="og:type" content="website">

<!-- Twitter Card -->
<meta name="twitter:card" content="summary_large_image">
```

### 7.2 Structured Data (JSON-LD)

```json
{
  "@context": "https://schema.org",
  "@type": "LocalBusiness",
  "name": "lokalnewww.pl",
  "description": "Tworzenie stron WWW dla małych lokalnych firm",
  "url": "https://lokalnewww.pl",
  "email": "kontakt@lokalnewww.pl",
  "areaServed": "PL",
  "serviceType": ["Web Design", "SEO", "E-commerce"]
}
```

### 7.3 Performance

- Obrazy: `.webp` format, lazy loading (`loading="lazy"`)
- Tailwind: purgeCSS w buildzie (tylko używane klasy)
- Fonty: `preconnect` + `font-display: swap`
- Core Web Vitals target: LCP < 2.5s, CLS < 0.1, FID < 100ms

---

## 8. Responsywność

| Breakpoint | Wartość | Zachowanie |
|---|---|---|
| Mobile | < 768px | 1 kolumna, hamburger menu, bento 1-kolumnowy |
| Tablet | 768px–1024px | 2 kolumny, uproszczony bento |
| Desktop | > 1024px | Pełny layout, sticky nav, bento grid |

---

## 9. Dostępność (a11y)

- Kontrast: minimum 4.5:1 dla tekstu normalnego (WCAG AA)
- `alt` na wszystkich obrazach
- Fokus visible na elementach interaktywnych
- Semantyczne nagłówki (h1 → h2 → h3, hierarchia)
- `aria-label` na ikonach bez tekstu
- Formularze: `<label>` powiązane z `<input>`

---

## 10. Kolejność implementacji (milestones)

| Etap | Zakres | Priorytet |
|---|---|---|
| **M1** | Setup projektu (HTML boilerplate, Tailwind CDN, Inter font, CSS vars, reset) | Krytyczny |
| **M2** | Nawigacja + Hero | Krytyczny |
| **M3** | Sekcja Usługi (bento) | Wysoki |
| **M4** | Sekcja Co zyskasz + Jak współpracujemy | Wysoki |
| **M5** | Sekcja O mnie + Co mnie wyróżnia | Średni |
| **M6** | Sekcja Kontakt + Footer | Wysoki |
| **M7** | Animacje scroll + micro-interactions | Średni |
| **M8** | SEO meta, JSON-LD, og:image | Wysoki |
| **M9** | Responsywność mobile/tablet | Krytyczny |
| **M10** | Optymalizacja performance, finalne testy | Wysoki |

---

## 11. Konwencje kodu

```html
<!-- Sekcje: id + klasa BEM-style lub Tailwind utility -->
<section id="uslugi" class="section-services py-24 bg-white">
  <div class="container mx-auto max-w-6xl px-6">
    ...
  </div>
</section>

<!-- Animowane elementy: data-anim + klasa initial -->
<div class="card fade-up opacity-0" data-delay="100">...</div>

<!-- JS selektory: data-* atrybuty, nie klasy CSS -->
<button data-menu-toggle>Menu</button>
```

```js
// main.js — struktura
// 1. DOM ready
// 2. Navigation (scroll behavior, hamburger)
// 3. IntersectionObserver dla animacji
// 4. Smooth scroll dla linków kotwicowych
// 5. Formularz kontaktowy (walidacja + submit)
```

---

## 12. Notatki i decyzje otwarte

| # | Temat | Status |
|---|---|---|
| 1 | Zdjęcie właściciela do sekcji "O mnie" | ⏳ Do dostarczenia |
| 2 | Numer telefonu i miasto | ⏳ Do uzupełnienia |
| 3 | Przykłady realizacji / portfolio | ⏳ Do dostarczenia lub placeholder |
| 4 | Formularz: Formspree vs mailto | ⏳ Decyzja właściciela |
| 5 | Blog: osobna strona czy zewnętrzna platforma | ⏳ Poza scopem v1.0 |
| 6 | Zdjęcie OG (1200×630px) | ⏳ Do przygotowania |
| 7 | Miasto/region w tagowaniu SEO | ⏳ Do uzupełnienia |

---

*Dokument żyje razem z projektem. Aktualizuj przy każdej istotnej zmianie scopu.*
