```md
# PATTERN GUIDE v1.0

Cel dokumentu:

Wyjaśnienie jak tworzyć, aktualizować i utrzymywać bibliotekę Pattern Cards.

Dokument jest niezależny od konkretnego narzędzia (Obsidian, GitHub, Notion itp.) i opisuje metodologię budowania bazy wiedzy o podatnościach.

==================================================

1. CZYM JEST PATTERN?
   ==================================================

Pattern nie jest rozwiązaniem laba.

Pattern nie jest payloadem.

Pattern nie jest konkretnym exploitem.

Pattern opisuje powtarzalny mechanizm prowadzący do podatności.

Przykład:

NIE:

* quantity=-846
* zmiana price=0.01
* konkretny endpoint

TAK:

* Negative Quantity Compensation
* Client Controlled Price
* Password Reset Poisoning

Pattern powinien być użyteczny niezależnie od aplikacji.

==================================================
2. CEL PATTERN CARD
===================

Celem Pattern Card jest:

* rozpoznawanie wzorców
* szybkie przypominanie wiedzy
* budowanie mentalnych modeli podatności
* przenoszenie wiedzy pomiędzy aplikacjami

Po przeczytaniu Pattern Card po roku powinieneś nadal rozumieć mechanizm podatności.

==================================================
3. KIEDY TWORZYĆ NOWY PATTERN?
==============================

Nową Pattern Card tworzymy wyłącznie wtedy gdy:

* CORE IDEA jest nowa

LUB

* ROOT CAUSE jest nowy

Jeżeli zmienia się wyłącznie:

* endpoint
* parametr
* payload
* aplikacja
* workflow

to NIE tworzymy nowego patternu.

==================================================
4. KIEDY AKTUALIZOWAĆ ISTNIEJĄCY PATTERN?
=========================================

Jeżeli nowy przypadek wykorzystuje ten sam mechanizm:

* nie twórz nowej karty
* zaktualizuj istniejącą

Najczęściej aktualizowane sekcje:

* SIGNALS
* QUESTIONS
* DETECTION
* GENERALIZATION
* SEEN IN

==================================================
5. REGUŁA ANTY-DUPLIKACJI
=========================

Przed utworzeniem nowego patternu zadaj pytanie:

"Czy mogę dopisać ten przypadek do sekcji SEEN IN istniejącego patternu?"

Jeżeli odpowiedź brzmi TAK:

→ nie twórz nowej karty

==================================================
6. JAK WYPEŁNIAĆ POSZCZEGÓLNE SEKCJE?
=====================================

---

1. CORE IDEA

---

Jedno zdanie.

Najkrótszy możliwy opis mechanizmu.

Pytanie:

"Jaka jest istota tego patternu?"

---

2. ROOT CAUSE

---

Przyczyna występowania patternu.

Błąd projektowy, logiczny lub architektoniczny.

Pytanie:

"Co zostało zaprojektowane nieprawidłowo?"

---

3. SIGNALS

---

Sygnały wskazujące na obecność patternu.

Anomalie, komunikaty, nietypowe zachowania.

Pytanie:

"Po czym mogę podejrzewać ten pattern?"

---

4. PREREQUISITES

---

Warunki konieczne do wystąpienia patternu.

Pytanie:

"Co musi istnieć w aplikacji, aby ten pattern miał sens?"

---

5. QUESTIONS

---

Pytania prowadzące do odkrycia patternu.

Pytanie:

"O co powinienem zapytać aplikację?"

---

6. DETECTION

---

Szybkie testy służące do wykrycia patternu.

Pytanie:

"Jak mogę szybko potwierdzić lub wykluczyć ten pattern?"

---

7. GENERALIZATION

---

Miejsca, gdzie ten sam pattern może występować.

Nie aplikacje.

Klasy problemów.

Pytanie:

"Gdzie jeszcze mogę wykorzystać ten pattern?"

---

8. WHY IT WORKS

---

Mechanizm → Skutek

Połączenie:

ROOT CAUSE → IMPACT

Pytanie:

"Dlaczego ten mechanizm prowadzi do podatności?"

---

9. SEEN IN

---

Lista miejsc gdzie pattern został zaobserwowany.

Może zawierać:

* laby
* raporty
* bug bounty
* projekty

==================================================
7. CZEGO NIE UMIESZCZAMY W PATTERNACH?
======================================

Nie umieszczamy:

* payloadów
* pełnych requestów
* nazw hostów
* nazw użytkowników
* kroków exploita
* szczegółów konkretnego laba

Te informacje należą do:

LAB NOTE

nie do

PATTERN CARD

==================================================
8. FILOZOFIA SYSTEMU
====================

Lab jest przykładem.

Pattern jest wiedzą.

Lab pokazuje:

"Co zadziałało?"

Pattern wyjaśnia:

"Dlaczego to zadziałało?"

Celem systemu nie jest pamiętanie setek labów.
```

Celem systemu jest rozpoznawanie kilkudziesięciu powtarzalnych wzorców, które pojawiają się w różnych aplikacjach i różnych technologiach.
