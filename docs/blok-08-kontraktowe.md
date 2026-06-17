# Blok 8 — Testy kontraktowe

## Scenariusz 8.1 — testy kontraktowe sterowane konsumentem

> Dla pary współpracujących usług zaprojektuj kontrakt po stronie konsumenta, weryfikację po stronie dostawcy i bramkę zgodności, która zadziała przed wdrożeniem.

**Przykład:** usługa **Raportowanie** (Konsument) odpytuje usługę **Zamówienia** (Dostawca).

### 1. Kontrakt po stronie konsumenta (Consumer-Driven)

- **Projekt:** konsument definiuje w swoich testach jednostkowych, **jakich konkretnie danych wymaga** — nie testuje logiki biznesowej dostawcy
- **Przykład:** test konsumenta określa: *"Wysyłając żądanie o zamówienie, oczekuję odpowiedzi ze statusem HTTP 200 oraz pola 'status' w formacie tekstowym"*
- Z tego testu generowany jest **plik kontraktu**, który trafia do brokera

### 2. Weryfikacja po stronie dostawcy (Provider)

- **Akcja:** dostawca pobiera opublikowany kontrakt od brokera w ramach swojego potoku **CI/CD**
- **Weryfikacja:** serwer dostawcy automatycznie uruchamia pobrane testy **przeciwko własnemu API**
- Sprawdza w **izolacji**, czy jego aktualne odpowiedzi są w 100% zgodne z oczekiwaniami zapisanymi w kontrakcie

### 3. Bramka zgodności (Compliance Gate)

- **Działanie:** centralny **broker kontraktów** (np. **Pact Broker**) zna wersje aplikacji obu stron na każdym środowisku
- **Zastosowanie przed wdrożeniem:** w potoku CI/CD uruchamiany jest skrypt **"Can I deploy?"**
- Zanim nowa wersja zostanie wdrożona, broker sprawdza, czy aktualnie wdrożona wersja dostawcy spełnia kontrakt
- Jeśli programista zmienił nazwę kluczowego pola → bramka **natychmiast blokuje wdrożenie**, zapobiegając awarii na produkcji

### 4. Rola brokera kontraktów (np. Pact Broker)

- Scentralizowane **repozytorium**, w którym przechowywane i weryfikowane są kontrakty w ramach CI/CD
- !!! warning "Nie mylić"
    Brokera **nie należy mylić z formatami serializacji danych** typu Protobuf
- Broker zna wersje aplikacji obu stron na każdym środowisku
- Działa jak **sędzia** pilnujący, czy nowe wersje serwisów nie łamią ustalonych umów

!!! quote "Jak to powiedzieć egzaminatorowi"
    **O kontrakcie konsumenta:** "Konsument definiuje w swoich testach jednostkowych, jakich konkretnie danych wymaga, nie testując logiki biznesowej dostawcy. Test konsumenta określa: *wysyłając żądanie o zamówienie, oczekuję odpowiedzi ze statusem HTTP 200 oraz pola status w formacie tekstowym*. Z tego testu generowany jest plik kontraktu, który trafia do brokera."

    **O weryfikacji dostawcy:** "Dostawca pobiera opublikowany kontrakt od brokera w ramach swojego potoku CI/CD. Serwer dostawcy automatycznie uruchamia pobrane testy przeciwko swojemu własnemu API. Sprawdza w izolacji, czy jego aktualne odpowiedzi są w 100% zgodne z oczekiwaniami zapisanymi w kontrakcie przez konsumenta."

    **O bramce zgodności:** "W potoku CI/CD uruchamiany jest skrypt *Can I deploy?*. Zanim nowa wersja zostanie wdrożona na środowisko docelowe, broker sprawdza, czy aktualnie wdrożona wersja dostawcy spełnia jej kontrakt. Jeśli programista zmienił nazwę kluczowego pola, łamiąc umowę, bramka natychmiast blokuje wdrożenie, zapobiegając awarii na produkcji."

    **O brokerze:** "Broker to scentralizowane repozytorium, w którym przechowywane i weryfikowane są kontrakty w ramach potoku CI/CD. Nie należy go mylić z formatami serializacji danych takimi jak Protobuf. Broker zna wersje aplikacji obu stron na każdym środowisku i działa jak sędzia, który pilnuje, czy nowe wersje serwisów nie łamią ustalonych umów."

