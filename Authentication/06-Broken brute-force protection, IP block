LAB: Broken brute-force protection, IP block
Kategoria: Authentication / Brute-force / Rate limiting
Utworzono: 2026-03-29 17:xx (Europe/Amsterdam)

---

1. CEL TESTU

   Sprawdzenie mechanizmu blokady brute-force opartego o IP
   oraz próba jego obejścia w celu brute-force hasła użytkownika carlos.

---

2. KONTEKST APLIKACJI

   Endpoint:
   POST /login

   Mechanizmy:
   - blokada po kilku nieudanych próbach
   - komunikat w response body (HTML)
   - brak blokady na poziomie HTTP (status nadal 200)

   Konto testowe:
   - wiener:peter

   Cel:
   - carlos + hasło

---

3. OBSERWACJA

   - Poprawne logowanie:
     → 302 → /my-account?id=wiener

   - Niepoprawne logowanie:
     → 200 + komunikat błędu

   - Po kilku próbach:
     → komunikat:
       "You have made too many incorrect login attempts"

   Testy:
   - zmiana username → brak wpływu
   - zmiana session → brak wpływu
   - blokada nadal występuje

   Kluczowe:
   - sekwencja:
     fail → fail → success → brak blokady

---

4. HIPOTEZA

   - licznik prób powiązany z IP
   - failed login zwiększa licznik
   - successful login zmienia stan licznika (reset lub neutralizacja)

---

5. ANALIZA MECHANIZMU

   Mechanizm:

   1. failed login:
      → licznik +1

   2. po przekroczeniu progu:
      → blokada (aplikacyjna)

   3. successful login:
      → reset lub zmniejszenie licznika

   Wniosek:
   → licznik nie jest monotoniczny
   → można nim sterować poprzez odpowiednią sekwencję requestów

---

6. REPRODUCTION / EXPLOIT

   Sekwencja:

   fail → fail → success → repeat

   Implementacja (Burp Intruder, Pitchfork):

   Payload 1 (username):
   wiener
   carlos
   carlos
   ...

   Payload 2 (password):
   peter
   pass1
   pass2
   ...

   Warunki:
   - concurrency = 1
   - dokładna synchronizacja payloadów

   Analiza:
   - filtrowanie:
     Status: 302

   Wynik:
   → username: carlos
   → password: thomas

---

7. IMPACT

   - obejście brute-force protection
   - możliwość brute-force bez blokady
   - przejęcie kont użytkowników

---

8. DEBUGGING / PITFALLS

   - brute-force bez analizy mechanizmu → blokada
   - brak kontroli sekwencji requestów
   - zbyt duża concurrency → zaburzenie logiki licznika
   - problemy z Turbo Intruder:
     - Content-Length mismatch
     - błędna konstrukcja requestów
   - brak rozróżnienia:
     - co zwiększa licznik
     - co go resetuje

---

9. MENTAL MODEL / PATTERN

   [State-dependent brute-force bypass]

   rdzeń:

   - mechanizm ochrony oparty o stan (counter)
   - stan zmienia się różnie w zależności od typu requestu
   - możliwa manipulacja stanem

   schemat:

   1. identify protection (counter)
   2. identify increment condition (fail)
   3. identify state change (success)
   4. build controlled sequence
   5. avoid threshold
   6. exploit

   skrót:

   system counts attempts
   → find what changes state
   → control sequence
   → bypass protection

---

[MENTAL MODEL – do repo]

Nazwa: State Manipulation in Rate Limiting

Opis:
Mechanizmy ochrony oparte na liczniku prób mogą być podatne,
jeśli różne typy requestów wpływają na stan w różny sposób.

Reguła:

if system counts → find increment + reset → control sequence

Przykłady:
- brute-force protection
- MFA attempts
- lockout mechanisms
- API rate limits

Anty-pattern:
- brute-force bez analizy mechanizmu
- traktowanie wszystkich requestów jako równoważnych
- brak kontroli kolejności requestów
