```md
LAB: Weak isolation on dual-use endpoint
Kategoria: Business Logic
Utworzono: 2026-06-10 17:45 (Europe/Amsterdam)

---

1. SCAN (Entry & Signals)

Endpointy:

* POST /my-account/change-password
* GET /my-account
* GET /admin

Obiekty biznesowe:

* User Account
* Password
* Authentication State
* Administrator Account

Sygnały:

* endpoint zmiany hasła przyjmuje username jako parametr
* endpoint przyjmuje current-password oraz new-password
* operacja zmiany hasła wykonywana jest przez pojedynczy endpoint
* parametr current-password wygląda na obowiązkowy element procesu
* użytkownik kontroluje username przesyłany do backendu

---

2. KONTEKST APLIKACJI

Aplikacja umożliwia użytkownikom zmianę hasła po zalogowaniu.

Proces obejmuje:

* identyfikację użytkownika
* weryfikację aktualnego hasła
* zapis nowego hasła

Założenie biznesowe:

* użytkownik może zmienić wyłącznie własne hasło
* przed zmianą hasła należy potwierdzić znajomość aktualnego hasła

---

3. OBSERWACJA

Request zmiany hasła zawiera:

* username
* current-password
* new-password

Po usunięciu parametru current-password operacja nadal zostaje wykonana.

Jednocześnie możliwa jest modyfikacja wartości username.

Oznacza to, że backend nie traktuje current-password jako wymaganego warunku wykonania operacji.

---

4. HIPOTEZA

Backend wykorzystuje current-password wyłącznie wtedy, gdy parametr jest obecny.

Brak parametru nie zatrzymuje procesu zmiany hasła.

Jeżeli backend ufa również wartości username dostarczanej przez klienta, możliwa może być zmiana hasła dowolnego użytkownika.

---

5. ANALIZA MECHANIZMU

Normalny przepływ wygląda następująco:

1. Użytkownik wysyła username.
2. Użytkownik wysyła current-password.
3. Użytkownik wysyła new-password.
4. Backend wykonuje zmianę hasła.

W praktyce backend zachowuje się inaczej.

Po usunięciu current-password:

* request nadal jest akceptowany,
* operacja nadal jest wykonywana.

Powstaje rozjazd pomiędzy:

* założeniami procesu biznesowego,
* rzeczywistą implementacją backendu.

Mechanizm przypomina warunkową walidację:

* jeżeli parametr istnieje → wykonaj sprawdzenie,
* jeżeli parametr nie istnieje → pomiń sprawdzenie.

---

6. REPRODUCTION / EXPLOIT

6.1. Zaloguj się jako zwykły użytkownik.

6.2. Przejdź do My Account.

6.3. Zmień hasło.

6.4. Przechwyć request:

POST /my-account/change-password

6.5. Wyślij request do Repeater.

6.6. Usuń parametr:

current-password

6.7. Wyślij request.

6.8. Potwierdź, że operacja nadal zostaje wykonana.

6.9. Zmień parametr:

username=administrator

6.10. Wyślij request ponownie.

6.11. Ustaw nowe hasło administratora.

6.12. Wyloguj się.

6.13. Zaloguj się jako administrator.

6.14. Przejdź do panelu administracyjnego.

6.15. Usuń użytkownika carlos.

6.16. Potwierdź rozwiązanie laboratorium.

---

7. IMPACT

Atakujący może:

* zmieniać hasła innych użytkowników
* przejmować konta
* eskalować uprawnienia
* przejmować konto administratora
* wykonywać operacje administracyjne

---

8. DEBUGGING / PITFALLS

Główna pułapka:

Założenie, że parametr obecny w formularzu jest automatycznie wymagany przez backend.

Błędny kierunek:

* analiza wyłącznie wartości parametrów
* skupienie się na payloadach
* ignorowanie możliwości usunięcia parametrów

Właściwy kierunek:

* testowanie obecności parametrów
* testowanie pustych wartości
* testowanie brakujących wartości
* identyfikacja założeń backendu

---

9. MENTAL MODEL / PATTERN

Pattern Candidate:

Required Parameter Assumption

Core Idea:

Backend zakłada obecność parametru dostarczanego przez frontend, ale nie wymusza jego istnienia.

Pytania przewodnie:

* Czy parametr jest naprawdę wymagany?
* Co stanie się po jego usunięciu?
* Czy backend waliduje wartość czy tylko jej obecność?
* Czy operacja zostanie wykonana mimo braku danych?

Typowe miejsca występowania:

* current-password
* csrf
* otp
* verification-code
* confirmation-token
* role
* account-id

---

10. WHY IT WORKS

Backend nie wymusza obecności current-password przed wykonaniem operacji zmiany hasła.

Proces biznesowy zakłada konieczność potwierdzenia znajomości aktualnego hasła, jednak implementacja nie egzekwuje tego wymagania.

Dodatkowo backend wykorzystuje username dostarczany przez klienta do określenia konta docelowego.

Połączenie:

* pominięcia walidacji current-password
* zaufania do username

umożliwia zmianę hasła administratora bez znajomości jego aktualnego hasła.

```
