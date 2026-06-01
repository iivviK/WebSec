```md
NAME:
Workflow Inconsistency

STATUS:
Candidate

CLASS:
Business Logic

==================================================
1. CORE IDEA
==================================================

Różne workflow obsługujące ten sam obiekt biznesowy lub ten sam proces stosują różne reguły bezpieczeństwa.

==================================================
2. OBSERVED VARIANTS
==================================================

Variant #1:
Inconsistent Validation Across Workflows

Mechanism:
Ten sam obiekt biznesowy podlega różnym regułom walidacji w różnych workflow.

Seen In:
- [[inconsistent-security-controls]]

--------------------------------------------------

Variant #2:
Incomplete Workflow Authorization

Mechanism:
Jeden krok procesu sprawdza autoryzację, podczas gdy inny krok tego samego procesu nie wykonuje tej kontroli.

Seen In:
- [uzupełnij po wystąpieniu]

==================================================
3. COMMON SIGNALS
==================================================

- ten sam obiekt występuje w wielu workflow
- wiele funkcji prowadzi do tego samego rezultatu biznesowego
- różne endpointy obsługują ten sam obiekt
- część procesu zawiera dodatkowe kontrole bezpieczeństwa
- alternatywne ścieżki osiągają ten sam efekt
- ograniczenia występują tylko w wybranych miejscach procesu

==================================================
4. COMMON QUESTIONS
==================================================

- Czy istnieje alternatywny sposób osiągnięcia tego samego celu?
- Czy wszystkie workflow stosują te same reguły?
- Czy każdy krok procesu wykonuje te same kontrole?
- Czy ten sam obiekt jest walidowany wszędzie tak samo?
- Czy mogę osiągnąć ten sam stan inną ścieżką?

==================================================
5. COMMON ROOT CAUSE
==================================================

[DO USTALENIA]

Potencjalna hipoteza:

Różne workflow rozwijane były niezależnie i implementują własne reguły bezpieczeństwa zamiast korzystać ze wspólnego mechanizmu kontroli.

==================================================
6. OPEN QUESTIONS
==================================================

- Czy Workflow Inconsistency jest samodzielnym patternem?
- Czy jest rodziną patternów?
- Jakie kolejne warianty należą do tej grupy?
- Czy wszystkie warianty mają ten sam root cause?

==================================================
7. PROMOTION RULE
==================================================

Awans do pełnego Pattern Card dopiero po zebraniu wielu niezależnych przykładów i potwierdzeniu wspólnego mechanizmu.

==================================================
8. NEXT OBSERVATIONS
==================================================

- [ ]
- [ ]
- [ ]

==================================================
9. NOTES
==================================================

To nie jest jeszcze Pattern Card.

To jest kandydat na pattern / rodzinę patternów.

Nowe laby należy dopisywać tutaj do momentu potwierdzenia lub odrzucenia hipotezy.
```
