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

!!! quote "Jak to powiedzieć egzaminatorowi"
    **O strategii w startupie:** "W środowisku o wysokiej zmienności liczy się szybki feedback, więc doskonałym wyborem będzie strategia reaktywna — testowanie eksploracyjne. Dopuszczamy mniejsze błędy w imię szybkiego wejścia na rynek."

    **O strategii w banku:** "Wybieram strategię analityczną, opartą na ryzyku. Obliczamy prawdopodobieństwo i skutki finansowe błędów, a następnie skupiamy siły testowe na krytycznych transakcjach, aby chronić kapitał i wizerunek."

    **O strategii w systemie medycznym:** "Wybieram strategię zgodności ze standardami. Wymagany jest absolutny rygor formalny, udowodnienie bezpieczeństwa z użyciem macierzy śledzenia i nierzadko najwyższy poziom niezależności — np. certyfikacja przez zewnętrzne laboratorium."

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

!!! quote "Jak to powiedzieć egzaminatorowi"
    **O ryzykach projektowych:** "Dotyczą organizacji, procesu i zarządzania. Na przykład opóźnienia w dostarczeniu kodu, braki kadrowe — choroba kluczowego dewelopera, cięcia budżetowe, niedokładne szacunki, problemy z zewnętrznym dostawcą technologii."

    **O ryzykach produktowych:** "Dotyczą samej jakości i poprawnego działania oprogramowania. Na przykład błędna logika biznesowa — zła kalkulacja zniżek, luki w cyberbezpieczeństwie — wyciek danych, problemy z wydajnością czy brak skalowalności przy dużym ruchu."

    **O wzorze ryzyka:** "Poziom ryzyka to iloczyn dwóch wartości — prawdopodobieństwo wystąpienia razy wpływ. Na podstawie tego równania buduję macierz ryzyka. Testy o najwyższym priorytecie alokuję w obszarach, które mają najwyższy wpływ biznesowy i dużą szansę na awarię — np. kluczowy moduł płatności."

    **O ograniczeniu (mitygacji):** "Aktywne działanie, aby zmniejszyć szansę wystąpienia problemu lub jego skutki. Dopisuję testy automatyczne, wprowadzam analizę statyczną kodu lub dodatkowe Code Review."

    **O akceptacji:** "Świadome przyjęcie ryzyka. Wiemy o błędzie występującym na 1% starych urządzeń, ale biznes decyduje się go zignorować, bo koszt naprawy jest wyższy niż potencjalna strata. Wymaga akceptacji osób wyżej postawionych."

    **O przeniesieniu:** "Przerzucenie odpowiedzialności na podmiot trzeci. Wykupujemy ubezpieczenie od cyberataków lub delegujemy obsługę płatności do zewnętrznego, certyfikowanego operatora, np. PayU."

    **O planie awaryjnym:** "Przygotowanie strategii ratunkowej na wypadek zmaterializowania się ryzyka — np. zaplanowanie ręcznego skalowania serwerów w razie nagłego piku ruchu lub szybkie wycofywanie zmian i przywracanie backupu."

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

!!! quote "Jak to powiedzieć egzaminatorowi"
    **O Planning Pokerze:** "Wybieram jako standard dla zespołów zwinnych. Członkowie zespołu szacują zadania w izolacji, co zapobiega sugerowaniu się opinią innych. Znaczące odchylenia są następnie dyskutowane, aż zespół osiągnie konsensus."

    **O szacowaniu trójpunktowym:** "Wybieram dla projektów innowacyjnych lub o dużej niepewności. Uwzględnia wariant optymistyczny, pesymistyczny oraz najbardziej prawdopodobny. Wyciągnięcie z nich średniej minimalizuje margines błędu szacowania."

    **O metodzie opartej na danych historycznych:** "Wybieram dla dojrzałych zespołów lub powtarzalnych zadań. Opiera się na twardych danych o wydajności z poprzednich iteracji, a nie na zgadywaniu."

    **O kolejności wykonania:** "Kolejność wykonywania testów to kompromis między wartością dla biznesu a ograniczeniami technicznymi. Najpierw wykonuję testy funkcji o najwyższym ryzyku biznesowym, tych najczęściej używanych i tych z największą historyczną liczbą defektów. Nawet jeśli przypadek testowy ma najwyższy priorytet, nie wykonam go, jeśli jest zablokowany przez inny, nieprzetestowany komponent — wtedy muszę najpierw wykonać testy odblokowujące."

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

