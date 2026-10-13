# parafiagrzmiaca.pl: notatki projektu

Stan na 13.10.2026. Opracowane na podstawie transkryptu sesji Claude Code (2.09–5.10.2026) i kopii żywej strony z 13.10.2026.

## 1. Co to jest
- **Strona:** https://parafiagrzmiaca.pl, Parafia pw. Matki Bożej Królowej Polski w Grzmiącej (Grzmiąca, Mieszałki, Nosibądy, Sucha).
- **Technologia:** WordPress, motyw Neve, Yoast SEO, WP Super Cache.
- **Stary system PHP:** działa obok WordPressa w katalogach na serwerze: `/ogloszenia/`, `/panel/`. Obsługuje logowanie proboszcza, ogłoszenia i intencje mszalne.
- **Hosting:** konto `srv110238` (dokładny dostawca nie jest zapisany w transkrypcie).
- **Kod strony nie istnieje jako pliki.** Wszystko żyje w bazie WordPressa: strony z blokami Custom HTML ze skryptami i stylami oraz widgety w stopce. Folder `strona/` to kopia tej zawartości.

## 2. Mapa strony (30 stron)
| ID | Adres | Tytuł | Ostatnia zmiana |
|---|---|---|---|
| 1289 | `/` (slug `aktualnosci`) | Aktualności: **strona główna** | 2026-09-27 |
| 80 | `/ogloszenia-parafialne/` | Ogłoszenia | 2026-09-27 |
| 66 | `/historia-kosciola/` | Historia + gra „Pielgrzymka przez wieki” | 2026-09-04 |
| 381 | `/zywy-rozaniec/` | Żywy różaniec (intencje 5 kół) | 2026-10-03 |
| 640 | `/galeria/` | Galeria | 2026-09-27 |
| 64 | `/kontakt/` | Kontakt | 2026-09-03 |
| 68 / 75 / 82 / 124 | `/godziny-mszy-…/` | Godziny mszy: Grzmiąca, Mieszałki, Nosibądy, Sucha | 2026-09-03 |
| 86, 90, 92, 94, 467, 637 | sakramenty | Bierzmowanie, I Komunia, Namaszczenie, Małżeństwo, Chrzest, Spowiedź | 2026-09-03 |
| 202 | `/kaplani/` | Duszpasterze | 2026-09-03 |
| 1223–1227 | profile | Ks. Mariusz Kuropatnicki (proboszcz), Piotr Sztygowski, Dariusz Śliwka, LSO, Schola | 2026-09-03 |
| 319 | `/caritas/` | Caritas (kod QR) | 2026-09-03 |
| **1240** | `/zarzadzanie/` | **Panel zarządzania (hub logowania)** | 2026-09-27 |
| 1242 | `/dodaj-ogloszenie/` | Panel: ogłoszenia | 2026-09-27 |
| 1243 | `/intencje-mszalne/` | Panel: intencje mszalne | 2026-09-27 |
| 1306 | `/zarzadzaj-rozancem/` | Panel: intencje Żywego Różańca | 2026-09-28 |
| 1315 | `/zarzadzaj-galeria/` | Panel: galeria | 2026-09-28 |
| 1259 | `/swiece-modlitwy-dane/` | Techniczna: dane świec (nieużywana) | 2026-09-03 |
| 1297 | `/amp/` | Techniczna: przekierowanie starego linku AMP | 2026-09-03 |

## 3. Panel zarządzania `/zarzadzanie/`: jak działa
Logowanie kontem **proboszcz** (stary system PHP). Po zalogowaniu są 4 kafelki:

| Kafelek | Gdzie zapisuje | Jak publikuje |
|---|---|---|
| Ogłoszenia | stary system PHP `/ogloszenia/` | od razu (AJAX). Usuwanie naprawione 27.09 |
| Intencje mszalne | stary system PHP `/panel/` | od razu (AJAX). Usuwanie naprawione 27.09 |
| 📿 Żywy Różaniec | strona WP 381 | **tylko gdy w tej samej przeglądarce jesteś zalogowany do `/wp-admin/`** (kontem `marcel-claude`). Bez tego pokaże czerwony komunikat i podsumowanie do skopiowania |
| Galeria | strona WP 640 + biblioteka mediów | jak wyżej: wymaga sesji `/wp-admin/` w tej samej przeglądarce |

