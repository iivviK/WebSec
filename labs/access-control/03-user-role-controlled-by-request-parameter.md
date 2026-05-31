USER ROLE CONTROLLED BY REQUEST PARAMETER – ACCESS CONTROL

Utworzono: 2026-01-21 16:32

---

1. LAB

   Platforma: PortSwigger Web Security Academy  
   Nazwa laba: User role controlled by request parameter

---

2. CEL TESTU

   2.1. Eskalacja uprawnień standardowego użytkownika  
   2.2. Uzyskanie dostępu do funkcjonalności administracyjnej  
   2.3. Usunięcie konta użytkownika „carlos”

---

3. KONTEKST TESTU

   3.1. Typ aplikacji  
        - Aplikacja webowa typu e-commerce

   3.2. Poziom dostępu  
        - Użytkownik uwierzytelniony (standardowy user)

   3.3. Uwierzytelnienie  
        - Poprawne logowanie (wiener:peter)

   3.4. Autoryzacja  
        - Oparta na danych kontrolowanych przez klienta (cookie)

   3.5. Zakres testu  
        - Access Control / Vertical Privilege Escalation

---

4. RECONNAISSANCE – MAPOWANIE APLIKACJI

   4.1. Zidentyfikowane publiczne adresy URL  
        - /  
        - /product?productId=X  
        - /login  

   4.2. Endpointy dostępne po zalogowaniu  
        - /my-account?id=wiener  

   4.3. Obserwacje  
        - Brak widocznych funkcji administracyjnych w UI  
        - Brak jawnej informacji o roli użytkownika  

---

5. HIPOTEZY

   5.1. Informacja o roli użytkownika jest przesyłana w requestach HTTP  
   5.2. Backend podejmuje decyzje autoryzacyjne na podstawie danych od klienta  

---

6. ANALIZA REQUESTÓW (DISCOVERY)

   6.1. Analiza requestu do /my-account  

   6.2. Zidentyfikowany nagłówek Cookie  
        - session=<session_id>  
        - Admin=false  

   6.3. Wniosek  
        - Rola użytkownika jest kontrolowana przez klienta  
        - Backend ufa wartości cookie przy podejmowaniu decyzji  

---

7. WERYFIKACJA KONTROLI DOSTĘPU

   7.1. Modyfikacja wartości cookie  
        - Admin=false → Admin=true  

   7.2. Rezultat  
        - Zmiana zachowania aplikacji  
        - Pojawienie się linku „Admin panel” w interfejsie  

   7.3. Dodatkowa obserwacja  
        - UI reaguje na zmianę cookie  
        - Autoryzacja nie jest zapisana po stronie serwera  

---

8. EKSPLOATACJA

   8.1. Przechwycenie requestu do /admin  

   8.2. Modyfikacja cookie w requestcie  
        - Admin=true  

   8.3. Efekt  
        - Dostęp do panelu administracyjnego  

   8.4. Wykonana akcja  
        - Usunięcie konta użytkownika „carlos”  

---

9. KLASYFIKACJA PODATNOŚCI

   9.1. Kategoria  
        - Broken Access Control  

   9.2. Typ  
        - User role controlled by request parameter  

   9.3. Root cause  
        - Autoryzacja oparta na danych kontrolowanych przez klienta  
        - Brak walidacji roli po stronie serwera  

   9.4. Uwaga  
        - Każdy request oceniany jest niezależnie  
        - Rola musi być dostarczana w każdym requestcie  

---

10. IMPACT

   10.1. Możliwości atakującego  
         - Eskalacja uprawnień do administratora  
         - Dostęp do panelu administracyjnego  
         - Usuwanie kont użytkowników  

   10.2. Konsekwencje  
         - Pełne przejęcie kontroli nad aplikacją  
         - Utrata integralności danych  
         - Krytyczny wpływ na bezpieczeństwo systemu  

---

11. REKOMENDACJE

   11.1. Przechowywać rolę użytkownika wyłącznie po stronie serwera  

   11.2. Backend powinien  
         - ignorować dane o roli przesyłane przez klienta  
         - podejmować decyzje autoryzacyjne na podstawie sesji  

   11.3. Nie polegać na  
         - cookie kontrolowanych przez klienta  
         - parametrach requestu  
         - logice frontendowej  

---

12. WNIOSKI KOŃCOWE

   12.1. Dane kontrolowane przez klienta nie mogą decydować o uprawnieniach  
   12.2. Jeśli rola jest przesyłana w requestcie, backend jej nie zna  
   12.3. Autoryzacja per-request bez walidacji prowadzi
