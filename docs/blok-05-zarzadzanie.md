# Blok 5 — Zarządzanie czynnościami testowymi

## Scenariusz 5.1 — plan i strategia testów

> Dla wydania naszkicuj plan: cele, zakres, ryzyka, zasoby, harmonogram. Dobierz strategię testowania do kontekstu: inaczej w startupie, inaczej w banku, jeszcze inaczej w systemie medycznym.

### 1. Szkic planu testów

- **Cele:** co konkretnie chcemy osiągnąć (stabilność krytycznej funkcji, bezpieczeństwo transakcji)
- **Zakres:** co testujemy (**in-scope**) i czego świadomie unikamy (**out-of-scope**) + ograniczenia
- **Ryzyka:** identyfikacja i klasyfikacja zagrożeń podczas wydania
- **Zasoby:** sprzęt, narzędzia, środowiska (staging), testerzy, deweloperzy
- **Harmonogram:** ramy czasowe + jasne **kryteria wejścia** (kiedy zaczynamy) i **kryteria wyjścia** (kiedy kończymy)

### 2. Dobór strategii do kontekstu

!!! tip "Nie ma jednej uniwersalnej strategii"

| Kontekst | Strategia | Uzasadnienie |
|----------|-----------|--------------|
| **Startup** | **Reaktywna** (testowanie eksploracyjne) | Wysoka zmienność, liczy się szybki feedback. Dopuszczamy mniejsze błędy w imię szybkiego wejścia na rynek |
| **Bank** | **Analityczna** (oparta na ryzyku) | Obliczamy prawdopodobieństwo i skutki finansowe błędów, skupiamy siły na krytycznych transakcjach. Chronimy kapitał i wizerunek |
| **System medyczny** | **Zgodności ze standardami** (regulacyjna) | Absolutny rygor formalny, macierze śledzenia, najwyższy poziom niezależności (certyfikacja w zewnętrznych laboratoriach) |

---

## Scenariusz 5.2 — analiza ryzyka i testowanie oparte na ryzyku

> Oddziel ryzyka projektowe od produktowych i oceń je przez prawdopodobieństwo i wpływ. Na tej podstawie ustal priorytety testów i zaproponuj reakcję: ograniczenie ryzyka, jego akceptację, przeniesienie albo plan awaryjny.

### 1. Ryzyka projektowe vs produktowe

**Ryzyka projektowe** — dotyczą organizacji, procesu i zarządzania

- opóźnienia w dostarczeniu kodu
- braki kadrowe (choroba kluczowego dewelopera)
- cięcia budżetowe
- niedokładne szacunki
- problemy z zewnętrznym dostawcą technologii

**Ryzyka produktowe** — dotyczą samej jakości i działania oprogramowania

- błędna logika biznesowa (zła kalkulacja zniżek)
- luki w cyberbezpieczeństwie (wyciek danych)
- problemy z wydajnością
- brak skalowalności przy dużym ruchu

### 2. Ocena ryzyka i priorytetyzacja testów

**Wzór:**

```
Poziom ryzyka = Prawdopodobieństwo wystąpienia × Wpływ (skutek)
```

**Priorytetyzacja:** macierz ryzyka. Testy o najwyższym priorytecie → obszary o najwyższym wpływie biznesowym i dużej szansie na awarię (np. kluczowy moduł płatności). Obszary o niskim ryzyku → testy na końcu lub w ograniczonym zakresie.

### 3. Reakcje na ryzyko

| Strategia | Opis | Przykład |
|-----------|------|----------|
| **Ograniczenie** (złagodzenie) | Aktywne działanie zmniejszające szansę lub skutki | Dopisanie testów automatycznych, analiza statyczna, dodatkowe Code Review |
| **Akceptacja** | Świadome przyjęcie ryzyka | Błąd na 1% starych urządzeń — biznes ignoruje, koszt naprawy > strata. Wymaga akceptacji wyżej postawionych osób |
| **Przeniesienie** (transfer) | Przerzucenie na podmiot trzeci | Ubezpieczenie od cyberataków, wydelegowanie płatności do PayU |
| **Plan awaryjny** | Strategia ratunkowa na wypadek materializacji | Ręczne skalowanie serwerów przy piku ruchu, wycofanie zmian + przywrócenie backupu |

---

## Scenariusz 5.3 — szacowanie nakładu i priorytetyzacja testów

> Dobierz technikę szacowania nakładu do kontekstu projektu i uzasadnij wybór. Ustal kolejność wykonania przypadków testowych, biorąc pod uwagę priorytety i zależności.

### 1. Dobór techniki szacowania

| Technika | Kiedy wybrać | Uzasadnienie |
|----------|--------------|--------------|
| **Planning Poker** (metoda delficka) | Standard dla zespołów Agile | Członkowie szacują w izolacji → brak sugerowania się opinią innych. Odchylenia → dyskusja do konsensusu |
| **Szacowanie trójpunktowe** | Projekty innowacyjne, duża niepewność | Uwzględnia wariant optymistyczny, pesymistyczny, najbardziej prawdopodobny. Średnia minimalizuje margines błędu |
| **Oparte na wskaźnikach** (dane historyczne) | Dojrzałe zespoły, powtarzalne zadania | Twarde dane o wydajności z poprzednich iteracji, nie zgadywanie |

