# CLAUDE.md — lokalnewww.pl

Strona landing page freelancera web-developera. Cel: pozyskiwanie klientów — małe lokalne firmy. Stack: czysty HTML + własny CSS (`style.css`) + Tailwind CDN (utility classes) + Vanilla JS. Jeden plik `index.html`, jeden plik `style.css`. Brak buildu, brak frameworka.

Pełne wymagania produktowe: `PRD_lokalnewww.md`.

---

## Struktura plików

```
index.html      ← cała strona
style.css       ← wszystkie custom style (bez Tailwinda)
img/            ← logopin3Dv1.png (logo), logolokalnewww.png
examples/       ← screenshoty referencyjne (heronaviagation.png, services.png)
PRD_lokalnewww.md
```

---

## Sekcje strony (kolejność w index.html)

| ID | Klasa/styl sekcji | Status |
|---|---|---|
| `#nav` | sticky, transparent → white po scrollu | ✅ gotowe |
| `#hero` | `bg-white`, pełna wysokość (`min-height: 100svh`) | ✅ gotowe |
| `#co-zyskasz` | `.benefit-section` — tło `#F8FAFC` | ✅ gotowe |
| `#uslugi` | `.bento-section` — tło niebieski gradient | ✅ gotowe |

Sekcje brakujące (z PRD): Jak współpracujemy (6.5), Co mnie wyróżnia (6.6), O mnie (6.7), Kontakt (6.8), Footer (6.9).

---

## Design system — kolory

```css
/* CSS variables (zdefiniowane w style.css :root) */
--color-navy:        #1A2540   /* granat — primary dark */
--color-blue:        #2563EB   /* niebieski — CTA, akcenty */
--color-blue-light:  #60A5FA
--color-yellow:      #FACC15   /* żółty akcent */
--color-text-primary:   #0F172A
--color-text-secondary: #475569
--shadow-lg: 0 12px 40px rgba(15,23,42,0.12)...
```

Tailwind config (w `<script>` w head): `navy: #1A2540`, `navy-mid: #243155`, font `Inter`, `max-w-container: 1200px`.

---

## Sekcja 6.3 — CO ZYSKASZ (`.benefit-section`)

Tło: `#F8FAFC`, padding: `5rem 0 7rem`. Karty w `.benefit-grid` — 3 kolumny, białe tło, obrys `#E2E8F0`, brak ciężkich cieni.

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

`.benefit-arrow--featured` = wypełniony niebieski krąg (pierwsza karta). Hover na karcie: tło `#F8FAFC` + strzałka wypełnia się czarno.

---

## Sekcja 6.4 — BENTO GRID (`.bento-section`)

Tło: `linear-gradient(145deg, #e4edfc 0%, #cfdaf8 100%)`. Wszystkie kafelki: białe (`#fff`), `border-radius: 24px`, padding `26px`.

Grid: 3 kolumny, tile-1 zajmuje `grid-row: 1/3`, tile-6 zajmuje `grid-column: 1/4`.

**Tile z zdjęciami** (1, 2, 3) używają `.bento-photo-frame`:
```html
<div class="bento-photo-frame bento-photo-frame--lg">   <!-- tile 1: flex:1, rotate(1.5deg) -->
<div class="bento-photo-frame bento-photo-frame--sm bento-photo-frame--tilt-l">  <!-- tile 2: rotate(-3.5deg) -->
<div class="bento-photo-frame bento-photo-frame--sm bento-photo-frame--tilt-r">  <!-- tile 3: rotate(3.5deg) -->
```

**Tile bez zdjęć** (4, 5): `.bento-num` + `.bento-copy`.

**Tile 6 (WordPress, full-width)**: `.bento-tile6-body` (flex row) z `.bento-copy` + `.bento-tile6-badges`.

Numery: `.bento-num` — małe, szare (`#94A3B8`), górny lewy róg.

Foto — źródła Unsplash:
- Tile 1: `photo-1517694712202-14dd9538aa97` (kod na ekranie laptopa)
- Tile 2: `photo-1512941937669-90a1b58e7e9c` (telefon)
- Tile 3: `photo-1460925895917-afdab827c52f` (analytics/wykres)

---

## Animacje scroll

Klasa `.fade-up`: `opacity: 0; transform: translateY(30px)` → po dodaniu `.in-view` przechodzi do widocznego stanu. JS w `<script>` na dole index.html używa `IntersectionObserver`. Delay przez `style="transition-delay: Xms"`.

---

## Konwencje CSS

- Custom style TYLKO w `style.css` — nie dodawaj `<style>` inline do HTML
- Tailwind używany tylko do utility (grid, flex, padding, margin, text-center) — nie do komponentów
- Inline style (`style="..."`) na elementach HTML tylko tam gdzie już są — nie dodawaj nowych
- Nazwy klas: kebab-case, prefiks sekcji: `.benefit-*`, `.bento-*`, `.services-*`
- Media queries na końcu `style.css`, zawsze w kolejności: `max-width: 767px` (mobile) → `min-width: 768px and max-width: 1023px` (tablet)

---

## Ważne decyzje projektowe

- Sekcja 6.3 zmieniła styl z ciemnego (navy) na jasny — karty na białym tle z okrągłymi przyciskami strzałki
- Sekcja 6.4 zmieniła styl z ciemnych kafelków na białe kafelki na niebieskim gradiencie (inspiracja: dental clinic bento grid reference)
- Zdjęcia w bento gridzie mają efekt lekkiego obrotu (rotate ±3.5°) i box-shadow dla złudzenia unoszenia
- Tile 1 (Strony internetowe) ma zdjęcie jako `flex: 1` — wypełnia środkową część karty między numerem a tekstem
- Logo: `img/logopin3Dv1.png` (64×64px) + tekst HTML: `<span font-weight:800>lokalne</span><span font-weight:400>www.pl</span>` w kolorze `#0F172A`
