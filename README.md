# Bipmapa

Mapa zamawiających publicznych w Polsce, zbudowana na podstawie ogłoszeń z Biuletynu Zamówień Publicznych (BZP), publikowanego na platformie e-Zamówienia. Pozwala zobaczyć, kto i gdzie ogłasza przetargi, przefiltrować ogłoszenia i obserwować wybranych zamawiających.

> **Niezależny projekt studencki.** Bipmapa nie jest serwisem Urzędu Zamówień Publicznych ani administracji publicznej. Dane pochodzą z publicznie dostępnego Biuletynu Zamówień Publicznych (UZP).

## Problem

Każdego roku instytucje publiczne w Polsce — gminy, szpitale, szkoły, uczelnie, spółki komunalne — ogłaszają dziesiątki tysięcy zamówień: od remontów dróg, przez dostawy sprzętu, po usługi informatyczne. Wszystkie te ogłoszenia są jawne i trafiają do Biuletynu Zamówień Publicznych, ale sposób ich prezentacji nie pomaga w zrozumieniu, co dzieje się w konkretnym miejscu. BZP pokazuje ogłoszenia jako listę z wyszukiwarką, więc żeby sprawdzić, kto ogłasza przetargi w danej okolicy i jak często, trzeba przeglądać je jedno po drugim.

Dotyczy to różnych grup. Mała firma budowlana czy informatyczna chce wiedzieć, którzy zamawiający w jej regionie regularnie zlecają prace z jej branży. Dziennikarz lokalny sprawdza, czym zajmują się instytucje w jego mieście. Mieszkaniec chce zobaczyć, na co wydawane są publiczne pieniądze w jego gminie. Każda z tych osób myśli o zamówieniach przez pryzmat miejsca, a dostępne narzędzia tego nie ułatwiają.

Dodatkową przeszkodą jest jakość samych danych. Formularz ogłoszenia zawiera pola na adres i miejsce realizacji zamówienia, ale zamawiający często wypełniają je niepoprawnie albo wcale — zamiast tego lokalizację podają tylko w treści ogłoszenia, w opisie przedmiotu zamówienia lub w tytule. Przez to filtrowanie po regionie na podstawie samych rubryk formularza pomija część ogłoszeń albo wskazuje niewłaściwe miejsce.

Bipmapa pobiera ogłoszenia z BZP przez publiczne API platformy e-Zamówienia, wyodrębnia z nich zamawiających i ustala ich położenie — z rubryk formularza, a gdy są puste lub niepełne, z treści ogłoszenia. Lokalizacje ustalone w przybliżeniu są na mapie wyraźnie oznaczone.

## Funkcje

| Obszar          | Co robi                                                               |
| --------------- | --------------------------------------------------------------------- |
| Strona startowa | punkt wejścia aplikacji obsługiwany przez `DefaultController`         |
| Routing         | dopasowanie adresu do kontrolera, w tym parametrów (`/projects/{id}`) |
| Obsługa błędów  | widok 404 i status HTTP 404 dla nieznanych adresów                    |

Planowane:

- mapa zamawiających z filtrami (data, typ ogłoszenia, branża, województwo, słowo kluczowe)
- profil zamawiającego z listą jego ogłoszeń
- konto użytkownika z obserwowaniem zamawiających i powiadomieniami e-mail o nowych ogłoszeniach
- panel administratora do importu danych i korekty lokalizacji
- codzienny import ogłoszeń z BZP (CRON)

## Tech stack

| Warstwa        | Technologie           |
| -------------- | --------------------- |
| Backend        | PHP 8.3               |
| Frontend       | HTML, CSS, JavaScript |
| Baza danych    | PostgreSQL 17         |
| Serwer HTTP    | Nginx                 |
| Poczta (dev)   | Mailpit               |
| Konteneryzacja | Docker Compose        |

## Szybki start

```bash
git clone https://github.com/Gekimaru/bipmapa.git
cd bipmapa
cp .env.example .env
docker compose up --build -d
```

| Usługa     | Adres                 |
| ---------- | --------------------- |
| Aplikacja  | http://localhost:8080 |
| Mailpit    | http://localhost:8025 |
| pgAdmin    | http://localhost:5050 |
| PostgreSQL | localhost:5433        |

## Struktura projektu

| Katalog / plik       | Zawartość                                               |
| -------------------- | ------------------------------------------------------- |
| `public/index.php`   | front controller — jedyny punkt wejścia, definicje tras |
| `public/views/`      | widoki HTML                                             |
| `src/controllers/`   | kontrolery, dziedziczą po `AppController`               |
| `Routing.php`        | router                                                  |
| `docker/`            | konfiguracja nginx, PHP i bazy danych                   |
| `docker/db/init.sql` | schemat bazy tworzony przy pierwszym uruchomieniu       |

## Zasady developmentu

- Praca bezpośrednio na gałęzi `main`.
- Małe, logiczne commity (`feat:`, `fix:`, `docs:`, `refactor:`, `chore:`).
- Sekrety wyłącznie w `.env`.

## Licencja i źródła danych

- Kod: [MIT](LICENSE)
- Dane o zamówieniach: Biuletyn Zamówień Publicznych, Urząd Zamówień Publicznych, pobierane z platformy [e-Zamówienia](https://ezamowienia.gov.pl)
- Mapa i geokodowanie: © [OpenStreetMap](https://www.openstreetmap.org/copyright) contributors, licencja ODbL

## Autorzy

| Osoba         | Zakres                                                                                                    |
| ------------- | --------------------------------------------------------------------------------------------------------- |
| Grzegorz Cios | całość projektu: import danych, backend (PHP, MVC), frontend (HTML, CSS, JS), baza danych, Docker, deploy |
