```md
# Authentication Oracle

CLASS: Authentication / Information Disclosure

---

1. CORE IDEA

---

Aplikacja ujawnia informacje o stanie uwierzytelniania poprzez obserwowalne różnice w odpowiedziach.

---

2. ROOT CAUSE

---

Backend wykonuje różne ścieżki logiki dla różnych stanów uwierzytelniania i pozwala, aby wynik tej decyzji był widoczny dla użytkownika.

---

3. SIGNALS

---

* różne komunikaty błędów logowania
* różnice długości odpowiedzi
* różnice liczby słów lub linii
* różnice w strukturze HTML
* różnice w nagłówkach HTTP
* różnice w redirectach
* zmiana zachowania po wielu próbach logowania
* komunikaty typu:

  * account locked
  * too many attempts
  * incorrect password
  * invalid username

---

4. PREREQUISITES

---

* aplikacja podejmuje decyzję zależną od stanu użytkownika
* istnieją co najmniej dwa różne stany backendowe
* wynik tej decyzji wpływa na odpowiedź aplikacji
* atakujący może wielokrotnie obserwować odpowiedzi

---

5. QUESTIONS

---

* Czy odpowiedź zmienia się dla różnych użytkowników?
* Czy odpowiedź zmienia się dla istniejących i nieistniejących kont?
* Czy pojawiają się różnice po wielu próbach?
* Czy istnieją różnice w redirectach?
* Czy istnieją różnice w długości odpowiedzi?
* Czy aplikacja ujawnia wynik pośrednich decyzji backendu?

---

6. DETECTION

---

* porównywanie odpowiedzi dla różnych danych wejściowych
* sortowanie odpowiedzi według:

  * Length
  * Words
  * Lines
* analiza redirectów
* analiza nagłówków HTTP
* wymuszanie zmian stanu (np. lockout)
* wyszukiwanie anomalii i outlierów

---

7. GENERALIZATION

---

Pattern może występować w:

* logowaniu
* resetowaniu hasła
* rejestracji
* odzyskiwaniu konta
* aktywacji konta
* invitation flows
* MFA
* mechanizmach lockout
* systemach anty-bruteforce

---

8. WHY IT WORKS

---

Backend zna informację, której użytkownik nie powinien znać (np. czy konto istnieje).

Jeżeli wynik tej decyzji wpływa na odpowiedź aplikacji, powstaje oracle umożliwiające pozyskanie tej informacji bez bezpośredniego dostępu do systemu.

Mechanizm:

Stan backendu → różnica w odpowiedzi → wyciek informacji → dalszy atak

---

9. SEEN IN

---

* Username Enumeration via Different Responses
* Username Enumeration via Subtly Different Responses
* Username Enumeration via Account Lock

```
