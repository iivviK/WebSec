```md
NAME:
Inconsistent Validation Across Workflows

CLASS:
Business Logic

==================================================
1. CORE IDEA
==================================================

Ten sam obiekt biznesowy podlega różnym regułom walidacji w różnych workflow.

==================================================
2. ROOT CAUSE
==================================================

Backend implementuje oddzielne kontrole dla tego samego obiektu biznesowego i nie egzekwuje tych samych ograniczeń we wszystkich ścieżkach procesu.

==================================================
3. SIGNALS
==================================================

- możliwość utworzenia i późniejszej edycji obiektu
- wiele funkcji operujących na tym samym obiekcie
- ograniczenia obecne w jednym workflow
- brak tych samych ograniczeń w innym workflow
- funkcje "edit profile", "change email", "update account"
- alternatywne ścieżki modyfikacji danych

==================================================
4. PREREQUISITES
==================================================

- ten sam obiekt dostępny w wielu workflow
- możliwość modyfikacji obiektu po utworzeniu
- możliwość przejścia przez alternatywną ścieżkę procesu
- niespójna walidacja pomiędzy workflow

==================================================
5. QUESTIONS
==================================================

- Czy ten obiekt można zmienić później?
- Czy wszystkie workflow stosują te same reguły?
- Czy ograniczenie istnieje tylko podczas tworzenia?
- Czy istnieje funkcja edycji tego obiektu?
- Czy mogę osiągnąć ten sam stan inną ścieżką?

==================================================
6. DETECTION
==================================================

Testy:

- utwórz obiekt zgodnie z regułami
- zmodyfikuj obiekt po utworzeniu
- porównaj walidację pomiędzy workflow
- spróbuj osiągnąć ten sam stan alternatywną ścieżką
- porównaj komunikaty błędów

Szczególnie zwracaj uwagę na:

- change email
- edit profile
- account settings
- membership settings
- role management

==================================================
7. GENERALIZATION
==================================================

Pattern często występuje w:

- rejestracji i zarządzaniu kontem
- zmianie email
- profilach użytkowników
- systemach członkostwa
- zarządzaniu rolami
- panelach administracyjnych
- checkout flows
- systemach kuponów

==================================================
8. WHY IT WORKS
==================================================

Atakujący wykorzystuje workflow, który operuje na tym samym obiekcie biznesowym, ale nie egzekwuje tych samych ograniczeń co pozostałe części aplikacji.

==================================================
9. SEEN IN
==================================================

- [[inconsistent-security-controls]]
```
