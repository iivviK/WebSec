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

- Informacje o uprawnieniach widoczne po stronie klienta
- Dane autoryzacyjne obecne w requestach HTTP
- Dane związane z rolą użytkownika zwracane przez API
- Pełne obiekty użytkownika zwracane przez backend
- Nagłówki wpływające na decyzje autoryzacyjne
- Parametry sugerujące poziom dostępu lub rolę
- Atrybuty użytkownika niewidoczne w UI, ale obecne w modelu danych

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
- Czy użytkownik może wpływać na dane używane podczas autoryzacji?
- Czy zmiana tych danych wpływa na dostęp?
- Czy backend posiada własne źródło informacji o roli?
- Czy response ujawnia pola związane z uprawnieniami?
- Czy mogę dopisać dodatkowe pola do requestu?
- Czy backend stosuje allowlistę pól?
- Czy mogę modyfikować atrybuty niewidoczne w UI?
- Czy decyzja autoryzacyjna zależy od danych kontrolowanych przez klienta?

==================================================
6. DETECTION
==================================================

- Zidentyfikuj dane związane z uprawnieniami
- Zmodyfikuj wartości wpływające na autoryzację
- Porównaj zachowanie aplikacji przed i po zmianie
- Zweryfikuj dostęp do funkcji uprzywilejowanych
- Analizuj pełne odpowiedzi API
- Porównaj model danych z formularzem UI
- Testuj możliwość dodawania niewidocznych pól
- Testuj over-posting i mass assignment
- Testuj wpływ nagłówków na decyzje autoryzacyjne

==================================================
7. GENERALIZATION
==================================================

- Role użytkowników
- Systemy RBAC
- Poziomy dostępu
- Uprawnienia administracyjne
- Mechanizmy autoryzacji oparte o atrybuty
- Zarządzanie profilami użytkowników
- API i aplikacje webowe
- Dane przechowywane po stronie klienta
- Systemy wykorzystujące dane użytkownika do podejmowania decyzji bezpieczeństwa

==================================================
8. WHY IT WORKS
==================================================

Backend zakłada, że informacje o uprawnieniach przesłane przez klienta są prawidłowe.

Ponieważ użytkownik kontroluje te dane, może wpłynąć na decyzję autoryzacyjną i uzyskać dostęp do funkcji, które powinny być niedostępne.

==================================================
9. SEEN IN
==================================================

- PortSwigger: User role controlled by request parameter
- PortSwigger: User role can be modified in user profile
- PortSwigger: Referer-based access control
```
