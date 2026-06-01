```md
LAB: Inconsistent Security Controls
Kategoria: Business Logic
Utworzono: 2026-06-01 21:11 (Europe/Amsterdam)

---

1. SCAN (Entry & Signals)

   Endpointy:

   * POST /register
   * POST /login
   * POST /my-account/change-email
   * GET /admin

   Obiekty biznesowe:

   * User
   * Email
   * Role / Employee Status

   Sygnały:

   * "Admin interface only available if logged in as a DontWannaCry user"
   * "If you work for DontWannaCry, please use your @dontwannacry.com email address"

---

2. KONTEKST APLIKACJI

   Dostęp do panelu administracyjnego powinien być ograniczony do pracowników firmy DontWannaCry.

   Aplikacja umożliwia:

   * rejestrację konta
   * aktywację konta
   * logowanie
   * zmianę adresu email

---

3. OBSERWACJA

   Komunikaty aplikacji wielokrotnie wskazywały, że dostęp do funkcji administracyjnych zależy od domeny email.

   Początkowo analiza została skierowana na poziom HTTP i potencjalne różnice w interpretacji danych wejściowych.

   Kluczowa funkcjonalność znajdowała się jednak w zwykłym workflow użytkownika.

---

4. HIPOTEZA

   Status pracownika zależy od adresu email przypisanego do konta.

   Kontrole związane z rejestracją i późniejszą modyfikacją emaila mogą wykorzystywać różne reguły walidacji.

---

5. ANALIZA MECHANIZMU

   Podczas rejestracji aplikacja kontroluje domenę email.

   Po aktywacji konta użytkownik może zmienić adres email.

   Mechanizm zmiany emaila nie egzekwuje tych samych reguł bezpieczeństwa co proces rejestracji.

   Powoduje to niespójność pomiędzy dwoma kontrolami bezpieczeństwa dotyczącymi tego samego obiektu biznesowego.

---

6. REPRODUCTION / EXPLOIT

   1. Utwórz zwykłe konto użytkownika.
   2. Aktywuj konto.
   3. Zaloguj się.
   4. Przejdź do ustawień konta.
   5. Zmień email na adres wykorzystujący domenę @dontwannacry.com.
   6. Odśwież aplikację.
   7. Uzyskaj dostęp do panelu administracyjnego.
   8. Usuń użytkownika carlos.

---

7. IMPACT

   Dowolny użytkownik może uzyskać funkcje administracyjne przeznaczone wyłącznie dla pracowników firmy.

   Prowadzi to do eskalacji uprawnień oraz pełnego przejęcia funkcji administracyjnych.

---

8. DEBUGGING / PITFALLS

   Główna pułapka:

   Skupienie się na implementacji HTTP przed wyczerpaniem analizy workflow.

   Błędny kierunek:

   * analiza nagłówków
   * analiza Referer
   * analiza sposobu parsowania emaila

   Właściwy kierunek:

   * analiza wszystkich miejsc, w których użytkownik może modyfikować email

---

9. MENTAL MODEL / PATTERN

   Jeżeli decyzja biznesowa zależy od konkretnego obiektu (email, rola, kupon, saldo itd.), należy najpierw odnaleźć wszystkie miejsca umożliwiające modyfikację tego obiektu.

   Dopiero po wyczerpaniu warstwy workflow warto schodzić na poziom HTTP i implementacji.

---

10. WHY IT WORKS

Rejestracja oraz zmiana emaila wykorzystują niespójne reguły bezpieczeństwa dla tego samego obiektu biznesowego.

Użytkownik może więc przejść kontrolę w jednym workflow i ominąć ją w drugim.

```
