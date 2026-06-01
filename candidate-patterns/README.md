````md
# Candidate Patterns

## Purpose

Folder przechowuje kandydatów na Pattern Cards.

Znajdują się tutaj obserwacje, hipotezy oraz grupy mechanizmów, które pojawiły się w jednym lub kilku labach, ale nie zostały jeszcze potwierdzone jako samodzielne patterny.

---

## Knowledge Flow

```text
LAB
↓
Candidate Pattern
↓
Pattern Card
````

---

## Rules

### Create Candidate Pattern When

* pojawia się potencjalnie nowy mechanizm
* istnieje podejrzenie nowej rodziny patternów
* nie ma jeszcze wystarczającej liczby przykładów
* root cause nie jest jeszcze w pełni zrozumiany

---

### Promote To Pattern Card When

* mechanizm został zaobserwowany wielokrotnie

* istnieje wspólny root cause

* można zdefiniować stabilne:

  * CORE IDEA
  * ROOT CAUSE
  * SIGNALS
  * QUESTIONS
  * DETECTION

* pattern daje się zastosować poza pojedynczym labem

---

### Do Not Create Pattern Cards For

* pojedyncze laby
* pojedyncze exploity
* pojedyncze payloady
* implementacyjne szczegóły konkretnej aplikacji

---

## Goal

Celem folderu jest zapobieganie:

* utracie wiedzy
* tworzeniu duplikatów Pattern Cards
* zbyt wczesnej formalizacji wzorców

Folder pełni rolę „inkubatora patternów”.

Dopiero po potwierdzeniu wzorzec trafia do:

```text
patterns/
```

```
```
