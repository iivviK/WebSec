```md
NAME:

Optional Security Control

CLASS:

Business Logic

==================================================

1. CORE IDEA
   ==================================================

Kontrola bezpieczeństwa istnieje w procesie biznesowym, ale jej wykonanie nie jest obowiązkowe do wykonania operacji.

==================================================
2. ROOT CAUSE
=============

Backend traktuje kontrolę bezpieczeństwa jako opcjonalny element procesu zamiast wymaganego warunku wykonania operacji.

Logika biznesowa zakłada obecność lub wykonanie kontroli, ale nie egzekwuje jej w miejscu podejmowania decyzji.

==================================================
3. SIGNALS
==========

* parametr wyglądający na obowiązkowy
* krok weryfikacyjny przed wykonaniem operacji
* formularze bezpieczeństwa
* procesy wymagające potwierdzenia tożsamości
* dodatkowe kontrole przed wykonaniem akcji wysokiego ryzyka
* możliwość kontrolowania danych związanych z walidacją

Przykłady kontroli:

* current password
* OTP
* MFA
* verification code
* confirmation token
* approval token
* security question

==================================================
4. PREREQUISITES
================

* istnieje kontrola bezpieczeństwa
* aplikacja wykonuje operację wysokiego ryzyka
* backend nie wymusza obowiązkowego przejścia kontroli
* możliwa jest manipulacja requestem lub przebiegiem procesu

==================================================
5. QUESTIONS
============

* Czy ta kontrola jest naprawdę wymagana?
* Czy operacja zostanie wykonana bez tej kontroli?
* Czy backend sprawdza obecność kontroli?
* Czy backend sprawdza wynik kontroli?
* Czy istnieje ścieżka wykonania operacji bez przejścia wymaganej walidacji?
* Czy aplikacja zakłada, że użytkownik zawsze przejdzie przez określony krok?

==================================================
6. DETECTION
============

Dla kontroli bezpieczeństwa:

* usuń kontrolę
* pozostaw pustą wartość
* użyj niepoprawnej wartości
* pomiń etap procesu
* porównaj zachowanie aplikacji

Zwracaj uwagę na:

* wykonanie operacji mimo niespełnienia warunku
* brak błędów walidacyjnych
* różnicę pomiędzy wymaganiami procesu a zachowaniem backendu

==================================================
7. GENERALIZATION
=================

Pattern może występować w:

* zmianie hasła
* resetach haseł
* MFA
* OTP
* weryfikacji email
* approval workflows
* transferach środków
* operacjach administracyjnych
* procesach onboardingowych
* workflow wymagających dodatkowej autoryzacji

==================================================
8. WHY IT WORKS
===============

Proces biznesowy zakłada wykonanie określonej kontroli bezpieczeństwa przed wykonaniem operacji.

Backend nie egzekwuje jednak tej kontroli jako warunku koniecznego.

Atakujący może doprowadzić do wykonania operacji mimo niespełnienia wymagań bezpieczeństwa przewidzianych przez projekt procesu.

==================================================
9. SEEN IN
==========

* PortSwigger: Weak isolation on dual-use endpoint

```
