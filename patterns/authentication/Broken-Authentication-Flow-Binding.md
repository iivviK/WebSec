```md
# Broken Authentication Flow Binding

CLASS: Authentication / Workflow Logic

---

1. CORE IDEA

---

Poszczególne etapy procesu uwierzytelniania nie są poprawnie powiązane ze sobą.

---

2. ROOT CAUSE

---

Backend traktuje etapy uwierzytelniania jako niezależne lub nie weryfikuje spójności danych identyfikujących użytkownika pomiędzy kolejnymi krokami procesu.

---

3. SIGNALS

---

* wieloetapowe logowanie
* MFA / 2FA
* dodatkowe cookies używane podczas auth
* hidden fields przechowujące kontekst użytkownika
* session tworzona przed zakończeniem procesu auth
* różne identyfikatory użytkownika w różnych etapach
* endpointy typu:

  * /login2
  * /verify
  * /mfa
  * /challenge
* możliwość przechodzenia pomiędzy etapami poza normalnym flow

---

4. PREREQUISITES

---

* proces uwierzytelniania składa się z wielu kroków
* aplikacja przechowuje stan pomiędzy etapami
* istnieje więcej niż jedno źródło identyfikacji użytkownika
  lub więcej niż jeden etap weryfikacji

---

5. QUESTIONS

---

* Czy wszystkie etapy odnoszą się do tego samego użytkownika?
* Czy mogę pominąć któryś etap?
* Czy mogę przejść bezpośrednio do kolejnego kroku?
* Czy mogę zmienić kontekst użytkownika pomiędzy etapami?
* Czy backend sprawdza ukończenie wcześniejszych etapów?
* Czy istnieją dodatkowe cookies lub parametry opisujące tożsamość?

---

6. DETECTION

---

* analiza wszystkich cookies ustawianych podczas logowania
* analiza redirectów pomiędzy etapami
* próby pomijania kroków workflow
* manipulacja:

  * cookies
  * hidden fields
  * URL parameters
* bezpośredni dostęp do chronionych endpointów
* porównywanie stanu przed i po ukończeniu kolejnych etapów

---

7. GENERALIZATION

---

Pattern może występować w:

* MFA / 2FA
* password reset flows
* account activation
* email verification
* magic link authentication
* invitation workflows
* SSO
* federated authentication
* onboarding workflows

---

8. WHY IT WORKS

---

Proces uwierzytelniania zakłada, że kolejne kroki dotyczą tej samej tożsamości i tego samego stanu użytkownika.

Jeżeli backend nie wymusza tego powiązania, atakujący może:

* pominąć wymagany etap
* zmienić kontekst użytkownika
* wykorzystać częściowo ukończony proces uwierzytelniania

Mechanizm:

Rozdzielenie etapów auth → brak spójności stanu → obejście workflow → nieautoryzowany dostęp

---

9. SEEN IN

---

* 2FA Simple Bypass
* 2FA Broken Logic

```
