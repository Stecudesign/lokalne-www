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
style.css       ← wszystkie custom style — wspólny dla index.html i podstron oferta/
oferta/
  strony-internetowe.html  ← szablon podstrony oferty (skopiuj dla kolejnych usług)
img/
  logopin3Dv1.png          ← logo pin 3D (64×64)
  herov8.png               ← tło hero (desktop)
  herov8mobile.png         ← tło hero (mobile)
  lenovoomniev2.png        ← zdjęcie właściciela (sekcja O mnie)
  bento-stronyinternetowe.png
  bento-seo.png
  obslugawww.png
  responsywnauslugav3.png       ← mockup urządzeń (tile-4 Responsywność)
examples/       ← screenshoty referencyjne
PRD_lokalnewww.md
```

---

## Sekcje strony (kolejność w index.html)

| ID | Klasa/styl sekcji | Status |
|---|---|---|
| `#nav` | `.nav-island` — floating pill, transparent → shadow po scrollu | ✅ gotowe |
| `#hero` | tło `img/herov8.png` + overlay `rgba(255,255,255,0.2)`, treść do lewej | ✅ gotowe |
| `#co-zyskasz` | `.benefit-section` — tło `#F8FAFC`, 3 karty w gridzie | ✅ gotowe |
| `#uslugi` | `.bento-section` — tło niebieski gradient, 6 kafli | ✅ gotowe |
| `#realizacje` | `.portfolio-section` — tło `#080C14`, slider poziomy 5 kart, prev/next + drag-scroll | ✅ gotowe |
| `#proces` | `.process-section` — tło `#0F172A`, 4 kroki z numerami | ✅ gotowe |
| `#wyrozniam-sie` | `.usp-section` — tło `#fff`, 2×2 grid kart | ✅ gotowe |
| `#o-mnie` | `.about-section` — tło `#1A2540`, 2 kolumny foto+tekst | ✅ gotowe |
| `#kontakt` | — | ❌ brakuje (PRD 6.8) |
| `#footer` | `.footer-section` — tło `#F8FAFC`, logo+nav+social | ✅ gotowe |

---

## Podstrony oferty (`oferta/`)

Szablon: `oferta/strony-internetowe.html`. Skopiować dla każdej kolejnej usługi.

**Sekcje szablonu (w kolejności):**

1. `.offer-hero` — dark navy gradient, 2 kolumny: tekst (breadcrumb + H1 + opis + badges + CTA) | SVG mockup przeglądarki z floating stat-card
2. `.offer-benefits-section` — tło `#F0F4FF`, 3×2 grid kart `.offer-benefit-card` (ikona + tytuł + opis)
3. `.process-section` — reuse z index.html (tło `#F8FAFC`), 4 kroki
4. `.usp-section` — reuse z index.html (tło `#fff`), layout 2-kolumnowy
5. `#realizacje .portfolio-section` — reuse z index.html (tło `#080C14`)
6. `.offer-cta-section` — tło `var(--color-navy)`, centered CTA z przyciskiem + numerem telefonu
7. `.footer-section` — reuse z index.html

**Ścieżki zasobów w podstronach:** `../img/`, `../style.css`

**Nawigacja:** linki w nav i footer zawierają `/#section` (prefix ukośnikiem do roota)

**Nav JS:** `darkSectionIds = ['offer-hero', 'realizacje']` — białe linki gdy nav nad ciemnymi sekcjami

**Klasy CSS podstron (prefix `offer-`):** `.offer-hero`, `.offer-hero-inner`, `.offer-hero-text`, `.offer-hero-visual`, `.offer-mockup-wrap`, `.offer-hero-mockup`, `.offer-stat-card`, `.offer-label`, `.offer-h1`, `.offer-h1-accent`, `.offer-sub`, `.offer-hero-badges`, `.offer-badge`, `.offer-hero-actions`, `.offer-benefits-section`, `.offer-benefits-grid`, `.offer-benefit-card`, `.offer-benefit-icon`, `.offer-benefit-title`, `.offer-benefit-desc`, `.offer-cta-section`, `.offer-cta-inner`, `.offer-cta-h2`, `.offer-cta-sub`, `.offer-cta-actions`, `.btn-offer-ghost`, `.btn-offer-phone`

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
