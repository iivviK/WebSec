LAB: Username enumeration via different responses
Kategoria: Authentication / Username Enumeration
Utworzono: 2026-03-14 17:30 (Europe/Amsterdam)

---

1. CEL TESTU

   Sprawdzenie czy mechanizm logowania ujawnia informację o istnieniu
   użytkownika poprzez różnice w odpowiedziach aplikacji.

   Jeśli aplikacja rozróżnia:
   - nieistniejący użytkownik
   - błędne hasło

   możliwe jest przeprowadzenie username enumeration.

---

2. KONTEKST APLIKACJI

   - aplikacja webowa z formularzem logowania
   - endpoint POST /login
   - dwa parametry w body:

     username
     password

   test polega na sprawdzeniu czy odpowiedzi aplikacji zdradzają
   informację o poprawności username.

---

3. OBSERWACJA

   Podczas testów logowania zauważono dwa różne komunikaty błędów.

   Dla nieistniejącego użytkownika:

   "Invalid username"

   Dla istniejącego użytkownika z błędnym hasłem:

   "Incorrect password"

   Wniosek:

   Aplikacja ujawnia czy użytkownik istnieje w systemie.

   Narzędzia użyte w tym etapie:

   Burp Proxy – przechwycenie requestu logowania  
   Burp Intruder – enumeracja username  
   Turbo Intruder – brute force hasła

---

4. HIPOTEZA

   Jeśli aplikacja zwraca różne komunikaty dla:

   - nieistniejącego użytkownika
   - błędnego hasła

   możliwe jest enumerowanie poprawnych username poprzez
   analizę odpowiedzi.

---

5. ANALIZA MECHANIZMU

   Login endpoint działa jako **authentication oracle**.

   Schemat działania backendu:

   1. sprawdzenie czy użytkownik istnieje
   2. jeśli nie → "Invalid username"
   3. jeśli tak → sprawdzenie hasła
   4. jeśli hasło błędne → "Incorrect password"

   Ta różnica umożliwia ustalenie czy użytkownik istnieje.

---

6. REPRODUCTION / EXPLOIT

   Step 1 – przechwycenie requestu logowania

   POST /login

   body:

   username=test
   password=test

   Step 2 – enumeracja username (Intruder)

   payload ustawiony na parametr username.

   username=§username§
   password=test

   obserwacja odpowiedzi:

   - większość → "Invalid username"
   - jeden wynik → "Incorrect password"

   znaleziony użytkownik:

   aq

   Step 3 – brute force hasła

   username=aq
   password=§password§

   użyto wordlisty haseł.

   poprawne hasło:

   password

   Step 4 – identyfikacja sukcesu

   login success rozpoznany po:

   HTTP 302 redirect  
   Location: /my-account

---

7. IMPACT

   Atakujący może:

   - enumerować istniejących użytkowników
   - przeprowadzić brute force haseł
   - przejąć konto użytkownika

   W połączeniu z dużą wordlistą haseł może to prowadzić
   do pełnego przejęcia kont.

---

8. DEBUGGING / PITFALLS

   Podczas automatyzacji testów pojawiło się kilka problemów.

   Content-Length mismatch

   Gdy Turbo Intruder modyfikował hasło w body,
   nagłówek Content-Length nie zgadzał się z faktyczną
   długością requestu.

   Skutkowało to:

   - 400 Bad Request
   - "Missing parameter"

   Rozwiązanie:

   usunięcie nagłówka Content-Length i pozwolenie Burp
   na automatyczne obliczanie długości.

   Dodatkowo Turbo Intruder działa stabilniej z:

   HTTP/1.1 zamiast HTTP/2

---

9. MENTAL MODEL / PATTERN

   Login endpoint jako **authentication oracle**.

   Aplikacja ujawnia informacje poprzez:

   - komunikaty błędów
   - status HTTP
   - redirect
   - długość odpowiedzi

   Nawet niewielka różnica w odpowiedzi może ujawniać
   informacje o stanie systemu.

   Pattern do zapamiętania:

   różne komunikaty logowania → potencjalna username enumeration.
