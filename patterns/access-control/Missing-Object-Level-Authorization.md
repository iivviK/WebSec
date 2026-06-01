```md
NAME:
Missing Object-Level Authorization

CLASS:
Access Control

==================================================
1. CORE IDEA
==================================================

Użytkownik może wskazać dowolny obiekt, a backend nie weryfikuje czy ma prawo do dostępu do tego obiektu.

==================================================
2. ROOT CAUSE
==================================================

Backend identyfikuje obiekt na podstawie danych dostarczonych przez klienta, ale nie sprawdza własności lub uprawnień do tego obiektu.

==================================================
3. SIGNALS
==================================================

- Identyfikatory obiektów w URL
- Identyfikatory obiektów w parametrach requestu
- UUID / GUID
- Losowe identyfikatory obiektów
- Publiczne referencje do obiektów
- Linki do profili użytkowników
- Linki do dokumentów i plików
- Redirect po próbie dostępu do obiektu
- Dane użytkownika zwracane po zmianie identyfikatora
- Content-Disposition
- filename=

==================================================
4. PREREQUISITES
==================================================

- Istnienie obiektów należących do różnych użytkowników
- Możliwość wskazania identyfikatora obiektu
- Brak autoryzacji na poziomie obiektu

==================================================
5. QUESTIONS
==================================================

- Czy mogę zmienić identyfikator obiektu?
- Czy backend sprawdza właściciela obiektu?
- Czy dostęp zależy wyłącznie od przekazanego identyfikatora?
- Czy aplikacja ujawnia identyfikatory innych obiektów?
- Czy losowy identyfikator jest jedynym zabezpieczeniem?
- Czy body odpowiedzi zawiera dane mimo blokady dostępu?
- Czy dostęp do obiektu prowadzi do dalszej eskalacji?
- Czy plik lub dokument jest traktowany jako obiekt chroniony?

==================================================
6. DETECTION
==================================================

- Zidentyfikuj parametr wskazujący obiekt
- Podmień identyfikator na inny
- Porównaj odpowiedzi aplikacji
- Zweryfikuj dostęp do obiektów innych użytkowników
- Szukaj miejsc ujawniających identyfikatory
- Analizuj profile, linki i relacje między obiektami
- Analizuj pełną odpowiedź HTTP
- Wyłącz automatyczne follow redirect
- Analizuj dane zwracane dla obiektu
- Testuj dostęp do plików, dokumentów i załączników

==================================================
7. GENERALIZATION
==================================================

- Profile użytkowników
- Dokumenty
- Zamówienia
- Faktury
- Klucze API
- Dane klientów
- Zasoby API
- Pliki i załączniki
- Raporty i eksporty danych
- Obiekty identyfikowane przez UUID / GUID
- Relacje między użytkownikami
- Funkcje społecznościowe

==================================================
8. WHY IT WORKS
==================================================

Backend ufa identyfikatorowi obiektu dostarczonemu przez użytkownika i nie sprawdza czy użytkownik jest właścicielem wskazanego zasobu.

W rezultacie użytkownik może uzyskać dostęp do obiektów należących do innych użytkowników.

==================================================
9. SEEN IN
==================================================

- PortSwigger: User ID controlled by request parameter
- PortSwigger: User ID controlled by request parameter, with unpredictable user IDs
- PortSwigger: User ID controlled by request parameter with data leakage in redirect
- PortSwigger: User ID controlled by request parameter with password disclosure
- PortSwigger: Insecure Direct Object References
```
