```md
NAME:
Client-Controlled Authorization Data

CLASS:
Access Control

==================================================
1. CORE IDEA
==================================================

Informacje wykorzystywane do podjęcia decyzji autoryzacyjnej są kontrolowane przez użytkownika.

==================================================
2. ROOT CAUSE
==================================================

Backend ufa danym autoryzacyjnym pochodzącym od klienta zamiast korzystać z własnego źródła prawdy.

==================================================
3. SIGNALS
==================================================

- role=admin
- isAdmin=true
- accessLevel=premium
- permission=full
- group=administrators
- Uprawnienia widoczne w cookie
- Uprawnienia widoczne w parametrach requestu
- Uprawnienia widoczne w nagłówkach HTTP

==================================================
4. PREREQUISITES
==================================================

- Istnienie różnych poziomów uprawnień
- Możliwość modyfikacji danych wysyłanych przez klienta
- Backend wykorzystujący te dane do autoryzacji

==================================================
5. QUESTIONS
==================================================

- Skąd backend pobiera informacje o uprawnieniach?
- Czy użytkownik może zmienić dane autoryzacyjne?
- Czy zmiana tych danych wpływa na dostęp?
- Czy backend posiada własne źródło informacji o roli?

==================================================
6. DETECTION
==================================================

- Zidentyfikuj dane związane z uprawnieniami
- Zmodyfikuj wartości sugerujące rolę lub poziom dostępu
- Porównaj zachowanie aplikacji przed i po zmianie
- Zweryfikuj dostęp do funkcji uprzywilejowanych

==================================================
7. GENERALIZATION
==================================================

- Role użytkowników
- Systemy RBAC
- Grupy użytkowników
- Poziomy dostępu
- Uprawnienia administracyjne
- Mechanizmy autoryzacji oparte o atrybuty

==================================================
8. WHY IT WORKS
==================================================

Backend zakłada, że informacje o uprawnieniach przesłane przez klienta są prawidłowe.

Ponieważ użytkownik kontroluje te dane, może wpłynąć na decyzję autoryzacyjną i uzyskać dostęp do funkcji, które powinny być niedostępne.

==================================================
9. SEEN IN
==================================================

- PortSwigger: User role controlled by request parameter
```
