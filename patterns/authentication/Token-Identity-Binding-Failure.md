```md
# Token-Identity Binding Failure

CLASS: Authentication / Identity Binding

---

1. CORE IDEA

---

Token bezpieczeństwa nie jest jednoznacznie powiązany z tożsamością, dla której ma być użyty.

---

2. ROOT CAUSE

---

Backend jednocześnie ufa tokenowi bezpieczeństwa oraz identyfikatorowi użytkownika pochodzącemu z innego źródła i nie weryfikuje ich wzajemnej zgodności.

---

3. SIGNALS

---

* token oraz username występują w tym samym workflow
* token oraz user identifier przesyłane są niezależnie
* formularze zawierają:

  * token
  * username
  * email
  * userId
* backend akceptuje dodatkowe identyfikatory mimo obecności tokenu
* operacje bezpieczeństwa wykonywane są na podstawie wielu źródeł tożsamości

---

4. PREREQUISITES

---

* istnieje token bezpieczeństwa
* token ma identyfikować użytkownika lub operację
* backend korzysta z dodatkowych danych identyfikujących użytkownika
* istnieje możliwość wpływu na te dane

---

5. QUESTIONS

---

* Czy token sam identyfikuje użytkownika?
* Czy backend używa dodatkowo username, email lub userId?
* Czy mogę zmienić identyfikator użytkownika bez zmiany tokenu?
* Czy token wskazuje konkretną tożsamość?
* Które źródło jest traktowane jako źródło prawdy?

---

6. DETECTION

---

* analiza wszystkich parametrów przesyłanych razem z tokenem
* manipulacja:

  * username
  * email
  * userId
  * accountId
* porównanie zachowania aplikacji po zmianie identyfikatora przy niezmienionym tokenie
* sprawdzenie czy backend ignoruje dane wynikające z tokenu

---

7. GENERALIZATION

---

Pattern może występować w:

* password reset
* account activation
* email verification
* invitation systems
* recovery workflows
* magic links
* account linking
* delegated access workflows

---

8. WHY IT WORKS

---

Token bezpieczeństwa powinien jednoznacznie określać użytkownika lub operację.

Jeżeli backend dodatkowo ufa danym dostarczanym przez klienta i nie sprawdza zgodności pomiędzy nimi, atakujący może wykorzystać prawidłowy token do wykonania operacji na innej tożsamości.

Mechanizm:

Token → poprawny

Identity → podmieniona

Brak powiązania → operacja wykonywana dla niewłaściwego użytkownika

---

9. SEEN IN

---

* Password Reset Broken Logic

```