!!! quote "Jak to powiedzieć egzaminatorowi"
    **O metrykach:** "Postęp wskazuje, na jakim etapie jesteśmy — np. odsetek wykonanych przypadków testowych w stosunku do zaplanowanych. Pokrycie określa, jaka część wymagań biznesowych lub kodu została faktycznie sprawdzona. Gęstość defektów wskazuje liczbę błędów przypadających na konkretny moduł, co pomaga zidentyfikować klastrowanie."

    **O raporcie dla biznesu:** "Raport dla Product Ownera czy Zarządu musi być wysokopoziomowy. Skupiam się na ogólnym postępie, kluczowych ryzykach biznesowych oraz jasnej rekomendacji — czy oprogramowanie jest gotowe do wdrożenia, czyli Go albo No-Go."

    **O raporcie technicznym:** "W raporcie dla deweloperów i DevOps liczą się detale. Dołączam dzienniki błędów, kroki niezbędne do reprodukcji defektów, informacje o środowisku testowym i wskazuję konkretne luki w pokryciu instrukcji."

    **O kryteriach wejścia:** "Lista warunków, które muszą być spełnione, aby w ogóle rozpocząć testy. Środowisko testowe jest stabilne, kod został wdrożony, podstawowe szybkie smoke testy przeszły pomyślnie. Jeśli kryteria nie są spełnione, testerzy nie podejmują zadania."

    **O kryteriach wyjścia:** "Warunki określające, kiedy testowanie można uznać za zakończone. Osiągnięto założone pokrycie wymagań, wykonano wszystkie zaplanowane testy, żaden defekt o statusie krytycznym nie pozostał otwarty."

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

!!! quote "Jak to powiedzieć egzaminatorowi"
    **O piramidzie tradycyjnej:** "Podstawę stanowią liczne, szybkie i tanie testy jednostkowe. Wyżej jest mniej testów integracyjnych, a na samym szczycie najmniej testów End-to-End i UI, ponieważ są drogie, wolne i trudne w utrzymaniu."

    **O podejściu praktycznym:** "Prowadzący zaznaczył, że w nowoczesnych architekturach — np. mikroserwisach — to testy integracyjne często dają największą wartość i to na nich skupia się główny wysiłek, minimalizując kruche testy E2E."

    **O kwadrantach:** "Q1 to wsparcie zespołu po stronie technologicznej — testy jednostkowe i komponentowe, zazwyczaj w pełni zautomatyzowane przez programistów. Q2 to wsparcie zespołu po stronie biznesowej — testy funkcjonalne, weryfikacja prototypów i historyjek, sprawdzają czy dobrze zrozumiano wymagania. Q3 to ocena produktu po stronie biznesowej — testy eksploracyjne, użyteczności, alfa i beta, głównie manualne, by ocenić czy system jest intuicyjny dla klienta. Q4 to ocena produktu po stronie technologicznej — testy niefunkcjonalne, wydajnościowe, bezpieczeństwa, skalowalności."

    **O korzyściach automatyzacji:** "Automatyzacja jest niezbędna do wdrożenia procesów CI/CD, zapewnia natychmiastową informację zwrotną po napisaniu kodu i uwalnia czas testerów."

    **O ryzykach automatyzacji:** "Automatyzowanie niewłaściwych rzeczy — np. ogromnej liczby testów UI lub E2E — skutkuje powolnym działaniem i ogromnymi kosztami utrzymania kruchych testów. Z kolei pisanie testów jednostkowych tylko dla procentowego pokrycia, a nie faktycznej weryfikacji logiki, sprawia, że testy te stają się bezużyteczne i psują się przy każdym refaktorze."
