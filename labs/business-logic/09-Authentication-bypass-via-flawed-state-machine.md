```md
LAB: Authentication bypass via flawed state machine
Kategoria: Authentication / State Machine
Utworzono: 2026-06-14 21:00 (Europe/Amsterdam)

---

1. SCAN (Entry & Signals)

Endpointy:

* POST /login
* GET /role-selector
* POST /role-selector
* GET /admin

Obiekty biznesowe:

* User Session
* Authentication State
* User Role
* Authentication Workflow

Sygnały:

* logowanie składa się z wielu etapów
* po poprawnym loginie następuje redirect do dodatkowego kroku
* nowa sesja tworzona jest przed zakończeniem całego procesu
* użytkownik musi wybrać rolę przed wejściem do aplikacji
* istnieje panel administracyjny dostępny pod osobnym endpointem
* workflow opiera się na przejściach pomiędzy kolejnymi stanami

---

2. KONTEKST APLIKACJI

Aplikacja wykorzystuje wieloetapowy proces uwierzytelniania.

Normalny proces:

1. Użytkownik podaje login i hasło.
2. Backend weryfikuje credentiale.
3. Tworzona jest sesja.
4. Użytkownik zostaje przekierowany do wyboru roli.
5. Rola zostaje przypisana.
6. Użytkownik trafia do aplikacji.

Dostępne role:

* user
* content-author

Założenie bezpieczeństwa:

* użytkownik powinien uzyskać dostęp dopiero po ukończeniu całego workflow
* przypisanie roli powinno nastąpić przed przyznaniem uprawnień

---

3. OBSERWACJA

Po poprawnym logowaniu:

```http
POST /login
```

serwer zwraca:

```http
302 Found
Location: /role-selector
Set-Cookie: session=...
```

Oznacza to, że sesja zostaje utworzona jeszcze przed ukończeniem procesu wyboru roli.

Próba wejścia na:

```text
/admin
```

bezpośrednio z poziomu strony wyboru roli nie daje dostępu administracyjnego.

Istotnym elementem workflow jest request:

```http
GET /role-selector
```

wykonywany bezpośrednio po logowaniu.

---

4. HIPOTEZA

Aplikacja może niepoprawnie obsługiwać sytuację, w której wymagany etap workflow nie zostanie wykonany.

Pominięcie kroku odpowiedzialnego za wybór roli może pozostawić użytkownika w stanie nieobsłużonym przez logikę backendu.

---

5. ANALIZA MECHANIZMU

Normalny przepływ:

```text
Anonymous
    ↓
POST /login
    ↓
Session Created
    ↓
GET /role-selector
    ↓
Role Assignment
    ↓
Application Access
```

Zaobserwowany przepływ:

```text
Anonymous
    ↓
POST /login
    ↓
Session Created
    ↓
DROP GET /role-selector
    ↓
Browse /
    ↓
Administrator Role
    ↓
/admin
```

Kluczowa obserwacja:

Problem nie wynika z manipulacji parametrem:

```text
role=admin
```

Problem wynika z pominięcia wymaganego przejścia w workflow.

Backend zakłada, że etap wyboru roli zawsze zostanie wykonany.

Po jego pominięciu użytkownik trafia do nieobsłużonego stanu skutkującego przypisaniem uprawnień administratora.

Powstaje rozjazd pomiędzy:

* założonym workflow,
* rzeczywistym stanem aplikacji.

---

6. REPRODUCTION / EXPLOIT

6.1. Zaloguj się jako:

```text
wiener:peter
```

6.2. Włącz Intercept w Burp Suite.

6.3. Wyślij:

```http
POST /login
```

6.4. Przepuść request logowania.

6.5. Przechwyć:

```http
GET /role-selector
```

6.6. Odrzuć request (`Drop`).

6.7. Przejdź na stronę główną aplikacji.

6.8. Otwórz:

```text
/admin
```

6.9. Zaobserwuj dostęp do panelu administratora.

6.10. Usuń użytkownika:

```text
carlos
```

6.11. Potwierdź rozwiązanie laboratorium.

---

7. IMPACT

Atakujący może:

* uzyskać uprawnienia administratora
* omijać wymagane etapy uwierzytelniania
* wykonywać operacje administracyjne
* przejmować funkcje zarządzania aplikacją
* doprowadzić do pełnej kompromitacji systemu

---

8. DEBUGGING / PITFALLS

Główna pułapka:

Skupienie się na parametrze:

```text
role=admin
```

Błędny kierunek:

* parameter tampering
* manipulacja rolą
* brute force wartości role

Właściwy kierunek:

* mapowanie pełnego workflow logowania
* analiza redirectów
* identyfikacja stanów pośrednich
* testowanie zachowania po pominięciu poszczególnych kroków

Szczególnie istotne:

* moment utworzenia sesji
* dodatkowe kroki po logowaniu
* przejścia pomiędzy stanami
* przypadki niepełnego workflow

---

9. MENTAL MODEL / PATTERN

Pattern Candidate:

Workflow Default-State Privilege Escalation

Core Idea:

Aplikacja zakłada wykonanie wszystkich kroków workflow.

Pominięcie wymaganego etapu pozostawia użytkownika w nieobsłużonym stanie skutkującym nadaniem nadmiernych uprawnień.

Pytania przewodnie:

* Czy workflow zawiera obowiązkowe kroki po logowaniu?
* Kiedy tworzona jest sesja?
* Co stanie się po pominięciu danego etapu?
* Jak aplikacja zachowuje się przy niepełnym stanie użytkownika?
* Czy istnieją niebezpieczne wartości domyślne?

Typowe miejsca występowania:

* MFA
* role selection
* account activation
* onboarding
* email verification
* password reset workflows

---

10. WHY IT WORKS

Backend tworzy sesję jeszcze przed zakończeniem procesu przypisywania roli.

Aplikacja zakłada, że użytkownik zawsze przejdzie przez etap:

```http
GET /role-selector
```

Założenie to nie jest jednak egzekwowane.

Po pominięciu wymaganego kroku użytkownik trafia do nieobsłużonego stanu workflow, który skutkuje przypisaniem uprawnień administratora.

W efekcie możliwe jest uzyskanie dostępu do panelu administracyjnego bez ukończenia przewidzianego procesu uwierzytelniania.

```
