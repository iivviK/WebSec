```md
NAME:
Client-Controlled Business Value

CLASS:
Business Logic

==================================================
1. CORE IDEA
==================================================

Aplikacja wykorzystuje wartość biznesową dostarczoną przez klienta zamiast wyliczać lub pobierać ją po stronie backendu.

==================================================
2. ROOT CAUSE
==================================================

Backend ufa danym przesłanym przez użytkownika i traktuje je jako źródło prawdy podczas podejmowania decyzji biznesowych.

==================================================
3. SIGNALS
==================================================

- price wysyłane przez klienta
- discount wysyłany przez klienta
- credits lub points kontrolowane przez klienta
- balance przesyłane w requestach
- shippingCost przesyłany przez klienta
- wartości biznesowe widoczne w hidden fields
- brak podpisu (HMAC)
- zmodyfikowana wartość pojawia się później w koszyku lub podsumowaniu

==================================================
4. PREREQUISITES
==================================================

- wartość biznesowa znajduje się w requestcie
- możliwość przechwycenia i modyfikacji requestów
- backend wykorzystuje dane klienta podczas obliczeń
- brak ponownej weryfikacji po stronie serwera
- brak pobierania wartości z backendowego źródła danych

==================================================
5. QUESTIONS
==================================================

- Czy użytkownik kontroluje wartość używaną przez backend?
- Czy backend pobiera tę wartość z własnej bazy danych?
- Czy mogę zmienić price, discount lub balance?
- Czy zmodyfikowana wartość pojawia się później w koszyku?
- Czy backend wykonuje własne obliczenia?
- Czy aplikacja ufa danym otrzymanym od klienta?

==================================================
6. DETECTION
==================================================

Testy:

- zmiana price na niższą wartość
- price=0
- price=-1
- bardzo duże wartości
- wartości ułamkowe
- wartości tekstowe
- manipulacja discount
- manipulacja shippingCost
- manipulacja credits lub points

Szczególnie zwracaj uwagę na:

- zmiany widoczne w koszyku
- zmiany widoczne podczas checkoutu
- brak błędów walidacyjnych
- zaakceptowanie zmodyfikowanych danych biznesowych

==================================================
7. GENERALIZATION
==================================================

Pattern często występuje w:

- sklepach internetowych
- koszykach zakupowych
- checkout flows
- systemach rabatowych
- programach lojalnościowych
- systemach punktowych
- voucherach
- marketplace
- aplikacjach finansowych
- systemach rozliczeń

==================================================
8. WHY IT WORKS
==================================================

Backend wykorzystuje dane kontrolowane przez użytkownika podczas podejmowania decyzji biznesowych. Atakujący może więc dostarczyć własne wartości i wpłynąć na wynik operacji bez naruszania mechanizmów technicznych aplikacji.

==================================================
9. SEEN IN
==================================================

- [[excessive-trust-in-client-side-controls]]
```
