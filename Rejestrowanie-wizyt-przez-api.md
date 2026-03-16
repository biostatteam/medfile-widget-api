# Specyfikacja trasy do rejestrowania wizyty

## Żądanie HTTP

```
POST https://rejestracja.medfile.pl/rejestracja/order
Accept: application/json
Content-Type: application/x-www-form-urlencoded
```

## Parametry żądania HTTP POST

| Parametr | Typ wartości | Komentarz |
| :---- | :---- | :---- |
| reg | string | Przyjmuje wartość różną od pustej np. „1”, „true”. Brak wartości oznacza brak zgody na regulamin. |
| terms | string | Przyjmuje wartość różną od pustej np. „1”, „true”. Brak wartości oznacza brak zgody na politykę prywatności. |
| chosen_user | string | UUID lekarza |
| chosen_service | string | UUID usługi |
| chosen_facility | string | UUID lokalizacji – opcjonalne  |
| office_id | Integer | ID gabinetu – wymagane jeżeli nie podano chosen_facility |
| chosen_date | string | Termin wizyty tzw. slot wybrany przez pacjenta w formacie „Y-m-d H:i” np. „2025-01-01 10:30” |
| email | string | Adres e-mail pacjenta |
| phone | string | Numer telefonu pacjenta (bez kierunku kraju) |
| phone_direction | string | Numer kierunkowy kraju dla telefonu pacjenta np. +48 |
| patient_name | string | Imię pacjenta |
| patient_lastname | string | Nazwisko pacjenta |
| patient_pesel | string | PESEL pacjenta – wymagany tylko jeżeli tak zdefiniowano w ustawieniach systemu rejestracji online |
| patient_zip | string | Adres pacjenta – Kod pocztowy – wymagany tylko jeżeli tak zdefiniowano w ustawieniach systemu rejestracji online |
| patient_city | string | Adres pacjenta – Miasto – wymagany tylko jeżeli tak zdefiniowano w ustawieniach systemu rejestracji online |
| patient_street | string | Adres pacjenta – Ulica – wymagany tylko jeżeli tak zdefiniowano w ustawieniach systemu rejestracji online |
| patient_home_number | string | Adres pacjenta – Numer domu – wymagany tylko jeżeli tak zdefiniowano w ustawieniach systemu rejestracji online |
| patient_home_place | string | Adres pacjenta – Numer mieszkania – opcjonalny zawsze |
| source | string | Opcjonalne, możliwość wpisania źródła np. „halodoctor”. |
| message | string | Treść pola uwagi – opcjonalne |
| discount_uuid | string | UUID Kodu rabatowego – opcjonalne  |
| patient_language | string | Kod domyślnego języka pacjenta – pl, en, ua itd. \- opcjonalne |
| referer | string | Adres z którego pacjent trafił do formularza rezerwacji – do analityki – opcjonalny |
| allow_payment | string | Przyjmuje wartość różną od pustej np. „1”, „true”. Oznacza, że system powinien utworzyć link do płatności, jeżeli usługa w wizycie tego wymaga i zwrócić go jako paymentUrl. |

## Uwaga

Wizyty zarejestrowane przez powyższą trasę automatycznie mają wyłączone powiadomienia z linkiem do płatności, nawet jeżeli ustawią Państwo allow_payment=1. Odpowiedzialność za poinformowanie pacjenta o linku przechodzi na integratora.

## Odpowiedź na żądanie

W przypadku błędu:

```json
{
  "msg": "",
  "error": "Akceptacja regulaminu jest obowiązkowa",
  "output": {
    "noLogin": true
  }
}
```

lub:  

```json
{
  "msg": "",
  "error": "Godzina jest już zajęta.",
  "output": {
    "noLogin": true
  }
}
```

W przypadku powodzenia:

```json
{
  "msg": "",
  "error": "",
  "output": {
    "noLogin": true,
    "doctorUrl": "https://{link do pokoju telekonsultacji dla Lekarza}", 
    "patientUrl": "https://{link do pokoju telekonsultacji dla Pacjenta}",
    "cancelUrl": "https://{link do anulowania wizyty}",
    "paymentUrl": "https://{link do opłacenia wizyty}",
    "visit": {
      "start": "2025-02-06 10:00:00",
      "end": "2025-02-06 10:30:00",
      "date": "2025-02-06",
      "time": "10:00:00",
      "to": "10:30:00",
      "office_id": 81,
      "update_date": "2025-02-06 00:36:58",
      "add_date": "2025-02-06 00:36:58",
      "type": "default",
      "remote": 1,
      "patient": "Jan Kowalski",
      "uuid": "ab1b6a03-fcff-bdcc-521e-6ea003e9ac57"
    }
  }
}
```
