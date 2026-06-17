# Blok 2 — Testowanie statyczne

## Scenariusz 2.1 — Przeglądy: dobór typu, procesu i ról

> Dla danego artefaktu i jego krytyczności dobierz typ przeglądu (nieformalny, przejrzenie, techniczny, inspekcja) i obsadź role. Powiedz też, które artefakty w projekcie nadają się do analizy statycznej.

### 1. Dobór typu przeglądu do krytyczności

| Krytyczność | Typ przeglądu | Uzasadnienie |
|-------------|---------------|--------------|
| **Niska** (wstępny zarys funkcji, mało ważny dokument) | **Przegląd nieformalny** (pair programming, konsultacja) lub **Przejrzenie** (walkthrough) | Szybki feedback, generowanie pomysłów, sprawdzanie alternatyw. Przejrzenie prowadzi autor, brak ścisłej formalizacji. |
| **Średnia/Wysoka** (ważny moduł, logika biznesowa) | **Przegląd techniczny** | Konsensus techniczny, wykrycie defektów przez ekspertów. Często z raportem. |
| **Krytyczna/Maksymalna** (system finansowy, medyczny, architektura bezpieczeństwa) | **Inspekcja** | Najbardziej formalny proces, twarde zasady, **listy kontrolne (checklists)**. Tam, gdzie błąd kosztuje najwięcej. |

### 2. Obsadzenie ról (szczególnie w inspekcji)

- **Kierownik** — decyduje o przeprowadzeniu, daje budżet/czas
- **Autor** — twórca artefaktu
  - !!! warning "Kluczowe"
      W formalnej inspekcji autor **nie może** pełnić roli lidera ani protokolanta — obiektywizm
- **Moderator/Facylitator** — prowadzi spotkanie (najlepiej osoba niezależna, nieznająca domeny, by uniknąć stronniczości)
- **Przeglądający** — eksperci techniczni szukający defektów
- **Protokolant** — notuje błędy i przebieg (niezbędny przy przeglądzie technicznym i inspekcji)

### 3. Artefakty podatne na analizę statyczną

Analizie statycznej można poddać **wszystko, co ma sformalizowaną strukturę i składnię** oraz da się "przeczytać":

- **Kod źródłowy** (najczęstszy cel)
- **Modele i architektura** systemu
- **Dokumentacja specyfikacji** — np. historyjki użytkownika, w tym DoD i DoR
- **Skrypty testowe i pliki konfiguracyjne**

!!! note
    Dokumenty niesformalizowane lub trudne do zinterpretowania według jasnych reguł słabo nadają się do analizy statycznej.

!!! quote "Jak to powiedzieć egzaminatorowi"
    **O przeglądzie nieformalnym/przejrzeniu:** "Wybieram dla niskiej krytyczności — daje szybki feedback, generowanie pomysłów i sprawdzanie alternatyw. Przejrzenie jest prowadzone przez samego autora i nie wymaga ścisłej formalizacji."

    **O przeglądzie technicznym:** "Celem jest osiągnięcie konsensusu technicznego i wykrycie defektów przez ekspertów o odpowiednich kwalifikacjach. Często tworzy się z niego raport."

    **O inspekcji:** "To najbardziej formalny proces, oparty o twarde zasady i listy kontrolne. Wymagany dla systemów, gdzie błąd kosztuje najwięcej — finansowych, medycznych, architektury bezpieczeństwa."

    **O rolach w inspekcji:** "W formalnej inspekcji autor nie może pełnić roli lidera ani protokolanta, aby zachować obiektywizm. Moderator najlepiej, gdy jest osobą niezależną, nieznającą domeny, by unikać stronniczości."

    **O artefaktach do analizy statycznej:** "Analizie statycznej można poddać wszystko, co posiada sformalizowaną strukturę i składnię oraz da się przeczytać — kod źródłowy, modele architektury, dokumentację specyfikacji, historyjki użytkownika z DoD i DoR, skrypty testowe i pliki konfiguracyjne."

---

## Scenariusz 2.2 — wczesna informacja zwrotna i bramki jakości

