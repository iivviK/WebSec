```md
NAME

Required Parameter Assumption

CLASS

Business Logic

---

1. CORE IDEA

Aplikacja zakłada, że określony parametr zawsze będzie obecny w requestach generowanych przez frontend.

Backend może walidować wartość parametru, ale nie wymusza jego obecności.

W rezultacie usunięcie parametru prowadzi do pominięcia istotnej kontroli bezpieczeństwa lub logiki biznesowej.

---

2. ROOT CAUSE

Programista implementuje logikę:

"jeżeli parametr istnieje → sprawdź go"

zamiast:

"parametr musi istnieć, a następnie zostać poprawnie zwalidowany"

Backend ufa założeniom frontendu dotyczącym struktury requestu.

---

3. SIGNALS

* parametr wygląda na obowiązkowy
* parametr pochodzi z formularza bezpieczeństwa
* parametr uczestniczy w procesie autoryzacji lub weryfikacji
* endpoint wykonuje operację wysokiego ryzyka
* aplikacja zakłada określoną kolejność działań użytkownika

Przykłady:

* current-password
* otp
* verification-code
* csrf
* confirmation-token
* security-answer

---

4. PREREQUISITES

* możliwość modyfikacji requestów
* parametr kontrolowany przez klienta
* backend wykonuje logikę warunkowo
* brak twardego sprawdzania obecności parametru

---

5. QUESTIONS

* Czy parametr jest faktycznie wymagany?
* Co stanie się po jego usunięciu?
* Czy backend waliduje obecność czy tylko wartość?
* Czy operacja nadal zostanie wykonana?
* Czy istnieją alternatywne ścieżki wykonania przy braku parametru?

---

6. DETECTION

Dla podejrzanego parametru wykonaj:

1. poprawna wartość
2. błędna wartość
3. pusta wartość
4. brak parametru

Porównaj:

* status odpowiedzi
* treść odpowiedzi
* skutki biznesowe
* zmiany stanu aplikacji

---

7. GENERALIZATION

Wzorzec może występować wszędzie tam, gdzie aplikacja oczekuje określonego etapu procesu.

Szczególnie często dotyczy:

* zmiany hasła
* resetu hasła
* potwierdzenia email
* MFA
* transferów środków
* operacji administracyjnych

---

8. WHY IT WORKS

Backend nie traktuje obecności parametru jako warunku koniecznego do wykonania operacji.

Usunięcie parametru powoduje pominięcie kontroli bezpieczeństwa zamiast odrzucenia requestu.

Aplikacja egzekwuje wymaganie na poziomie UI, ale nie egzekwuje go na poziomie backendu.

---

9. SEEN IN

Weak isolation on dual-use endpoint

STATUS:
CANDIDATE PATTERN
(1 observation)

```
