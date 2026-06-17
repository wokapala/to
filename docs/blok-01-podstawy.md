# Blok 1 — Podstawy testowania

## Scenariusz 1.1 — cele i kontekstowość testowania oraz siedem zasad

> Weź dwa kontrastowe projekty (np. finansowy produkt w wersji minimalnej i system medyczny) i dla każdego ustal cele oraz podejście do testów. Następnie z obserwacji projektu (brak nowych defektów regresyjnych, klastrowanie defektów) wskaż, które zasady to tłumaczą i jakie decyzje testowe z nich płyną.

Zgodnie z zasadą, że **testowanie jest zależne od kontekstu**, do każdego projektu należy podejść zupełnie inaczej.

### Projekt A: Finansowy produkt w wersji minimalnej (Startup MVP)

**Charakterystyka i cele:** Głównym celem startupu w fazie MVP jest szybkie dostarczenie wartości na rynek, budowanie zaufania pierwszych użytkowników oraz zwalidowanie, czy produkt w ogóle odpowiada na rzeczywiste potrzeby (walidacja użyteczności i UX).

**Podejście do testów:** Szybkość kosztem pełnego pokrycia. Testowanie gruntowne jest niemożliwe, a wręcz blokowałoby rozwój biznesu.

- Elastyczne podejście — weryfikuje się głównie **"happy paths"** i podstawowe procesy (logowanie, czy przelew wychodzi)
- Dopuszcza się testowanie na użytkownikach końcowych (na produkcji) — **testy A/B**, **feature flags** (np. nowa funkcja tylko dla 5% ruchu)
- Akceptuje się pewne ryzyko — startupy świadomie wypuszczają niedopracowane oprogramowanie, by szybciej zebrać feedback

### Projekt B: System medyczny (silnie regulowany)

**Charakterystyka i cele:** Priorytetem jest **bezpieczeństwo pacjentów i niezawodność**. Nadrzędny cel testowania — zgodność z rygorystycznymi regulacjami prawnymi i standardami.

**Podejście do testów:** Metodyczne, skrupulatne, dobrze udokumentowane.

- Bardzo duży nacisk na **wczesne testowanie statyczne** — weryfikacja specyfikacji, architektury pod kątem zgodności z prawem zanim powstanie kod
- Wymagana ścisła **identyfikowalność (traceability)** przez macierze śledzenia — udowodnienie audytorom, że każde wymaganie prawne jest pokryte testami
- Najwyższy poziom **niezależności testowania** — np. certyfikacja w zewnętrznych laboratoriach

### Obserwacja 1: Brak nowych defektów regresyjnych

System się rozwija, programiści dostarczają nowe funkcje, ale od dłuższego czasu nie ma nowych błędów regresyjnych.

**Zasada:** *Testy się zużywają* (**Paradoks pestycydów**). Kod i deweloperzy "uodparniają się" na te same testy. Jeśli zestaw nie ewoluuje, przestanie wykrywać nowe defekty.

**Decyzje:**

- Nieustannie ewaluować, modyfikować i rozszerzać strategie testowe
- Usunąć zdezaktualizowane / mało wartościowe testy, dopisać nowe
- **Sesja testowania eksploracyjnego** w wykonaniu doświadczonego testera — szukanie nowych wektorów usterek poza standardowymi ścieżkami

### Obserwacja 2: Klastrowanie defektów

Większość błędów pochodzi z jednego modułu, podczas gdy inne działają bezawaryjnie.

**Zasada:** *Defekty lubią się grupować* (**Zasada Pareto w testowaniu**). Błędy skupiają się w niewielkiej liczbie miejsc — tam, gdzie złożona logika, "dług technologiczny" lub najwięcej poprawek.

**Decyzje:**

- Po identyfikacji klastra (monitorowanie metryk awaryjności) — **zmiana alokacji zasobów**, skupienie wysiłków na ryzykownym obszarze
- Zaplanować dodatkowe testy (np. integracyjne sprawdzające przepływy)
- Długofalowo: rekomendacja zespołowi programistycznemu **dogłębnej analizy** (np. metodą "5 × Dlaczego") i **refaktoryzacji** problematycznego kodu — zamiast w nieskończoność doklejać testy

!!! tip "Wskazówka na egzamin"
    Prowadzący nie chce regułek. Używaj sformułowań pokazujących **myślenie problemowe**:

    > "W startupie ryzyko błędu przekłada się najwyżej na gorszy UX, co naprawiamy hotfixem, ale w systemie medycznym ten sam błąd oznacza naruszenie prawa i potencjalne zagrożenie życia, więc strategia musi być skrajnie inna"

    Zawsze podawaj własne uzasadnienie decyzji.

