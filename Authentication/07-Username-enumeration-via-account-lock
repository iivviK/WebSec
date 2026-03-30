```md
LAB: Username enumeration via account lock  
Kategoria: Authentication  
Utworzono: 2026-03-30 21:50 (Europe/Amsterdam)

---

1. CEL TESTU

   Sprawdzenie czy aplikacja ujawnia istnienie użytkowników poprzez różnice w zachowaniu podczas logowania.

   Testowany mechanizm:
   - ochrona przed brute force (account lock / rate limit)

---

2. KONTEKST APLIKACJI

   Prosta aplikacja webowa z formularzem logowania.

   - typ aplikacji: web (login form)
   - wymagania logowania: username + password
   - endpoint:
     - POST /login
   - inne:
     - aplikacja implementuje mechanizm blokady po kilku nieudanych próbach

---

3. OBSERWACJA

   Początkowo:

   - każdy request:
     - `Invalid username or password`
     - status: 200
     - brak różnic między użytkownikami

   Po kilku próbach dla jednego username:

   - pojawia się inny komunikat:
     - `You have made too many incorrect login attempts`
   - długość odpowiedzi się zmienia

   Wniosek:
   - tylko jeden user wywołuje zmianę zachowania

   Narzędzia użyte w tym etapie:

   Proxy  
   Repeater  
   Intruder  
   Python (custom script)

---

4. HIPOTEZA

   Aplikacja utrzymuje licznik nieudanych prób tylko dla istniejących użytkowników.

   Po przekroczeniu limitu:
   - zmienia odpowiedź (account lock)

   ➜ umożliwia to enumerację użytkowników poprzez wymuszenie zmiany stanu.

---

5. ANALIZA MECHANIZMU

   Backend:

   - sprawdza czy user istnieje
   - jeśli TAK:
     - zwiększa licznik prób
     - po kilku próbach → blokada
     - zwraca inny komunikat
   - jeśli NIE:
     - brak licznika
     - brak zmiany odpowiedzi

   Klucz:

   - mechanizm jest **stateful**
   - zmiana stanu następuje tylko dla valid usera

---

6. REPRODUCTION / EXPLOIT

   Step 1 – Username enumeration

   - wysyłanie wielu prób logowania dla każdego usera
   - ten sam password
   - obserwacja różnic w response

```

username=app&password=test123

```

Wynik:
- wykryto usera:
  `app`

---

Step 2 – Password brute force

- brute force dla jednego usera
- kontrola tempa (lock po ~3 próbach)

Strategia:

```

2 próby → wait → 2 próby → wait

```

Detekcja:
- HTTP 302 redirect = sukces

---

Step 3 – Login

```

username: app
password: <FOUND>

```

➜ dostęp do konta → LAB SOLVED

---

7. IMPACT

- enumeracja użytkowników
- możliwość targeted brute force
- zwiększona skuteczność credential stuffing
- ujawnienie struktury użytkowników

---

8. DEBUGGING / PITFALLS

Problemy:

- brak różnicy przy pojedynczym request
- błędne założenie liczby prób (3 vs 5)
- false positive przy analizie treści
- blokada konta wpływająca na brute

Narzędzia:

- Turbo Intruder:
  - problemy z konfiguracją (%s vs §)
- Intruder:
  - wolny, ale stabilny
- Python:
  - zbyt szybki → trigger lock

Rozwiązania:

- batch requests
- cooldown (60s)
- detekcja redirect zamiast treści

---

9. MENTAL MODEL / PATTERN

STATE-BASED USER ENUMERATION

Jeśli aplikacja:
- zmienia odpowiedź po kilku próbach
- tylko dla istniejącego użytkownika

➜ można wymusić zmianę stanu i wykryć różnicę

---

Sygnały ostrzegawcze:

- brak różnic w pojedynczym request
- zmiana odpowiedzi po wielu próbach
- komunikaty typu:
  - "too many attempts"
  - "account locked"
- różnice w długości response

---
```
