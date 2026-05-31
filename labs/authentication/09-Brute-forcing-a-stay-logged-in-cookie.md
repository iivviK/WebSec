```md
LAB: Brute-forcing a stay-logged-in cookie  
Kategoria: Authentication  
Utworzono: 2026-04-01 22:28 (Europe/Amsterdam)

---

1. CEL TESTU

   Sprawdzenie czy mechanizm „stay logged in” jest bezpieczny oraz czy dane używane do automatycznego uwierzytelnienia są poprawnie chronione.

   Testowany mechanizm:
   - persistent login (remember me / stay logged in)

---

2. KONTEKST APLIKACJI

   Aplikacja webowa z klasycznym login flow.

   - typ aplikacji: web (login form)
   - wymagania logowania:
     - username + password
   - opcjonalna funkcja:
     - checkbox „stay logged in”
   - endpointy:
     - POST /login
     - GET /my-account
   - inne:
     - aplikacja ustawia dodatkowe cookie `stay-logged-in`

---

3. OBSERWACJA

   Po zalogowaniu z zaznaczoną opcją:

   Response:
   - HTTP 302 → /my-account?id=wiener
   - Set-Cookie:
     - stay-logged-in=d2llbmVyOjUxZGMzMGRkYzQ3M2Q0M2E2MDExZTllYmJhNmNhNzcw

---

   Po dekodowaniu Base64:

```

wiener:51dc30ddc473d43a6011e9ebba6ca770

```

Struktura:
- username:MD5(password)

---

Kluczowe:

- cookie zawiera dane uwierzytelniające
- brak podpisu / brak integralności
- wartość deterministyczna (zależna tylko od hasła)

---

4. HIPOTEZA

Backend opiera uwierzytelnienie na wartości cookie `stay-logged-in`.

Jeśli:
- znamy strukturę tokena
- hash jest deterministyczny

➜ możliwe jest odtworzenie poprawnego tokena brute-force’em

---

5. ANALIZA MECHANIZMU

Backend:

- przy kolejnych requestach:
  - odczytuje `stay-logged-in`
  - parsuje:
    - username
    - hash(password)
  - używa tego do uwierzytelnienia

---

Klucz:

- brak serwerowego źródła prawdy
- brak sekretu
- pełna kontrola po stronie klienta

➜ auth oparty na danych klienta

---

6. REPRODUCTION / EXPLOIT

Step 1 – Login jako zwykły user

POST /login

username=wiener&password=peter&stay-logged-in=on

---

Step 2 – Analiza cookie

- decode base64
- identyfikacja struktury:
  - username:MD5(password)

---

Step 3 – Przygotowanie payloadów

Dla każdego hasła:

- MD5(password)
- carlos:<hash>
- base64(carlos:<hash>)

---

Step 4 – Wysyłanie requestów

GET /my-account?id=carlos

Cookie:
stay-logged-in=<payload>

---

Detekcja:
- HTTP 200 + dostęp do konta carlos

---

Step 5 – Wynik

Password: andrew  
➜ dostęp do konta → LAB SOLVED

---

7. IMPACT

- przejęcie konta dowolnego użytkownika
- brak potrzeby znajomości hasła wprost
- obejście login flow
- możliwość pełnej automatyzacji

---

8. DEBUGGING / PITFALLS

Problemy:

- brak zaznaczenia „stay logged in”
- skupienie się na login endpoint zamiast cookie
- brak decode base64
- użycie plaintext zamiast MD5
- zła kolejność:
- MD5 → prefix → base64
- atak w body zamiast w cookie

---

Narzędzia:

- Proxy  
- Repeater  
- Intruder (opcjonalnie)  
- Python (custom script)

---

Rozwiązania:

- analiza wszystkich cookie po loginie
- decode i zrozumienie struktury
- identyfikacja deterministycznych elementów
- brute-force poprzez rekonstrukcję tokena

---

9. MENTAL MODEL / PATTERN

CLIENT-CONTROLLED AUTH TOKEN

Jeśli aplikacja:
- przechowuje dane auth w cookie
- używa encodingu (np. base64)
- używa hasha bez sekretu

➜ możliwe odtworzenie tokena

---

Klucz:

> "Czy mogę odtworzyć tę wartość bez znajomości sekretu?"

---

Sygnały ostrzegawcze:

- cookie typu:
- remember me
- stay-logged-in
- base64 + czytelna struktura po decode
- hash bez podpisu
- brak weryfikacji po stronie serwera

---
```
