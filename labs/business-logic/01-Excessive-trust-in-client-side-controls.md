```md
LAB: Excessive trust in client-side controls
Kategoria: Business Logic / Client-side Trust / Price Manipulation
Utworzono: 2026-05-29 14:15 (Europe/Amsterdam)

---

1. SCAN (Entry & Signals)

Endpoint:

* POST /cart

Controllable input:

* productId
* quantity
* price
* redir

Przechwycony request:

productId=1&redir=PRODUCT&quantity=1&price=133700

Signals:

* parametr price wysyłany przez klienta
* cena produktu znajduje się bezpośrednio w requestcie
* brak podpisu / HMAC
* brak ponownego wyliczenia ceny przez backend
* zmodyfikowana cena pojawia się później w koszyku

Obserwacja:

* price wygląda na wartość biznesową kontrolowaną przez użytkownika

---

2. LOCK (Vector)

Vector:

* price manipulation
* excessive trust in client-side controls

Why this:

* backend prawdopodobnie wykorzystuje cenę przesłaną przez klienta zamiast własnej ceny zapisanej po stronie serwera

Hipoteza:

* zmiana wartości price wpłynie bezpośrednio na końcową wartość zamówienia

---

3. PRESS (Exact Steps)

Step 1 – Dodanie produktu do koszyka

Dodaj produkt do koszyka.

Przechwyć request:

POST /cart

productId=1&redir=PRODUCT&quantity=1&price=133700

---

Step 2 – Test manipulacji ceny

Zmień:

price=133700

na:

price=0.01

Wyślij request.

Wynik:

* request zaakceptowany
* produkt dodany do koszyka

---

Step 3 – Weryfikacja koszyka

Przejdź do koszyka.

Obserwacja:

* cena produktu wynosi 0.01$

Wniosek:

* backend wykorzystuje wartość price dostarczoną przez klienta

---

Step 4 – Final exploit

Pozostaw zmodyfikowaną cenę.

Kup produkt.

Wynik:

* zakup produktu za 0.01$

➜ LAB SOLVED

---

4. BREAK (Proof)

Potwierdzone:

* użytkownik kontroluje cenę produktu
* backend ufa wartości przesłanej przez klienta
* cena nie jest pobierana z backendowej bazy produktów
* możliwa manipulacja logiką zakupową

Dowody:

* cena zmieniona w requestcie
* cena zmieniona w koszyku
* możliwość zakupu po zmodyfikowanej wartości

---

5. POST (Exploit Path)

Testy rozszerzające:

* price=0
* price=-1
* price=-100
* price=999999
* price=0.0001
* price=
* price=abc

Cel:

* określenie granic walidacji backendu

Możliwe skutki w realnej aplikacji:

* darmowe zakupy
* rabaty nieprzewidziane przez biznes
* generowanie salda
* manipulacja rozliczeniami

---

6. ROOT CAUSE

Backend:

* przyjmuje cenę produktu od klienta
* wykorzystuje ją podczas obliczania wartości zamówienia

Problem:

* krytyczna wartość biznesowa znajduje się po stronie klienta

Prawidłowy model:

Client
↓
productId
↓
Backend pobiera cenę z bazy
↓
Wylicza wartość zamówienia

Obecny model:

Client
↓
price
↓
Backend ufa wartości
↓
Business Impact

Trust break:

* Client Value ≠ Trusted Business Value

---

7. PATTERN (Reusable)

Name:

* Excessive trust in client-side controls
* Client-controlled business value
* Price manipulation

Conditions:

* sklepy internetowe
* checkout flows
* koszyki zakupowe
* kupony rabatowe
* systemy punktowe
* aplikacje finansowe

Sygnały:

* price
* discount
* credits
* points
* balance
* shippingCost

---

8. PITFALLS (My mistakes)

* skupienie się wyłącznie na interfejsie użytkownika
* założenie że hidden field jest bezpieczny
* nieuwzględnienie że każda wartość z requestu jest kontrolowana przez użytkownika
* zakończenie testów po pierwszym sukcesie bez sprawdzenia granic walidacji

---

9. DETECTION (Fast trigger)

Sygnały:

* cena znajduje się w requestcie
* wartość biznesowa wysyłana przez klienta
* brak podpisu kryptograficznego
* brak backendowego przeliczenia danych

Testy:

* zmiana ceny
* wartości ujemne
* wartości zerowe
* bardzo duże liczby
* wartości tekstowe

Pytanie kontrolne:

"Czy użytkownik może bezpośrednio wpływać na dane używane do podjęcia decyzji biznesowej?"

Jeżeli TAK:

→ sprawdź czy backend ufa tym danym.

---

10. REPLAY (🔥 EXEC)

11. Dodaj produkt do koszyka.

12. Przechwyć:

    POST /cart

13. Znajdź parametr:

    price

14. Zmień:

    133700 → 0.01

15. Wyślij request.

16. Otwórz koszyk.

17. Potwierdź zmianę ceny.

18. Kup produkt.

19. LAB SOLVED

---

11. TAKEAWAY

Najważniejszy błąd nie polegał na możliwości zmiany requestu.

Najważniejszy błąd polegał na tym, że backend zaufał wartości biznesowej dostarczonej przez użytkownika.

W Business Logic najważniejsze pytanie brzmi:

"Kto kontroluje dane, na podstawie których backend podejmuje decyzję?"

Jeżeli odpowiedź brzmi:

"użytkownik"

to prawdopodobnie właśnie znalazłeś podatność.

```
