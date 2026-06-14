```md

NAME

Workflow Default-State Privilege Escalation

CLASS

Authentication / State Machine / Authorization

---

## 1. CORE IDEA

Aplikacja wymaga wykonania określonego workflow przed przypisaniem końcowego stanu użytkownika.

Jeżeli jeden z obowiązkowych kroków zostanie pominięty, backend pozostawia użytkownika w niezainicjalizowanym lub nieobsłużonym stanie, który skutkuje przyznaniem nadmiernych uprawnień.

---

## 2. ROOT CAUSE

Logika aplikacji zakłada, że wszystkie kroki workflow zostaną wykonane.

Przypadek pominięcia wymaganego kroku nie jest poprawnie obsłużony, przez co backend korzysta z niebezpiejnego stanu domyślnego.

---

## 3. SIGNALS

* wieloetapowy proces logowania
* wieloetapowy onboarding
* MFA
* role selection
* account verification
* password reset workflow
* redirect pomiędzy kolejnymi etapami
* nowa sesja tworzona przed ukończeniem całego procesu
* możliwość ręcznego przechodzenia pomiędzy endpointami workflow

---

## 4. PREREQUISITES

* możliwość przerwania lub pominięcia kroku workflow
* backend utrzymuje częściowy stan użytkownika
* brak poprawnej obsługi nieukończonego workflow

---

## 5. QUESTIONS

* Co stanie się po pominięciu tego kroku?
* Czy backend wymaga ukończenia wszystkich etapów?
* Czy sesja jest tworzona przed zakończeniem workflow?
* Czy istnieją wartości domyślne dla roli lub uprawnień?
* Jak zachowuje się aplikacja przy niepełnym stanie użytkownika?

---

## 6. DETECTION

1. Zidentyfikuj wszystkie etapy workflow.
2. Ustal moment utworzenia sesji.
3. Zatrzymaj lub pomiń jeden z kroków.
4. Spróbuj uzyskać dostęp do zasobów chronionych.
5. Obserwuj różnice w przypisanych rolach i uprawnieniach.
6. Szukaj stanów nieobsłużonych przez logikę backendu.

---

## 7. GENERALIZATION

Mechanizm może występować w:

* logowaniu
* MFA
* wyborze roli
* aktywacji konta
* resetowaniu hasła
* workflow administracyjnych
* onboardingach użytkownika

Wspólnym elementem jest błędna obsługa niepełnego stanu procesu.

---

## 8. WHY IT WORKS

Backend podejmuje decyzję autoryzacyjną dla użytkownika znajdującego się w stanie pośrednim lub niezainicjalizowanym.

Brak wymuszenia poprawnego zakończenia workflow powoduje wykorzystanie niebezpiecznego stanu domyślnego.

---

## 9. SEEN IN

* PortSwigger: Authentication bypass via flawed state machine

```
