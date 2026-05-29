```md
NAME:
Negative Quantity Compensation

CLASS:
Business Logic

==================================================
1. CORE IDEA
==================================================

Jedna pozycja koszyka może zostać wykorzystana do kompensacji wartości drugiej pozycji poprzez użycie ujemnej ilości.

==================================================
2. ROOT CAUSE
==================================================

Backend waliduje końcowy wynik operacji (total koszyka) zamiast poprawności danych wejściowych (quantity każdej pozycji).

==================================================
3. SIGNALS
==================================================

- quantity kontrolowane przez klienta
- możliwość edycji quantity po dodaniu produktu
- akceptacja wartości ujemnych
- komunikaty błędów odnoszące się do totalu koszyka
- jedna pozycja wpływa na wartość drugiej

==================================================
4. PREREQUISITES
==================================================

- quantity wysyłane przez klienta
- możliwość modyfikacji requestów
- koszyk zawierający wiele pozycji
- backend wykonujący obliczenia na podstawie quantity
- możliwość wpływu jednej pozycji na końcowy total

==================================================
5. QUESTIONS
==================================================

- Czy quantity może być ujemne?
- Czy jedna pozycja wpływa na końcową wartość innej?
- Czy backend waliduje quantity czy jedynie total?
- Czy mogę wykorzystać tani produkt jako kompensator drogiego produktu?
- Czy obliczenia wykonywane są przed walidacją?

==================================================
6. DETECTION
==================================================

Testy:

- quantity=-1
- quantity=0
- bardzo duże quantity
- kombinacje produktów drogich i tanich
- modyfikacja quantity po dodaniu produktu do koszyka

Szczególnie zwracaj uwagę na:

- błędy dotyczące totalu
- błędy dotyczące wartości koszyka
- brak błędów dotyczących quantity

==================================================
7. GENERALIZATION
==================================================

Pattern często występuje w:

- koszykach zakupowych
- systemach rabatowych
- cashback
- punktach lojalnościowych
- voucherach
- systemach kredytowych
- marketplace
- systemach zwrotów

==================================================
8. WHY IT WORKS
==================================================

Ujemna wartość jednej pozycji obniża całkowity koszt zamówienia, a backend sprawdza jedynie końcowy wynik obliczeń zamiast poprawności każdej pozycji osobno.

==================================================
9. SEEN IN
==================================================

- [[high-level-logic-vulnerability]]
```
