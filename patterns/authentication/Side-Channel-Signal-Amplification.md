```md
# Side-Channel Signal Amplification

CLASS: Side Channel / Information Disclosure

---

1. CORE IDEA

---

Słaby side-channel może zostać wzmocniony poprzez zwiększenie kosztu operacji wykonywanej przez backend.

---

2. ROOT CAUSE

---

Koszt wykonywania operacji zależy od danych wejściowych, a różnice w czasie lub zachowaniu systemu są obserwowalne z zewnątrz.

---

3. SIGNALS

---

* niewielkie różnice czasowe
* niestabilne wyniki pomiarów
* pojedyncze outliery
* operacje zależne od:

  * długości danych
  * rozmiaru danych
  * liczby iteracji
  * kosztownych obliczeń
* widoczne różnice po zwiększeniu inputu

---

4. PREREQUISITES

---

* istnieje obserwowalny side-channel
* koszt operacji zależy od danych wejściowych
* atakujący może kontrolować input
* możliwe jest wielokrotne wykonywanie testów

---

5. QUESTIONS

---

* Czy backend wykonuje dodatkową pracę dla określonych przypadków?
* Czy koszt operacji zależy od długości danych?
* Czy mogę zwiększyć koszt przetwarzania?
* Czy mogę zmniejszyć szum pomiarowy?
* Czy obserwowany sygnał staje się wyraźniejszy przy większym inputcie?

---

6. DETECTION

---

* zwiększanie długości danych wejściowych
* zwiększanie liczby elementów przetwarzanych przez backend
* analiza czasów odpowiedzi
* ograniczenie concurrency
* wielokrotne pomiary
* wyszukiwanie stabilnych outlierów

---

7. GENERALIZATION

---

Pattern może występować w:

* authentication timing attacks
* cryptographic operations
* password verification
* database queries
* cache mechanisms
* race condition analysis
* resource-intensive workflows
* parser logic

---

8. WHY IT WORKS

---

Side-channel często istnieje, ale jest ukryty w szumie środowiska.

Jeżeli atakujący zwiększy koszt wykonywanej operacji, różnice pomiędzy ścieżkami wykonania stają się większe i łatwiejsze do zaobserwowania.

Mechanizm:

Mały sygnał → zwiększenie kosztu → większa różnica → łatwiejsza obserwacja → wyciek informacji

---

9. SEEN IN

---

* Username Enumeration via Response Timing

```
