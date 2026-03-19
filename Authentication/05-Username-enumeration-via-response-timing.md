LAB: Username enumeration via response timing
Kategoria: Authentication / Access Control
Utworzono: 2026-03-19 11:29 (Europe/Amsterdam)

---

1. CEL TESTU

   Sprawdzenie, czy możliwa jest enumeracja użytkownika na podstawie czasu odpowiedzi (side-channel),
   a następnie brute-force hasła z obejściem mechanizmu rate limit.

---

2. KONTEKST APLIKACJI

   Endpoint logowania:
   POST /login

   Mechanizmy:
   - brak jednoznacznej różnicy w response body (dla valid/invalid)
   - różnice w czasie odpowiedzi (timing leak)
   - IP-based brute-force protection

---

3. OBSERWACJA

   - Wszystkie odpowiedzi dla błędnych loginów zwracają:
     - Status: 200
     - Podobna długość odpowiedzi
   - Przy standardowym haśle różnice timingów są minimalne (noise)
   - Po zwiększeniu długości hasła (~100 znaków):
     - jeden username powoduje ogromny wzrost czasu odpowiedzi (~1800 ms)
   - Rate limit blokuje powtarzające się requesty z jednego IP

---

4. HIPOTEZA

   - Backend:
     - dla nieistniejącego usera → szybki return
     - dla istniejącego → hash(password) + porównanie
   - Dłuższe hasło zwiększa koszt obliczeń → większy delay
   - Możliwe spoofowanie IP przez nagłówki HTTP

---

5. ANALIZA MECHANIZMU

   Mechanizm podatności:

   1. Brak usera:
      → brak sprawdzania hasha
      → szybka odpowiedź

   2. Poprawny user:
      → hash(password)
      → porównanie z DB
      → zależne od długości hasła

   3. Rate limit:
      → oparty o IP
      → możliwy do obejścia przez:
        X-Forwarded-For

   Wniosek:
   → timing + kontrolowany input = side-channel leak

---

6. REPRODUCTION / EXPLOIT

   Krok 1: Przechwycenie requestu login

   Krok 2: Przygotowanie requestu do enumeracji:

   POST /login

   Headers:
   X-Forwarded-For: §IP§

   Body:
   username=§USER§
   password=AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA

   Krok 3: Burp Intruder:
   - Attack type: Pitchfork
   - Payload 1: IP (1–100)
   - Payload 2: lista username
   - concurrency = 1

   Krok 4: Analiza:
   - sort po Response time
   - znalezienie dużego outliera (~1800 ms)

   Wynik:
   → username = agenda

   Krok 5: Brute-force hasła:

   POST /login

   Headers:
   X-Forwarded-For: §IP§

   Body:
   username=agenda
   password=§PASS§

   Krok 6: Analiza:
   - szukanie:
     Status: 302

   Wynik:
   → password = austin

---

7. IMPACT

   - Możliwość enumeracji użytkowników
   - Brute-force haseł mimo rate limit
   - Możliwość przejęcia konta

---

8. DEBUGGING / PITFALLS

   - Zbyt krótkie hasło → brak widocznego sygnału timing
   - Za dużo requestów → jitter / noise
   - Fałszywe spike’i przy dużych batchach
   - Brak IP rotation → blokada
   - Analiza timingu zamiast status code przy brute-force

---

9. MENTAL MODEL / PATTERN

   [Side-channel amplification pattern]

   rdzeń:

   - side-channel istnieje → ale sygnał jest słaby
   - atakujący zwiększa koszt operacji
   - sygnał staje się wyraźny

   schemat:

   1. detect leak (timing)
   2. amplify signal (np. długość inputu)
   3. stabilize environment (concurrency=1)
   4. bypass protections (header spoofing)
   5. identify outlier
   6. exploit

   skrót:

   timing → too small?
   → increase cost
   → find spike
   → confirm
   → exploit

---

[MENTAL MODEL – do repo]

Nazwa: Side-Channel Signal Amplification

Opis:
Side-channel attack często daje bardzo słaby sygnał (np. różnice 10–20 ms).
Zamiast próbować go „zgadywać”, należy zwiększyć koszt operacji po stronie backendu,
aby wzmocnić różnicę.

Reguła:

signal too weak → amplify input → observe clearer signal

Przykłady:
- auth timing → długie hasło
- SQLi → ciężkie zapytania
- crypto → większe dane
- race condition → więcej requestów

Anty-pattern:
- próba analizy noise zamiast jego eliminacji
- brak kontroli środowiska (rate limit, concurrency)
