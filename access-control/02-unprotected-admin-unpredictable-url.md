UNPROTECTED ADMIN FUNCTIONALITY WITH UNPREDICTABLE URL – ACCESS CONTROL

Utworzono: 2026-01-22 15:41

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
        - Frontend-only access control

   3.5. Zakres testu  
        - Access Control / Admin functionality

---

4. RECONNAISSANCE – MAPOWANIE APLIKACJI

   4.1. Zidentyfikowane publiczne adresy URL  
        - /  
        - /product?productId=X  
        - /login  

   4.2. Obserwacje  
        - Brak widocznych elementów administracyjnych w UI  
        - Brak informacji o rolach użytkowników  
        - Funkcjonalność administracyjna nie jest linkowana  

---

5. HIPOTEZY

   5.1. Panel administracyjny istnieje, ale nie jest ujawniony w interfejsie  
   5.2. Dostęp do funkcji admina oparty jest na ukrywaniu URL  
   5.3. Brak backendowej kontroli autoryzacji  

---

6. ANALIZA FRONTENDU (DISCOVERY)

   6.1. Analiza kodu źródłowego strony (View Page Source)  

   6.2. Zidentyfikowany fragment JavaScript  
        - Zmienna isAdmin ustawiona na false  
        - Warunek if (isAdmin) kontroluje wyłącznie widoczność linku  

   6.3. Odkryta ścieżka panelu administracyjnego  
        - /admin-xrvw7o  

   6.4. Wniosek  
        - URL panelu administracyjnego jest jawny dla klienta  
        - Frontend decyduje jedynie o widoczności linku  

---

7. WERYFIKACJA KONTROLI DOSTĘPU

   7.1. Ręczne wejście na odkrytą ścieżkę  
        - /admin-xrvw7o  

   7.2. Rezultat  
        - Dostęp do panelu administracyjnego bez uwierzyt