!!! quote "Jak to powiedzieć egzaminatorowi"
    **O kontekstowości:** "Zgodnie z zasadą, że testowanie jest zależne od kontekstu, do każdego projektu należy podejść zupełnie inaczej."

    **O MVP:** "Głównym celem startupu w fazie MVP jest szybkie dostarczenie wartości na rynek, budowanie zaufania pierwszych użytkowników oraz zwalidowanie, czy produkt w ogóle odpowiada na rzeczywiste potrzeby. Stawia się tu na szybkość kosztem pełnego pokrycia testami — testowanie gruntowne wręcz blokowałoby rozwój biznesu. Weryfikuje się głównie happy paths, dopuszcza się testy A/B na produkcji albo wdrożenie nowej funkcji feature flagiem dla 5% ruchu."

    **O systemie medycznym:** "W systemach medycznych priorytetem jest bezpieczeństwo pacjentów i niezawodność, a nadrzędnym celem testowania staje się zgodność z rygorystycznymi regulacjami prawnymi. Bardzo duży nacisk kładzie się na wczesne testowanie statyczne — weryfikację specyfikacji i analizę architektury, jeszcze zanim powstanie kod. Wymagana jest ścisła identyfikowalność przez macierze śledzenia, aby udowodnić audytorom, że każde wymaganie prawne zostało pokryte testami."

    **O paradoksie pestycydów:** "Kod, oprogramowanie, a także sami deweloperzy z czasem uodparniają się na te same, wielokrotnie powtarzane testy. Jeśli zestaw testów nie ewoluuje, przestanie w końcu wykrywać nowe defekty wprowadzane przy okazji rozwoju systemu. Dobrym krokiem będzie przeprowadzenie sesji testowania eksploracyjnego w wykonaniu doświadczonego testera."

    **O klastrowaniu:** "Defekty nie rozkładają się równomiernie. Najczęściej skupiają się w niewielkiej liczbie miejsc, np. tam, gdzie występuje bardzo złożona logika biznesowa, nagromadzenie długu technologicznego lub tam, gdzie nanoszono najwięcej poprawek. Zamiast w nieskończoność doklejać nowe testy, tester może zarekomendować zespołowi programistycznemu dogłębną analizę metodą 5 × Dlaczego oraz refaktoryzację problematycznego kodu."

---

## Scenariusz 1.2 — proces testowy, identyfikowalność i niezależność

> Dla wybranej funkcjonalności rozpisz czynności od analizy, przez projektowanie i implementację, po wykonanie. Rozróżnij warunki testowe i przypadki, pokaż przykład identyfikowalności: od podstawy testów, przez przypadki testowe, po ich status wykonania, a na końcu dobierz poziom niezależności testowania pasujący do sytuacji.

### 1. Czynności w procesie testowym

- **Analiza:** analizujesz wymagania (podstawę testów) i wyodrębniasz **warunki testowe** — *co* należy przetestować
- **Projektowanie:** przekształcasz warunki w szczegółowe **przypadki testowe** — *jak* będziesz testować (kroki, dane wejściowe, oczekiwany rezultat)
- **Implementacja:** środowisko, dane testowe, skrypty automatyczne
- **Wykonanie:** uruchamiasz testy, porównujesz wyniki, nadajesz status (Pass/Fail)

### 2. Schemat identyfikowalności (macierz śledzenia)

**Przepływ:**

```
[Wymaganie biznesowe] → [Warunek testowy] → [Przypadek testowy] → [Status wykonania]
```

**Po co to:**

- udowodnienie biznesowi **stopnia pokrycia wymagań**
- szybka **analiza wpływu** przy zmianach w kodzie
- ocena ryzyka i raportowanie

### 3. Poziomy niezależności testowania

| Poziom | Plusy | Minusy | Kiedy |
|--------|-------|--------|-------|
| **Autor kodu** (brak niezależności) | znajomość kodu, szybkość | ryzyko **confirmation bias** | szybkie weryfikacje |
| **Tester (QA) w zespole** | wiedza domenowa, szybka komunikacja | ryzyko ulegania wpływom devów | standard w **Agile** |
| **Dedykowany zespół QA** | obiektywność, specjalizacja | silosy organizacyjne, "rzucanie zadaniami przez płot" | dojrzałe organizacje |
| **Zewnętrzne laboratorium** | najwyższy obiektywizm | wysoki koszt, długi czas | systemy regulowane (medyczne, bankowe, wojskowe), certyfikacja |

