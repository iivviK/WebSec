UNPROTECTED ADMIN FUNCTIONALITY – ACCESS CONTROL

Utworzono: 2026-01-21 15:18

---

1. LAB

   Platforma: PortSwigger Web Security Academy  
   Nazwa laba: Unprotected admin functionality

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
        - Brak backendowej kontroli dostępu

   3.5. Zakres testu  
        - Access Control / Admin functionality

---

4. RECONNAISSANCE – MAPOWANIE APLIKACJI

   4.1. Zidentyfikowane endpointy publiczne  
        - /home  
        - /product?productId=X  
        - /login  

   4.2. Obserwacje  
        - Brak elementów administracyjnych w UI  
        - Brak informacji o kontach użytkowników  
        - Funkcjonalność administracyjna nie jest eksponowana  

   4.3. Wniosek  
        - Istnieje funkcjonalność administracyjna niewidoczna w UI  

---

5. HIPOTEZY

   5.1. Panel administracyjny dostępny pod ukrytą ścieżką URL  
   5.2. Brak backendowej kontroli autoryzacji  
   5.3. Security by obscurity / frontend-only access control  

---

6. DISCOVERY – INFORMATION DISCLOSURE

   6.1. Sprawdzenie pliku robots.txt  

   6.2. Zawartość pliku  
        - Disallow: /administrator-panel  

   6.3. Wniosek  
        - robots.txt ujawnia istnienie wrażliwej funkcji  
        - Plik robots.txt nie jest mechanizmem bezpieczeństwa  

---

7. WERYFIKACJA KONTROLI DOSTĘPU

   7.1. Ręczne wejście na ujawnioną ścieżkę  
        - /administrator-panel  

   7.2. Rezultat  
        - Dostęp do panelu administracyjnego bez uwierzytelnienia  
        - Brak weryfikacji sesji  
        - Brak sprawdzania roli użytkownika  

---

8. EKSPLOATACJA

   8.1. Wykonana akcja  
        - Usunięcie konta użytkownika „carlos”

   8.2. Efekt  
        - Pełny dostęp do funkcji administracyjnych  
        - Brak jakichkolwiek zabezpieczeń backendowych  

---

9. KLASYFIKACJA PODATNOŚCI

   9.1. Kategoria  
        - Broken Access Control  

   9.2. Typ  
        - Unprotected Admin Functionality  

   9.3. Root cause  
        - Brak backendowej kontroli autoryzacji  
        - Poleganie na ukrywaniu funkcji  

   9.4. Uwaga  
        - Podatność nie jest IDOR  
        - Brak manipulacji identyfikatorami obiektów  

---

10. IMPACT

   10.1. Możliwości atakującego  
         - Dostęp do panelu administracyjnego  
         - Usuwanie kont użytkowników  
         - Modyfikacja danych aplikacji  

   10.2. Konsekwencje  
         - Utrata integralności danych  
         - Krytyczny wpływ na bezpieczeństwo systemu  

---

11. REKOMENDACJE

   11.1. Wprowadzić backendową kontrolę autoryzacji  

   11.2. Backend powinien  
         - weryfikować uwierzytelnienie  
         - weryfikować rolę użytkownika  

   11.3. Nie polegać na  
         - ukrywaniu ścieżek URL  
         - robots.txt  
         - logice frontendowej  

---

12. WNIOSKI KOŃCOWE

   12.1. robots.txt może prowadzić do information disclosure  
   12.2. Frontend nie jest mechanizmem bezpieczeństwa  
   12.3. Brak backendowej autoryzacji = podatność krytyczna  
   12.4. Najpierw szukaj funkcji, potem paneli
