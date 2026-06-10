```md
LAB: Inconsistent handling of exceptional input
Kategoria: Business Logic
Utworzono: 2026-06-10 11:26 (Europe/Amsterdam)

---

1. SCAN (Entry & Signals)

Endpointy:

* POST /register
* GET /confirm
* GET /my-account
* GET /admin

Obiekty biznesowe:

* User Account
* Email Address
* Email Verification
* Employee Status
* Admin Access

Sygnały:

* panel /admin istnieje, ale jest niedostępny dla zwykłych użytkowników
* komunikat zachęcający pracowników do używania adresów @dontwannacry.com
* możliwość podania bardzo długiego adresu email
* adres email widoczny w profilu jest krótszy niż adres użyty podczas rejestracji
* różne komponenty aplikacji wykorzystują ten sam email do różnych decyzji biznesowych

---

2. KONTEKST APLIKACJI

Aplikacja wykorzystuje adres email do określania poziomu uprawnień użytkownika.

Proces rejestracji obejmuje:

* utworzenie konta
* wysłanie wiadomości aktywacyjnej
* potwierdzenie adresu email
* określenie statusu użytkownika

Założenie biznesowe:

* użytkownicy domeny @dontwannacry.com są traktowani jako pracownicy
* pracownicy uzyskują dostęp do panelu administracyjnego

---

3. OBSERWACJA

Aplikacja pozwala na podanie bardzo długiego adresu email.

Po rejestracji adres widoczny w sekcji My Account zostaje obcięty do określonej długości.

Jednocześnie wiadomość aktywacyjna jest dostarczana na pełny adres użyty podczas rejestracji.

Oznacza to, że różne komponenty systemu wykorzystują różne reprezentacje tej samej wartości.

---

4. HIPOTEZA

Komponent odpowiedzialny za dostarczanie wiadomości email wykorzystuje pełną wartość adresu.

Komponent odpowiedzialny za określanie statusu użytkownika wykorzystuje adres po truncation.

Jeżeli po obcięciu adres będzie kończył się na:

@dontwannacry.com

system może błędnie zaklasyfikować użytkownika jako pracownika organizacji.

---

5. ANALIZA MECHANIZMU

Użytkownik rejestruje konto przy użyciu adresu:

[AAAA....AAAA@dontwannacry.com.exploit-server.net](mailto:AAAA....AAAA@dontwannacry.com.exploit-server.net)

Komponent wysyłki wiadomości interpretuje pełny adres:

[AAAA....AAAA@dontwannacry.com.exploit-server.net](mailto:AAAA....AAAA@dontwannacry.com.exploit-server.net)

i poprawnie dostarcza wiadomość do domeny kontrolowanej przez atakującego.

Podczas zapisu lub późniejszego przetwarzania adres zostaje obcięty.

Po truncation aplikacja interpretuje adres jako:

[AAAA....AAAA@dontwannacry.com](mailto:AAAA....AAAA@dontwannacry.com)

Logika biznesowa wykorzystuje obciętą wartość podczas określania przynależności do organizacji.

Powstaje interpretation mismatch:

* komponent dostarczający wiadomość widzi pełny adres
* komponent nadający uprawnienia widzi adres po truncation

Oba komponenty podejmują decyzje na podstawie różnych reprezentacji tej samej danej.

---

6. REPRODUCTION / EXPLOIT

6.1. Otwórz aplikację i przejdź do Target → Site Map.

6.2. Uruchom Content Discovery.

   a. Kliknij prawym przyciskiem na domenę aplikacji.
   b. Wybierz Engagement Tools → Discover Content.
   c. Uruchom skanowanie.

6.3. Potwierdź odkrycie endpointu /admin.

6.4. Spróbuj uzyskać dostęp do /admin.

    a. Zaobserwuj komunikat wskazujący, że dostęp posiadają użytkownicy domeny @dontwannacry.com.

6.5. Otwórz stronę rejestracji.

    a. Zwróć uwagę na komunikat dla pracowników korzystających z adresów @dontwannacry.com.

6.6. Otwórz klienta poczty.

    a. Zanotuj kontrolowaną domenę email przypisaną do laboratorium.

6.7. Zarejestruj konto przy użyciu bardzo długiego adresu email.

    a. Użyj ponad 200 znaków przed znakiem @.
    b. Potwierdź rejestrację.
    c. Zaloguj się.
    d. Przejdź do My Account.
    e. Potwierdź, że adres email został obcięty.

6.8. Wyloguj się.

6.9. Przygotuj nowy adres email w formacie:

    [long-string]@dontwannacry.com.[controlled-domain]

6.10. Dobierz długość części [long-string].

    a. Zwiększaj długość ciągu przed @.
    b. Doprowadź do sytuacji, w której znak "m" w @dontwannacry.com będzie ostatnim znakiem zachowanym po truncation.
    c. Zweryfikuj wynik w My Account.

6.11. Zarejestruj nowe konto.

    a. Potwierdź adres email przy użyciu otrzymanej wiadomości.
    b. Zaloguj się do aplikacji.

6.12. Potwierdź uzyskanie dostępu do panelu administracyjnego.

6.13. Przejdź do /admin.

6.14. Usuń użytkownika carlos.

6.15. Potwierdź rozwiązanie laboratorium.

---

7. IMPACT

Atakujący może:

* uzyskać nieautoryzowane uprawnienia
* podszyć się pod użytkownika zaufanej domeny
* obejść mechanizmy kontroli dostępu
* wykonywać operacje administracyjne
* naruszyć integralność procesu autoryzacji

---

8. DEBUGGING / PITFALLS

Główna pułapka:

Skupienie się na długości adresu email zamiast na decyzjach podejmowanych przez system.

Błędny kierunek:

* liczenie znaków bez zrozumienia celu
* traktowanie truncation jako głównej podatności
* analiza składni adresu email
* skupienie się wyłącznie na payloadzie

Właściwy kierunek:

* identyfikacja komponentów wykorzystujących email
* analiza momentu zmiany reprezentacji danych
* ustalenie, które decyzje są podejmowane na pełnej wartości
* ustalenie, które decyzje są podejmowane na wartości po truncation

---

9. MENTAL MODEL / PATTERN

Pattern:

Interpretation Mismatch

Core Idea:

Ten sam input jest interpretowany przez różne komponenty aplikacji w odmienny sposób.

Pytania przewodnie:

* Czy wszystkie komponenty widzą tę samą wartość?
* Czy wszystkie decyzje bezpieczeństwa wykorzystują tę samą reprezentację danych?
* Czy istnieje etap truncation, normalizacji lub transformacji danych?
* Czy różne komponenty podejmują decyzje na podstawie różnych wersji tego samego inputu?

Typowe miejsca występowania:

* email
* username
* URL
* Host Header
* cookies
* filename
* reverse proxy
* backend services

---

10. WHY IT WORKS

Komponent odpowiedzialny za dostarczanie wiadomości email oraz komponent odpowiedzialny za określanie poziomu uprawnień operują na różnych reprezentacjach tego samego adresu.

Wiadomość aktywacyjna zostaje dostarczona na pełny adres kontrolowany przez atakującego.

Jednocześnie logika autoryzacji wykorzystuje obciętą wersję adresu, która wygląda jak prawidłowy adres pracownika domeny @dontwannacry.com.

Interpretation mismatch pomiędzy komponentami prowadzi do błędnego przypisania uprawnień administracyjnych.
```
