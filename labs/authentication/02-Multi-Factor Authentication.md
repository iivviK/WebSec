LAB: 2FA simple bypass
Kategoria: Authentication / Multi-Factor Authentication
Utworzono: 2026-03-14 17:55 (Europe/Amsterdam)

---

1. CEL TESTU

   Sprawdzenie czy mechanizm 2FA jest poprawnie egzekwowany przez backend.

   Test polega na sprawdzeniu czy drugi etap uwierzytelniania
   można pominąć manipulując ścieżką aplikacji.

---

2. KONTEKST APLIKACJI

   Aplikacja posiada dwuetapowy proces logowania:

   Step 1:
   username + password

   Step 2:
   kod 2FA

   Po poprawnym loginie użytkownik jest przekierowywany na:

   /login2

   gdzie powinien podać kod weryfikacyjny.

---

3. OBSERWACJA

   Po zalogowaniu jako:

   wiener:peter

   aplikacja zwraca:

   HTTP 302
   Location: /login2

   Jednocześnie ustawiana jest nowa sesja:

   Set-Cookie: session=<token>

   Wniosek:

   użytkownik posiada już aktywną sesję przed ukończeniem 2FA.

---

4. HIPOTEZA

   Jeśli backend nie sprawdza czy etap 2FA został ukończony,
   możliwe jest pominięcie ekranu /login2 i bezpośrednie
   przejście do panelu użytkownika.

---

5. ANALIZA MECHANIZMU

   Proces logowania wygląda następująco:

   username + password → OK  
   backend tworzy session cookie  
   redirect → /login2  

   Problem polega na tym, że backend nie sprawdza
   czy użytkownik przeszedł drugi etap uwierzytelniania.

   Sesja jest już aktywna.

---

6. REPRODUCTION / EXPLOIT

   Step 1

   Logowanie jako:

   wiener:peter

   POST /login

   Response:

   HTTP 302
   Location: /login2

   Step 2

   Przechwycenie requestu i manipulacja ścieżką.

   Zamiast:

   /login2

   użyto:

   /my-account

   Step 3

   Wysłanie requestu z aktywną sesją:

   GET /my-account
   Cookie: session=<session-token>

   Rezultat:

   dostęp do konta użytkownika.

   W labie celem było uzyskanie dostępu do konta:

   carlos

   co zakończyło lab.

---

7. IMPACT

   Atakujący może pominąć drugi etap uwierzytelniania.

   Oznacza to:

   - obejście mechanizmu 2FA
   - dostęp do kont użytkowników
   - przejęcie sesji po samym loginie

   Mechanizm MFA traci swoją wartość bezpieczeństwa.

---

8. DEBUGGING / PITFALLS

   Ważna obserwacja:

   backend tworzy session cookie przed zakończeniem procesu 2FA.

   To oznacza, że aplikacja traktuje użytkownika jako
   częściowo uwierzytelnionego, ale nie ogranicza dostępu
   do chronionych endpointów.

---

9. MENTAL MODEL / PATTERN

   Ten typ podatności to:

   Authentication state machine flaw.

   Aplikacja posiada dwa etapy uwierzytelniania:

   1. password verification
   2. 2FA verification

   Backend traktuje jednak:

   password OK = authenticated

   co umożliwia pominięcie drugiego etapu.

   Pattern do zapamiętania:

   jeśli aplikacja używa ścieżki typu:

   /login2
   /mfa
   /verify

   zawsze sprawdzić czy można bezpośrednio
   wejść na chronione endpointy.