> Wyjaśnij, co testowanie statyczne daje w porównaniu z dynamicznym i czego nim nie wykryjesz. Zdefiniuj dla zespołu kryteria gotowości i ukończenia.

### 1. Testowanie statyczne vs dynamiczne

**Co daje testowanie statyczne:**

- Badasz artefakty **bez uruchamiania kodu** (przeglądy wymagań, architektury, statyczna analiza kodu)
- Wykrywa **defekty bezpośrednio** — wskazuje konkretny problem w dokumencie/kodzie (np. nieosiągalny kod, błędy w architekturze) zanim powstanie działająca aplikacja
- **Tańsze i szybsze** — feedback w stylu **shift-left**. Naprawa błędnego wymagania na papierze kosztuje ułamek tego, co naprawa gotowego systemu
- Pozwala testować rzeczy **"niewykonywalne"** — testami dynamicznymi nie sprawdzisz specyfikacji

**Czego NIE wykryje testowanie statyczne:**

- Awarii w **środowisku uruchomieniowym** (kod nie jest uruchamiany)
- Problemów z **wydajnością** pod rzeczywistym obciążeniem
- **Wycieków pamięci** z upływem czasu
- **Zakleszczeń (deadlocków)**
- Problemów z integracją zewnętrznych bibliotek

### 2. Bramki jakości: DoR i DoD

Fundament pracy w **Agile/Scrum**.

**DoR — Definition of Ready (kryteria gotowości)**

- **Definicja:** lista warunków (**bramka wejściowa**), które muszą być spełnione, zanim zespół rozpocznie pracę nad zadaniem
- **Cel:** zapobiec "przepalaniu" czasu na szukanie brakujących informacji
- **Przykład:** historyjka ma jasny cel biznesowy, dołączone makiety (UI), określone kryteria akceptacyjne, rozwiązane zależności od innych zespołów (brak blokerów)

**DoD — Definition of Done (kryteria ukończenia)**

- **Definicja:** uniwersalny standard jakości (**bramka wyjściowa**), który musi zostać spełniony, aby uznać zadanie za w pełni zakończone
- **Cel:** pewność, że praca jest gotowa dla klienta lub do wdrożenia
- **Przykład:** kod przeszedł Code Review, napisano testy jednostkowe (z określonym pokryciem), testy automatyczne na stagingu świecą się na zielono, zaktualizowano dokumentację

!!! quote "Jak to powiedzieć egzaminatorowi"
    **O testowaniu statycznym:** "Badam artefakty bez uruchamiania kodu — przeglądy wymagań, architektury, statyczna analiza kodu. Wykrywa defekty bezpośrednio — wskazuje konkretny problem w dokumencie lub kodzie, zanim w ogóle powstanie działająca aplikacja. Jest tańsze i daje znacznie szybszą informację zwrotną — tak zwany shift-left. Naprawa błędnego wymagania na papierze kosztuje ułamek tego, co naprawa gotowego systemu."

    **O ograniczeniach:** "Nie wykryje awarii w środowisku uruchomieniowym. Ponieważ kod nie jest uruchamiany, nie złapię błędów, które ujawniają się dopiero w boju — problemów z wydajnością pod rzeczywistym obciążeniem, wycieków pamięci z upływem czasu, zakleszczeń czy problemów z integracją zewnętrznych bibliotek."

    **O DoR:** "Definition of Ready to lista warunków — bramka wejściowa — które muszą być spełnione, zanim zespół w ogóle rozpocznie pracę nad zadaniem. Ma zapobiegać przepalaniu czasu na szukanie brakujących informacji. Na przykład: historyjka ma jasno opisany cel biznesowy, dołączone są makiety, określono kryteria akceptacyjne, rozwiązano wszystkie zależności od innych zespołów."

    **O DoD:** "Definition of Done to uniwersalny standard jakości — bramka wyjściowa — który musi zostać spełniony, aby uznać zadanie za w pełni zakończone. Daje pewność, że praca jest gotowa dla klienta lub do wdrożenia."
