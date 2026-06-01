```md
NAME:
Request Interpretation Mismatch

CLASS:
Access Control

==================================================
1. CORE IDEA
==================================================

Różne komponenty aplikacji interpretują ten sam request w różny sposób, co pozwala ominąć mechanizmy bezpieczeństwa.

==================================================
2. ROOT CAUSE
==================================================

Brak spójności pomiędzy komponentami odpowiedzialnymi za routing, autoryzację lub przetwarzanie requestów.

==================================================
3. SIGNALS
==================================================

- Reverse proxy przed aplikacją
- WAF przed aplikacją
- API Gateway przed backendem
- Nietypowe odpowiedzi 403
- Bardzo krótkie odpowiedzi Access Denied
- Różnice pomiędzy zachowaniem frontendu i backendu
- Zachowanie zależne od metody HTTP
- Różne odpowiedzi dla tego samego zasobu
- Różne komunikaty błędów dla podobnych requestów
- Nagłówki wpływające na routing lub autoryzację
- Endpointy administracyjne dostępne przez wiele ścieżek

==================================================
4. PREREQUISITES
==================================================

- Co najmniej dwa komponenty analizujące request
- Różne źródła informacji o ścieżce lub metodzie
- Możliwość wpływu na dane interpretowane przez backend

==================================================
5. QUESTIONS
==================================================

- Kto podejmuje decyzję o dostępie?
- Czy front-end i backend widzą ten sam request?
- Czy istnieją alternatywne źródła informacji o ścieżce?
- Czy backend ufa dodatkowym nagłówkom?
- Czy różne komponenty interpretują request identycznie?
- Czy ACL działa identycznie dla wszystkich metod HTTP?
- Czy różne metody trafiają do tej samej logiki?
- Czy różne ścieżki przetwarzania mają te same kontrole bezpieczeństwa?
- Czy różne middleware podejmują różne decyzje?

==================================================
6. DETECTION
==================================================

- Testowanie alternatywnych źródeł informacji o ścieżce
- Testowanie nagłówków wpływających na routing
- Porównywanie interpretacji requestu przez różne komponenty
- Analiza różnic między odpowiedziami 403, 404 i 200
- Testowanie różnych metod HTTP
- Testowanie nieoczekiwanych metod HTTP
- Porównywanie zachowania aplikacji dla różnych metod
- Analiza różnic w autoryzacji pomiędzy metodami
- Identyfikacja punktów decyzyjnych ACL

==================================================
7. GENERALIZATION
==================================================

- Reverse Proxy
- Load Balancers
- API Gateways
- WAF
- CDN
- Middleware
- Routing Frameworks
- Mikroserwisy
- Architektury wielowarstwowe
- Systemy wykorzystujące wiele komponentów do obsługi requestu

==================================================
8. WHY IT WORKS
==================================================

Komponent odpowiedzialny za kontrolę dostępu analizuje inną wersję requestu niż komponent wykonujący operację.

W rezultacie mechanizm bezpieczeństwa zatwierdza request, który backend interpretuje jako dostęp do chronionego zasobu.

==================================================
9. SEEN IN
==================================================

- PortSwigger: URL-based access control can be circumvented
- PortSwigger: Method based access control can be circumvented
```
