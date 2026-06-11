```md
# State Manipulation in Rate Limiting

CLASS: Business Logic / State Management

---

1. CORE IDEA

---

Mechanizm ochronny oparty na stanie może zostać obejty poprzez kontrolowanie sposobu, w jaki stan jest zmieniany.

---

2. ROOT CAUSE

---

Backend pozwala użytkownikowi wpływać na przejścia stanu mechanizmu ochronnego w sposób, który nie był uwzględniony podczas projektowania.

---

3. SIGNALS

---

* liczniki prób
* rate limiting
* account lockout
* cooldown periods
* MFA attempt counters
* zmiana zachowania po określonej liczbie operacji
* różne akcje wpływają na stan w różny sposób
* operacje sukcesu i porażki modyfikują stan inaczej

---

4. PREREQUISITES

---

* aplikacja utrzymuje stan
* stan wpływa na decyzje bezpieczeństwa
* istnieje więcej niż jedna operacja zmieniająca stan
* atakujący może wykonywać te operacje wielokrotnie

---

5. QUESTIONS

---

* Co zwiększa licznik?
* Co resetuje licznik?
* Co zmniejsza licznik?
* Kiedy następuje blokada?
* Jakie akcje wpływają na stan systemu?
* Czy wszystkie operacje są traktowane jednakowo?
* Czy mogę sterować przejściami stanu?

---

6. DETECTION

---

* identyfikacja mechanizmu ochronnego
* analiza zmian stanu po:

  * sukcesie
  * porażce
  * anulowaniu operacji
* porównywanie sekwencji działań
* budowanie kontrolowanych ciągów operacji
* obserwacja momentów resetu i inkrementacji

---

7. GENERALIZATION

---

Pattern może występować w:

* brute-force protection
* account lockout
* MFA protection
* API rate limiting
* coupon limits
* redemption systems
* loyalty programs
* voting systems
* abuse prevention mechanisms

---

8. WHY IT WORKS

---

Mechanizm ochronny zakłada określony sposób zmiany stanu.

Jeżeli atakujący odkryje, które działania zwiększają, resetują lub modyfikują stan, może sterować systemem tak, aby nigdy nie osiągnąć warunku blokady lub osiągać pożądany stan w kontrolowany sposób.

Mechanizm:

Stan → kontrolowane przejścia → nieoczekiwany stan końcowy → obejście zabezpieczenia

---

9. SEEN IN

---

* Broken Brute-Force Protection, IP Block

```
