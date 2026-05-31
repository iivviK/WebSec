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
- roleId
- userRoleId
- groupId
- permissionId
- Pola związane z uprawnieniami ujawnione w JSON response
- Pełne obiekty użytkownika zwracane przez API
- Referer
- Origin
- X-User
- X-Role
- X-Admin
- X-Forwarded-User
- X-Authenticated-User

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
- Czy response ujawnia pola związane z uprawnieniami?
- Czy mogę dopisać dodatkowe pola do requestu?
- Czy backend stosuje allowlistę pól?
- Czy mogę modyfikować atrybuty niewidoczne w UI?
- Czy decyzja autoryzacyjna zależy od nagłówków?
- Czy backend ufa Referer?
- Czy backend ufa Origin?
- Czy użytkownik może wpływać na dane używane podczas autoryzacji?

==================================================
6. DETECTION
==================================================

- Zidentyfikuj dane związane z uprawnieniami
- Zmodyfikuj wartości sugerujące rolę lub poziom dostępu
- Porównaj zachowanie aplikacji przed i po zmianie
- Zweryfikuj dostęp do funkcji uprzywilejowanych
- Analiza pełnych obiektów JSON
- Próba dodania niewidocznych pól do requestu
- Porównanie modelu danych z formularzem UI
- Testowanie over-postingu / mass assignment
- Modyfikacja Referer
- Usunięcie Referer
- Modyfikacja Origin
- Dodawanie własnych nagłówków
- Porównanie zachowania dla różnych wartości nagłówków

==================================================
7. GENERALIZATION
==================================================

- Role użytkowników
- Systemy RBAC
- Grupy użytkowników
- Poziomy dostępu
- Uprawnienia administracyjne
- Mechanizmy autoryzacji oparte o atrybuty
- API REST
- Aktualizacja profilu użytkownika
- Operacje PATCH / PUT
- Frameworki automatycznie mapujące obiekty
- Mechanizmy ORM
- Cookies
- Query Parameters
- Hidden Fields
- HTTP Headers
- JWT Claims
- Client-side Storage

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
