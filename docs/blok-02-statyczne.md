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