**Najczęstszy problem (zgłoszony 28.09 „Nadal nie działa samo”):** telefon nie był zalogowany do wp-admin.
Rozwiązanie: na tym samym telefonie i w tej samej przeglądarce wejdź na `parafiagrzmiaca.pl/wp-admin/` i zaloguj się, potem wróć do panelu. Od 28.09 panel pokazuje dokładny powód niepowodzenia.
**Nie wiadomo, czy to finalnie zadziałało.** Sesja urwała się przed Twoją odpowiedzią. Strona 381 (Żywy Różaniec) została zmieniona 3.10, już po zakończeniu sesji. Ktoś więc ją zaktualizował, panelem albo w wp-admin.

## 4. Funkcje autorskie na stronie
- **Aktualności (strona główna):**
  - pasek „Dziś w parafii” (imieniny, okres liturgiczny),
  - statystyki parafii,
  - przyciski: Wyrusz w cyfrową pielgrzymkę, Ogłoszenia, Galeria,
  - ściana świec modlitwy (zapis tylko lokalny),
  - sekcja **„Odmów dziesiątkę różańca”**: 12 klikalnych koralików, tajemnica dnia, odliczanie do 7.10, passa dni, limit raz na dzień.
- **Ogłoszenia:** odliczanie do najbliższej mszy, lista ogłoszeń. Przyciski: Poznaj Żywy Różaniec, Godziny mszy, Aktualności.
- **Historia:** gra **„Pielgrzymka przez wieki”**: 19 pytań, 4 przystanki, paszport pielgrzyma ze stemplami, najlepszy wynik w localStorage. Gra i opis Grzmiącej ładują się od razu, bez animacji.
- **Żywy Różaniec:** tajemnica dnia + owoc duchowy. Intencje 5 kół są renderowane z tablicy JS `ROZANIEC_WSPOLNOTY`, którą edytuje panel.
- **Galeria:** wydarzenia z opisami: Wielkanoc 2026, Boże Narodzenie 2025, Borne Sulinowo, Turniej piłki nożnej.
- **Caritas:** kod QR do darowizn.
- **Google:** logo Organization i favicon to złoty krzyż na granacie (media 1298). Nagłówek strony to napis „Parafia Grzmiąca” (logo w nagłówku zdjęte 4.09 na Twoje polecenie).

## 5. Identyfikacja wizualna
- Kolory:
  - granat `#1E3A5F`,
  - ciemny granat `#12233A`,
  - złoto `#C9A24B`.
- Nagłówki pisane czcionką Playfair Display.
- Klasy CSS z prefiksem `kx-`. Animacje `kx-reveal` są w widgecie block-43 (`strona/globalne/`).
- Logo: `grafiki/logo/1298-logo-parafia-grzmiaca.png` (1024×1024).

## 6. Pułapki techniczne (obowiązkowe przy każdej zmianie)
1. **`&&` w skryptach:** WordPress potrafi zamienić je na `&#038;&#038;` i zabić cały skrypt strony. Unikaj `<` przed i `>` po `&&` w jednym wyrażeniu. Sprawdzaj `node --check` przed i po wysłaniu.
2. **Kolizje slugów:**
   - `/ogloszenia/` i `/panel/` to prawdziwe katalogi PHP i zawsze wygrywają z WordPressem,
   - nie nadawaj stronom slugów `ogloszenia` ani `panel`.
3. **Szary pas pod menu (`.nv-post-cover`):**
   - strona z treścią: `featured_media: 1175`,
   - panel: ukryj CSS-em.
4. **Cache:** po każdej zmianie wyczyść WP Super Cache. Przeglądarka trzyma stronę do 5 minut (`max-age=300`), więc testuj w oknie prywatnym.
5. **Yoast:** tytułów i opisów SEO nie da się zmienić przez REST. Ustawiaj je ręcznie w wp-admin.
6. **Komentarze anonimowe są zablokowane** (przyczyna nieznana). Dlatego świece są tylko lokalne.
7. **Limit 429:** przy serii zapytań rób przerwy.

