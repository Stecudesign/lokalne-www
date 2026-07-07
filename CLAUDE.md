# CLAUDE.md — lokalnewww.pl

Strona landing page freelancera web-developera. Cel: pozyskiwanie klientów — małe lokalne firmy. Stack: czysty HTML + własny CSS (`style.css`) + Tailwind CDN (utility classes) + Vanilla JS. Jeden plik `index.html`, jeden plik `style.css`. Brak buildu, brak frameworka.

Pełne wymagania produktowe: `PRD_lokalnewww.md`.
Design system marki: `.claude/skills/design.md` — czytaj przed każdą pracą nad UI.

**Skille dostępne w projekcie** (`.claude/skills/`):
- `frontend-design` — używaj przy budowaniu nowych sekcji/komponentów
- `webdesign` — używaj przy audycie UI/UX/dostępności
- `simplify` — używaj przy przeglądaniu CSS/HTML pod kątem redundancji (globalny skill, nie w folderze)

---

## Struktura plików

```
index.html      ← cała strona (landing page)
oferta.html     ← strona Oferta (root, URL /oferta) — usługi w akordeonie, motyw ciemny
style.css       ← wszystkie custom style — wspólny dla index.html i oferta.html
img/
  logopin3Dv1.png          ← logo pin 3D (64×64)
  herov8.png               ← tło hero (desktop)
  herov8mobile.png         ← tło hero (mobile)
  lenovoomniev2.png        ← zdjęcie właściciela (sekcja O mnie)
  bento-stronyinternetowe.png
  bento-seo.png
  obslugawww.png
  responsywnauslugav3.png       ← mockup urządzeń (tile-4 Responsywność)
  oferta-seo.webp               ← ilustracja usługi SEO (oferta.html, Higgsfield)
  oferta-strony.webp            ← ilustracja usługi Strony internetowe (oferta.html)
  oferta-design.webp            ← ilustracja usługi Indywidualny design (oferta.html)
  oferta-responsywnosc.webp     ← ilustracja usługi Responsywność (oferta.html)
  oferta-opieka.webp            ← ilustracja usługi Opieka nad stroną (oferta.html)
  oferta-analityka.webp         ← ilustracja usługi Analityka Google (oferta.html)
examples/       ← screenshoty referencyjne
PRD_lokalnewww.md
```

---

## Sekcje strony (kolejność w index.html)

| ID | Klasa/styl sekcji | Status |
|---|---|---|
| `#nav` | `.nav-island` — floating pill, białe półprzezroczyste szkło (`rgba(255,255,255,0.28)`), ciemne linki, aktywna pozycja = niebieski pill (`.is-active`, scrollspy) | ✅ gotowe |
| `#hero` | tło `img/herov8.png` + overlay `rgba(255,255,255,0.2)`, treść do lewej | ✅ gotowe |
| `#co-zyskasz` | `.benefit-section` — tło `#F8FAFC`, 3 karty w gridzie | ✅ gotowe |
| `#uslugi` | `.bento-section` — tło niebieski gradient, 6 kafli | ✅ gotowe |
| `#realizacje` | `.portfolio-section` — tło `#080C14`, slider transform-driven (GSAP `x`/translate3d) 5 kart, coverflow (aktywna karta = wyśrodkowana, `scale 1`/`opacity 1`; pozostałe `0.96`/`0.62`), prev/next + drag/touch (Pointer Events) + wolny autoplay z pauzą na hover | ✅ gotowe |
| `#proces` | `.process-section` — tło `#0F172A`, 4 kroki z numerami | ✅ gotowe |
| `#wyrozniam-sie` | `.usp-section` — tło `#fff`, 2×2 grid kart | ✅ gotowe |
| `#o-mnie` | `.about-section` — tło `#1A2540`, 2 kolumny foto+tekst | ✅ gotowe |
| `#kontakt` | — | ❌ brakuje (PRD 6.8) |
| `#footer` | `.footer-section` — tło `#F8FAFC`, logo+nav+social | ✅ gotowe |

