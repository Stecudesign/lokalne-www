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
kontakt.html    ← strona Kontakt (root, URL /kontakt) — ciemny motyw: hero + dane/formularz + FAQ (akordeon)
o-mnie.html     ← strona O mnie (root, URL /o-mnie) — jasny motyw: hero „Cześć"+portret, misja+statystyka, numerowana lista
proces.html     ← strona Proces (root, URL /proces) — jasny motyw editorial (styl Snøhetta): hero + lista 6 kroków z ilustracjami
style.css       ← wszystkie custom style — wspólny dla index.html, oferta.html, kontakt.html, o-mnie.html i proces.html
img/
  logopin3Dv1.png          ← logo pin 3D (64×64)
  herov8.png               ← tło hero (desktop)
  herov8mobile.png         ← tło hero (mobile)
  lenovoomniev2.png        ← zdjęcie właściciela (sekcja O mnie)
  bento-stronyinternetowe.png
  bento-seo.png
  obslugawww.png
  responsywnauslugav3.png       ← mockup urządzeń (tile-4 Responsywność)
  szkolaplywaniamockup.webp     ← mockup laptop + telefon (karta 5 slidera #realizacje na index.html)
  szkolaplywaniamockup-laptop.webp ← UŻYWANY w hero oferta.html: ten sam kadr co wyżej, ale bez telefonu — laptop (MacBook Pro)
                                   na słupku startowym, basen w tle. 1254×1254, krok pośredni pipeline’u opisanego niżej
  oferta-hero-laptop.webp       ← WYCINEK samego laptopa, tło przezroczyste, 1400×995. **Nieużywany** — hero wrócił do kadru z basenem
                                   (szkolaplywaniamockup-laptop.webp). Zostaje jako gotowy asset, gdyby wrócić do lewitującego laptopa.
                                   Pipeline: szkolaplywaniamockup.png → Higgsfield nano_banana_pro (usuń telefon) → nano_banana_pro
                                   (laptop lewitujący na białym tle, bez słupka startowego; model NIE usuwa podpory za pierwszym
                                   razem — trzeba osobnego przebiegu z „levitating in mid-air”) → remove_background → przycięcie
                                   przezroczystych marginesów skryptem node (bbox po kanale alfa) → cwebp -q 92 -alpha_q 100
  oferta-seo.webp               ← ilustracja usługi SEO (oferta.html) — pasek wyszukiwarki + podium słupków + pin
  oferta-strony-foto.webp       ← **nieużywana** (23.08.2026 pas „Strony internetowe" wrócił do ilustracji `oferta-strony.webp`) —
                                   FOTOGRAFIA (nie ilustracja): laptop + telefon + tablet
                                   na GRANATOWYM GRADIENCIE z niebieskim blaskiem za urządzeniami (paleta marki: --color-navy /
                                   --color-blue). Na ekranach granatowy interfejs z fotografią budynku, żółte CTA, białe karty.
                                   Tło celowo ciemne — pas `#svc-strony` jest ciemny, więc panel wtapia się w sekcję, a blask
                                   powtarza jej radialną poświatę. 1200×900 (dokładne 4:3).
                                   Pipeline (do powtórzenia na kolejnych sekcjach): nano_banana_pro text-to-image (kompozycja,
                                   ciepłoszare tło) → nano_banana_pro **image-to-image** z `medias[].role = "image_references"`
                                   i promptem „zmień TYLKO tło" (urządzenia i ekrany zostają) → złożenie na canvasie.
                                   ⚠️ **Urządzenia zajmują tylko ~84% × 53% kadru i to jest CELOWE** — patrz „bezpieczny margines"
                                   przy animacjach pasów. Canvas: bbox urządzeń (x=262 y=378 1992×939 w źródle 2400×1792, próg
                                   luminancji — na ciemnym tle urządzenia są jasne) przeskalowany ×0.505 i wyśrodkowany, brzegi
                                   dociągnięte edge-clampem. **Nie „docinaj" kadru do krawędzi urządzeń** — GSAP powiększa obraz
                                   o 12–16%, a ogranicza tu SZEROKOŚĆ (84% z dopuszczalnych 86%), nie wysokość.
                                   Wariant z jasnym chłodnym tłem (periwinkle) też został wygenerowany i odrzucony — do odtworzenia
                                   tym samym promptem, gdyby pas kiedyś zrobił się jasny.
                                   **To jedyny obrazek w secie oferta-* będący zdjęciem** — reszta to płaskie ilustracje (do wymiany).
  oferta-strony.webp            ← UŻYWANA w sekcji „Strony internetowe" — ilustracja (okno przeglądarki + stos podstron + żółte CTA),
                                   spójna z resztą setu oferta-*; przywrócona po epizodzie z fotografią (patrz oferta-strony-foto.webp)
  oferta-design.webp            ← ilustracja usługi Indywidualny design — artboard z kształtami + ramka zaznaczenia z uchwytami + kursor + paleta swatchy (motyw narzędzia projektowego, bez pędzla). WYJĄTEK w secie: nie z Higgsfield — ręcznie rysowany SVG wyrenderowany do webp (źródło: `img/src/oferta-design.svg`, render: Chrome → PNG 1800×1344 → `cwebp -resize 1200 896 -q 92 -m 6`). Edycja = popraw SVG i przerenderuj
  oferta-responsywnosc.webp     ← ilustracja usługi Responsywność — monitor + tablet + telefon, żółte strzałki skalowania
  oferta-opieka.webp            ← ilustracja usługi Opieka nad stroną — okno w pętli odświeżania + kłódka + tarcza
  oferta-analityka.webp         ← ilustracja usługi Analityka Google — dashboard z wykresem liniowym + donut
  proces-hero.webp              ← kadr hero `proces.html` (1254×1254, Higgsfield nano_banana_pro) — FOTOGRAFIA: MacBook na jasnym
                                   biurku z układem strony w palecie marki (navy/niebieski + żółte CTA), obok otwarty szkicownik
                                   z odręcznymi wireframe'ami, ołówek i żółte karteczki. Motyw „od szkicu do gotowej strony".
                                   Kadrowany przez `object-fit: cover` + `object-position: 50% 42%` do 7/5 (mobile 4/3)
  kontakt-hero.webp             ← kadr hero `kontakt.html` (1254×1254, Higgsfield nano_banana_pro) — FOTOGRAFIA: domowe biurko
                                   przy oknie, laptop z formularzem kontaktowym (żółty przycisk SEND), telefon z ekranem połączenia,
                                   kawa i notes, ciepłe światło. Motyw „napisz lub zadzwoń"
  (oba hero-* : ten sam pipeline co reszta — prompt text-to-image 1:1, wybór z 2 wariantów, potem
   `cwebp -resize 1254 1254 -q 90 -m 6`. To FOTOGRAFIE, nie płaskie ilustracje — celowo, bo kadr hero
   `.svc-hero-device` ma radius + cień + `object-fit: cover` i jest zaprojektowany pod zdjęcie, tak jak
   `szkolaplywaniamockup-laptop.webp` na oferta.html. Drobny tekst na ekranach jest przegenerowany
   przez model — w docelowym rozmiarze nieczytelny, ale przy powiększeniu widać)
  (wszystkie oferta-*.webp: przegenerowane 14.08.2026 modelem Higgsfield nano_banana_pro w stylu
   zgodnym z proces-*.webp — płaski front-on wektor, grube zaokrąglone kształty, długie miękkie cienie,
   paleta navy #1B2440 / niebieski #2563EB / jasnoniebieski #7EAAF5 / żółty #FBD667 na off-white #F4F7FC,
   bez tekstu; 1200×896, 4:3. Poprzedni set był izometryczny/line-art na kremowym tle)
  pawel_nobackgroundv1.png      ← zdjęcie hero (o-mnie.html) — wycinek bez tła (RGBA), postać z iPadem; 2274×2766 = wersja 2× (retina) pliku poniżej.
                                   UWAGA przy podmianie: postać ma NIEBIESKĄ bluzę o luminancji bliskiej `--color-blue` — w niebieskim hero o-mnie
                                   leży wprost na tle i od tła oddziela ją wyłącznie podwójny `drop-shadow` (`.svc-hero-device--portrait img`).
                                   Inny wycinek = sprawdź kontrast sylwetki na `#2563EB`, zanim podmienisz. Maska w `.me-hero-photo--cutout::before`
                                   już nie istnieje (stary jasny hero usunięty)
  pawel_nobackground.png        ← oryginał wycinka 1137×1383 (już nieużywany)
  omnie-bgremoved2.png          ← wcześniejszy wariant wycinka (już nieużywany)
  omnie-bgremoved.png           ← wcześniejszy wariant wycinka (już nieużywany)
  omnie-koduje.webp             ← koduje stronę w domowym biurze nocą (o-mnie.html, sekcja misji, 16:9, Higgsfield nano_banana_pro) — klimat: ciepłe światło, kod + preview na monitorze, roślina
  omnie-praca.webp              ← wcześniejsze „przy pracy" przy laptopie (już nieużywane w sekcji misji)
  omnie_kodowanie.png           ← przy biurku od tyłu, kod na dwóch ekranach, niebieski neon fotela (o-mnie.html, sekcja „jak pracuję", 2244×2804) — UWAGA: 6,4 MB, do konwersji na webp
  omnie-portret.webp            ← wcześniejszy close-up portret (już nieużywany)
  proces-01-konsultacja.webp    ← ilustracja kroku 01 (proces.html, Higgsfield nano_banana_pro) — konsultacja: laptop z rozmową + dymek czatu
  proces-02-analiza.webp        ← ilustracja kroku 02 — analiza/strategia: lupa nad wykresem + checklista
  proces-03-projekt.webp        ← ilustracja kroku 03 — projekt+wycena: makieta strony + dokument z ołówkiem
  proces-04-realizacja.webp     ← ilustracja kroku 04 — projektowanie+kodowanie: monitor z layoutem + nawiasy kodu
  proces-05-wdrozenie.webp      ← ilustracja kroku 05 — testy+wdrożenie: rakieta z okna przeglądarki + laptop/telefon
  proces-06-wsparcie.webp       ← ilustracja kroku 06 — wsparcie: tarcza z checkmarkiem, klucz, strzałki odświeżania
  (wszystkie proces-*.webp: minimal flat, paleta marki navy/niebieski/żółty na off-white #F8FAFC — spójny set, bez tekstu)
examples/       ← screenshoty referencyjne
PRD_lokalnewww.md
```

---

## Sekcje strony (kolejność w index.html)

| ID | Klasa/styl sekcji | Status |
|---|---|---|
| `#nav` | `.nav-bar` — jeden floating pill (grid `1fr auto 1fr`), **dwa stany**: hero (80px, szkło `alpha 0.32`, bez cienia) ↔ compact po 60px scrolla (62px, `alpha 0.86`, cień, pill węższy o 2×18px). Wymiary w custom properties animowanych tweenem GSAP. Nad ciemnymi sekcjami ciemne szkło + jasne linki (`.nav--light`); aktywna strona = niebieski pill (`.is-active` + `aria-current`, ustawiane po URL-u). Szczegóły w sekcji „Nawigacja" niżej | ✅ gotowe |
| `#hero` | tło `img/herov8.png` + overlay `rgba(255,255,255,0.2)`, treść do lewej | ✅ gotowe |
| `#co-zyskasz` | `.benefit-section` — tło `#F8FAFC`, 3 karty w gridzie | ✅ gotowe |
| `#uslugi` | `.bento-section` — tło niebieski gradient, 6 kafli | ✅ gotowe |
| `#realizacje` | `.portfolio-section` — tło `#080C14`, slider transform-driven (GSAP `x`/translate3d) 5 kart, coverflow (karta najbliżej środka = `scale 1`/`opacity 1`, pozostałe `0.96`/`0.62` — podświetlenie przechodzi przez kolejne kafelki w trakcie przesuwania), pierwsza karta wyrównana do lewej krawędzi nagłówka, na ekranie 3 karty + fragment 4., prev/next + drag/touch (Pointer Events) + wolny autoplay z pauzą na hover | ✅ gotowe |
| `#proces` | `.process-section` — tło `#F8FAFC` (jasne), 4 kroki z numerami | ✅ gotowe |
| `#wyrozniam-sie` | `.usp-section` — tło `#fff`, 2×2 grid kart | ✅ gotowe |
| `#o-mnie` | mini-sekcja na index (`#o-mnie-v2 .about2-section`, tło `#1A2540`) + osobna strona `o-mnie.html`; wszystkie linki „O mnie" w nav/footer prowadzą do `o-mnie.html` | ✅ gotowe |
| `#kontakt` | osobna strona `kontakt.html` (nie sekcja na index) — wszystkie linki „Kontakt" i CTA konsultacji prowadzą do `kontakt.html` | ✅ gotowe |
| `#footer` | `.footer-section` — tło `#F8FAFC`, logo+nav+social | ✅ gotowe |


---

## Nawigacja — dwustanowy floating pill

Nav jest **jednym pillem** (`.nav-bar`) wewnątrz `.nav-wrapper`, wspólnym dla wszystkich stron. Zamiast osobnego „wyspowego" pilla tylko na linkach (dawne `.nav-island`, klasa **usunięta**) cały pasek — logo, linki i CTA — siedzi w jednym zaokrąglonym panelu ze szkłem.

**Layout:** `.nav-bar` to grid `1fr auto 1fr` z jawnym przypisaniem kolumn (`.nav-logo` → 1, `.nav-links-pill` → 2, `.nav-actions` → 3). Dzięki temu linki są wyśrodkowane **w całym pasku**, a nie w przestrzeni między logo a CTA. Jawne `grid-column` jest konieczne: gdy na mobile `.nav-links-pill` dostaje `display: none`, ukryty item wypada z gridu i bez przypisania kolumn `.nav-actions` wskoczyłoby do środkowej kolumny (nachodząc na logo).

**Dwa stany (rdzeń efektu — zmienia się WYSOKOŚĆ, nie tylko tło):**

| | hero (scrollY < 30) | compact (scrollY > 60) |
|---|---|---|
| wysokość pilla | 80px (mobile 68) | 62px (mobile 56) |
| padding poziomy | 22px (mobile 14) | 16px (mobile 12) |
| logo | 42px (mobile 34) | 34px (mobile 30) |
| CTA | 52px (mobile 44) | 44px (mobile 40) |
| tło | `alpha 0.32` | `alpha 0.86` (mobile 0.88) |
| blur | 12px | 18px |
| cień | ledwie widoczny (0.035) | `0 10px 40px rgba(15,23,42,0.12)` |
| szerokość | pełny box kontenera | węższy o 2×18px (mobile 2×8) |

**Jak to działa (`style.css` sekcja „Navigation — dwustanowy floating pill" + blok nawigacji w JS każdej strony):**

- Wszystkie animowane wymiary to **custom properties na `#nav`** (`--nav-h`, `--nav-px`, `--nav-logo`, `--nav-cta-h`, `--nav-link-px`, `--nav-blur`, `--nav-alpha`, `--nav-shadow`, `--nav-squeeze`), używane w CSS przez `calc(var(--nav-h) * 1px)` itd.
- JS trzyma **jeden tween GSAP** na obiekcie `{ p: 0 }` (`duration 0.55`, `ease power3.out`), który w `onUpdate` lerpuje wszystkie wartości między presetem `hero` a `compact` i wpisuje je inline na `#nav`. Zjazd w dół = `play()`, powrót na górę = `reverse()` **tego samego** tweena — stąd płynne przejście w obie strony, bez skoku klasą.
- Presety są osobne dla desktopu i mobile (`NAV_STATES.desktop` / `.mobile`), wybierane przez `matchMedia('(max-width: 767px)')` i przemalowywane na `change`.
- Próg z **histerezą**: kurczy się powyżej 60px, rozwija dopiero poniżej 30px (bez migotania na granicy). Detekcja siedzi w istniejącym `updateNav` (ten sam listener co `nav--light`) — celowo bez ScrollTrigger, żeby nav nie zależał od pluginu i działał identycznie na wszystkich 5 stronach.
- Gdy tweenem steruje GSAP, `#nav` dostaje klasę **`.nav--gsap`**, która **wyłącza CSS-owe `transition`** na `.nav-bar`, logo i CTA (inaczej transition goniłoby tween).
- **Fallback bez GSAP / przy `prefers-reduced-motion`:** JS nie wpisuje inline zmiennych; stan ustawia sama klasa `.scrolled` (te same wartości w CSS), a przejście dowozi `transition` na `.nav-bar` (`cubic-bezier(0.215, 0.61, 0.355, 1)` ≈ `power3.out`). Przy `prefers-reduced-motion` transition jest dodatkowo zerowany.

**Paleta (świadome odstępstwo od referencji „luxury real-estate"):** referencja ma ciemne szkło + kremowe CTA. Tu zostaje paleta marki, a szkło **adaptuje się do tła**: domyślnie jasne (`--nav-glass: 255,255,255`) z ciemnymi linkami, a nad ciemnymi sekcjami (`.nav--light`, sterowane `darkSectionIds`) ciemne (`10,16,30`) z jasnymi linkami i białym hamburgerem. Ciemne szkło `0.35` na jasnym hero index.html dawałoby biały tekst na prawie białym tle. Obrys w wariancie jasnym jest **grafitowy** (`rgba(15,23,42,0.07)`), nie biały — biały obrys znika na jasnym hero.

**Uwagi przy zmianach:**

- Logo zjechało z 64px do 42/34px — 64px nie mieści się w 80px pillu. `.logo-text` ma stabilny rozmiar (1.2rem desktop / 0.95rem mobile), żeby przy kolapsie nie skakał layout. Rozmiar zszedł z 1.6/1.15rem razem z przejściem wordmarku na wersaliki (uppercase jest szerszy o ~35% przy tej samej wielkości — patrz „Ważne decyzje projektowe").
- `.btn-nav-brand` ma teraz **stałą wysokość** z `--nav-cta-h` (padding pionowy usunięty), a kółko ikony `--nav-cta-h − 14`.
- Linki: `0.9375rem`/`500` (referencja: 14–16px, waga 400–500) zamiast dawnych `1rem`/`600`.
- CTA jest widoczne do 480px (poniżej `display: none`) — wcześniej znikało już poniżej 768px.
- Zmiana wartości stanów wymaga edycji **dwóch miejsc**: presetów `NAV_STATES` w JS **każdej z 5 stron** i fallbackowych wartości w CSS (`#nav`, `#nav.scrolled` + ich warianty mobilne).
---

## Szablon podstron — `oferta.html` jest wzorcem

Projekt **nie ma buildu ani include'ów** — każda podstrona to samodzielny plik HTML. `oferta.html` jest **kanonicznym szablonem powłoki podstrony**: przy tworzeniu nowej podstrony (np. `blog.html`, `kontakt.html`, `polityka-prywatnosci.html`) **kopiuj z `oferta.html`** trzy współdzielone bloki i zmieniaj tylko treść między nimi.

**Bloki do skopiowania z `oferta.html` (1:1):**

1. **`<head>`** — struktura meta/SEO/fontów/Tailwind/`style.css`. Podmień tylko `<title>`, `<meta name="description">`, `og:*` i `<link rel="canonical">`. Zostaw `gsap-enhance` inline-script, `link` do fontów, Tailwind config i `style.css`.
2. **Header** — `<nav id="nav">` (`.nav-wrapper` > `.nav-bar`: logo + `.nav-links-pill` + `.nav-actions` CTA/hamburger) **oraz** `#mobile-menu` tuż pod nim. Kopiuj oba razem.
3. **Footer** — `<footer id="footer" class="footer-section">` z całą zawartością (`.footer-top` brand+nav+social, `.footer-bottom`).
4. **Skrypty na dole** — bloki `<script>` GSAP/ScrollTrigger/Lenis CDN + IIFE animacji + główny `DOMContentLoaded` (nawigacja, hamburger, IntersectionObserver, smooth scroll). Sekcje specyficzne dla treści (np. akordeon `.svc-acc-head`) usuń, jeśli podstrona ich nie ma.

**Reguły przy kopiowaniu (łatwo o pomyłkę):**

- **Ścieżki root-relative:** `img/…`, `style.css`, logo `/`. Podstrony są w katalogu głównym, więc bez `../`.
- **Linki do sekcji strony głównej z prefiksem `/#` — jednolite na wszystkich stronach:** `href="/#o-mnie"`, `/#blog`, `/#kontakt` (identycznie w nav i footer na `index.html` **oraz** podstronach). Link „Oferta" → `oferta.html`. Dzięki temu blok nav + `#mobile-menu` + footer-nav jest **identyczny** na każdej stronie (kopiuj 1:1). Na `index.html` obsługę `/#…` (smooth scroll bez przeładowania + scrollspy) zapewnia JS: `resolveSection` i handler smooth-scroll normalizują `/#sekcja` → `#sekcja`; handler łapie selektor `a[href^="#"], a[href^="/#"]`.
- **`darkSectionIds` w nav-JS jest per-strona:** lista id ciemnych sekcji, nad którymi nav ma białe linki (`nav--light`). Ustaw ją pod realne ciemne pasy danej podstrony (na `index.html`: `['o-mnie-v2','realizacje']`; na `oferta.html`: `['svc-hero','svc-strony','svc-design','svc-opieka']` (hero jest tam niebieski)). Nowa podstrona bez ciemnych sekcji → pusta tablica `[]`.
- **Aktywna strona w nav — blok wspólny, kopiuj 1:1:** w nav-JS (tuż po `const nav`/`darkSectionIds`) jest snippet, który porównuje `location.pathname` z `href` linków i nadaje `.is-active` + `aria-current="page"` pozycji odpowiadającej bieżącej stronie (dopasowanie po nazwie pliku bez `.html`, więc działa i dla `/proces`, i dla `/proces.html`). Obejmuje pill (`.nav-links-pill a`) i `#mobile-menu` (z pominięciem CTA `.btn-primary`); linki z `#` pomija. Snippet jest **identyczny na wszystkich stronach, łącznie z `index.html`**. Styl: pill = `.is-active` (niebieski), mobile = `#mobile-menu a[aria-current="page"]` (żółty + podkreślenie).
- **Scrollspy nie istnieje** — nawigacja podświetla wyłącznie aktywną **stronę** (patrz punkt wyżej), nie sekcję. `updateNav` odpowiada już tylko za `.scrolled` i `nav--light`.
- **Header + footer trzymaj zsynchronizowane ręcznie** — zmiana w nav/footer musi trafić do `index.html`, `oferta.html` i wszystkich podstron (brak include'ów = brak automatycznej propagacji).

---

## Strona Oferta (`oferta.html`)

Plik `oferta.html` w katalogu głównym (URL `/oferta`). Jedna strona opisująca całą ofertę; 6 usług = te z „zajawki" na index.html (bento), każda w osobnym pełnoszerokościowym pasie z akordeonem. Ścieżki zasobów root-relative (`img/`, `style.css`).

**Motyw: jasny hero z siatką + naprzemienne pasy** — `<body class="offer-page">`. Hero jasny (gradient `#F0F4FF→#fff`) z **siatką („kratka") w tle** (`.svc-hero::before` = grid 54px w kolorze `rgba(37,99,235,0.09)`, maska radialna, żeby wygasała po bokach) + niebieska poświata u dołu (`::after`). Poniżej **6 pasów usług naprzemiennie ciemny/jasny** (`.svc-band--dark` / `.svc-band--light`): Strony internetowe (ciemny), SEO (jasny), Indywidualny design (ciemny), Responsywność (jasny), Opieka nad stroną (ciemny), Analityka Google (jasny) — **kolejność i kolory wg faktycznego HTML**; zgadza się z `darkSectionIds` w nav-JS. Pas ciemny = `#0A1120` + radialne blue glow; pas jasny = `#F8FAFC`. Style w `style.css` w sekcji „PODSTRONA OFERTY — jasny hero z siatką + naprzemienne pasy" (po „REDESIGN PODSTRONY OFERTY"). Stara sekcja „MOTYW CIEMNY PODSTRONY OFERTY" (`body.offer-dark`) jest teraz **martwym kodem**.

**Sekcje szablonu (w kolejności):**

1. `#svc-hero .svc-hero.svc-hero--b.svc-hero--case` — **hero „case study" na pełnym niebieskim tle** (wariant wspólny dla **wszystkich podstron**: `oferta.html`, `proces.html`, `kontakt.html` i `o-mnie.html` — patrz sekcje tych stron; jasny wariant `--b` nie jest już nigdzie używany). **Proporcje wzorowane na `marceldigital.com/case-studies`** (zmierzone w DevTools: H1 102px, kolumny 579/709 px, mockup wychodzący poza kontener, karta wcięta pod mockupem). Layout 2 kolumny (grid `55fr / 45fr`, gap `clamp(2.5rem, 4.5vw, 4rem)`, `align-items: center`; na tablecie `48fr / 52fr`, żeby karta nie robiła się wąska): lewo badge + H1 + lead, prawo `.svc-hero-media` = pierścień + mockup + karta realizacji. Padding sekcji `17.5rem 0 12rem` (tablet `14rem / 8.75rem`, mobile `11rem / 8.5rem` + `gap: 6rem` w kolumnie). **Te wartości to nie "ładne liczby", tylko wynik celu "hero +20% wysokości"** — wysokość sekcji wyznacza kolumna medialna (na 1440: media 598px vs tekst ~247px), więc wzrost trzeba było dołożyć w paddingu. Zmierzone: 894→1070 (+19.7%), 656→788 (+20.1%), 888→1068 (+20.3%). Zmieniając padding, przelicz od nowa — sam `rem` nic nie powie.
   - **Tło:** płaskie `var(--color-blue)`; `::before` (kratka) i `::after` (poświata) wyłączone przez `content: none`.
   - **H1:** dwie linie zamiast `<br>` + akcentu kolorem — `.svc-hero-h1-l1` (`font-weight: 300`) nad `.svc-hero-h1-l2` (`font-weight: 800`), obie białe. **Akcent robi grubość, nie kolor** (na niebieskim tle niebieski akcent byłby niewidoczny), więc `.svc-hero-h1-accent` nie jest tu używany. Rozmiar `clamp(2.35rem, 5vw, 4.5rem)` (72px na 1440), `line-height: 1.08`, `letter-spacing: -0.03em`.
   - **⚠️ Metryka, która decyduje o „powietrzu" w tym hero — `prześwit tekst → pierścień`.** Nie paddingi. Referencja ma tu **201px**, bo „Case Studies" to dwa krótkie słowa i najdłuższa linia zajmuje **65%** kolumny. Nasza fraza ma 18 znaków na linię, więc przy 78px zajmowała **98%** kolumny i zostawiała **18px** do pierścienia — stąd wrażenie ścisku. Wzór: `prześwit = (szer. kolumny − ink najdłuższej linii) + gap − wcięcie pierścienia`. Aktualnie: `(720 − 643) + 67 − 6 ≈ 138px`. **Zanim podbijesz rozmiar H1 albo zmienisz treść, przelicz to** — Inter 300 potrzebuje ok. `8.9 × font-size` px na „Kompleksowa oferta" przy trackingu −0.03em. Sprawdzenie na żywo: zmierz `Range.getBoundingClientRect()` na `.svc-hero-h1-l1` i porównaj z `.svc-hero-ring`. Przy tej treści sufit to ~72px; **większy nagłówek wymaga krótszej frazy, nie innych marginesów.**
   - **Pierścień** `.svc-hero-ring` — inline SVG (`circle` + `stroke-dasharray 452 114` + gradient żółty→pomarańczowy), nie CSS-owa maska: `radial-gradient` w `mask` liczy stopy względem `farthest-corner`, więc ring z `conic-gradient` + maski renderował się jako pełne koło. Wymiary/offsety w `%` szerokości kolumny (`width: 49%`, `left: -1%`), żeby relacja pierścień↔mockup skalowała się płynnie; `top: -76px` daje prześwit pod pływającym navem. `left` bliskie zeru jest celowe — każdy ujemny procent zjada prześwit do nagłówka (patrz wzór wyżej).
   - **Mockup** `.svc-hero-device` — **kadr ze zdjęciem** (`img/szkolaplywaniamockup-laptop.webp`, 1254×1254, MacBook Pro na słupku startowym, basen w tle). Kontener ma `border-radius: 24px` + `box-shadow`, obraz `aspect-ratio: 7/5` (`4/3` na mobile), `object-fit: cover`, `object-position: 50% 42%` (kadr wycentrowany na laptopie — przy 50% 50% ucinało górę ekranu). `width: 100%` **plus** `margin-left: 17%` → element jest szerszy niż zostawione mu miejsce i **wychodzi poza kontener w prawo** (przycina go `overflow: hidden` na `.svc-hero`) — przy kadrze `cover` to wygląda na celowe wyjście poza ramę. `width: 100%` jest obowiązkowe: bez niego `width: auto` bloku wchłania margines i mockup tylko się zwęża. **Uwaga przy podmianie na wycinek z alfą** (np. `oferta-hero-laptop.webp`): wtedy trzeba zdjąć `radius`/`box-shadow`/`object-fit`, dać cień przez `filter: drop-shadow` na `img` i zejść z `margin-left` do ~2%, bo `overflow: hidden` utnie sylwetce prawą krawędź i będzie to wyglądać na błąd.
   - **Karta** `.svc-hero-case` — **zwykły `<div>`, nie link** (był `<a href="#realizacje">` z żółtą strzałką w rogu; usunięte na życzenie). Nie ma więc `:hover`, `transition` ani `text-decoration` — nic nie sugeruje klikalności. `width: 72%`, `margin: -4.35rem 0 0 24%` → jest **węższa od mockupu i wcięta względem jego lewej krawędzi** (24% = 17% offsetu mockupu + 7% wcięcia) i wjeżdża pod kadr. Padding `1.75rem 1.75rem 1.875rem` — **symetryczny po zdjęciu strzałki**; wcześniej prawy padding wynosił `5.75rem` wyłącznie po to, żeby zrobić jej miejsce (analogicznie w media queries: `4.5rem`/`4rem` → `1.35rem`). Tło pełne `var(--color-blue)` (przezroczyste jak na referencji przepuszczałoby zdjęcie), obrys `rgba(255,255,255,0.42)`, radius 20px, tytuł `1.5rem`, opis `1rem`.
   - **Realizacja w hero:** `img/szkolaplywaniamockup-laptop.webp` (Szkoła Pływania Aqua) — laptop z ekranem strony, basen w tle, bez telefonu. Slider `#realizacje` na index.html używa oryginału `szkolaplywaniamockup.webp` (laptop + telefon). Uwaga: drobny tekst na ekranie (menu, przyciski) jest przegenerowany przez model i częściowo bełkotliwy — w docelowym rozmiarze nieczytelny, ale przy powiększeniu widać.
   - **Nav:** hero jest ciemny → `'svc-hero'` **musi** być w `darkSectionIds` (inaczej ciemne linki na niebieskim tle).
2. **6× `.svc-band`** (id `svc-seo`, `svc-strony`, `svc-design`, `svc-responsywnosc`, `svc-opieka`, `svc-analityka`) — każdy pas ma jeden `.svc-block` (grid 2 kol; `.svc-block--rev` = obraz z lewej dla pasów jasnych). Tekst: `.svc-block-num` + `.svc-block-h3` + `.svc-block-desc` + **akordeon** `.svc-accordion` z 3 pozycjami „co obejmuje" (`.svc-acc-item`, pierwsza `.is-open`; panel przez `grid-template-rows 0fr→1fr`, chevron-down obraca się o 180° + niebieskie kółko). **Bez tagów** (`.svc-tag` usunięte). Obraz `.svc-block-visual` z ilustracją Higgsfield (`img/oferta-*.webp`, minimal flat, paleta marki) — **ten sam język wizualny co `proces-*.webp`** (płaski front-on, off-white tło, navy/niebieski/żółty); przy podmianie któregokolwiek obrazka trzymaj oba sety spójne.
3. `.svc-cta` — baner: ciemna navy karta `.svc-cta-card` (radius 28px, blue glow) z H2 + żółty `btn-primary`.
4. `#realizacje .svc-work` — tło `#F8FAFC`, siatka 3 kart `.svc-work-card` (thumb + tagi + „Zobacz stronę ↗" + nazwa).
5. `.footer-section` — reuse z index.html.

> Sekcja opinii (`.testi-section`) została **usunięta** z oferta.html (CSS `.testi-*` pozostał jako martwy kod).

**Ścieżki zasobów:** root-relative — `img/`, `style.css` (plik jest w katalogu głównym)

**Nawigacja:** linki w nav i footer zawierają `/#section` (prefix ukośnikiem do roota)

**Nav JS:** `updateNav` wymusza `nav--light` gdy scroll nad pasem ciemnym — `darkSectionIds = ['svc-hero','svc-strony','svc-design','svc-opieka']` — **'svc-hero' doszedł razem z niebieskim hero**. Akordeon: klik na `.svc-acc-head` toggluje `.is-open` (zamyka pozostałe w tym samym `.svc-accordion`).
>
> **⚠️ Bezpieczny margines w grafikach pasów (łatwo się na tym przejechać).** `gsap.set(img, { scale: 1.12 })` + hover `1.16` + parallax `yPercent 5→-5` sprawiają, że obraz jest **zawsze** przycięty: ~6,9% z każdego boku i do ~11% z góry albo z dołu (skrajne stany parallaxu). Wniosek: **każda grafika w `.svc-block-visual` musi mieścić istotną treść w centralnych ~86% szerokości i ~77% wysokości.** Płaskim ilustracjom `oferta-*.webp` to nie przeszkadza (mają duże puste marginesy), ale zdjęcie dociśnięte do krawędzi obiektów zostanie obcięte — tak stało się przy pierwszej wersji `oferta-strony-foto.webp`. Przy podmianie grafiki sprawdź to na realnych transformach, a nie z wyzerowanym `transform` — inaczej problem nie wyjdzie na zrzucie. Szybki test: wymuś `transform: scale(1.16) translateY(±5%)` na `.svc-block-img` i zobacz, czy nic nie ucieka poza kadr.

**Animacje GSAP pasów usług (`oferta.html`).** Analogicznie do index.html: GSAP + ScrollTrigger + Lenis z CDN na dole strony, klasa `.gsap-enhance` dodawana synchronicznie w `<head>`, zdejmowana przez JS gdy brak GSAP / `prefers-reduced-motion` (treść zostaje statycznie widoczna). Osobny `<script>` IIFE ze **spięciem Lenis z tickerem GSAP** oraz `gsap.matchMedia()`:
> - **Desktop/tablet (`min-width: 768px`)** — *alternating reveal* per pas (`ScrollTrigger` na `.svc-block`, `start: 'top 78%'`, `once`), `ease: power3.out`. Tekst wjeżdża od swojej strony: pasy ciemne (tekst z lewej) `x:-30, y:30`, pasy jasne `.svc-block--rev` (tekst z prawej) `x:+30, y:30`. Kolejność timeline: **numer/label** (`y:12`, `letter-spacing 0.16→0.02em`) → **H3** (`y:40`) → **opis** (`y:24`) → **`.svc-acc-item` stagger 0.1**; `.svc-block-visual` w tym samym timeline `x:±60, y:30, scale:0.94→1, rotation:±1.5°→0`, `duration:1`. Po revealu `clearProps: 'transform'` na tekstach (żeby działał CSS accordionu). **Parallax** grafiki: osobny scrub-`ScrollTrigger` na `.svc-block-img` (`yPercent 5→-5`, `start: 'top bottom'`, `end: 'bottom top'`); baza `scale:1.12` żeby ruch nie odsłaniał krawędzi w kadrze `overflow:hidden`. **Hover** grafiki (`scale 1.12↔1.16`) sterowany GSAP-em (`gsap.quickTo`) — komponuje się z parallaxem, bez konfliktu transformów.
> - **Mobile (`max-width: 767px`)** — lżej: tylko `opacity + y` (numer/H3/opis `y:24`, akordeon `y:20` stagger 0.08, grafika `y:30`), **bez `x`, `scale`, `rotation` i bez parallaxu**.
>
> **Wydajność:** animowane wyłącznie `transform` + `opacity` (`force3D`), `will-change: transform` tylko na `.svc-block-img`. Stany początkowe (`opacity:0`) chowane w CSS pod `html.gsap-enhance body.offer-page .svc-band …` (sekcja „Stany początkowe animacji GSAP (podstrona oferty)" w `style.css`); wrapper `.svc-block.fade-up` zneutralizowany (`opacity:1; transform:none`), a CSS-owy `transition`/`:hover` na `.svc-block-img` zdjęty (transform grafiki należy do GSAP). `@media (prefers-reduced-motion: reduce)` un-hide jako zabezpieczenie zanim JS zdejmie klasę. Pozostałe `.fade-up` na podstronie (hero, CTA, realizacje) dalej obsługuje `IntersectionObserver`.

**Klasy CSS podstron (prefiksy `svc-`, `testi-`):** `.svc-hero`, `.svc-hero--b`, `.svc-hero--case`, `.svc-hero-inner`, `.svc-hero-label`, `.svc-hero-h1`, `.svc-hero-h1-l1`, `.svc-hero-h1-l2`, `.svc-hero-h1-accent`, `.svc-hero-media`, `.svc-hero-ring`, `.svc-hero-device`, `.svc-hero-case`, `.svc-hero-case-title`, `.svc-hero-case-desc`, `.svc-hero-sub`, `.svc-hero-actions`, `.svc-section`, `.svc-services-grid`, `.svc-service-card`, `.svc-service-icon`, `.svc-service-title`, `.svc-service-desc`, `.svc-service-btn`, `.svc-block`, `.svc-block--rev`, `.svc-block-text`, `.svc-block-num`, `.svc-block-h3`, `.svc-block-desc`, `.svc-block-visual`, `.svc-block-img`, `.svc-accordion`, `.svc-acc-item`, `.svc-acc-head`, `.svc-acc-title`, `.svc-acc-chevron`, `.svc-acc-panel`, `.svc-acc-panel-inner`, `.svc-tags`, `.svc-tag`, `.svc-cta`, `.svc-cta-card`, `.svc-cta-glow`, `.svc-cta-h2`, `.svc-cta-btn`, `.svc-work`, `.svc-work-header`, `.svc-work-label/h2/sub`, `.svc-work-grid`, `.svc-work-card`, `.svc-work-thumb`, `.svc-work-meta`, `.svc-work-tags`, `.svc-work-tag`, `.svc-work-visit`, `.svc-work-name`, `.testi-section`, `.testi-header`, `.testi-label`, `.testi-h2`, `.testi-nav`, `.testi-nav-btn`, `.testi-nav-btn--active`, `.testi-track`, `.testi-card`, `.testi-stars`, `.testi-quote`, `.testi-foot`, `.testi-author`, `.testi-avatar`, `.testi-author-info`, `.testi-name`, `.testi-role`, `.testi-date`

> **Uwaga:** stare klasy podstrony (`.offer-hero*`, `.offer-benefits*`, `.offer-cta*`, `.feat-*`) pozostały w `style.css` jako martwy kod po redesignie — do usunięcia przy sprzątaniu (nie są używane przez index.html).

---

## Strona Kontakt (`kontakt.html`)

Plik `kontakt.html` w katalogu głównym (URL `/kontakt`), zbudowany na szablonie podstron (`oferta.html`, patrz sekcja „Szablon podstron"): współdzielony `<head>`, header (`nav` + `#mobile-menu`), footer i skrypty. **Wszystkie linki „Kontakt" oraz CTA konsultacji** (`btn-nav-brand`, `btn-footer-cta`, mobile CTA, `svc-cta-btn`, dawne `#kontakt` w sekcji O mnie na index) w `index.html` i `oferta.html` prowadzą teraz do `kontakt.html` (wcześniej martwe `/#kontakt`).

**Motyw: jasny hero + ciemna reszta** — `<body class="contact-page">`, tło body `#080C14`. **Hero to ten sam komponent co na `oferta.html`** — `.svc-hero.svc-hero--b` bez żadnych nadpisań (gradient `#F0F4FF→#fff` + niebieska kratka + poświata, badge + H1 + lead), więc header renderuje się identycznie jak na oferta (jasna pigułka nav, ciemne linki). Poniżej sekcje Kontakt i FAQ ciemne. Layout inspirowany referencją (wielki hero + 2 kolumny dane/formularz + FAQ w akordeonie), przełożony na markę (Inter, niebieski/żółty). Ciemne są dopiero sekcje pod hero (łącznie z granatowym footerem), dlatego nav-JS ma `darkSectionIds = ['svc-hero','kontakt','faq','footer']` — **`svc-hero` doszło razem z niebieskim hero** (wcześniej hero było jasne i go tu nie było), więc nav ma jasne linki na całej stronie.

**Sekcje (w kolejności):**

1. `#svc-hero .svc-hero.svc-hero--b.svc-hero--case` — **pełny reuse hero „case" z `oferta.html`** (żadnych nadpisań w sekcji „PODSTRONA KONTAKT"): dwuliniowy H1 (`.svc-hero-h1-l1` 300 „Porozmawiajmy" nad `.svc-hero-h1-l2` 800 „o Twoim projekcie") + `.svc-hero-b-lead`, a po prawej `.svc-hero-media` = pierścień + kadr `img/kontakt-hero.webp` + karta „Odpowiadam tego samego dnia". **Bez badge’a i bez akcentu kolorem** — jak na oferta.html akcent robi grubość fontu. Hero jest ciemne → `'svc-hero'` **musi** być w `darkSectionIds`. Zmierzone na 1440: wysokość 1003px (identycznie jak oferta), prześwit H1 → pierścień 125px (linia 2). Dawny `.contact-hero*` (wielki uppercase H1) został **usunięty** ze `style.css`.
2. `#kontakt .contact-section` — grid 2 kol (`.contact-grid`). Lewo: eyebrow `.contact-eyebrow` („● KONTAKT", kropka przez `::before`) + `.contact-h2` + `.contact-list` z wierszami `.contact-row` (ikona `.contact-row-icon` + tekst, rozdzielone `border-top`; `a.contact-row` klikalne — email `mailto:`, telefon `tel:`, adres statyczny `.contact-row--static`). Prawo: karta `.contact-form-card` (`#0F1626`) z `.contact-form-title` + polami `.contact-field`/`.contact-label`/`.contact-input`/`.contact-textarea` + żółty `.btn-primary.contact-submit` + `.contact-form-note` (status). **Dane fejkowe:** tel `+48 512 340 118`, adres `ul. Kwiatowa 8/3, 30-002 Kraków`; email `kontakt@lokalnewww.pl`.
3. `<hr class="contact-rule contact-rule--wrap">` — linia podziału w szerokości kontenera. **Szerokość liczona `calc()`-iem** (`width: calc(100% - 3rem)`, `max-width: calc(1440px - 3rem)`), żeby zgadzała się z content-boxem `.max-w-container.px-6`. Wcześniej było `width: 100%` + `padding: 0 1.5rem` + `box-sizing: content-box`, czyli **viewport + 48px → cała strona kontaktu przewijała się poziomo o 48px na każdej szerokości** (naprawione 01.09.2026). Nie wracaj do paddingu na tym elemencie.
4. `#faq .contact-section.contact-faq` — grid 2 kol: lewo eyebrow „● FAQ" + `.contact-h2`; prawo **akordeon reużyty z oferty** (`.svc-accordion` / `.svc-acc-*`, pierwszy `.is-open`) — ciemne warianty przez `body.contact-page .svc-acc-*`. 6 pytań o tworzenie stron WWW (koszt, czas, treść/zdjęcia, Google, samodzielna edycja, po wdrożeniu). Obsługuje go ten sam handler `.svc-acc-head` co na oferta.html.

**Formularz:** front-end only (brak backendu) — handler `#contact-form` robi `checkValidity()` + pokazuje potwierdzenie w `#contact-form-note` i czyści pola. Do podpięcia realnej wysyłki (mailto/Formspree/endpoint) w przyszłości.

**Klasy CSS strony kontakt (prefiks `contact-`):** `.contact-page`, `.contact-rule` (+ `--wrap`), `.contact-section`, `.contact-faq`, `.contact-grid`, `.contact-col`, `.contact-eyebrow`, `.contact-h2`, `.contact-list`, `.contact-row` (+ `--static`), `.contact-row-icon`, `.contact-form-wrap`, `.contact-form-card`, `.contact-form-title`, `.contact-field`, `.contact-label`, `.contact-input`, `.contact-textarea`, `.contact-submit`, `.contact-form-note`, `.contact-faq-col`. Style w `style.css` w sekcji „PODSTRONA KONTAKT" (koniec pliku).

---

## Strona O mnie (`o-mnie.html`)

Plik `o-mnie.html` w katalogu głównym (URL `/o-mnie`), zbudowany na szablonie podstron (`oferta.html`): współdzielony `<head>`, header (`nav` + `#mobile-menu`), footer i skrypty. **Wszystkie linki „O mnie"** w nav i footer (`index.html`, `oferta.html`, `kontakt.html`, `o-mnie.html`) prowadzą do `o-mnie.html` (wcześniej `/#o-mnie` → mini-sekcja na stronie głównej, która dalej istnieje jako `#o-mnie-v2`, ale nie jest już celem nav).

**Motyw: niebieskie hero „case" + jasna reszta** — `<body class="aboutme-page">`. Hero korzysta z klas `oferta.html` (`.svc-hero.svc-hero--b.svc-hero--case`, pełne niebieskie tło), ale jest **rozebrany do dwóch elementów**: dwuliniowy H1 + lead po lewej, przycięty wycinek postaci po prawej. **Nie ma badge’a, CTA, pierścienia, kadru ani karty realizacji** — wszystkie usunięte z HTML. Hero jest ciemne → `darkSectionIds = ['me-hero']` (nav z jasnymi linkami nad hero, domyślny niżej). Reszta strony jasna. Persona: **Paweł, freelancer od stron dla lokalnych firm**. Sekcje pod hero wzorowane na referencjach (misja + „dlaczego ja" + numerowana lista), przełożone na markę (Inter, niebieski/żółty).

**Sekcje (w kolejności):**

1. `#me-hero .svc-hero.svc-hero--b.svc-hero--case` — kratka i poświata wyłączone przez `content: none`, tło płaskie `var(--color-blue)`. Lewo: H1 w dwóch liniach (`.svc-hero-h1-l1` 300 „Cześć, jestem Paweł." nad `.svc-hero-h1-l2` 800 „Tworzę strony WWW" — akcent robi **grubość, nie kolor**) + lead `.svc-hero-b-lead` z akcentem w `<strong>` na **żółto** (na niebieskim tle niebieski akcent byłby niewidoczny). Prawo `.svc-hero-media` → `.svc-hero-device.svc-hero-device--portrait` z wycinkiem postaci. **Świadomie NIE ma tu:** badge’a `.svc-hero-label`, CTA, pierścienia `.svc-hero-ring` i karty `.svc-hero-case` — hero to nagłówek plus postać.
   - **Wycinek leży wprost na niebieskim tle**, bez panelu, ramki i radiusu: `--portrait` zeruje tło/radius/cień/overflow z `.svc-hero-device`. Sylwetkę od tła oddziela **podwójny `drop-shadow`** (szeroki „na podłogę" + krótki przy krawędziach) — bluza postaci jest ciemniejszym niebieskim niż tło, więc bez cienia zaczyna się w nim rozpływać. **Przy podmianie zdjęcia sprawdź ten kontrast**; wcześniejsza wersja rozwiązywała to jasnym panelem pod postacią (usunięty).
   - **Kadr: górne 90% oryginału.** `aspect-ratio: 2274 / 2489` (= `2274 / (2766 × 0.90)`) + `object-fit: cover` + `object-position: 50% 0` obcina sam dół. Progi zmierzone linijką procentową na PNG: iPad kończy się na **~88%**, ręka w kieszeni zaczyna się na **~92%**, spodnie na **~93%** — 90% wycina rękę i spodnie, zostawiając iPadowi 2% zapasu. **Zmiana zdjęcia => zmierz progi od nowa** (najszybciej: tymczasowa strona z obrazkiem i liniami co 2% wysokości, zrzut w DevTools). `object-fit` kadruje sam z siebie, więc kontener nie potrzebuje `overflow: hidden`.
   - **Linia cięcia siada na dolnej krawędzi sekcji** — `padding-bottom: 0` + `body.aboutme-page .svc-hero-media { align-self: end }`. Dzięki temu ucięta sylwetka czyta się jak kadr, a nie jak przypadkowe ucięcie; cień poniżej przycina `overflow: hidden` z `.svc-hero`.
   - **Skala postaci:** `width: 124%` z `margin-left: -24%` (tablet `116% / -16%`, mobile `100% / 0`) — ramka **rośnie w lewo**, bo w prawo wyszłaby poza viewport przy 1440.
   - **Wysokość hero dobrana do `oferta.html`** (987px vs 1003px na 1440): sekcję wyznacza ramka portretu (~859px) plus sam górny padding `8rem` (tablet `6.5rem`, mobile `7.5rem`). Zmiana szerokości portretu zmienia wysokość ramki → **przelicz padding**, nie zgaduj.
   - **Rozmiar H1 limituje prześwit do pierścienia** (ta sama metryka co na oferta): linia 2 ma ~10.03em ink (Inter 800, „WWW" jest szerokie), więc przy kolumnie 687px i foncie 57.6px zostaje ~116px do pierścienia. Dłuższa fraza („Projektuję strony WWW") zostawiała ~58px — stąd `clamp(2.2rem, 4vw, 3.6rem)` + osobne capy dla tabletu (`3.6vw`), mobile (`7.6vw`) i ≥1920px (`57px` — cap w px, bo to ograniczenie geometryczne, nie skala typografii). **Zmiana treści H1 = przelicz ink w DevTools** (`Range.getBoundingClientRect`).
2. `#me-mission .me-mission` — `.me-mission-top` (grid: `.me-eyebrow` „● O MNIE" + `.me-mission-text` duży akapit z `<strong>` w kolorze navy). Niżej `.me-mission-lower` — jedno szerokie zdjęcie 16:9 `.me-mission-photo.me-mission-photo--wide` `img/omnie-koduje.webp` (kodowanie strony w domowym biurze, klimat jak referencja). **Karta statystyki `.me-statcard` (+20% z mini wykresem `.me-bars`) usunięta z HTML — klasy `.me-statcard*`/`.me-bar*` pozostały w `style.css` jako martwy kod.**
3. `#me-why .me-why` — **„Dlaczego warto pracować ze mną"**: wyśrodkowany nagłówek `.me-why-head` (`.me-eyebrow` „● DLACZEGO JA" + `.me-why-h2` z akcentem `.me-why-h2-accent` + `.me-why-sub`) + `.me-why-grid` (3 kol) z 9× `.me-why-card` (ikona w kolorowym boksie `.me-why-icon` + warianty `--amber/--navy/--green` cyklicznie, `.me-why-card-title`, `.me-why-card-desc`). Karta: białe tło + `border`, hover = lift `translateY(-4px)` + `shadow-lg` + niebieski border, ikona `scale(1.08) rotate(-3deg)`. Animacja wejścia przez `.fade-up` (IntersectionObserver) ze staggerem kolumnowym `transition-delay` 0/90/180ms. Treść wzorowana na screenie referencyjnym (9 atutów), paleta marki (nie różowa jak referencja).
4. `#me-approach .me-sol` — `.me-sol-h2` + `.me-sol-grid` (lewo `.me-sol-photo` `img/omnie_kodowanie.png`, kadr `object-position: 50% 50%`; prawo `.me-sol-list` `<ol>` z `.me-sol-row` = `.me-sol-num` + `.me-sol-title` + `.me-sol-arrow`; wiersz `.me-sol-row--active` wyróżniony na niebiesko, hover przesuwa strzałkę i wcięcie).
5. `.svc-cta` (reuse z oferty) + `.footer-section`.

**Grafiki (Higgsfield):** portret klienta `img/gamingomniev2.jpg` (ciemny, fotel z niebieskim neonem) przerobiony modelem **`nano_banana_pro`** (image-to-image, zachowuje twarz) na jasne edytorskie ujęcia na jasnym tle — soul_2 odpadł, bo zawsze wzbogaca prompt i ciągnął z powrotem do ciemnej sceny referencji. Pliki zapisane jako `min.webp` (pełne 2k, ~100–470 KB).

**Klasy CSS strony O mnie (prefiks `me-`):** `.aboutme-page`, `.me-mission`, `.me-mission-top`, `.me-eyebrow`, `.me-mission-text`, `.me-mission-lower`, `.me-statcard`, `.me-bars`, `.me-bar` (+ `--accent`), `.me-statcard-num`, `.me-statcard-desc`, `.me-mission-photo`, `.me-why`, `.me-why-head`, `.me-why-h2` (+ `-accent`), `.me-why-sub`, `.me-why-grid`, `.me-why-card`, `.me-why-icon` (+ `--amber`/`--navy`/`--green`), `.me-why-card-title`, `.me-why-card-desc`, `.me-sol`, `.me-sol-h2`, `.me-sol-grid`, `.me-sol-photo`, `.me-sol-list`, `.me-sol-row` (+ `--active`), `.me-sol-num`, `.me-sol-title`, `.me-sol-arrow`. Hero używa klas `svc-` (`.svc-hero--case`, `.svc-hero-device--portrait`) — stare `.me-hero*`, `.me-hello*`, `.me-stat*`, `.me-scroll*` zostały **usunięte** ze `style.css` razem z jasnym hero. Style w `style.css` w sekcji „PODSTRONA O MNIE" (koniec pliku).

---

## Strona Proces (`proces.html`)

Plik `proces.html` w katalogu głównym (URL `/proces`), zbudowany na szablonie podstron (`oferta.html`): współdzielony `<head>`, header (`nav` + `#mobile-menu`), footer i skrypty. Podlinkowany w nav (pill + mobile menu) i w stopce jako „Proces" na wszystkich stronach — **zastąpił dawny link „Blog", który został usunięty** z całej nawigacji (`index.html`, `oferta.html`, `kontakt.html`, `o-mnie.html`, `proces.html`). Pokazuje proces tworzenia strony WWW od pierwszego kontaktu z klientem po wdrożenie i wsparcie.

**Motyw: niebieskie hero + jasna, editorialna reszta (inspiracja Snøhetta)** — `<body class="process-page">`, `darkSectionIds = ['svc-hero']`. Hero jak na oferta — od 01.09.2026 **niebieski `svc-hero--case`** (wcześniej jasny gradient + kratka), więc nad hero nav ma jasne linki (`darkSectionIds = ['svc-hero']`), a niżej wraca do domyślnych. Reszta strony jasna, dużo białej przestrzeni, cienkie linie między wierszami. Wizuały kroków to **ilustracje Higgsfield** (`img/proces-0X-*.webp`, `nano_banana_pro`, spójny flat set w palecie marki na off-white — tło grafiki zlewa się z panelem `.proc-step-visual`). Klasy `.proc-step-ghost` / `.proc-step-icon(-*)` + `.proc-step-visual::before` (kratka) pozostały w `style.css` jako **martwy kod** po podmianie ikon na obrazy.

**Animacje:** tylko `.fade-up` (IntersectionObserver, jak na index.html) — brak dedykowanych animacji GSAP na sekcjach. Skrypt Lenis (smooth scroll) zostaje, ale klasa `.gsap-enhance` jest zdejmowana zawsze, żeby stany startowe `.fade-up` nie były przechwytywane przez CSS pasów oferty.

**Sekcje (w kolejności):**

1. `#svc-hero .svc-hero.svc-hero--b.svc-hero--case` — **pełny reuse hero „case" z `oferta.html`** (żadnych nadpisań pod stronę): dwuliniowy H1 (`.svc-hero-h1-l1` 300 „Od pomysłu" nad `.svc-hero-h1-l2` 800 „do gotowej strony") + `.svc-hero-b-lead`, po prawej `.svc-hero-media` = pierścień + kadr `img/proces-hero.webp` + karta „6 kroków, zero niespodzianek". **Bez badge’a i bez akcentu kolorem** (akcent robi grubość fontu). Hero jest ciemne → `darkSectionIds = ['svc-hero']`. Zmierzone na 1440: wysokość 1003px (jak oferta), prześwit H1 → pierścień 130px (linia 2).
2. `#kroki .proc-steps` — **lista 6 kroków** (styl Snøhetta): nagłówek `.proc-steps-head` + 6× `.proc-step` (grid 3 kol `320px 1fr 360px`: `.proc-step-index` **sam numer** — `.proc-step-num` `clamp(5.5rem, 15vw, 16rem)`, `font-weight 800`, `line-height 0.8`, kolor `--color-blue` przygaszony `opacity: 0.34` (efekt „ghost"); rozmiar dobrany tak, żeby blok numeru miał **~połowę wysokości wiersza** (256px fontu ≈ 205px przy wierszu 410px na desktopie). Tekstowy label pod numerem usunięty · `.proc-step-body` H3+opis+`.proc-step-list` z checkmarkami+`.proc-step-meta` badge czasu · `.proc-step-visual` panel z ilustracją `.proc-step-img` `img/proces-0X-*.webp`). Hover: panel lift (`translateY`) + zoom obrazu (`scale 1.04`). Kroki: 01 Kontakt i konsultacja, 02 Analiza i strategia, 03 Projekt i wycena, 04 Projektowanie i kodowanie, 05 Testy i wdrożenie, 06 Wsparcie i rozwój.
3. `.svc-cta` (reuse) + `.footer-section` (reuse).

> Sekcje `.proc-intro` („Jak pracuję" + akapit) i `.proc-over` (timeline overview 01–06) zostały **usunięte** z `proces.html` — po hero od razu idzie lista 6 kroków. Ich CSS (`.proc-intro*`, `.proc-eyebrow`, `.proc-over*`) pozostał w `style.css` jako martwy kod.

**Klasy CSS strony Proces (prefiks `proc-`):** `.process-page`, `.proc-intro`, `.proc-intro-grid`, `.proc-eyebrow`, `.proc-intro-h2`, `.proc-intro-lead`, `.proc-over`, `.proc-over-grid`, `.proc-over-item` (+ `--final`), `.proc-over-num`, `.proc-over-title`, `.proc-over-desc` (wszystkie martwe po usunięciu sekcji intro/overview), `.proc-steps`, `.proc-steps-head`, `.proc-steps-h2`, `.proc-step`, `.proc-step-index`, `.proc-step-num`, `.proc-step-body`, `.proc-step-h3`, `.proc-step-desc`, `.proc-step-list`, `.proc-step-meta`, `.proc-step-visual`, `.proc-step-img`, `.proc-step-ghost` (martwy), `.proc-step-icon` (+ `--amber`/`--navy`/`--green`) (martwy). Style w `style.css` w sekcji „PODSTRONA PROCES" (koniec pliku).

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

## Design system — skala typograficzna sekcji

Jednolita na **wszystkich** stronach (index + podstrony). Przy dodawaniu sekcji używaj tych wartości zamiast wymyślać nowe:

| Poziom | Rozmiar | Klasy |
|---|---|---|
| eyebrow / label | `0.75rem` uppercase | `.benefit-label`, `.services-label`, `.portfolio-label`, `.usp-label`, `.process-label`, `.svc-work-label`, `.contact-eyebrow`, `.me-eyebrow` |
| H2 sekcji | `clamp(1.875rem, 3.5vw, 2.75rem)` | `.benefit-h2`, `.services-h2`, `.portfolio-h2`, `.usp-h2`, `.about2-h2`, `.process-h2`, `.svc-work-h2`, `.proc-steps-h2` |
| lead pod H2 | `1.0625rem` (17px) | `.benefit-sub`, `.services-sub`, `.usp-sub`, `.about2-sub`, `.svc-work-sub`, `.svc-hero-b-lead` |
| tytuł karty | `1.125rem` (18px) | `.benefit-title`, `.bento2-title`, `.portfolio-card-name`, `.usp-title`, `.process-title`, `.me-why-card-title`, `.svc-work-name` |
| opis karty | `1rem` (16px) | `.benefit-desc`, `.bento2-desc`, `.usp-desc`, `.process-desc`, `.me-why-card-desc`, `.svc-acc-panel-inner p`, `.proc-step-list li` |
| H1 hero podstrony | `clamp(2.25rem, 5vw, 3.75rem)` | `.svc-hero-h1` (wariant `.svc-hero--case` na oferta.html ma większy — patrz odstępstwa) |

**Nie dodawaj mobilnych override'ów `font-size` do H2** — `clamp()` obsługuje cały zakres; override łamie spójność (tak było w `.portfolio-h2` i `.about2-h2`). Jeśli nagłówek nie mieści się na wąskim ekranie, przyczyną jest zwykle `white-space: nowrap`, nie rozmiar czcionki — zdejmij `nowrap`, a nie zmniejszaj font.

**Skala bazowa: 16px, a powyżej 1920px — 18px.** Root `font-size` nie jest deklarowany (domyślne 16px przeglądarki), poza jedną regułą przy stylach bazowych w `style.css`: `@media (min-width: 1920px) { html { font-size: 18px } }`. Skaluje to **całą** typografię i wszystkie odstępy zapisane w `rem` o 12,5%; kontener zostaje na 1440px (to `px` w Tailwind config), więc treść na dużych monitorach robi się gęstsza, nie szersza.

Dwie pułapki przy dotykaniu tej reguły:
- **`min-width: 1920px` porównuje się z viewportem, nie z przekątną monitora.** Na ekranie 1920px viewport ma zwykle ~1905px (pasek przewijania), więc reguła tam **nie zadziała** — włącza się dopiero na 2K/4K. Zmierzone: okno 1930 → `innerWidth` 1908 → root nadal 16px; okno 2560 → 2538 → root 18px. Jeśli 1920 ma być objęte, breakpoint musi zejść do ~1900px.
- **Podbicie rootu przesuwa górne granice `clamp()`.** `.svc-hero--case .svc-hero-h1` miało max `4.5rem`, co przy 18px daje 81px — a kolumna tekstu ma tam ~683px i „Kompleksowa oferta" (~8,93 × font-size) zawijała się na dwie linie. Dlatego w sekcji hero oferty jest override `@media (min-width: 1920px)` z capem **w `px`** (`clamp(2.35rem, 5vw, 73px)`) — bo to ograniczenie geometryczne (długość frazy), które **nie ma** się skalować razem z rootem. Przy dodawaniu nowych `clamp()` z górną granicą w `rem` sprawdź, czy na ≥1920px nie przekracza swojego kontenera.

**Kafle wyróżnione bento** — `.bento2-t1-text .bento2-title` i `.bento2-tile-2 .bento2-title` mają `clamp(1.5rem, 2.2vw, 2rem)` (mobile `1.6rem`). To celowa hierarchia wewnątrz bento grid (2 kafle wiodące + 4 zwykłe), nie odstępstwo do naprawy.

**Świadome odstępstwa** (statement headings, nie zwykłe H2 sekcji): `.contact-h2` `clamp(2rem, 4.5vw, 3.25rem)`, `.me-why-h2` `clamp(1.9rem, 4vw, 3rem)`, `.svc-cta-h2` `clamp(1.75rem, 3vw, 2.5rem)`, `.svc-block-h3` `clamp(1.5rem, 2.5vw, 2rem)`, `.svc-hero--case .svc-hero-h1` `clamp(2.35rem, 5vw, 4.5rem)` (hero „case study" na oferta.html — celowo dominujący nagłówek w stylu referencji marceldigital.com; górna granica wynika z długości frazy, patrz sekcja o hero) i jego wariant na `o-mnie.html` `clamp(2.2rem, 4vw, 3.6rem)` (dłuższa fraza w linii 2 — patrz sekcja o hero O mnie).

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

Tło: `#F8FAFC` (jasne), padding: `7rem 0`. 4 kroki w gridzie 4 kolumny.

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

- **Sekcja `#realizacje` (slider):** osobny `<script>` IIFE. Struktura DOM: `.portfolio-slider` (viewport, `overflow: hidden` w trybie `.is-gsap`) → `.portfolio-track` (pozycja przez GSAP `x` = `translate3d`, `force3D: true`, jedyny `will-change: transform`) → `.portfolio-card` → `.portfolio-card-inner` (hover lift/zoom w CSS — osobna warstwa, by transformy się nie kolidowały). **Animujemy wyłącznie `transform` + `opacity`.** Ruch tracka: `moveTo(x)` z `clamp` do `[minX, 0]`; `step` = różnica `offsetLeft` dwóch kart; `minX` liczony z `clientWidth − padding − scrollWidth`. **Podświetlenie kart (coverflow) — sterowane jawnym indeksem.** Aktywna karta (`scale 1`/`opacity 1` + `.is-active`, nośnik `will-change`) vs pozostałe (`0.96`/`0.62`). Indeks `activeIdx` jest **stanem, nie funkcją pozycji tracka** — `setActive(i)` maluje stan, `xForIndex(i)` liczy pozycję, przy której karta `i` jest wyśrodkowana (z `clamp` do `[minX, 0]`), a `goTo(i)` robi jedno i drugie. Prev/next i autoplay operują na `activeIdx ± 1`, więc **podświetlenie przechodzi przez każdą kartę po kolei: 1→2→3→4→5** i z powrotem; przyciski są `disabled` na `activeIdx === 0` / `=== last`. Podczas dragu indeks liczy `nearestIndex()` (karta najbliżej środka, z przypięciem krańców przy `curX ≈ 0` / `≈ minX`), a `endDrag` robi `goTo(nearestIndex())`.

  > **Dlaczego jawny indeks:** przy 3 widocznych kartach track ma tylko ~2 kroki zapasu, więc skrajnych kart nie da się wyśrodkować (`clamp`). Gdy indeks wyliczano z pozycji, kroki strzałek dawały 1→3→4→5 (druga karta nigdy się nie podświetlała), a autoplay z przedostatniej pozycji wracał od razu na `0`, pomijając ostatnią kartę. Skutek uboczny do zapamiętania: przejście 1↔2 i przedostatnie↔ostatnie przesuwa track tylko o kilkadziesiąt px (karta jest już maksymalnie dosunięta) — rusza się głównie podświetlenie. Stan `disabled` przycisków obsługuje osobne `updateButtons`. Hover karty: lift + zoom obrazu + przyciemnienie overlaya (**bez okrągłej strzałki w rogu kafelka** — `.portfolio-card-arrow` usunięty). **Drag/touch:** Pointer Events + `setPointerCapture`, rubber-band 0.35 poza granicami, snap do `Math.round(x/step)*step` na `pointerup`; guard „ghost click" gdy `moved`. **Autoplay:** `setInterval` 5 s, `autoNext` przesuwa o `step` i wraca płynnie na `0` po ostatniej; pauza na `mouseenter`, wznowienie na `mouseleave`; włączany dopiero po revealu wejścia i tylko gdy `minX < 0`. **Reveal wejścia** (`ScrollTrigger`, `start: 'top 78%'`, `once`): label (`opacity`+`y16`+`letter-spacing 0.30→0.15em`) → H2 (`y40`) → przyciski (`scale 0.9→1`, `back.out`) → karty (`y60`, `scale 0.94→1`, stagger `0.12`); po zakończeniu `updateActive` + start autoplay. **Fallback** (brak GSAP / `reduce-motion`): natywny scroll poziomy, przyciski robią `scrollBy`. Stany startowe `opacity: 0` chowane w CSS przez `.gsap-enhance #realizacje …` (z un-hide dla `prefers-reduced-motion`).

  **Wymiarowanie kart (CSS).** `.portfolio-section` trzyma metrykę slidera w zmiennych: `--pf-gap: 26px`, `--pf-peek: 110px` (widoczny fragment 4. karty), `--pf-pad: max(1.5rem, calc((100vw − 1440px)/2 + 1.5rem))` = lewa krawędź `.max-w-container`. `padding-inline` slidera używa tej samej formuły na `100%`, dzięki czemu **pierwsza karta jest wyrównana do lewej krawędzi nagłówka sekcji**. Szerokość karty to `clamp(320px, calc((100vw − var(--pf-pad) − 3*var(--pf-gap) − var(--pf-peek)) / 3), 620px)` → na ekranie mieszczą się **3 pełne karty + zapowiedź 4.** (np. 1800px → karta ~470px). Mobile/tablet mają własne stałe szerokości w media queries. Zmiana `max-w-container` w Tailwind config wymaga zmiany `1440px` w obu miejscach.

---

## Konwencje CSS

- Custom style TYLKO w `style.css` — nie dodawaj `<style>` inline do HTML
- Tailwind używany tylko do utility (grid, flex, padding, margin, text-center) — nie do komponentów
- Inline style (`style="..."`) tylko dla `transition-delay` na animowanych elementach
- Nazwy klas: kebab-case, prefiks sekcji: `.benefit-*`, `.bento-*`, `.process-*`, `.usp-*`, `.about-*`, `.footer-*`
- Media queries bezpośrednio po stylach sekcji której dotyczą, w kolejności: `max-width: 767px` → `min-width: 768px and max-width: 1023px`

---

## Ważne decyzje projektowe

- Nagłówki hero podstron: **wszystkie 4 podstrony mają teraz to samo niebieskie hero `--case`** (`oferta.html`, `proces.html`, `kontakt.html`, `o-mnie.html`), więc akcent w H1 wszędzie robi **grubość fontu** (linia 300 nad linią 800) — niebieski na niebieskim byłby niewidoczny. Akcent kolorem przenosi się do leadu i jest **żółty**. Skutek uboczny ujednolicenia (01.09.2026): `.svc-hero-h1-accent` oraz `.svc-hero-label*` (badge) **nie są już używane w żadnym HTML-u** i zostają w `style.css` jako martwy kod — razem z jasnym wariantem `.svc-hero--b` (gradient + kratka) i regułą maski kratki dla `body.process-page` / `body.contact-page`. `index.html` ma własne wyróżnienie żółtym tekstem na granatowym skosie (`.hero-highlight-wrap`).
- Nav: `.nav-wrapper` ma **ten sam box co sekcje strony** — `max-width: 1440px; margin: 0 auto; padding: 0 24px` (odpowiednik tailwindowego `max-w-container mx-auto px-6`), a `#nav` nie ma własnego paddingu. Do tego boksu wpisany jest pill `.nav-bar`, więc **krawędzie pilla** równają się z krawędziami treści sekcji poniżej (logo i CTA są dodatkowo wcięte o padding pilla, `--nav-px`). Referencja proponowała `max-width: ~1500px` — zostało 1440px, żeby pill trzymał ten sam box co reszta strony. Na mobile padding schodzi do `16px` (w media query na `.nav-wrapper`, nie na `#nav`). Zmiana `max-w-container` w Tailwind config wymaga zmiany `max-width` w `.nav-wrapper`.
- Hero: treść do lewej, kontener `w-full` — bez wewnętrznego `max-width` żeby H1 mieścił się w jednej linii
- Hero highlight (`dla lokalnych biznesów`) — navy tło skewX(-6deg), animacja clip-path od lewej
- Bento: białe kafelki na niebieskim gradiencie (nie ciemne jak w PRD)
- Co zyskasz: jasne tło (nie ciemne jak w PRD) — karty z okrągłymi strzałkami
- Sekcja O mnie: zdjęcie z `opacity: 0.88` żeby miękko wtapiało się w navy tło
- Footer: ciemne tło `var(--color-navy)` (#1A2540) — spójność z sekcją O mnie, tekst dostosowany do ciemnego tła
- Logo (wordmark): `img/logopin3Dv1.png` (64×64px nav, 40×40px footer) + tekst w **wersalikach** — `LOKALNE` (800) + `WWW.PL` (300), **oba w tym samym kolorze**, `letter-spacing: 0.015em`. Kontrast robi wyłącznie grubość fontu, nie kolor (wcześniej druga część była szara `#64748B`/muted). Wersaliki wymusza CSS (`text-transform: uppercase` na `.logo-text` i `.footer-logo-text`), HTML zostaje pisany małymi literami — dzięki temu zmiana propaguje się na wszystkie 5 stron ze wspólnego `style.css`. W wariancie `nav--light` i w stopce druga część jest jasna z `opacity` ~0.9, nie przygaszona kolorem.

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