## 7. Sprawy otwarte i rekomendacje
| # | Sprawa | Kto | Priorytet |
|---|---|---|---|
| 1 | **Zmienić hasła i unieważnić hasło aplikacji `srv110238`.** Hasła krążyły w transkryptach czatu (patrz `DOSTEPY.md`) | Ty, w wp-admin → Użytkownicy → Hasła aplikacji | Wysoki |
| 2 | Potwierdzić, że publikacja na żywo z panelu Galerii i Różańca działa na telefonie po zalogowaniu do wp-admin | Ty | Średni |
| 3 | Google Search Console → Sprawdzanie adresu URL → „Poproś o zaindeksowanie” strony głównej, żeby Google pokazał nowe logo | Ty | Średni |
| 4 | Google Business Profile parafii: ustawić parafiagrzmiaca.pl jako oficjalną stronę. Ewentualnie poprosić diecezję o aktualizację linku | Ty / parafia | Średni |
| 5 | Potwierdzić odstępy w menu mobilnym (poprawka z 3.09, brak Twojego potwierdzenia) | Ty | Niski |
| 6 | Sekcja różańcowa na stronie głównej po październiku sama przełącza się na tekst ogólny. Zdecydować, czy zostaje na stałe | Ty | Niski |
| 7 | Kopia zapasowa plików serwera (stary system PHP) i bazy danych. Tego **nie ma** w tym folderze, bo nie było dostępu do plików | Hosting | Średni |

## 8. Dlaczego sesja się urwała i jak pracować dalej
- Sesja `session_019QtrzTCQLrpQiyXU8UAvP9` była przypięta do gałęzi `main` w repozytorium `marcel09fijolek-commits/marcel`. Tej gałęzi nie ma: repo ma tylko gałęzie `claude/…`, a domyślną jest `claude/parafiagrzmiaca-bugs-7tw22w`. Przy restarcie kontenera klonowanie się nie powiodło, więc sesji nie da się wznowić.
- Rozmowa nie przepadła: transkrypt jest na serwerze i z niego powstał ten folder.
- **Następnym razem** zacznij nową sesję i wskaż ten folder albo `notatki-parafiagrzmiaca.md` jako kontekst. W chmurze wybierz istniejącą gałąź, np. `claude/parafiagrzmiaca-bugs-7tw22w`. Możesz też pracować lokalnie z folderu `InternetSites\parafia-grzmiaca`, tak jak przy fotobox-koszalin.

## 9. Zawartość folderu
- `README.md`: start.
- `notatki-parafiagrzmiaca.md`: ten dokument.
- `DOSTEPY.md`: loginy i hasła. **Tylko lokalnie, nie trafia do gita.**
- `historia-rozmowy/`:
  - `01-podsumowanie-2-3-wrzesnia.md`: streszczenie początku prac,
  - `02-rozmowa-3-wrzesnia-5-pazdziernika.md`: pełna rozmowa (Twoje wiadomości i odpowiedzi).
- `strona/`:
  - `tresc-stron/`: treść każdej z 30 stron (HTML z WordPressa, ze skryptami paneli i widżetów),
  - `pelny-html/`: pełne strony tak, jak widzi je odwiedzający (z motywem),
  - `globalne/`: widgety stopki, w tym globalne style i skrypty (block-43) i Schema (block-46),
  - `dane/`: lista stron z tytułami i opisami SEO, pełny zrzut API, lista mediów, Schema Yoast, robots.txt, sitemap.
- `grafiki/`:
  - `logo/`: logo parafii i godło Żywego Różańca,
  - `media/`: wszystkie 30 plików z biblioteki mediów WordPress (oryginały).

**Ograniczenie kopii:** to treść wyrenderowana z publicznego API. Nie ma w niej surowych bloków edytora, ustawień, menu, użytkowników, bazy ogłoszeń i intencji ani plików PHP. Do pełnego odtworzenia strony po awarii potrzebny jest backup z hostingu (pliki + baza).
