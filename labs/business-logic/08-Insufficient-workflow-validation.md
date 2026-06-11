```md
LAB: Insufficient workflow validation
Kategoria: Business Logic
Utworzono: 2026-06-11 19:00 (Europe/Amsterdam)

---

1. SCAN (Entry & Signals)

Endpointy:

* POST /cart
* POST /cart/checkout
* GET /cart/order-confirmation
* GET /cart

Obiekty biznesowe:

* Shopping Cart
* Store Credit
* Order
* Purchase Workflow

Sygnały:

* zakup realizowany jest wieloetapowo
* po checkout występuje redirect do osobnego endpointu
* finalizacja zamówienia nie następuje bezpośrednio w requestcie checkout
* użytkownik posiada ograniczony kredyt sklepu ($100)
* istnieje produkt o wartości przekraczającej dostępne środki ($1337)

---

2. KONTEKST APLIKACJI

Aplikacja umożliwia zakup produktów za pomocą kredytu sklepowego.

Normalny proces biznesowy:

1. Dodanie produktu do koszyka.
2. Rozpoczęcie procesu checkout.
3. Weryfikacja środków.
4. Finalizacja zamówienia.
5. Potwierdzenie zakupu.

Założenie biznesowe:

* użytkownik może kupić wyłącznie produkty, na które posiada środki
* zamówienie powinno zostać sfinalizowane dopiero po poprawnym przejściu całego workflow

---

3. OBSERWACJA

Próba zakupu kurtki kończy się błędem:

* Store Credit = $100
* Product Price = $1337
* Purchase denied

Po zakupie taniego produktu obserwowany jest pełny łańcuch workflow:

POST /cart/checkout

↓

302 Redirect

↓

GET /cart/order-confirmation?order-confirmation=true

Oznacza to, że proces zakupu jest rozdzielony pomiędzy dwa niezależne endpointy.

---

4. HIPOTEZA

Endpoint odpowiedzialny za potwierdzenie zamówienia może ufać, że wszystkie wcześniejsze kroki workflow zostały już poprawnie wykonane.

Jeżeli endpoint finalizujący nie weryfikuje ponownie stanu zamówienia, możliwe może być wywołanie go poza normalnym procesem zakupu.

---

5. ANALIZA MECHANIZMU

Normalny przepływ:

1. Użytkownik rozpoczyna checkout.
2. Backend sprawdza środki.
3. Backend przygotowuje zamówienie.
4. Następuje redirect.
5. Endpoint potwierdzenia finalizuje proces.

Problem polega na rozdzieleniu:

* walidacji biznesowej
* wykonania operacji biznesowej

Backend zakłada, że endpoint:

GET /cart/order-confirmation

zostanie wywołany wyłącznie po poprawnym checkout.

W praktyce endpoint może zostać wywołany niezależnie od wcześniejszego przebiegu procesu.

Powstaje rozjazd pomiędzy:

* założeniem workflow,
* rzeczywistą implementacją.

---

6. REPRODUCTION / EXPLOIT

6.1. Zaloguj się jako użytkownik wiener.

6.2. Kup dowolny produkt mieszczący się w dostępnych środkach.

6.3. Przechwyć pełny workflow zakupu.

6.4. Zidentyfikuj request:

GET /cart/order-confirmation?order-confirmation=true

6.5. Wyślij request do Repeater.

6.6. Dodaj do koszyka:

Lightweight "l33t" Leather Jacket

6.7. Nie wykonuj checkout.

6.8. W Repeaterze wyślij ponownie:

GET /cart/order-confirmation?order-confirmation=true

6.9. Zaobserwuj, że zakup zostaje sfinalizowany.

6.10. Potwierdź rozwiązanie laboratorium.

---

7. IMPACT

Atakujący może:

* omijać ograniczenia workflow
* finalizować operacje bez spełnienia warunków biznesowych
* uzyskiwać produkty bez wymaganych środków
* wykonywać akcje poza przewidzianą kolejnością procesu

---

8. DEBUGGING / PITFALLS

Główna pułapka:

Skupienie się wyłącznie na parametrach requestów zamiast na całym workflow.

Błędny kierunek:

* quantity manipulation
* coupon guessing
* analiza pojedynczego endpointu checkout

Właściwy kierunek:

* wykonanie pełnego legalnego workflow
* mapowanie wszystkich requestów
* analiza redirectów
* identyfikacja endpointów kończących proces biznesowy

Szczególnie istotne:

* POST → 302 → GET
* endpointy confirmation
* endpointy success
* endpointy finalize

---

9. MENTAL MODEL / PATTERN

Pattern Candidate:

Unvalidated Workflow Finalization

Core Idea:

Aplikacja rozdziela walidację biznesową od finalizacji operacji.

Endpoint kończący proces ufa, że wcześniejsze kroki zostały wykonane poprawnie.

Pytania przewodnie:

* Czy proces składa się z wielu kroków?
* Gdzie wykonywana jest walidacja?
* Gdzie wykonywana jest właściwa operacja?
* Czy endpoint końcowy można wywołać bez przejścia całego workflow?
* Czy redirect prowadzi do krytycznej operacji biznesowej?

Typowe miejsca występowania:

* order-confirmation
* purchase-complete
* finalize
* success
* activate
* redeem
* verify

---

10. WHY IT WORKS

Backend sprawdza warunki biznesowe podczas wcześniejszego etapu workflow, ale nie wymusza ich ponownej weryfikacji podczas finalizacji operacji.

Endpoint:

GET /cart/order-confirmation

zakłada, że użytkownik przeszedł wcześniej poprawny checkout.

Ponieważ założenie to nie jest egzekwowane przez backend, możliwe jest bezpośrednie wywołanie endpointu finalizującego.

W efekcie operacja zakupu zostaje wykonana mimo niespełnienia wymagań biznesowych dotyczących dostępnych środków.

```
