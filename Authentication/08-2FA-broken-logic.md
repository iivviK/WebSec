```md
LAB: 2FA broken logic  
Kategoria: Authentication  
Utworzono: 2026-03-31 22:55 (Europe/Amsterdam)

---

1. CEL TESTU

   Sprawdzenie czy mechanizm 2FA jest poprawnie egzekwowany oraz czy wszystkie etapy uwierzytelniania są ze sobą spójnie powiązane.

   Testowany mechanizm:
   - multi-step authentication (login + 2FA)

---

2. KONTEKST APLIKACJI

   Aplikacja webowa z klasycznym flow logowania.

   - typ aplikacji: web (login form + 2FA)
   - wymagania logowania:
     - username + password
     - kod 2FA (4 cyfry)
   - endpointy:
     - POST /login
     - GET /login2
     - POST /login2
   - inne:
     - aplikacja używa dodatkowego cookie `verify` do obsługi 2FA

---

3. OBSERWACJA

   Po poprawnym loginie:

   - response:
     - HTTP 302 → /login2
     - Set-Cookie:
       - session=<value>
       - verify=wiener

   Kluczowe:

   - session ustawiana PRZED ukończeniem 2FA
   - dodatkowe cookie:
     - `verify=<username>`

---

   GET /login2:

   - formularz:
     - `mfa-code` (4-digit)
   - brak dodatkowej walidacji kontekstu użytkownika

---

   Manipulacja:

   - zmiana cookie:
     - verify=wiener → verify=carlos

   Wynik:

   - backend akceptuje zmienione cookie
   - zwraca formularz 2FA dla innego użytkownika

---

4. HIPOTEZA

   Mechanizm 2FA nie jest powiązany z session.

   Backend:
   - identyfikuje użytkownika na podstawie `verify`
   - nie sprawdza czy `verify` odpowiada session

   ➜ możliwa desynchronizacja tożsamości

---

5. ANALIZA MECHANIZMU

   Backend:

   - po loginie:
     - tworzy session
     - ustawia verify=<username>
   - podczas GET /login2:
     - odczytuje `verify`
     - NIE sprawdza powiązania z session
   - podczas POST /login2:
     - weryfikuje kod dla usera z `verify`

---

   Klucz:

   - brak spójności między:
     - session (user A)
     - verify (user B)

   ➜ możliwa zmiana kontekstu użytkownika w trakcie flow

---

6. REPRODUCTION / EXPLOIT

   Step 1 – Login jako zwykły user

```
POST /login

username=wiener&password=peter
```

   Response:
   - 302 → /login2
   - Set-Cookie:
     - session=...
     - verify=wiener

---

   Step 2 – Manipulacja cookie

```
Cookie:
session=<value>
verify=carlos
```

---

   Step 3 – Brute force 2FA

   - brak rate limit / lock
   - przestrzeń: 0000–9999

```
POST /login2

mfa-code=0000–9999
```

   Detekcja:
   - HTTP 302 redirect = sukces

---

   Step 4 – Login

```
username: carlos
mfa-code: <FOUND>
```

   ➜ dostęp do konta → LAB SOLVED

---

7. IMPACT

- przejęcie konta dowolnego użytkownika
- brak konieczności znajomości hasła
- obejście mechanizmu 2FA
- możliwość pełnej automatyzacji ataku

---

8. DEBUGGING / PITFALLS

Problemy:

- skupienie się na brute force zamiast analizie logiki
- brak analizy Set-Cookie po loginie
- ignorowanie dodatkowego cookie `verify`
- brak testu manipulacji kontekstu użytkownika

---

Narzędzia:

- Proxy  
- Repeater  
- Intruder (opcjonalnie)  
- Python (custom brute-force script)

---

Rozwiązania:

- analiza wszystkich cookie po loginie
- identyfikacja źródeł tożsamości (session vs verify)
- test spójności między etapami auth
- manipulacja cookie w Repeater

---

9. MENTAL MODEL / PATTERN

BROKEN AUTH FLOW BINDING

Jeśli aplikacja:
- ma wieloetapowe uwierzytelnianie
- używa różnych identyfikatorów (session, cookie, param)
- nie wiąże ich ze sobą

➜ możliwa desynchronizacja tożsamości

---

Klucz:

> "Czy wszystkie etapy auth odnoszą się do tego samego usera?"

---

Sygnały ostrzegawcze:

- session ustawiana przed zakończeniem 2FA
- dodatkowe cookies typu:
  - verify
  - user
  - email
- brak integrity (plain value, brak podpisu)
- możliwość manipulacji po stronie klienta

---
```
