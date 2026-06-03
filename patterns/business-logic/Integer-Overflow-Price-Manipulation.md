```md
NAME:
Integer Overflow Price Manipulation

CLASS:
Business Logic

==================================================
1. CORE IDEA
==================================================

Wartość biznesowa (np. cena koszyka, saldo, kredyt, punkty) może zostać doprowadzona do przepełnienia typu liczbowego. Po przekroczeniu maksymalnego zakresu liczba "zawija się", co prowadzi do nieoczekiwanego stanu biznesowego możliwego do wykorzystania przez atakującego.

==================================================
2. ROOT CAUSE
==================================================

Aplikacja wykonuje operacje arytmetyczne na danych biznesowych bez kontroli przepełnienia (overflow) oraz bez odpowiednich limitów biznesowych ograniczających wzrost wartości.

==================================================
3. SIGNALS
==================================================

- Bardzo duże ilości produktów.
- Możliwość wielokrotnego wykonywania tej samej operacji.
- Ceny przechowywane jako liczby całkowite (np. centy).
- Store credit, punkty, saldo, kredyty użytkownika.
- Liczniki lub wartości biznesowe stale rosnące.
- Nagłe przejście z dużej wartości dodatniej do dużej wartości ujemnej.
- Brak limitów ilościowych lub kwotowych.

==================================================
4. PREREQUISITES
==================================================

- Możliwość kontrolowania wartości wpływającej na obliczenia biznesowe.
- Możliwość wielokrotnego wykonywania operacji zwiększających wartość.
- Brak walidacji przepełnienia.
- Brak limitów biznesowych zatrzymujących wzrost wartości.

==================================================
5. QUESTIONS
==================================================

- Czy wartość może rosnąć bez ograniczeń?
- Czy operacja może być wykonywana wielokrotnie?
- Jak przechowywane są wartości finansowe?
- Czy backend posiada limity ilościowe lub kwotowe?
- Co stanie się po osiągnięciu bardzo dużych wartości?
- Czy pojawiają się nietypowe wartości ujemne?

==================================================
6. DETECTION
==================================================

- Zwiększaj kontrolowaną wartość stopniowo.
- Automatyzuj powtarzalne operacje.
- Obserwuj zmiany wartości biznesowych.
- Szukaj momentu przejścia:
  - dodatnia → ujemna
  - mała → bardzo duża
  - oczekiwana → nieoczekiwana
- Porównuj zachowanie aplikacji przed i po osiągnięciu skrajnych wartości.

==================================================
7. GENERALIZATION
==================================================

Pattern może występować w:

- Koszykach zakupowych.
- Programach lojalnościowych.
- Systemach punktowych.
- Saldzie użytkownika.
- Walletach i portfelach cyfrowych.
- Kredytach promocyjnych.
- Licznikach użyć.
- Systemach limitów i quota.

==================================================
8. WHY IT WORKS
==================================================

Typy liczbowe posiadają ograniczony zakres wartości. Po przekroczeniu maksymalnej wartości liczba zostaje zinterpretowana jako początek zakresu, co powoduje powstanie stanu nieprzewidzianego przez logikę biznesową. Aplikacja operuje dalej na błędnej wartości, umożliwiając obejście założeń biznesowych.

==================================================
9. SEEN IN
==================================================

- PortSwigger Web Security Academy:
  Integer overflow leading to price manipulation.

- Przypadki przepełnienia liczników, salda lub punktów w aplikacjach biznesowych.
```
