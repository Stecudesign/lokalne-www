# Product

<!-- impeccable:product-schema 1 -->

## Platform

web

## Users

Właściciele małych lokalnych firm z całej Polski — usługi, rzemiosło, gastronomia, handel, gabinety. Zwykle nie mają strony albo mają słabą; często są nieprzekonani do potrzeby jej posiadania albo rozczarowani wcześniejszą współpracą z agencją. Przeglądają ofertę często na telefonie, między obowiązkami, i szukają kogoś, komu mogą zaufać bez znajomości branżowego żargonu.

## Product Purpose

lokalnewww.pl to strona sprzedażowa freelancera (Paweł), który projektuje i wdraża strony WWW dla lokalnych firm. Sukces = kontakt: wypełniony formularz, telefon lub mail z prośbą o bezpłatną konsultację. Cele pośrednie: zapoznanie się z ofertą (`oferta.html`) i projektami, zbudowanie zaufania przez transparentność i profesjonalizm samej strony.

## Positioning

Bezpośrednia współpraca 1:1 z jedną osobą — bez agencji, pośredników i działu obsługi. Specjalizacja w małych lokalnych firmach, a nie w dużych projektach. Obsługa zdalna w całej Polsce.

## Operating Context

- Ścieżka klienta: strona główna → oferta / proces / o mnie → `kontakt.html` (formularz, telefon, e-mail) → bezpłatna konsultacja.
- Proces współpracy: konsultacja → analiza → projekt i wycena → projektowanie i kodowanie → testy i wdrożenie → wsparcie.
- Usługi: strony internetowe, SEO, indywidualny design, responsywność, opieka nad stroną, analityka Google.

## Capabilities and Constraints

- Strona statyczna: czysty HTML + `style.css` + Vanilla JS, bez buildu i frameworka, bez CMS i backendu. Hosting: GitHub Pages (project page) — brak przekierowań po stronie serwera, ścieżki relatywne.
- Formularz kontaktowy jest tylko front-endowy; realna wysyłka (Formspree / endpoint) — **nieustalona**.
- Otwarte: polityka prywatności (brak strony), obraz Open Graph (`img/og-image.jpg` nie istnieje), docelowe profile LinkedIn/Facebook, cennik.

## Brand Commitments

- Nazwa i wordmark: LOKALNE**WWW.PL** z logo pinu 3D (`img/logopin3Dv1.webp`).
- Język: polski. Zwrot do klienta na „Ty”, **neutralnie płciowo** — bez form męskich/żeńskich („dowiesz się więcej”, nie „będziesz wiedział”).
- Ton: prosty, konkretny, bez żargonu technicznego.

## Evidence on Hand

- Portfolio (`#realizacje`: Barber, Serwis BMW, Gabinet Fizjoterapii, Burgerownia, Szkoła Pływania) to **projekty koncepcyjne/demo, nie realni klienci**. Nie wolno ich przedstawiać jako wdrożeń, case study ani opisywać wynikami klientów.
- Brak prawdziwych opinii, liczby klientów, statystyk efektów i logotypów klientów — **nie fabrykować**.
- Dane kontaktowe w `kontakt.html` (telefon, adres w Krakowie) są placeholderami; e-mail `kontakt@lokalnewww.pl`.
- Portret w hero `omnie.html` (`img/pawel-portret-*.webp`, źródło `img/pawel_nobackgroundv1.png`) przedstawia właściciela. Pozostałe fotografie i wideo (`img/aboutmevideo.mp4`, hero-* podstron) to ujęcia generowane (Higgsfield) — ilustracyjne, nie dokumentacja realnej pracy z klientem.

## Product Principles

1. Zaufanie przed efektem: strona sama jest dowodem umiejętności — musi działać szybko, czytelnie i bez błędów na telefonie.
2. Uczciwość dowodów: pokazujemy tylko to, co prawdziwe; demo nazywamy demo.
3. Jedna osoba, jeden kontakt: każda ścieżka prowadzi do rozmowy z Pawłem.
4. Prosto dla laika: każda usługa tłumaczona w kategoriach korzyści dla lokalnej firmy.

## Accessibility & Inclusion

Cel: WCAG 2.1 AA (z PRD). Animacje jako progresywne wzbogacenie — treść musi być widoczna bez JS i przy `prefers-reduced-motion`.
