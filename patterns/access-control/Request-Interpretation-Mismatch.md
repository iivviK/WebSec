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
- Nietypowe odpowiedzi 403
- Bardzo krótkie odpowiedzi Access Denied
- Nagłówki wpływające na routing
- X-Original-URL
- X-Rewrite-URL
- X-Forwarded-Path
- X-Forwarded-Prefix
- Zachowanie zależne od metody HTTP
- POST = 401
- GET = 200/302
- Różne komunikaty błędów dla różnych metod
- Nietypowe reakcje na POSTX
- Endpointy administracyjne obsługujące wiele metod

==================================================
4. PREREQUISITES
==================================================

- Co najmniej dwa komponenty analizujące request
- Różne źródła informacji o ścieżce
- Możliwość wpływu na dane interpretowane przez backend

==================================================
5. QUESTIONS
==================================================

- Kto podejmuje decyzję o dostępie?
- Czy front-end i backend widzą ten sam URL?
- Czy istnieją nagłówki nadpisujące routing?
- Czy backend ufa dodatkowym nagłówkom?
- Czy różne komponenty interpretują request identycznie?
- Czy ACL działa identycznie dla wszystkich metod?
- Czy GET i POST trafiają do tej samej logiki?
- Czy PUT, DELETE, PATCH są chronione?
- Czy różne metody przechodzą przez różne middleware?

==================================================
6. DETECTION
==================================================

- Testowanie X-Original-URL
- Testowanie X-Rewrite-URL
- Testowanie X-Forwarded-Path
- Testowanie X-Forwarded-Prefix
- Porównywanie zachowania proxy i backendu
- Analiza różnic między 403 a 404
- Zamień POST na GET
- Zamień POST na PUT
- Zamień POST na DELETE
- Zamień POST na PATCH
- Wyślij niepoprawną metodę (POSTX)
- Porównuj odpowiedzi między metodami

==================================================
7. GENERALIZATION
==================================================

- Reverse Proxy
- Load Balancery
- API Gateways
- WAF
- CDN
- Middleware routing
- Mikroserwisy
- Method-based Access Control
- REST API
- Reverse Proxy Routing
- Middleware ACL
- Framework Routing
- API Gateways

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