!!! quote "Jak to powiedzieć egzaminatorowi"
    **O warunkach vs przypadkach:** "Z wymagań wyodrębniam ogólne warunki testowe — określam *co* należy przetestować. Potem przekształcam warunki w szczegółowe przypadki testowe — określam *jak* będę testować, czyli definiuję kroki, dane wejściowe i oczekiwany rezultat."

    **O identyfikowalności:** "Identyfikowalność służy do udowodnienia biznesowi stopnia pokrycia wymagań, pozwala na szybką analizę wpływu przy zmianach w kodzie oraz ułatwia ocenę ryzyka i raportowanie."

    **O autorze kodu jako testerze:** "Daje dobrą znajomość kodu i szybkość, ale niesie ogromne ryzyko błędów poznawczych — confirmation bias. Programista nieświadomie testuje tak, żeby potwierdzić, że jego kod działa."

    **O testerze w zespole:** "To standard w zwinnym wytwarzaniu oprogramowania. Daje dobrą wiedzę domenową i szybką komunikację, ale jest ryzyko ulegania wpływom deweloperów z tego samego zespołu."

    **O dedykowanym QA:** "Wysoka obiektywność i specjalizacja, ale tworzy silosy organizacyjne, co wydłuża komunikację i powoduje rzucanie zadaniami przez płot."

    **O zewnętrznym laboratorium:** "Wybieram, gdy mamy system silnie regulowany prawnie — np. medyczny, bankowy, wojskowy. Najwyższy obiektywizm, ale bardzo wysoki koszt i długi czas."

---

## Scenariusz 1.3 — łańcuch pomyłka → defekt i raport defektu

> Z opisu awarii odtwórz łańcuch przyczynowy i przeprowadź analizę przyczyny źródłowej. Oceń przy tym dołączony raport defektu: czy jest kompletny i czy przekazuje problem w sposób konstruktywny, uzupełnij go, jeśli czegoś brakuje.

### 1. Łańcuch przyczynowy

- **Awaria (skutek):** to, co widzi użytkownik/tester w działającym systemie — aplikacja się zawiesza, podaje błędny wynik
- **Defekt (wada):** to, co fizycznie jest zepsute w kodzie / konfiguracji / architekturze — brak obsługi pustego pola, literówka w zapytaniu do bazy
- **Pomyłka (czynnik ludzki):** przyczyna wprowadzenia wady — presja czasu, zmęczenie, zła komunikacja, błędne zrozumienie wymagań

### 2. Analiza przyczyny źródłowej (Root Cause Analysis)

**Technika "5 × Dlaczego" (Five Whys)** — prosta metoda polegająca na kilkukrotnym (zazwyczaj pięciokrotnym) zadaniu pytania "dlaczego?", by dotrzeć do najgłębszego źródła problemu.

**Cel:** zidentyfikować **prawdziwą lukę w procesie** (np. brak procedur szkoleniowych), a nie zatrzymać się na powierzchownym błędzie ludzkim. **Nie szukamy winnego człowieka.**

### 3. Ocena raportu defektu

**Kompletność** — sprawdź, czy raport ma:

- precyzyjne kroki do reprodukcji
- wynik rzeczywisty
- wynik oczekiwany
- dane o środowisku

**Konstruktywność:**

- żadnych ataków personalnych ani emocjonalnego tonu
- tester to **"posłaniec złych wieści"** — obiektywizm, fakty
- kultura zgłaszania błędów musi być **blameless** (bez obwiniania)

**Wartość dodana:** uzupełnij raport o dodatkowe logi, zrzuty ekranu, propozycję **workaroundu**.

!!! quote "Jak to powiedzieć egzaminatorowi"
    **O łańcuchu przyczynowym:** "Awaria to skutek, czyli to, co bezpośrednio widzi użytkownik lub tester w działającym systemie — np. aplikacja się zawiesza albo podaje błędny wynik. Defekt to wada — to, co fizycznie jest zepsute w kodzie, konfiguracji lub architekturze, np. brak obsługi pustego pola, literówka w zapytaniu do bazy. Pomyłka to czynnik ludzki — przyczyna wprowadzenia wady przez człowieka: presja czasu, zmęczenie, zła komunikacja, błędne zrozumienie wymagań."

    **O 5 × Dlaczego:** "To prosta metoda polegająca na kilkukrotnym, zazwyczaj pięciokrotnym zadaniu pytania *dlaczego?*, aby dotrzeć do najgłębszego źródła problemu. Jej celem jest zidentyfikowanie prawdziwej luki w procesie — np. braku odpowiednich procedur szkoleniowych — a nie tylko zatrzymanie się na powierzchownym błędzie ludzkim. Celem analizy jest znalezienie luki w procesie, a nie szukanie winnego człowieka."

    **O konstruktywności raportu:** "Tester to posłaniec złych wieści i musi zachować obiektywizm, skupiając się wyłącznie na faktach. Kultura zgłaszania błędów musi być blameless — bez obwiniania. Krytykuję raport, jeśli zawiera ataki personalne lub emocjonalny ton."
