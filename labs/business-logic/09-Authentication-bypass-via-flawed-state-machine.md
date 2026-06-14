```md
LAB: Authentication bypass via flawed state machine
Kategoria: Authentication / State Machine
Utworzono: 14-06-2026 14:41 Europe/Amsterdam

# 1. CEL TESTU

Uzyskać dostęp do panelu administratora poprzez analizę procesu logowania i identyfikację błędów w obsłudze stanów uwierzytelniania.

# 2. KONTEKST APLIKACJI

Aplikacja wykorzystuje wieloetapowy proces logowania:

1. Użytkownik podaje login i hasło.
2. Backend tworzy nową sesję.
3. Użytkownik zostaje przekierowany do `/role-selector`.
4. Następuje wybór roli.
5. Użytkownik trafia do aplikacji.

Dostępne role:

* user
* content-author

W aplikacji istnieje również endpoint administracyjny:

```text
/admin
```

# 3. OBSERWACJA

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

Oznacza to, że sesja zostaje utworzona przed zakończeniem całego workflow.

Próba bezpośredniego wejścia na:

```text
/admin
```

z poziomu strony wyboru roli nie daje dostępu administracyjnego.

# 4. HIPOTEZA

Backend może niepoprawnie obsługiwać sytuację, w której wymagany etap workflow nie zostanie wykonany.

Pominięcie kroku wyboru roli może pozostawić użytkownika w nieobsłużonym stanie skutkującym błędnym przypisaniem uprawnień.

# 5. ANALIZA MECHANIZMU

Normalny workflow:

```text
Anonymous
    ↓
POST /login
    ↓
Session Created
    ↓
GET /role-selector
    ↓
Role Assigned
    ↓
Application Access
```

Workflow wykorzystany podczas ataku:

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

Backend zakłada, że krok wyboru roli zostanie wykonany.

Po jego pominięciu użytkownik trafia do stanu nieobsłużonego przez logikę aplikacji.

# 6. REPRODUCTION / EXPLOIT

1. Zalogować się jako:

```text
wiener:peter
```

2. Włączyć Intercept w Burp Suite.

3. Wysłać:

```http
POST /login
```

4. Przepuścić request logowania.

5. Przechwycić:

```http
GET /role-selector
```

6. Odrzucić request (`Drop`).

7. Przejść na stronę główną aplikacji.

8. Otworzyć:

```text
/admin
```

9. Uzyskać dostęp do panelu administratora.

10. Usunąć użytkownika:

```text
carlos
```

# 7. IMPACT

Nieautoryzowany użytkownik może uzyskać uprawnienia administratora.

Możliwe skutki:

* przejęcie funkcji administracyjnych
* usuwanie użytkowników
* modyfikacja danych
* pełna kompromitacja aplikacji

# 8. DEBUGGING / PITFALLS

* Skupienie się na parametrze:

```text
role=admin
```

prowadzi na fałszywy trop.

* Istotny był nie parametr roli, lecz stan workflow.

* Samo wejście na:

```text
/admin
```

z poziomu strony wyboru roli nie działa.

* Kluczowe jest całkowite pominięcie requestu:

```http
GET /role-selector
```

* Należy analizować moment utworzenia sesji oraz przejścia pomiędzy stanami.

# 9. MENTAL MODEL / PATTERN

Nazwa robocza:

```text
Workflow Default-State Privilege Escalation
```

Wzorzec:

```text
Workflow
    ↓
Required Step
    ↓
Step Skipped
    ↓
Uninitialized State
    ↓
Unsafe Default
    ↓
Privilege Escalation
```

Pytanie kontrolne:

```text
Co stanie się, jeśli wymagany krok workflow nie zostanie wykonany?
```

WHY IT WORKS:

Aplikacja nie wymusza ukończenia pełnego workflow i po pominięciu wymaganego etapu pozostawia użytkownika w stanie skutkującym nadmiernymi uprawnieniami.

```
