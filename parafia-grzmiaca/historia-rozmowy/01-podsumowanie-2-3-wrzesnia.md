# Podsumowanie prac z 2–3 września 2026 (przed kompakcją kontekstu)

Źródło: automatyczne podsumowanie, które sesja Claude Code zapisała 3.09.2026 o 20:38 UTC, gdy skończyło jej się miejsce w kontekście. Treść przetłumaczona i uporządkowana. Szczegóły z najwcześniejszych godzin (2.09) były już wtedy streszczone wcześniejszą kompakcją, więc część rzeczy jest opisana ogólnie.

## Zakres prac
Strona parafiagrzmiaca.pl (WordPress, motyw Neve) przeszła wtedy kilka etapów:
1. Naprawa błędów i audyt bezpieczeństwa.
2. Pełny redesign: granat `#1E3A5F` / `#12233A` + złoto `#C9A24B`, własny system klas `kx-`, czcionka Playfair Display w nagłówkach.
3. Połączenie dwóch starych paneli PHP (osobne logowanie do ogłoszeń i intencji) w jeden hub logowania `/zarzadzanie/` z wyborem: ogłoszenie albo msza.
4. Przebudowa formularzy dodawania ogłoszeń i intencji mszalnych.
5. SEO: usunięcie wtyczki AMP, dane strukturalne (Schema `Church`, widget block-46).
6. Porządki we wtyczkach i mediach.
7. „Innowacje”:
   - odliczanie do najbliższej mszy (`kx-nm`),
   - pasek statystyk parafii (`kx-stats`),
   - wirtualna ściana świec modlitwy (`kx-candles`),
   - interaktywna oś czasu na stronie Historia,
   - pasek „Dziś w parafii” z imieninami i okresem liturgicznym (`kx-today`),
   - tajemnica różańca dnia i owoc duchowy na stronie Żywy Różaniec,
   - kod QR do darowizn na stronie Caritas,
   - szybka nawigacja po kategoriach w Galerii.
8. Nowy wygląd 5 stron duchownych i zespołów: 1223, 1224, 1225, 1226, 1227.
9. Podział strony głównej na **Ogłoszenia** (praktyczne, tygodniowe) i nową stronę **Aktualności** (społeczność). Aktualności zostały stroną główną.
10. Wygląd w Google: nowe logo i favicon (szczegóły w historii rozmowy, 3.09 wieczorem).

## Twoje polecenia z tego okresu (dosłownie)
- „Zmień dodawanie ogloszen oraz mszy na stronę. Jest utworzona strona do dodawania ogloszen (parafiagrzmiaca.pl/ogloszenia) oraz panel do dodawania intencji (parafiagrzmiaca.pl/panel). Do oby dwu trzeba sie osobno logować… Zamiast dwóch stron stworz jedna…”
- „login: proboszcz hasło: [→ DOSTEPY.md] Zanim cos zmienisz zrob kopię zapasową”
- „Zrób zeby tutaj tez byl wyświetlany obraz ktory jest pod nawigatorem przy innych podstronach”
- „wszystko super. Teraz zajmij sie żywym rozancem, Caritasem i galeria”
- „ok, teraz zajmij sie seo, pozycjonowaniem, wygladem strony w internecie…”
- „sprawdź jeszcze media, czy są nieużywane zdjęcia” → wybrano „Usuń wszystkie 23”
- „cofnij to, usunąłeś zdjęcia z zakladki historia na stronie”
- „Przeanalizuj jeszcze raz całą stronę… Zanim cos zmienisz napisz mi w co celujesz.” → „wymyśl cos jeszcze” → „Wszystkie pięć, zrób to co proponujesz”
- „zamień kolejność w kontaktach. zamień mapę z godzinami pracy kancelarii”
- „Utwórz całkowicie nowa podstrone. Aktualności… Podziel… podstrone ogloszen na ogloszenia i aktualności.”
- „podnieś wyzej wszystkie te podstrony, tzn zmniejsz odstęp” (menu mobilne)
- „w aktualnościach nie pokazuje obrazu u gory…” → „…brak przestrzeni miedzy nawigatorem a headerem…”
- „Zrób aby główna strona byla strona z aktualnosciami”
- „…gdy klikam zakladke ogloszenia na stronie to pokazuje mi pole do logowania i dodawania ogloszen”
- „Teraz zajmij sie wyglądem w wyszukiwarce… Slabo to wyglada. Psuje więź ze strona zanim na nia sie wejdzie.”

