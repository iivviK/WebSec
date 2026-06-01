```md
NAME:
Incomplete Workflow Authorization

CLASS:
Access Control

==================================================
1. CORE IDEA
==================================================

Proces składający się z wielu kroków nie egzekwuje autoryzacji na każdym etapie wykonania.

==================================================
2. ROOT CAUSE
==================================================

Aplikacja zakłada, że wcześniejsze kroki procesu zostały wykonane poprawnie i pomija ponowną weryfikację uprawnień w kolejnych etapach.

==================================================
3. SIGNALS
==================================================

- Wieloetapowe procesy
- Ekrany potwierdzenia
- Kreatory (wizard)
- Workflow administracyjne
- Oddzielne kroki przygotowania i wykonania akcji
- Końcowe requesty realizujące rzeczywistą operację
- Procesy wymagające zatwierdzenia lub akceptacji

==================================================
4. PREREQUISITES
==================================================

- Proces składający się z wielu kroków
- Możliwość bezpośredniego wywołania późniejszego etapu
- Brak pełnej autoryzacji na jednym z kroków

==================================================
5. QUESTIONS
==================================================

- Czy mogę pominąć wcześniejsze kroki?
- Czy mogę wywołać krok końcowy bez przejścia procesu?
- Czy każdy etap sprawdza uprawnienia?
- Czy backend ufa stanowi procesu bardziej niż sesji użytkownika?

==================================================
6. DETECTION
==================================================

- Zmapuj cały workflow
- Zidentyfikuj request wykonujący finalną akcję
- Wywołaj końcowy etap bez przechodzenia wcześniejszych kroków
- Testuj możliwość pomijania etapów procesu
- Zweryfikuj autoryzację dla każdego kroku osobno

==================================================
7. GENERALIZATION
==================================================

- Procesy rejestracji
- Reset hasła
- Zakupy i checkout
- Workflow zatwierdzeń
- Zarządzanie rolami
- Zarządzanie kontami
- Operacje finansowe
- Operacje administracyjne
- Procesy wymagające potwierdzenia

==================================================
8. WHY IT WORKS
==================================================

Backend zakłada, że użytkownik przeszedł wcześniejsze etapy procesu i posiada odpowiednie uprawnienia.

Jeżeli końcowy krok nie wykonuje własnej autoryzacji, użytkownik może wywołać go bezpośrednio i ominąć zabezpieczenia obecne we wcześniejszych etapach.

==================================================
9. SEEN IN
==================================================

- PortSwigger: Multi-step process with no access control on one step
```
