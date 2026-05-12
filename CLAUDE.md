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
index.html      ← cała strona
style.css       ← wszystkie custom style (bez Tailwinda)
img/
  logopin3Dv1.png          ← logo pin 3D (64×64)
  herov8.png               ← tło hero (desktop)
  herov8mobile.png         ← tło hero (mobile)
  lenovoomniev2.png        ← zdjęcie właściciela (sekcja O mnie)
  bento-stronyinternetowe.png
  bento-seo.png
  obslugawww.png
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
| `#proces` | `.process-section` — tło `#0F172A`, 4 kroki z numerami | ✅ gotowe |
| `#wyrozniam-sie` | `.usp-section` — tło `#fff`, 2×2 grid kart | ✅ gotowe |
| `#o-mnie` | `.about-section` — tło `#1A2540`, 2 kolumny foto+tekst | ✅ gotowe |
| `#kontakt` | — | ❌ brakuje (PRD 6.8) |
| `#footer` | `.footer-section` — tło `#F8FAFC`, logo+nav+social | ✅ gotowe |

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

## Sekcja 6.4 — BENTO GRID (`.bento-section`)

Tło: `#F0F4FF` (jasny niebieski). Kafelki: białe (`#fff`), `border-radius: 20px`, padding `28px`.

Grid: 3 kolumny. tile-1: `grid-column: 1/3; grid-row: 1/3`. tile-5: navy background. tile-6: ostatni w rzędzie.

Dekoracje — obrazy absolute w tile-1 (`.bento-tile1-deco`), tile-3 (`.bento-tile3-deco`), tile-4 (`.bento-tile4-deco`) z `opacity: 0.4`.

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
- Footer: jasne tło (nie ciemne jak w PRD) — spójność z resztą strony
- Logo: `img/logopin3Dv1.png` (64×64px nav, 40×40px footer) + tekst `lokalne` (800) + `www.pl` (400, muted)
