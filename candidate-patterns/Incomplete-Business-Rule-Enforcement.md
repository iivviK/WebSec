```md
NAME:
Incomplete Business Rule Enforcement

STATUS:
PATTERN CANDIDATE

CLASS:
Business Logic

==================================================
1. CORE IDEA
==================================================

Aplikacja implementuje mechanizm egzekwowania reguły biznesowej,
ale kontrola obejmuje jedynie część możliwych przypadków naruszenia.

W efekcie ograniczenie można obejść poprzez użycie ścieżki,
stanu lub kombinacji działań, których walidacja nie uwzględnia.

==================================================
2. ROOT CAUSE
==================================================

Backend implementuje uproszczoną wersję reguły biznesowej
i sprawdza jedynie lokalny warunek zamiast rzeczywistego celu biznesowego.

System weryfikuje symptom naruszenia reguły,
a nie samą regułę.

==================================================
3. SIGNALS
==================================================

- ograniczenie biznesowe wydaje się istnieć
- część oczywistych prób jest blokowana
- niewielka zmiana sekwencji działań omija ograniczenie
- komunikaty sugerują poprawną walidację
- kontrola działa dla prostych przypadków
- alternatywna kombinacja operacji daje inny rezultat
- aplikacja śledzi tylko fragment stanu biznesowego

==================================================
4. PREREQUISITES
==================================================

- istnieje reguła biznesowa ograniczająca działanie użytkownika
- użytkownik może wykonywać operacje wielokrotnie
- istnieje więcej niż jeden możliwy stan lub sekwencja działań
- backend nie modeluje pełnego kontekstu biznesowego

==================================================
5. QUESTIONS
==================================================

- Jaki jest rzeczywisty cel reguły biznesowej?
- Co backend faktycznie sprawdza?
- Czy backend sprawdza skutek czy tylko konkretny przypadek?
- Jakie alternatywne sekwencje działań prowadzą do tego samego rezultatu?
- Jakie stany nie są uwzględnione przez walidację?
- Czy mogę osiągnąć ten sam efekt inną kombinacją operacji?

==================================================
6. DETECTION
==================================================

Testy:

- powtarzanie operacji w różnych kolejnościach
- naprzemienne wykonywanie podobnych operacji
- zmiana stanu pomiędzy próbami
- porównywanie zachowania dla różnych sekwencji działań
- testowanie kombinacji funkcji prowadzących do tego samego celu

Szczególnie zwracaj uwagę na:

- limity użycia
- kupony rabatowe
- programy lojalnościowe
- punkty i kredyty
- checkout flows
- wieloetapowe procesy biznesowe

==================================================
7. GENERALIZATION
==================================================

Pattern może występować w:

- systemach rabatowych
- voucherach
- promocjach
- programach lojalnościowych
- limitach zakupowych
- cashback
- systemach punktowych
- limitach wykorzystania zasobów
- wieloetapowych procesach biznesowych

==================================================
8. WHY IT WORKS
==================================================

Backend egzekwuje uproszczoną reprezentację reguły biznesowej.

Atakujący znajduje kombinację działań,
która narusza rzeczywisty cel biznesowy,
jednocześnie pozostając zgodna z implementowaną walidacją.

==================================================
9. SEEN IN
==================================================

- PortSwigger: Flawed enforcement of business rules

==================================================
10. VALIDATION STATUS
==================================================

OBSERVED:
- 1 przypadek

REQUIRED:
- minimum 2-3 dodatkowe przypadki

DECISION:
- nie promować jeszcze do pełnego Pattern Card
- obserwować kolejne laby Business Logic
- sprawdzić czy CORE IDEA powtarza się niezależnie od kuponów
```
