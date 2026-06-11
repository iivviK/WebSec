```md
# Trusted URL Construction from User Input

CLASS: Trust Boundary Violation / URL Generation

---

1. CORE IDEA

---

Aplikacja buduje bezpieczeństwowo istotne adresy URL na podstawie danych kontrolowanych przez użytkownika.

---

2. ROOT CAUSE

---

Backend ufa danym pochodzącym od klienta podczas generowania adresów URL wykorzystywanych w procesach bezpieczeństwa.

---

3. SIGNALS

---

* reset password
* account activation
* email verification
* invitation links
* magic links
* absolutne URL-e generowane przez backend
* użycie:

  * Host
  * X-Forwarded-Host
  * X-Host
  * Forwarded
* obecność proxy lub middleware

---

4. PREREQUISITES

---

* aplikacja generuje linki bezpieczeństwa
* link zawiera token lub identyfikator operacji
* użytkownik otrzymuje link poza aplikacją
* część adresu URL pochodzi z danych kontrolowanych przez klienta

---

5. QUESTIONS

---

* Skąd backend bierze hostname?
* Czy host jest stały czy dynamiczny?
* Czy mogę wpłynąć na generowany URL?
* Czy aplikacja ufa forwarded headers?
* Czy link bezpieczeństwa trafia tam, gdzie powinien?

---

6. DETECTION

---

* analiza wszystkich nagłówków związanych z hostem
* modyfikacja:

  * Host
  * X-Forwarded-Host
  * X-Host
  * Forwarded
* inicjowanie workflowów generujących linki
* obserwacja miejsca dostarczenia tokenu
* analiza zachowania proxy i middleware

---

7. GENERALIZATION

---

Pattern może występować w:

* password reset
* account activation
* email verification
* invitation systems
* magic link authentication
* onboarding workflows
* federated authentication
* callback-based workflows

---

8. WHY IT WORKS

---

Linki bezpieczeństwa powinny być generowane na podstawie zaufanej konfiguracji serwera.

Jeżeli backend wykorzystuje dane kontrolowane przez użytkownika do budowy takich adresów, atakujący może spowodować wysłanie tokenów lub linków bezpieczeństwa do nieautoryzowanego miejsca.

Mechanizm:

User-controlled host → wygenerowany URL → błędny kanał dostarczenia → przejęcie tokenu lub workflow

---

9. SEEN IN

---

* Password Reset Poisoning via Middleware

```
