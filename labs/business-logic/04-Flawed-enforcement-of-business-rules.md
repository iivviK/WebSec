```md
LAB: Flawed enforcement of business rules
Kategoria: Business Logic
Utworzono: 2026-06-02 18:xx (Europe/Amsterdam)

---

1. SCAN (Entry & Signals)

   Endpointy:

   * POST /cart/coupon
   * POST /cart
   * POST /cart/checkout
   * POST /newsletter

   Obiekty biznesowe:

   * Coupon
   * Shopping Cart
   * Product
   * Discount
   * User Account

   Sygnały:

   * kod rabatowy NEWCUST5
   * kod dla nowych użytkowników
   * możliwość zapisania się do newslettera
   * dodatkowy kod rabatowy po zapisaniu do newslettera
   * komunikat "Coupon already applied"

---

2. KONTEKST APLIKACJI

   Aplikacja udostępnia sklep internetowy z mechanizmem kuponów rabatowych.

   Użytkownik może:

   * rejestrować konto
   * logować się
   * dodawać produkty do koszyka
   * używać kuponów rabatowych
   * zapisać się do newslettera i otrzymać dodatkowy kupon

   Reguła biznesowa zakłada ograniczenie możliwości wielokrotnego wykorzystywania rabatów.

---

3. OBSERWACJA

   Początkowo analiza skupiła się na sposobie śledzenia wykorzystania pojedynczego kuponu.

   Testy obejmowały:

   * ponowne użycie tego samego kuponu
   * zmianę koszyka
   * analizę stanu aplikacji po zastosowaniu kuponu

   Kluczowa informacja została przeoczona podczas początkowego przeglądu aplikacji.

   Na dole strony znajdowała się funkcjonalność newslettera generująca dodatkowy kupon rabatowy.

---

4. HIPOTEZA

   Backend śledzi wykorzystanie kuponów i ogranicza ich wielokrotne użycie.

   Mechanizm walidacji może jednak sprawdzać jedynie część rzeczywistej reguły biznesowej.

---

5. ANALIZA MECHANIZMU

   Aplikacja poprawnie blokuje wielokrotne użycie tego samego kuponu bezpośrednio po sobie.

   Przykład:

   * NEWCUST5
   * NEWCUST5

   skutkuje blokadą.

   Jednak użycie dwóch różnych kuponów naprzemiennie powoduje obejście ograniczenia.

   Przykład:

   * NEWCUST5
   * NEWSLETTER
   * NEWCUST5
   * NEWSLETTER

   Backend sprawdza jedynie lokalny przypadek ponownego użycia tego samego kuponu zamiast egzekwować rzeczywisty cel biznesowy ograniczenia rabatów.

---

6. REPRODUCTION / EXPLOIT

   1. Zaloguj się do aplikacji.
   2. Dodaj produkt do koszyka.
   3. Użyj kuponu NEWCUST5.
   4. Zapisz się do newslettera.
   5. Odbierz drugi kupon rabatowy.
   6. Zastosuj drugi kupon.
   7. Ponownie użyj NEWCUST5.
   8. Stosuj oba kupony naprzemiennie.
   9. Doprowadź cenę produktu do poziomu umożliwiającego zakup.
   10. Finalizuj zamówienie.

---

7. IMPACT

   Użytkownik może wielokrotnie korzystać z rabatów mimo istniejących ograniczeń biznesowych.

   Prowadzi to do:

   * obchodzenia zasad promocji
   * uzyskania nieautoryzowanych zniżek
   * zakupu produktów poniżej zakładanej ceny

---

8. DEBUGGING / PITFALLS

   Główna pułapka:

   Zbyt szybkie zejście na poziom modelowania backendu przed pełnym rozpoznaniem funkcjonalności aplikacji.

   Błędny kierunek:

   * analiza sesji
   * analiza koszyka
   * analiza sposobu śledzenia wykorzystania pojedynczego kuponu

   Właściwy kierunek:

   * pełny przegląd wszystkich źródeł kuponów
   * identyfikacja wszystkich funkcji wpływających na system rabatowy

---

9. MENTAL MODEL / PATTERN

   Jeżeli aplikacja implementuje ograniczenie biznesowe, najpierw należy ustalić rzeczywisty cel tej reguły.

   Następnie należy sprawdzić, czy backend egzekwuje pełną regułę biznesową, czy jedynie wybrane przypadki jej naruszenia.

   Szczególnie warto szukać:

   * alternatywnych kuponów
   * alternatywnych źródeł rabatów
   * alternatywnych sekwencji działań prowadzących do tego samego efektu

---

10. WHY IT WORKS

Backend nie kontroluje rzeczywistego wykorzystania korzyści biznesowej.

Zamiast tego sprawdza jedynie bezpośrednie ponowne użycie tego samego kuponu.

Naprzemienne używanie różnych kuponów pozwala więc obejść ograniczenie i wielokrotnie uzyskiwać rabaty.

```
