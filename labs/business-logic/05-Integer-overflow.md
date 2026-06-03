```md
LAB: Integer overflow
Kategoria: Business Logic
Utworzono: 2026-06-03 17:xx (Europe/Amsterdam)

---

1. SCAN (Entry & Signals)

   Endpointy:

   * POST /cart
   * POST /cart/checkout

   Obiekty biznesowe:

   * Shopping Cart
   * Product
   * Store Credit
   * Cart Total
   * Order

   Sygnały:

   * możliwość wielokrotnego dodawania produktu
   * bardzo drogi produkt (Leather Jacket)
   * cena przechowywana w centach
   * brak widocznego limitu wartości koszyka
   * nagła zmiana dużej wartości dodatniej na ujemną

---

2. KONTEKST APLIKACJI

   Aplikacja udostępnia sklep internetowy wykorzystujący saldo użytkownika (store credit) do realizacji zakupów.

   Użytkownik może:

   * dodawać produkty do koszyka
   * modyfikować ilości produktów
   * finalizować zamówienia

   Reguła biznesowa zakłada, że użytkownik nie może kupić produktu, którego wartość przekracza dostępne saldo.

---

3. OBSERWACJA

   Początkowo aplikacja zachowuje się prawidłowo.

   Próba zakupu Leather Jacket kończy się odrzuceniem zamówienia z powodu niewystarczających środków.

   Analiza ruchu pokazuje możliwość wielokrotnego wykonywania operacji dodawania produktu do koszyka.

   Podczas automatycznego zwiększania ilości produktów całkowita wartość koszyka rośnie zgodnie z oczekiwaniami.

   Po osiągnięciu bardzo dużej wartości następuje nieoczekiwana zmiana na wartość ujemną.

---

4. HIPOTEZA

   Backend zakłada, że całkowita wartość koszyka nigdy nie przekroczy zakresu liczbowego wykorzystywanego przez aplikację.

   Jeżeli wartość koszyka przekroczy maksymalną dopuszczalną wartość typu liczbowego, może dojść do przepełnienia (integer overflow), prowadzącego do błędnych obliczeń biznesowych.

---

5. ANALIZA MECHANIZMU

   Cena produktu jest przechowywana jako liczba całkowita reprezentująca centy.

   Każde dodanie produktu zwiększa wartość koszyka.

   Po przekroczeniu maksymalnej wartości typu liczbowego następuje przepełnienie.

   W rezultacie:

   * bardzo duża wartość dodatnia
   * zostaje zinterpretowana jako
   * bardzo duża wartość ujemna

   Backend kontynuuje przetwarzanie tej wartości tak, jakby była poprawna.

   Pozwala to manipulować końcową ceną koszyka.

---

6. REPRODUCTION / EXPLOIT

   1. Zaloguj się do aplikacji.
   2. Dodaj Leather Jacket do koszyka.
   3. Przechwyć żądanie POST /cart.
   4. Wyślij żądanie do Intrudera.
   5. Automatycznie wykonuj kolejne dodania produktu.
   6. Obserwuj wzrost wartości koszyka.
   7. Doprowadź do przepełnienia wartości całkowitej.
   8. Potwierdź pojawienie się ujemnej wartości koszyka.
   9. Dostosuj zawartość koszyka tak, aby końcowa cena mieściła się w dostępnym store credit.
   10. Finalizuj zamówienie.

---

7. IMPACT

   Użytkownik może doprowadzić aplikację do nieprawidłowego stanu finansowego.

   Prowadzi to do:

   * manipulacji ceną koszyka
   * zakupu produktów bez wymaganych środków
   * obejścia ograniczeń store credit
   * naruszenia integralności obliczeń biznesowych

---

8. DEBUGGING / PITFALLS

   Główna pułapka:

   Skupienie się wyłącznie na standardowych manipulacjach wartościami biznesowymi (np. quantity=-1 lub negative values).

   Błędny kierunek:

   * analiza pojedynczych requestów
   * szukanie klasycznych ujemnych wartości
   * szukanie błędów walidacji quantity

   Właściwy kierunek:

   * analiza zachowania aplikacji przy skrajnie dużych wartościach
   * obserwacja zmian wartości koszyka w czasie
   * testowanie ograniczeń liczbowych backendu

---

9. MENTAL MODEL / PATTERN

   Jeżeli wartość biznesowa może być zwiększana wielokrotnie, należy sprawdzić nie tylko minimalne, ale również maksymalne granice danych.

   Szczególnie warto analizować:

   * ceny
   * salda
   * punkty
   * limity
   * liczniki
   * wartości przechowywane w centach

   Pytanie przewodnie:

   "Co stanie się, jeśli wartość przekroczy maksymalny zakres przewidziany przez programistę?"

---

10. WHY IT WORKS

Backend wykonuje obliczenia finansowe na typie liczbowym o ograniczonym zakresie.

Po przekroczeniu maksymalnej wartości dochodzi do przepełnienia, a wynik zostaje zinterpretowany jako wartość ujemna.

Logika biznesowa nie wykrywa tego stanu i nadal traktuje wynik jako prawidłową wartość koszyka, co umożliwia obejście ograniczeń finansowych aplikacji.

```