---

## Strona Oferta (`oferta.html`)

Plik `oferta.html` w katalogu głównym (URL `/oferta`). Jedna strona opisująca całą ofertę; 6 usług = te z „zajawki" na index.html (bento), każda w osobnym pełnoszerokościowym pasie z akordeonem. Ścieżki zasobów root-relative (`img/`, `style.css`).

**Motyw: jasny hero z siatką + naprzemienne pasy** — `<body class="offer-page">`. Hero jasny (gradient `#F0F4FF→#fff`) z **siatką („kratka") w tle** (`.svc-hero::before` = grid 54px w kolorze `rgba(37,99,235,0.09)`, maska radialna, żeby wygasała po bokach) + niebieska poświata u dołu (`::after`). Poniżej **6 pasów usług naprzemiennie ciemny/jasny** (`.svc-band--dark` / `.svc-band--light`): SEO (ciemny), Strony internetowe (jasny), Indywidualny design (ciemny), Responsywność (jasny), Opieka nad stroną (ciemny), Analityka Google (jasny). Pas ciemny = `#0A1120` + radialne blue glow; pas jasny = `#F8FAFC`. Style w `style.css` w sekcji „PODSTRONA OFERTY — jasny hero z siatką + naprzemienne pasy" (po „REDESIGN PODSTRONY OFERTY"). Stara sekcja „MOTYW CIEMNY PODSTRONY OFERTY" (`body.offer-dark`) jest teraz **martwym kodem**.

**Sekcje szablonu (w kolejności):**

1. `#svc-hero .svc-hero` — **minimalny, wyśrodkowany**: H1 „Oferta" + lead na 3 zdania (`.svc-hero-sub`). Bez breadcrumbu i bez osobnej sekcji intro (scalone w hero). Jasny motyw z siatką w tle (`::before`) i poświatą (`::after`); content `.svc-hero-inner` na `z-index: 1`.
2. **6× `.svc-band`** (id `svc-seo`, `svc-strony`, `svc-design`, `svc-responsywnosc`, `svc-opieka`, `svc-analityka`) — każdy pas ma jeden `.svc-block` (grid 2 kol; `.svc-block--rev` = obraz z lewej dla pasów jasnych). Tekst: `.svc-block-num` + `.svc-block-h3` + `.svc-block-desc` + **akordeon** `.svc-accordion` z 3 pozycjami „co obejmuje" (`.svc-acc-item`, pierwsza `.is-open`; panel przez `grid-template-rows 0fr→1fr`, chevron-down obraca się o 180° + niebieskie kółko). **Bez tagów** (`.svc-tag` usunięte). Obraz `.svc-block-visual` z ilustracją Higgsfield (`img/oferta-*.webp`, minimal flat, paleta marki).
3. `.svc-cta` — baner: ciemna navy karta `.svc-cta-card` (radius 28px, blue glow) z H2 + żółty `btn-primary`.
4. `#realizacje .svc-work` — tło `#F8FAFC`, siatka 3 kart `.svc-work-card` (thumb + tagi + „Zobacz stronę ↗" + nazwa).
5. `.footer-section` — reuse z index.html.

> Sekcja opinii (`.testi-section`) została **usunięta** z oferta.html (CSS `.testi-*` pozostał jako martwy kod).

**Ścieżki zasobów:** root-relative — `img/`, `style.css` (plik jest w katalogu głównym)

**Nawigacja:** linki w nav i footer zawierają `/#section` (prefix ukośnikiem do roota)

**Nav JS:** `updateNav` wymusza `nav--light` gdy scroll nad pasem ciemnym — `darkSectionIds = ['svc-seo','svc-design','svc-opieka']` (jak scrollspy na index.html). Akordeon: klik na `.svc-acc-head` toggluje `.is-open` (zamyka pozostałe w tym samym `.svc-accordion`).

**Animacje GSAP pasów usług (`oferta.html`).** Analogicznie do index.html: GSAP + ScrollTrigger + Lenis z CDN na dole strony, klasa `.gsap-enhance` dodawana synchronicznie w `<head>`, zdejmowana przez JS gdy brak GSAP / `prefers-reduced-motion` (treść zostaje statycznie widoczna). Osobny `<script>` IIFE ze **spięciem Lenis z tickerem GSAP** oraz `gsap.matchMedia()`:
> - **Desktop/tablet (`min-width: 768px`)** — *alternating reveal* per pas (`ScrollTrigger` na `.svc-block`, `start: 'top 78%'`, `once`), `ease: power3.out`. Tekst wjeżdża od swojej strony: pasy ciemne (tekst z lewej) `x:-30, y:30`, pasy jasne `.svc-block--rev` (tekst z prawej) `x:+30, y:30`. Kolejność timeline: **numer/label** (`y:12`, `letter-spacing 0.16→0.02em`) → **H3** (`y:40`) → **opis** (`y:24`) → **`.svc-acc-item` stagger 0.1**; `.svc-block-visual` w tym samym timeline `x:±60, y:30, scale:0.94→1, rotation:±1.5°→0`, `duration:1`. Po revealu `clearProps: 'transform'` na tekstach (żeby działał CSS accordionu). **Parallax** grafiki: osobny scrub-`ScrollTrigger` na `.svc-block-img` (`yPercent 5→-5`, `start: 'top bottom'`, `end: 'bottom top'`); baza `scale:1.12` żeby ruch nie odsłaniał krawędzi w kadrze `overflow:hidden`. **Hover** grafiki (`scale 1.12↔1.16`) sterowany GSAP-em (`gsap.quickTo`) — komponuje się z parallaxem, bez konfliktu transformów.
> - **Mobile (`max-width: 767px`)** — lżej: tylko `opacity + y` (numer/H3/opis `y:24`, akordeon `y:20` stagger 0.08, grafika `y:30`), **bez `x`, `scale`, `rotation` i bez parallaxu**.
>
> **Wydajność:** animowane wyłącznie `transform` + `opacity` (`force3D`), `will-change: transform` tylko na `.svc-block-img`. Stany początkowe (`opacity:0`) chowane w CSS pod `html.gsap-enhance body.offer-page .svc-band …` (sekcja „Stany początkowe animacji GSAP (podstrona oferty)" w `style.css`); wrapper `.svc-block.fade-up` zneutralizowany (`opacity:1; transform:none`), a CSS-owy `transition`/`:hover` na `.svc-block-img` zdjęty (transform grafiki należy do GSAP). `@media (prefers-reduced-motion: reduce)` un-hide jako zabezpieczenie zanim JS zdejmie klasę. Pozostałe `.fade-up` na podstronie (hero, CTA, realizacje) dalej obsługuje `IntersectionObserver`.

**Klasy CSS podstron (prefiksy `svc-`, `testi-`):** `.svc-hero`, `.svc-hero-inner`, `.svc-hero-label`, `.svc-hero-h1`, `.svc-hero-h1-accent`, `.svc-hero-sub`, `.svc-hero-actions`, `.svc-section`, `.svc-services-grid`, `.svc-service-card`, `.svc-service-icon`, `.svc-service-title`, `.svc-service-desc`, `.svc-service-btn`, `.svc-block`, `.svc-block--rev`, `.svc-block-text`, `.svc-block-num`, `.svc-block-h3`, `.svc-block-desc`, `.svc-block-visual`, `.svc-block-img`, `.svc-accordion`, `.svc-acc-item`, `.svc-acc-head`, `.svc-acc-title`, `.svc-acc-chevron`, `.svc-acc-panel`, `.svc-acc-panel-inner`, `.svc-tags`, `.svc-tag`, `.svc-cta`, `.svc-cta-card`, `.svc-cta-glow`, `.svc-cta-h2`, `.svc-cta-btn`, `.svc-work`, `.svc-work-header`, `.svc-work-label/h2/sub`, `.svc-work-grid`, `.svc-work-card`, `.svc-work-thumb`, `.svc-work-meta`, `.svc-work-tags`, `.svc-work-tag`, `.svc-work-visit`, `.svc-work-name`, `.testi-section`, `.testi-header`, `.testi-label`, `.testi-h2`, `.testi-nav`, `.testi-nav-btn`, `.testi-nav-btn--active`, `.testi-track`, `.testi-card`, `.testi-stars`, `.testi-quote`, `.testi-foot`, `.testi-author`, `.testi-avatar`, `.testi-author-info`, `.testi-name`, `.testi-role`, `.testi-date`

> **Uwaga:** stare klasy podstrony (`.offer-hero*`, `.offer-benefits*`, `.offer-cta*`, `.feat-*`) pozostały w `style.css` jako martwy kod po redesignie — do usunięcia przy sprzątaniu (nie są używane przez index.html).

---

## Design system — kolory

```css
/* CSS variables (zdefiniowane w style.css :root) */
--color-navy:        #1A2540   /* granat — primary dark */
--color-navy-mid:    #243155   /* granat mid */
--color-blue:        #2563EB   /* niebieski — CTA, akcenty */
--color-blue-light:  #60A5FA
--color-yellow:      #FACC15   /* żółty akcent */
--color-yellow-dark: #EAB308
--color-text-primary:   #0F172A
--color-text-secondary: #475569
--color-text-muted:     #94A3B8
--color-border:      #E2E8F0
--shadow-lg: 0 12px 40px rgba(15,23,42,0.12), 0 4px 12px rgba(15,23,42,0.07)
```

Tailwind config (w `<script>` w head): `navy: #1A2540`, `navy-mid: #243155`, font `Inter`, `max-w-container: 1200px`.

---

## Sekcja 6.2 — HERO (`#hero`)

Tło: `img/herov8.png` (desktop), `img/herov8mobile.png` (mobile jako `background-image` none + `.hero-heading-block`).
Overlay: `#hero::before` z `rgba(255,255,255,0.2)`.

Treść wyrównana do lewej. Kontener: `w-full max-w-container mx-auto px-6`.

`.hero-highlight-wrap` — navy tło ze skosem `skewX(-6deg)`, animacja `highlight-in` odkrywa tło od lewej. Tekst wewnątrz: żółty, pojawia się przez `text-in` po 1s.

---

## Sekcja 6.3 — CO ZYSKASZ (`.benefit-section`)

Tło: `#F8FAFC`, padding: `5rem 0 7rem`. Karty w `.benefit-grid` — 3 kolumny, białe tło, obrys `#E2E8F0`.

Struktura karty:
```html
<div class="benefit-card">
  <div class="benefit-card-top">
    <h3 class="benefit-title">...</h3>
    <div class="benefit-arrow [benefit-arrow--featured]">↗ svg</div>
  </div>
  <p class="benefit-desc">...</p>
</div>
```

`.benefit-arrow--featured` = wypełniony niebieski krąg (pierwsza karta). Hover: strzałka wypełnia się czarno.

---

## Sekcja 6.4 — BENTO GRID (`.bento2-section`)

Tło: `#F0F4FF` (jasny niebieski). Kafelki: białe (`#fff`), `border-radius: 24px`, padding `32px`.

Grid: 6 kolumn, 3 rzędy (`minmax(160px, auto)` / `minmax(130px, auto)` / `auto`):

- tile-1: `1/3 × 1/3` (duży, tekst + obraz dół)
- tile-2: `3/7 × 1` (szeroki, obraz `.bento2-tile2-deco` absolute)
- tile-3: `3/5 × 2` (gradient niebieski)
- tile-4: `5/7 × 2` — **Responsywność** — tekst góra + `.bento2-t4-visual` (panel dół, `height: 190px`, `overflow: hidden`, `background: #fff`) z obrazem `img/responsywnauslugav3.png`
- tile-5: `1/4 × 3` (tekst góra + `.bento2-t5-visual` panel dół)
- tile-6: `4/7 × 3`

Wzorzec tile z obrazem dolnym (tile-4, tile-5): `padding: 28px 28px 0`, `.bento2-copy { flex: 0 0 auto }`, visual panel bleeding edge-to-edge przez ujemne marginesy `-28px`.

---

## Sekcja 6.5 — JAK WSPÓŁPRACUJEMY (`.process-section`)

Tło: `#0F172A`, padding: `7rem 0`. 4 kroki w gridzie 4 kolumny.

Każdy krok:
- `.process-badge` — okrągły, `border: blue-600/45%`, kolor `#60A5FA`
- `.process-badge--final` — wariant żółty (krok 04)
- `.process-ghost` — dekoracyjny numer `clamp(4.5rem, 7vw, 8.5rem)`, `color: rgba(37,99,235,0.09)`
- Przerywana linia między krokami: `::after` z `border-top: 2px dashed rgba(37,99,235,0.25)`

Mobile: grid 1-kolumnowy, linia zamienia się w pionową.

---

## Sekcja 6.6 — CO MNIE WYRÓŻNIA (`.usp-section`)

Tło: `#fff`, padding: `7rem 0`. Grid 2×2 (`max-width: 900px`, `margin: 0 auto`).

Karty `.usp-card`: tło `#F8FAFC` → hover białe + `shadow-lg` + `translateY(-4px)`.

Ikony `.usp-icon-box` — warianty kolorystyczne:

- domyślny: niebieski (`rgba(37,99,235,0.08)`)
- `--amber`: bursztynowy (`rgba(250,204,21,0.14)`, kolor `#B45309`)
- `--navy`: granatowy (`rgba(26,37,64,0.07)`)
- `--green`: zielony (`rgba(16,185,129,0.09)`, kolor `#059669`)

---

## Sekcja 6.7 — O MNIE (`.about-section`)

Tło: `#1A2540` (navy), padding: `7rem 0`. Layout 2 kolumny: foto lewo, tekst prawo.

Zdjęcie: `img/lenovoomniev2.png`, klasa `.about-photo-img` (`width: 100%; height: auto; border-radius: 20px; opacity: 0.88`).

Ramka dekoracyjna: `.about-photo-frame::before` — `border: 2px solid rgba(37,99,235,0.45)`, `inset: -12px`, `border-radius: 26px`.

CTA: `.btn-about-cta` — żółty pill z animowaną strzałką (`gap` rośnie na hover).

Mobile: zdjęcie nad tekstem, jedna kolumna.

---

## Footer (`.footer-section`)

Tło: `#F8FAFC`, `border-top: 1px solid #E2E8F0`, padding: `4rem 0 2.5rem`.

Layout `.footer-top` (3 elementy flex):

- **Lewo** `.footer-brand`: logo + tagline + `.btn-footer-cta` (żółty pill "Bezpłatna konsultacja")
- **Środek** `.footer-nav`: linki Oferta / O mnie / Blog / Kontakt
- **Prawo** `.footer-social`: ikony LinkedIn + Facebook w okrągłych ramkach

Bottom bar: copyright po lewej, linki Polityka prywatności + Realizacja po prawej. Oddzielony `border-top: 1px solid #E2E8F0`.

---

## Animacje scroll

Klasa `.fade-up`: `opacity: 0; transform: translateY(30px)` → po dodaniu `.in-view` przechodzi do widocznego stanu. JS w `<script>` na dole index.html używa `IntersectionObserver` (`threshold: 0.12`). Delay przez `style="transition-delay: Xms"`.

Animacje page-load (hero): klasa `.anim-init` + `.visible` dodawana przez `requestAnimationFrame`.

**Animacje GSAP (progresywne wzbogacenie).** Biblioteki GSAP + ScrollTrigger + Lenis ładowane z CDN na dole `index.html`. Klasa `.gsap-enhance` dodawana synchronicznie w `<head>`; usuwana przez JS gdy brak GSAP lub `prefers-reduced-motion` → treść zostaje statycznie widoczna. Stany początkowe (`opacity: 0`) chowane w CSS przez `.gsap-enhance #<section> ...` (analogicznie do `#proces` i `#uslugi`).

- **Sekcja `#uslugi` (bento):** osobny `<script>` IIFE. Master timeline (`ScrollTrigger` na `.bento2-grid`, `start: 'top 80%'`, `once`) robi sekwencyjny reveal per kafel ze staggerem `i * 0.12`: karta (`y40→0`, `scale 0.96→1`) → nagłówek → opis → CTA → grafika (`y30→0`, `scale 0.92→1`, `rotation 1→0`). Karty/teksty używają `clearProps: 'transform'`, by po revealu działał hover (CSS). **Parallax** grafik to osobny scrub-ScrollTrigger na `img` (nie na kontenerze) — `y: 12 → -12`; tile-1 ma `gsap.set(img, { yPercent: 22 })` odtwarzające bazowe `translateY(22%)`, żeby parallax się z nim składał, a nie nadpisywał. **Hover** grafiki (`scale 1.03`) sterowany GSAP-em (składa się z parallaxem), lift karty + cień w CSS (`transition 0.35s ease`). Micro-anim strzałki CTA tile-2: `translateX(3px)` (CSS).

- **Sekcja `#realizacje` (slider):** osobny `<script>` IIFE. Struktura DOM: `.portfolio-slider` (viewport, `overflow: hidden` w trybie `.is-gsap`) → `.portfolio-track` (pozycja przez GSAP `x` = `translate3d`, `force3D: true`, jedyny `will-change: transform` obok aktywnej karty) → `.portfolio-card` (GSAP `scale`/`opacity` stanu aktywnego) → `.portfolio-card-inner` (hover lift/zoom w CSS — osobna warstwa, by transformy się nie kolidowały). **Animujemy wyłącznie `transform` + `opacity`.** Ruch tracka: `moveTo(x)` z `clamp` do `[minX, 0]`; `step` = różnica `offsetLeft` dwóch kart; `minX` liczony z `clientWidth − padding − scrollWidth`. **Aktywna karta** (coverflow) wykrywana przez `getBoundingClientRect` (środek najbliżej środka viewportu) → `scale 1`/`opacity 1`, reszta `0.96`/`0.62` + toggle `.is-active` (nośnik `will-change`). **Drag/touch:** Pointer Events + `setPointerCapture`, rubber-band 0.35 poza granicami, snap do `Math.round(x/step)*step` na `pointerup`; guard „ghost click" gdy `moved`. **Autoplay:** `setInterval` 5 s, `autoNext` przesuwa o `step` i wraca płynnie na `0` po ostatniej; pauza na `mouseenter`, wznowienie na `mouseleave`; włączany dopiero po revealu wejścia i tylko gdy `minX < 0`. **Reveal wejścia** (`ScrollTrigger`, `start: 'top 78%'`, `once`): label (`opacity`+`y16`+`letter-spacing 0.30→0.15em`) → H2 (`y40`) → przyciski (`scale 0.9→1`, `back.out`) → karty (`y60`, `scale 0.94→1`, stagger `0.12`); po zakończeniu `updateActive` + start autoplay. **Fallback** (brak GSAP / `reduce-motion`): natywny scroll poziomy, przyciski robią `scrollBy`. Stany startowe `opacity: 0` chowane w CSS przez `.gsap-enhance #realizacje …` (z un-hide dla `prefers-reduced-motion`).

---

## Konwencje CSS

- Custom style TYLKO w `style.css` — nie dodawaj `<style>` inline do HTML
- Tailwind używany tylko do utility (grid, flex, padding, margin, text-center) — nie do komponentów
- Inline style (`style="..."`) tylko dla `transition-delay` na animowanych elementach
- Nazwy klas: kebab-case, prefiks sekcji: `.benefit-*`, `.bento-*`, `.process-*`, `.usp-*`, `.about-*`, `.footer-*`
- Media queries bezpośrednio po stylach sekcji której dotyczą, w kolejności: `max-width: 767px` → `min-width: 768px and max-width: 1023px`

---

## Ważne decyzje projektowe

- Hero: treść do lewej, kontener `w-full` — bez wewnętrznego `max-width` żeby H1 mieścił się w jednej linii
- Hero highlight (`dla lokalnych biznesów`) — navy tło skewX(-6deg), animacja clip-path od lewej
- Bento: białe kafelki na niebieskim gradiencie (nie ciemne jak w PRD)
- Co zyskasz: jasne tło (nie ciemne jak w PRD) — karty z okrągłymi strzałkami
- Sekcja O mnie: zdjęcie z `opacity: 0.88` żeby miękko wtapiało się w navy tło
- Footer: ciemne tło `var(--color-navy)` (#1A2540) — spójność z sekcją O mnie, tekst dostosowany do ciemnego tła
- Logo: `img/logopin3Dv1.png` (64×64px nav, 40×40px footer) + tekst `lokalne` (800) + `www.pl` (400, muted)

---

## Protokół aktualizacji dokumentacji

### Kiedy aktualizować CLAUDE.md

**Po każdej zrealizowanej sekcji:**

- Zmień status w tabeli sekcji: `❌ brakuje` → `✅ gotowe`
- Dodaj wpis opisujący sekcję (tło, layout, klasy CSS) analogicznie do istniejących

**Po każdej decyzji projektowej odbiegającej od PRD:**

- Dodaj wpis w sekcji "Ważne decyzje projektowe" z uzasadnieniem

**Po dodaniu nowego pliku graficznego:**

- Dodaj pozycję w liście `img/` w sekcji "Struktura plików"

**Po dodaniu nowej konwencji CSS/HTML:**

- Zaktualizuj sekcję "Konwencje CSS"

### Kiedy aktualizować PRD_lokalnewww.md

- Po rozstrzygnięciu otwartej decyzji → zmień status w tabeli sekcji 12 na ✅ i wpisz wynik
- Po zmianie zakresu lub wymagań → zaktualizuj odpowiednią sekcję 6.x
- Po ukończeniu etapu milestones (sekcja 10) → oznacz etap jako zrealizowany

### Kiedy i jak aktualizować design-system.html

`design-system.html` to żywa dokumentacja wizualna — musi odzwierciedlać aktualny stan `style.css` i `index.html`.

**Aktualizuj po każdej pracy UI, która wprowadza:**

- Nowy komponent lub wariant (nowa klasa CSS np. `.contact-*`)
- Zmianę w design tokensach (kolory, typography, shadows w `:root`)
- Nowy wzorzec animacji lub micro-interaction
- Nową sekcję strony (dodaj sekcję w sidebarsie + sekcję demo w main)

**Zasady aktualizacji design-system.html:**

- Każdy nowy komponent: osobna `.ds-section` z tytułem, opisem i live demo
- Tokeny CSS zawsze zsynchronizowane z wartościami z `style.css :root`
- Sidebar nawigacja: dodaj link do nowej sekcji w odpowiedniej grupie (`ds-nav-group`)
- Status badge sekcji: `Live` (zaimplementowane w index.html) vs `Draft` (planowane)

**Format nowej sekcji w design-system.html:**

```html
<section class="ds-section" id="contact">
  <div class="ds-section-header">
    <h2 class="ds-section-title">Kontakt</h2>
    <span class="ds-badge ds-badge--live">Live</span>
  </div>
  <p class="ds-section-desc">Opis komponentu...</p>
  <!-- live demo lub code snippet -->
</section>
```