### 2. Ustalanie kolejności — priorytety vs zależności

Kompromis między wartością dla biznesu a ograniczeniami technicznymi.

**Krok 1 — wyznaczenie priorytetów:**

- Testy funkcji o najwyższym **ryzyku biznesowym**
- Najczęściej używane przez klientów
- Z największą **historyczną liczbą defektów**

**Krok 2 — analiza zależności:**

- Nawet przypadek o najwyższym priorytecie nie zostanie wykonany, jeśli jest **zablokowany przez nieprzetestowany komponent**
- Najpierw wykonujesz testy **odblokowujące**
- Bierzesz pod uwagę **dostępność zasobów** w danym momencie

---

## Scenariusz 5.4 — monitorowanie, metryki i raportowanie

> Dobierz metryki jakości (gęstość defektów, pokrycie, postęp) i zaprojektuj raport z uwzględnieniem różnych potrzeb informacyjnych: interesariuszy biznesowych i zespołu technicznego. Zdefiniuj też kryteria wejścia i wyjścia.

### 1. Metryki jakości

- **Postęp:** odsetek wykonanych przypadków testowych w stosunku do zaplanowanych
- **Pokrycie:** jaka część wymagań biznesowych (lub kodu) została faktycznie sprawdzona testami
- **Gęstość defektów:** liczba błędów na konkretny moduł → identyfikacja najbardziej problematycznych obszarów (**klastrowanie defektów**)

### 2. Raport — dopasowanie do odbiorcy

!!! tip "Raport musi mówić językiem odbiorcy"

**Dla biznesu (Product Owner, Zarząd):** wysokopoziomowy

- ogólny postęp
- kluczowe ryzyka biznesowe
- jasna rekomendacja: **"czy oprogramowanie jest gotowe do wdrożenia?"** (Go / No-Go)

**Dla technicznych (Deweloperzy, DevOps):** detale

- dzienniki błędów (logi)
- kroki do reprodukcji defektów
- informacje o środowisku testowym
- konkretne luki w pokryciu instrukcji

### 3. Kryteria wejścia i wyjścia

**Kryteria wejścia (Definition of Ready)** — warunki konieczne, by rozpocząć testy

- Środowisko testowe jest stabilne
- Kod został wdrożony
- Podstawowe **smoke testy** przeszły pomyślnie

→ Jeśli nie są spełnione, testerzy **nie podejmują zadania**.

**Kryteria wyjścia (Definition of Done)** — kiedy testowanie można uznać za zakończone

- Osiągnięto założone pokrycie wymagań
- Wykonano wszystkie zaplanowane testy
- Żaden **defekt o statusie krytycznym** nie pozostał otwarty

---

## Scenariusz 5.5 — piramida testów, kwadranty i strategia automatyzacji

> Zaproponuj rozkład wysiłku między poziomami testów i przypisz typy testów do kwadrantów testowania zwinnego. Oceń korzyści automatyzacji testów oraz ryzyka wynikające z jej nieodpowiedniego zastosowania.

### 1. Piramida testów (rozkład wysiłku)

**Tradycyjne podejście:**

- Podstawa — liczne, szybkie, tanie **testy jednostkowe**
- Środek — mniej **testów integracyjnych**
- Szczyt — najmniej **testów E2E i UI** (drogie, wolne, trudne w utrzymaniu)

!!! tip "Wskazówka praktyczna (z wykładu)"
    W nowoczesnych architekturach (mikroserwisy) to **testy integracyjne często dają największą wartość** i to na nich skupia się główny wysiłek, minimalizując kruche testy E2E.

### 2. Kwadranty testowania zwinnego

| Kwadrant | Strona / Cel | Co tam jest |
|----------|--------------|-------------|
| **Q1** | Wsparcie zespołu / Technologiczne | Testy jednostkowe i komponentowe (zazwyczaj w pełni zautomatyzowane przez programistów) |
| **Q2** | Wsparcie zespołu / Biznesowe | Testy funkcjonalne, weryfikacja prototypów i historyjek — czy dobrze zrozumiano wymagania |
| **Q3** | Ocena produktu / Biznesowe | Testy eksploracyjne, użyteczności (UX), alfa/beta — głównie manualne, ocena intuicyjności |
| **Q4** | Ocena produktu / Technologiczne | Testy niefunkcjonalne — wydajnościowe, bezpieczeństwa, skalowalności |

### 3. Automatyzacja

**Korzyści:**

- Niezbędna do **CI/CD**
- Natychmiastowa informacja zwrotna po napisaniu kodu
- Uwalnia czas testerów

**Ryzyka nieodpowiedniego zastosowania:**

- Automatyzowanie **niewłaściwych rzeczy** (ogromna liczba testów UI/E2E) → powolne działanie, ogromne koszty utrzymania "kruchych" testów
- Pisanie testów jednostkowych **tylko dla procentowego pokrycia** (a nie weryfikacji logiki) → testy bezużyteczne, psują się przy każdym refaktorze
