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

- Funkcje administracyjne niewidoczne w UI
- Funkcje administracyjne dostępne po bezpośrednim wywołaniu endpointu
- Brak różnicy zachowania między administratorem a użytkownikiem
- Wrażliwe operacje wykonywane bez kontroli roli
- Funkcjonalność ukrywana zamiast chroniona
- Frontend odpowiada za ograniczanie dostępu
- Nieudokumentowane lub trudne do odnalezienia endpointy

==================================================
4. PREREQUISITES
==================================================

- Istnienie funkcji administracyjnej
- Możliwość wywołania funkcji przez użytkownika
- Brak backendowej kontroli uprawnień

==================================================
5. QUESTIONS
==================================================

- Czy backend sprawdza rolę użytkownika?
- Czy ta operacja powinna być dostępna dla każdego?
- Co się stanie po bezpośrednim wywołaniu funkcji?
- Czy funkcja jest jedynie ukryta w interfejsie?
- Czy istnieją dodatkowe endpointy niewidoczne w UI?
- Czy lokalizacja funkcji jest traktowana jako zabezpieczenie?

==================================================
6. DETECTION
==================================================

- Wykonanie funkcji jako użytkownik nieuprzywilejowany
- Bezpośrednie wywołanie endpointu administracyjnego
- Porównanie zachowania dla różnych poziomów uprawnień
- Analiza kodu JavaScript i źródeł strony
- Poszukiwanie ukrytych funkcji i endpointów
- Weryfikacja czy backend egzekwuje autoryzację

==================================================
7. GENERALIZATION
==================================================

- Panele administracyjne
- Funkcje moderatorskie
- Zarządzanie użytkownikami
- Zarządzanie konfiguracją
- Operacje utrzymaniowe
- Funkcje ukrywane przed użytkownikiem
- Funkcje dostępne poza główną nawigacją

==================================================
8. WHY IT WORKS
==================================================

Aplikacja zakłada, że dostęp do funkcji administracyjnej jest ograniczony przez jej ukrycie lub lokalizację.

Ponieważ backend nie sprawdza uprawnień, każdy użytkownik może wykonać operację administracyjną.

==================================================
9. SEEN IN
==================================================

- PortSwigger: Unprotected admin functionality
- PortSwigger: Unprotected admin functionality with unpredictable URL
```
