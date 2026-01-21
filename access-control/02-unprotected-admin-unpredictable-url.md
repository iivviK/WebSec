UNPROTECTED ADMIN FUNCTIONALITY WITH UNPREDICTABLE URL – ACCESS CONTROL

Utworzono: 2026-01-22 16:04

---

1. LAB

   Platforma: PortSwigger Web Security Academy  
   Nazwa laba: Unprotected admin functionality with unpredictable URL

---

2. CEL TESTU

   2.1. Uzyskanie dostępu do funkcjonalności administracyjnej  
   2.2. Usunięcie konta użytkownika „carlos”

---

3. KONTEKST TESTU

   3.1. Typ aplikacji  
        - Aplikacja webowa typu e-commerce

   3.2. Poziom dostępu  
        - Użytkownik anonimowy

   3.3. Uwierzytelnienie  
        - Brak

   3.4. Autoryzacja  
        - Brak backendowej kontroli uprawnień

   3.5. Zakres testu  
        - Access Control / Admin functionality

---

4. RECONNAISSANCE – MAPOWANIE APLIKACJI

   4.1. Zidentyfikowane publiczne adresy URL  
        - /home  
        - /product?productId=X  
        - /login  

   4.2. Obserwacje  
        - Brak linków administracyjnych w UI  
        - Brak danych logowania użytkowników  
        - Funkcjonalność administracyjna niewidoczna w interfejsie  

   4.3. Wniosek  
        - Funkcja administracyjna istnieje, ale nie jest widoczna dla użytkownika  

---

5. HIPOTEZY

   5.1. Panel administracyjny jest dostępny pod nieprzewidywalną ścieżką URL  
   5.2. Frontend ukrywa link administracyjny, ale backend nie weryfikuje uprawnień  

---

6. ANALIZA FRONTENDU (DISCOVERY)

   6.1. Analiza kodu źródłowego strony (View Page Source)  

   6.2. Zidentyfikowany fragment JavaScript odpowiedzialny za wyświetlanie linku admina  
        - Zmienna isAdmin ustawiona na false  
        - Warunek if (isAdmin) kontroluje jedynie widoczność linku  

   6.3. Odkryty URL panelu administracyjnego  
        - /admin-xrvw7o  

   6.4. Wniosek  
        - Frontend zna dokładny adres panelu administracyjnego  
        - Kontrola dostępu realizowana jest wyłącznie po stronie klienta  

---

7. WERYFIKACJA KONTROLI DOSTĘPU

   7.1. Sprawdzony adres  
        - /admin-xrvw7o  

   7.2. Rezultat  
        - Dostęp do panelu administracyjnego bez uwierzytelnienia  
        - Brak weryfikacji sesji użytkownika  
        - Brak sprawdzania roli (admin / user)  
        - Dostęp do funkcji administracyjnych  

---

8. EKSPLOATACJA

   8.1. Wykonana akcja  
        - Usunięcie konta użytkownika „carlos”  

   8.2. Efekt  
        - Pełna kontrola nad funkcjonalnościami administracyjnymi  
        - Brak jakichkolwiek zabezpieczeń backendowych  

---

9. KLASYFIKACJA PODATNOŚCI

   9.1. Kategoria  
        - Broken Access Control  

   9.2. Typ  
        - Unprotected Admin Functionality  
        - Frontend-only access control  

   9.3. Root cause  
        - Brak backendowej weryfikacji autoryzacji  
        - Poleganie na ukrywaniu linku w JavaScript  

   9.4. Uwaga  
        - Nie jest to IDOR  
        - Losowy URL nie stanowi zabezpieczenia  

---

10. IMPACT

   10.1. Możliwości atakującego  
         - Dostęp do panelu administracyjnego bez logowania  
         - Usuwanie kont użytkowników  
         - Modyfikacja danych aplikacji  

   10.2. Konsekwencje  
         - Utrata integralności danych  
         - Ryzyko reputacyjne  
         - Krytyczny wpływ na bezpieczeństwo systemu  

---

11. REKOMENDACJE

   11.1. Wprowadzić backendową kontrolę autoryzacji dla endpointów administracyjnych  

   11.2. Weryfikować po stronie serwera  
         - czy użytkownik jest uwierzytelniony  
         - czy użytkownik posiada odpowiednią rolę  

   11.3. Nie polegać na  
         - ukrywaniu linków w JavaScript  
         - losowych nazwach URL  
         - logice frontendowej  

---

12. WNIOSKI KOŃCOWE

   12.1. Jeśli URL istnieje w JavaScript, nie jest sekretem  
   12.2. Frontend nie jest mechanizmem bezpieczeństwa  
   12.3. Losowy URL nie zastępuje autoryzacji  
   12.4. Kontrola dostępu musi być egzekwowana po stronie backendu