---

## Scenariusz 8.2 — kontrakty a E2E i ewolucja kontraktu

> Uzasadnij, kiedy testy kontraktowe zastępują albo uzupełniają E2E, biorąc pod uwagę koszt, szybkość informacji zwrotnej i stabilność. Zaplanuj też, jak obsłużyć zmianę API i utrzymać kompatybilność wsteczną między usługami.

### 1. Kontrakty vs E2E

Testy kontraktowe to **lekarstwo na bolączki testów End-to-End**.

| Aspekt | E2E | Kontraktowe |
|--------|-----|-------------|
| **Koszt** | Drogie w utrzymaniu | Tanie |
| **Stabilność** | Niestabilne (zależność od całego środowiska i zewnętrznych usług) | Wysoka — działają w izolacji |
| **Szybkość** | Wolne | Tak szybkie jak jednostkowe |
| **Feedback** | Trzeba czekać, blokuje proces wydawniczy | Natychmiastowy w CI/CD (blokuje wdrożenie psujące integrację) |

**Zastępowanie vs uzupełnianie:**

- Kontrakty powinny **zastąpić** testy E2E w **szczegółowym weryfikowaniu komunikacji** między mikroserwisami
- Testy E2E je **uzupełniają** — zostawiasz ich niewiele, tylko do weryfikacji **głównych krytycznych ścieżek** całego systemu na produkcyjnym lub zbliżonym środowisku

### 2. Ewolucja API i kompatybilność wsteczna

**Strategia łagodnej zmiany:**

- Kiedy modyfikujesz API — zachowaj **kompatybilność wsteczną**
- Wdrażasz nową wersję API **obok starej** (by nie złamać działających kontraktów innych zespołów)

**Wygaszanie (kluczowy krok):**

- Czekasz, aż **wszyscy konsumenci** zaktualizują swoje aplikacje i zaczną korzystać z nowej wersji kontraktu
- Gdy tylko to nastąpi — **natychmiast usuwasz starą wersję API**
- Nie wspierasz jej w nieskończoność — generuje koszty i dług technologiczny

!!! quote "Jak to powiedzieć egzaminatorowi"
    **O kontraktach vs E2E:** "Testy kontraktowe to lekarstwo na bolączki testów End-to-End. Testy E2E są drogie w utrzymaniu, wolne i często niestabilne ze względu na zależność od całego środowiska i zewnętrznych usług. Testy kontraktowe uruchamiają się tak szybko jak jednostkowe i działają w izolacji, co gwarantuje ich taniość i wysoką stabilność."

    **O szybkości feedbacku:** "Kontrakty dają natychmiastowy feedback w potoku CI/CD — np. blokują wdrożenie psujące integrację. Na wyniki E2E trzeba czekać, co blokuje i opóźnia proces wydawniczy."

    **O zastępowaniu vs uzupełnianiu:** "Kontrakty powinny zastąpić testy E2E w szczegółowym weryfikowaniu komunikacji między poszczególnymi mikroserwisami. Testy E2E z kolei je uzupełniają — zostawiam ich niewiele, tylko do weryfikacji głównych, krytycznych ścieżek całego systemu na produkcyjnym lub zbliżonym środowisku."

    **O ewolucji API:** "Kiedy modyfikuję API, zachowuję kompatybilność wsteczną. Wdrażam nową wersję API obok starej, aby nie złamać działających kontraktów innych zespołów."

    **O wygaszaniu:** "Czekam, aż wszyscy konsumenci zaktualizują swoje aplikacje i zaczną korzystać z nowej wersji kontraktu. Gdy tylko to nastąpi, natychmiast usuwam starą wersję API, aby nie wspierać jej w nieskończoność i nie generować kosztów."
