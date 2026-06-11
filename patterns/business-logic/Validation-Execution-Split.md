```md
NAME: Validation-Execution Split

CLASS: Business Logic

---

1. CORE IDEA

Walidacja wymagań biznesowych i wykonanie operacji są rozdzielone pomiędzy różne etapy workflow.

---

2. ROOT CAUSE

Etap wykonujący operację ufa, że wcześniejszy etap workflow poprawnie zweryfikował wszystkie wymagania biznesowe.

Backend nie egzekwuje ponownej walidacji w miejscu wykonania operacji.

---

3. SIGNALS

* wieloetapowy workflow
* POST → Redirect → GET
* endpointy typu:

  * confirmation
  * finalize
  * success
  * complete
  * activate
  * redeem
* walidacja występuje wcześniej niż wykonanie operacji
* operacja biznesowa wykonywana jest przez osobny endpoint
* endpoint końcowy wygląda jak "potwierdzenie", ale wywołuje skutki biznesowe

---

4. PREREQUISITES

* aplikacja wykorzystuje wieloetapowy workflow
* walidacja i wykonanie operacji są rozdzielone
* istnieje etap finalizujący proces
* etap finalizujący nie weryfikuje samodzielnie warunków biznesowych

---

5. QUESTIONS

* Gdzie wykonywana jest walidacja?
* Gdzie wykonywana jest właściwa operacja?
* Czy są to te same endpointy?
* Czy endpoint końcowy można wywołać bez przejścia całego workflow?
* Czy późniejszy krok ufa wcześniejszym decyzjom?
* Czy redirect prowadzi do operacji biznesowej?
* Czy endpoint "confirmation" rzeczywiście tylko potwierdza?

---

6. DETECTION

* wykonaj pełny legalny workflow
* zmapuj wszystkie requesty i redirecty
* zidentyfikuj endpoint wykonujący finalną operację
* wyślij końcowy request niezależnie od wcześniejszych kroków
* sprawdź czy operacja zostanie wykonana mimo niespełnienia wymagań biznesowych
* porównaj zachowanie po przejściu pełnego workflow i po bezpośrednim wywołaniu kroku końcowego

---

7. GENERALIZATION

* procesy zakupowe
* potwierdzenia zamówień
* aktywacje kont
* resety haseł
* weryfikacje email
* realizacja voucherów
* refundy
* transfery środków
* upgrade planów
* procesy onboardingowe
* workflow wymagające wcześniejszej autoryzacji lub walidacji

---

8. WHY IT WORKS

Backend podejmuje decyzję bezpieczeństwa w jednym kroku workflow, a wykonuje operację w innym.

Jeżeli krok wykonujący operację nie sprawdza ponownie wymagań biznesowych, możliwe staje się bezpośrednie wywołanie operacji z pominięciem wcześniejszej walidacji.

Powstaje rozjazd pomiędzy:

* założonym przebiegiem procesu
* rzeczywistymi wymaganiami egzekwowanymi przez backend

---

9. SEEN IN

* PortSwigger Lab: Insufficient workflow validation

```
