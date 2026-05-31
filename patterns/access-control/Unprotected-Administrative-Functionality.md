```md
NAME:
Unprotected Administrative Functionality

CLASS:
Access Control

==================================================
1. CORE IDEA
==================================================

Funkcja administracyjna jest dostępna bez wymaganego sprawdzenia uprawnień po stronie backendu.

==================================================
2. ROOT CAUSE
==================================================

Backend nie egzekwuje autoryzacji dla operacji administracyjnych.

==================================================
3. SIGNALS
==================================================

- Funkcje administracyjne dostępne dla zwykłych użytkowników
- Brak różnicy zachowania między administratorem a użytkownikiem
- Wrażliwe operacje wykonywane bez kontroli roli
- Dostęp do funkcji po bezpośrednim wywołaniu endpointu

==================================================
4. PREREQUISITES
==================================================

- Istnienie funkcji administracyjnej
- Możliwość wywołania funkcji przez użytkownika

==================================================
5. QUESTIONS
==================================================

- Czy backend sprawdza rolę użytkownika?
- Czy ta operacja powinna być dostępna dla każdego?
- Co się stanie po bezpośrednim wywołaniu funkcji?

==================================================
6. DETECTION
==================================================

- Wykonanie funkcji jako użytkownik nieuprzywilejowany
- Porównanie zachowania dla różnych poziomów uprawnień
- Próba bezpośredniego dostępu do funkcji

==================================================
7. GENERALIZATION
==================================================

- Panele administracyjne
- Funkcje moderatorskie
- Zarządzanie użytkownikami
- Zarządzanie konfiguracją
- Operacje utrzymaniowe

==================================================
8. WHY IT WORKS
==================================================

Aplikacja zakłada, że dostęp do funkcji administracyjnej
jest ograniczony przez jej ukrycie lub lokalizację.

Ponieważ backend nie sprawdza uprawnień,
każdy użytkownik może wykonać operację administracyjną.

==================================================
9. SEEN IN
==================================================

- PortSwigger: Unprotected admin functionality
```
