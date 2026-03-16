# Specyfikacja formularza rejestracji online

## Wstęp

Proces rejestracji może zostać zrealizowany przez formularz dostępny w Medfile. 
Formularz rejestracji potrzebuje informacji nt lekarza i terminu wizyty ale posiada również dodatkowe opcjonalne parametry.

Adres formularza rejestracji to: [https://rejestracja.med-platform.pl/rejestracja/register/](https://rejestracja.med-platform.pl/rejestracja/register/)

## Portal pacjenta (jeśli dotyczy)

Portal pacjenta jest domyślnie skonfigurowany na usługę [https://pacjent.halodoctor.pl/](https://pacjent.halodoctor.pl/). Jeżeli klient posiada własną usługę indywidualnego portalu pacjenta to może go włączyć w formularzu rejestracji online parametrem "ppae=1" lub "/ppae/1/".

Domyślnie w formularzu rejestracji pierwszym krokiem jest logowanie do portalu pacjenta. Można jednak to zachowanie zmienić poprzez przesłanie zgłoszenia do Medfile. Zgłoszenie proszę wysłać na adres [integracja@medfile.pl](mailto:integracja@medfile.pl).

Style, logotypy, regulaminy i inne materiały skonfigurowane w indywidualnym portalu pacjenta są automatycznie aplikowane do formularza rejestracji.

Jeżeli nie posiadają Państwo usługi indywidualnego portalu pacjenta ale są nim zainteresowani to zapraszamy do kontaktu pod adresem [biuro@medfile.pl](mailto:biuro@medfile.pl). Opis usługi znajduje się tutaj [https://www.medfile.pl/portal-pacjenta](https://www.medfile.pl/portal-pacjenta).

## Ustawienia formularza rejestracji

Formularz rejestracji posiada szereg dodatkowych ustawień takich jak wymaganie numeru PESEL, własna treść zgody, regulamin, dodatkowy krok z informacjami itd. 

Szczegółowe informacje znajdą Państwo na stronie [Rejestracja pacjentów, ustawienia rejestracji online | e-Recepta | program do gabinetu](https://www.medfile.pl/wideotutoriale/rejestracja-pacjentow-ustawienia-rejestracji-online) oraz tutaj [Rejestracja pacjentów, ustawienia rejestracji online](https://www.medfile.pl/moje-konto-rejestracja-pacjentow-ustawienia-rejestracji-online). W przypadku dodatkowych pytań zachęcamy do kontaktu pod adresem [biuro@medfile.pl](mailto:biuro@medfile.pl).

## Parametry formularza

Wszystkie parametry przekazujemy do formularza poprzez adres URL żądania. 

Dostępne są dwie metody:

1. Format dynamiczny: "/klucz/wartość/klucz-2/wartość-2/" Przykład: [https://rejestracja.med-platform.pl/rejestracja/register/uuid/a6ae1130-cc00-3173-7bf1-cfda1e331d51/source/dedicated/](https://rejestracja.med-platform.pl/rejestracja/register/uuid/a6ae1130-cc00-3173-7bf1-cfda1e331d51/source/dedicated/) 

2. Zwykły query string "?klucz=wartosc&klucz-2=wartosc-2" Przykład
   [https://rejestracja.med-platform.pl/rejestracja/register/?uuid=a6ae1130-cc00-3173-7bf1-cfda1e331d51&source=dedicated](https://rejestracja.med-platform.pl/rejestracja/register/?uuid=a6ae1130-cc00-3173-7bf1-cfda1e331d51&source=dedicated)

| Parametr | Opis | Wymagany |
| :---- | :---- | :---- |
| uuid | Identyfikator specjalisty do którego pacjent się umawia w formacie UUID np. a6ae1130-cc00-3173-7bf1-cfda1e331d51 | Tak |
| remote | Potrzebny tylko gdy nie wskazujemy konkretnej usługi. 0 - wizyta stacjonarna, 1 - e-wizyta | Tak, jeżeli nie podano identyfikatora usługi |
| service | Identyfikator usługi na którą umawia się pacjent w formacie UUID np. 565ca876-a357-e70e-d34e-64109e2f4fe1 | Tak, jeżeli nie podano zmiennej "remote". |
| o | Identyfikatory gabinetu lub gabinetów do których pacjent chce się umówić. Wartość przesyłamy w postaci listy identyfikatorów liczbowych oddzielonych przecinkiem np. 1,2,3 | Nie |
| facility | Identyfikator lokalizacji w formacie UUID. Umożliwia wskazanie konkretnej lokalizacji zamiast listy gabinetów. Opcjonalnie, ważniejsze od parametru "o". | Nie |
| slot | Dokładny dzień i godzina na którą pacjent chce się umówić w formacie: YYYY-mm-dd HH:ii. Przykład: "2024-04-26 08:00" oznacza 26 kwietnia 2024 roku o godzinie 08:00 | Nie, brak parametru sprowokuje formularz do pokazania propozycji najbliższego terminu. (chyba że ustawimy parametr fs=1 wtedy zamiast pokazywać wyboru terminu, system samodzielnie wybierze najszybszy termin) |
| ppae | Włącz lub wyłącz synchronizację formularza rejestracji z indywidualnym portalem pacjenta. Jeżeli klient posiada usługę portalu pacjenta może użyć parametru ppae aby go włączyć w procesie rejestracji. 1 - włącz synchronizację i style z portalu pacjenta, 0 - wyłącz synchronizację i style z portalu pacjenta | Nie, domyślnie 0 |
| source | Wartość predefiniowana: "dedicated" - specjalny tryb dla klientów z indywidualnymi widżetami, tylko ta wartość umożliwia poprawne działanie przycisku "wróć" oraz "anuluj". | Tak |
| grouped_visit | Identyfikator wizyty grupowej do której pacjent chce dołączyć. Identyfikator w formacie UUID. | Nie |
| s | Czy pozwolić na zmianę usługi? 1 - tak, 0 - nie | Nie, domyślnie 0 |
| fs | Czy automatycznie wybrać pierwszy wolny termin? 1 - tak, 0 - nie | Nie, domyślnie 0 |

