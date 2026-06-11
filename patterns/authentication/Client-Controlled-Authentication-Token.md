```md
# Client-Controlled Authentication Token

CLASS: Authentication / Trust Boundary Violation

---

1. CORE IDEA

---

Aplikacja opiera uwierzytelnienie na tokenie, który może zostać odtworzony, zmodyfikowany lub przeanalizowany przez klienta.

---

2. ROOT CAUSE

---

Backend traktuje dane kontrolowane przez klienta jako źródło prawdy dla procesu uwierzytelniania.

---

3. SIGNALS

---

* remember me
* stay logged in
* persistent login
* custom auth cookies
* tokeny zawierające czytelne dane po dekodowaniu
* base64 bez dodatkowej ochrony
* tokeny zawierające:

  * username
  * email
  * hash hasła
  * identyfikatory użytkownika
* brak podpisu integralności
* deterministyczna struktura tokena

---

4. PREREQUISITES

---

* istnieje klientowski artefakt uwierzytelniający
* backend wykorzystuje go podczas logowania
* klient może odczytać lub przechwycić token
* token nie jest odpowiednio chroniony

---

5. QUESTIONS

---

* Co znajduje się w tokenie po dekodowaniu?
* Czy token zawiera dane uwierzytelniające?
* Czy mogę odtworzyć token samodzielnie?
* Czy backend ufa zawartości tokena?
* Czy istnieje podpis lub sekret serwerowy?
* Czy przejęcie tokena prowadzi do przejęcia tożsamości?

---

6. DETECTION

---

* analiza wszystkich cookies po logowaniu
* dekodowanie:

  * Base64
  * Hex
  * URL Encoding
* porównywanie tokenów pomiędzy użytkownikami
* identyfikacja deterministycznych elementów
* próby rekonstrukcji tokena
* analiza wpływu przejęcia tokena na proces auth

---

7. GENERALIZATION

---

Pattern może występować w:

* remember me cookies
* persistent login
* custom authentication cookies
* client-side auth tokens
* custom SSO implementations
* pseudo-session mechanisms
* offline credential storage
* auto-login mechanisms

---

8. WHY IT WORKS

---

Proces uwierzytelniania powinien opierać się na danych kontrolowanych przez serwer.

Jeżeli backend ufa artefaktowi przechowywanemu po stronie klienta, atakujący może go odtworzyć, zmodyfikować, przeanalizować lub wykorzystać po przejęciu.

Mechanizm:

Auth token po stronie klienta → zaufanie backendu → manipulacja lub odzyskanie danych → przejęcie tożsamości

---

9. SEEN IN

---

* Brute-Forcing a Stay-Logged-In Cookie
* Offline Password Cracking

```