## Kluczowe ustalenia techniczne
- **Dostęp:** tylko WordPress REST API z hasłem aplikacji (konto `srv110238`). Nie było dostępu do plików ani SFTP.
- **Błąd `&&` w skryptach:** WordPress (prawdopodobnie `wptexturize`) psuje `&&` na `&#038;&#038;`, jeśli w tym samym `<script>` przed nim jest `<`, a po nim `>`. Wtedy cały skrypt strony przestaje działać. Wystąpiło 3 razy. Obejście: przebudować wyrażenie, np. przez zmienną pośrednią. Obowiązkowo uruchamiać `node --check` przed i po każdym wysłaniu.
- **Szary pas `.nv-post-cover` w Neve:**
  - strona z treścią: ustawić `featured_media: 1175` (wspólne zdjęcie parafii),
  - strona narzędziowa lub panel: ukryć regułą `.page-id-{ID} .nv-post-cover{display:none!important}`.
- **Kolizja adresów:** serwer (`.htaccess`) zawsze wybiera prawdziwy katalog zamiast strony WordPressa. Katalog `/ogloszenia/` to stary panel PHP, dlatego strona 80 ma slug `ogloszenia-parafialne`. Przy każdej zmianie slugu trzeba sprawdzić kolizję.
- **Yoast SEO:** ustawienia tytułu i opisu z Yoast nie zapisują się przez REST (cichy błąd). Logo Organization udało się ustawić przez endpoint `/yoast/v1/configuration/site_representation`.
- **`comment_registration`:** nie dało się wyłączyć wymogu logowania do komentarzy (prawdopodobnie mu-plugin lub blokada hostingu). Dlatego ściana świec zapisuje świece tylko lokalnie (localStorage). Zaakceptowałeś to jako stan docelowy.
- **Cache:** po każdej zmianie trzeba wyczyścić WP Super Cache: `POST /wp-json/wp-super-cache/v1/cache {"delete_cache":true}`.
- **Limit zapytań (429):** zdarza się często. Pomaga odczekanie 10–20 s.
- **Menu:** pozycje typu `post_type` same aktualizują adres po zmianie slugu. Pozycja „AKTUALNOŚCI” ma ID 1291 i `menu_order` 3.
- **Widget block-43** (stopka, cała strona): import czcionki Playfair, animacje `kx-reveal` (`window.kxObserveReveal()`), gradient na `.nv-post-cover` i poprawki odstępów menu mobilnego.

## Najważniejsze zmiany na stronach
| Strona | Zmiana |
|---|---|
| 80 Ogłoszenia | Slug `ogloszenia-parafialne`. Hero, odliczanie do mszy, przyciski, shortcode `[lista_ogloszen]` |
| 1289 Aktualności | Strona główna (`page_on_front`). Pasek „Dziś”, statystyki, świece. `featured_media` 1175 |
| 1259 | Ukryta strona na dane świec (komentarze, ostatecznie nieużywane) |
| 1297 `/amp/` | Przekierowanie na stronę główną (stary link z Google) |
| 319 Caritas | Kod QR SVG do caritas.pl/formularz |
| 381 Żywy Różaniec | Tajemnica dnia i owoc duchowy |
| 640 Galeria | Kotwice i szybka nawigacja |
| 1223–1227 | Nowy szablon profili duchownych |
| 202 Duszpasterze | Celowo NIE zmieniana (na Twoje polecenie) |
| 11 stron (202, 319, 381, 640, 64, 637, 467, 90, 86, 94, 92) | Dodane animacje `kx-reveal` |
