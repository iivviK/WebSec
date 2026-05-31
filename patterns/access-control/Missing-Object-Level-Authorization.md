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

- id=
- user=
- account=
- profile=
- document=
- invoice=
- order=
- file=
- Numerowane identyfikatory w URL
- Identyfikatory obiektów w parametrach requestu
- UUID w URL
- GUID w URL
- Losowe identyfikatory obiektów
- Nieprzewidywalne identyfikatory użytkowników
- Publiczne profile ujawniające identyfikatory

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
- Czy otrzymam dane innego użytkownika?
- Czy backend sprawdza właściciela obiektu?
- Czy dostęp zależy wyłącznie od przekazanego identyfikatora?
- Czy aplikacja gdziekolwiek ujawnia UUID?
- Czy identyfikator obiektu można pozyskać z innych funkcji?
- Czy losowy identyfikator jest jedynym zabezpieczeniem?
- Czy obiekt pozostaje dostępny po zdobyciu poprawnego identyfikatora?

==================================================
6. DETECTION
==================================================

- Zidentyfikuj parametry wskazujące obiekt
- Podmień identyfikator na inny
- Porównaj odpowiedzi aplikacji
- Zweryfikuj dostęp do obiektów innych użytkowników
- Szukaj miejsc ujawniających identyfikatory obiektów
- Analizuj profile użytkowników
- Analizuj linki autorów
- Analizuj API responses
- Analizuj referencje między obiektami

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
- Pliki
- UUID
- GUID
- Hash-based identifiers
- Public profile references
- Relacje między użytkownikami
- Obiekty ujawniane przez funkcje społecznościowe

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
```
