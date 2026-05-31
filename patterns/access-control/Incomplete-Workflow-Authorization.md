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

- confirmed=true
- finalize=true
- execute=true
- complete=true
- approve=true
- Wieloetapowe procesy
- Ekrany potwierdzenia
- Kreatory (wizard)
- Workflow administracyjne

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
- Wywołaj go bez przechodzenia wcześniejszych etapów
- Testuj parametry typu confirmed=true
- Testuj kroki approve/finalize/execute

==================================================
7. GENERALIZATION
==================================================

- Password Reset
- Checkout
- Approval Workflows
- Role Management
- Account Management
- Financial Transactions
- Administrative Actions

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
