# Blok 3 — Testowanie w cyklu życia oprogramowania

## Scenariusz 3.1 — modele SDLC, poziomy testów i strategia integracji

> Dla wskazanego systemu przypisz typowe awarie do poziomów testów i dobierz podejście do modelu wytwarzania — sekwencyjnego albo zwinnego. Uzasadnij wybór strategii integracji: jednoczesna (wszystko naraz), zstępująca czy wstępująca.

### 1. Przypisanie awarii do poziomów testów

| Poziom | Co testujemy | Przykład awarii |
|--------|--------------|-----------------|
| **Modułowe (jednostkowe)** | Logika, kalkulacja, pojedyncza metoda | Błąd algorytmu obliczającego zniżkę w koszyku |
| **Integracyjne** | Komunikacja między modułami / zewnętrznymi systemami | Moduł sklepu wysyła PLN, bramka płatności (mikroserwis) oczekuje EUR → transakcja odrzucona |
| **Systemowe** | Cały system, E2E lub niefunkcjonalne | System zawiesza się przy 1000 użytkowników naraz (awaria wydajnościowa) |
| **Akceptacyjne** | Wymagania biznesowe, prawne, UX | System działa technicznie OK, ale rezerwacja jest tak nieintuicyjna, że klienci rezygnują |

### 2. Dobór modelu wytwarzania (SDLC)

**Podejście sekwencyjne (Waterfall, V-model)**

- **Kiedy:** systemy krytyczne (medyczne, lotnicze)
- **Dlaczego:** wymagania prawne są twarde, ustalone z góry, nie ulegają zmianom

**Podejście zwinne (Agile)**

- **Kiedy:** większość projektów biznesowych i startupów
- **Dlaczego:** wymagania zmienne, kluczowy szybki czas wejścia na rynek, ciągła adaptacja przez wczesny feedback

### 3. Strategia integracji

**Jednoczesna (Big Bang)** — integrujemy wszystkie moduły naraz

- ✅ tylko dla trywialnie małych systemów
- ❌ w dużych projektach powoduje chaos, utrudnia lokalizację przyczyny awarii

**Zstępująca (Top-down)** — testujemy od głównego interfejsu (UI) w dół, brakujące komponenty niższego poziomu zastępujemy **zaślepkami (stubami)**

- ✅ gdy chcesz **szybko pokazać klientowi działający prototyp interfejsu**

**Wstępująca (Bottom-up)** — od fundamentów (warstwy dostępu do danych) w górę, używając **sterowników (driverów)** do symulowania wyższych warstw

- ✅ gdy system opiera się na **złożonych, krytycznych algorytmach niskopoziomowych** i potrzeba pewności fundamentu

!!! quote "Jak to powiedzieć egzaminatorowi"
    **O poziomach testów:** "Testy modułowe lokalizują błąd w samej logice, kalkulacji lub pojedynczej metodzie — np. błąd algorytmu obliczającego zniżkę w koszyku. Testy integracyjne łapią awarię w komunikacji między modułami lub zewnętrznymi systemami — np. moduł sklepu wysyła kwotę w PLN, a bramka płatności jako osobny mikroserwis oczekuje EUR. Testy systemowe to problem z całym oprogramowaniem, np. system zawiesza się, gdy 1000 użytkowników naraz próbuje złożyć zamówienie. Testy akceptacyjne to niespełnienie wymagań biznesowych — system działa technicznie bez zarzutu, ale proces rezerwacji jest na tyle nieintuicyjny, że klienci masowo z niego rezygnują."

    **O wyborze Waterfall/V-modelu:** "Wybieram podejście sekwencyjne dla systemów krytycznych — medycznych, lotniczych — gdzie wymagania prawne są twarde, ustalone z góry i nie ulegają zmianom."

    **O wyborze Agile:** "Wybieram dla większości projektów biznesowych i startupów, gdzie wymagania są zmienne, a kluczowy jest szybki czas wejścia na rynek i ciągła adaptacja poprzez wczesny feedback."

    **O Big Bang:** "Wybieram tylko dla trywialnie małych systemów. W dużych projektach powoduje chaos i skrajnie utrudnia zlokalizowanie przyczyny awarii."

    **O top-down:** "Testuję od głównego interfejsu w dół. Brakujące komponenty niższego poziomu zastępuję zaślepkami — stubami. Wybieram, gdy chcę szybko pokazać klientowi działający prototyp interfejsu."

    **O bottom-up:** "Buduję od fundamentów — np. warstwy dostępu do danych — w górę, używając sterowników do symulowania wyższych warstw. Wybieram, gdy system opiera się na złożonych, krytycznych algorytmach niskopoziomowych i trzeba mieć pewność, że fundament jest stabilny."

---

## Scenariusz 3.2 — przesunięcie testów w lewo, podejścia "najpierw test" i testowanie zmianowe

> Wskaż konkretne działania przesuwające testy w lewo i powiedz, co dają. Dla wybranej historyjki zaprojektuj scenariusze w duchu "najpierw test" (warunek początkowy — działanie — oczekiwany rezultat). Na koniec z historii wykonania ustal, które uruchomienia to regresja, i zaplanuj jej zakres po zmianie.

### 1. Shift-left (przesunięcie testów w lewo)

**Konkretne działania:**

- Włączenie testerów w **przeglądy wymagań i historyjek** (np. Backlog Refinement)
- Pisanie **scenariuszy testowych jeszcze przed kodem** (współpraca biznes ↔ IT)
- **Wczesna analiza statyczna** dokumentacji

**Co daje:**

- Wykrycie problemów na etapie pomysłu jest znacznie tańsze niż naprawa gotowego systemu
- Zespół zyskuje **wspólną wizję** i szybszy feedback
- Zapobiega budowaniu błędnego oprogramowania

### 2. Scenariusze "najpierw test" (Given-When-Then / BDD)

**Historyjka:** *Jako klient chcę dodać produkt do koszyka, aby złożyć zamówienie.*

**Scenariusz pozytywny:**

- **Given (warunek początkowy):** klient jest zalogowany, ma pusty koszyk, produkt jest dostępny w magazynie
- **When (działanie):** klient klika "Dodaj do koszyka" przy wybranym produkcie
- **Then (oczekiwany rezultat):** ilość produktów w koszyku zmienia się na 1, system rezerwuje sztukę produktu w tle

### 3. Testowanie zmianowe i planowanie regresji

**Identyfikacja regresji:**

- Gdy programista naprawia błąd, wykonujesz najpierw **testy potwierdzające** (czy ten konkretny błąd zniknął)
- Każde **ponowne uruchomienie testów dla pozostałych, wcześniej działających modułów** = **regresja** — sprawdza, czy naprawa nie wywołała nowych awarii

**Planowanie zakresu:**

- Uruchamianie całej puli testów bywa za drogie i czasochłonne
- **Analiza wpływu** — wybierz testy tylko dla modułów stykających się ze zmodyfikowanym kodem
- Dodaj podstawowe **happy paths** dla najkrytyczniejszych funkcji

!!! quote "Jak to powiedzieć egzaminatorowi"
    **O shift-left:** "Konkretne działania to włączenie testerów w przeglądy wymagań i historyjek, np. podczas Backlog Refinement, pisanie scenariuszy testowych jeszcze przed napisaniem kodu we współpracy biznesu z IT oraz wczesna analiza statyczna dokumentacji. Wykrycie problemów na etapie pomysłu jest znacznie tańsze niż naprawa gotowego systemu, a sam zespół zyskuje wspólną wizję i szybszy feedback, co zapobiega budowaniu błędnego oprogramowania."

    **O regresji:** "Gdy programista naprawia błąd, wykonuję najpierw testy potwierdzające, aby upewnić się, że ten konkretny błąd zniknął. Każde ponowne uruchomienie testów dla pozostałych, wcześniej działających modułów to właśnie regresja — ma sprawdzić, czy naprawa nie wywołała nowych awarii w innych miejscach."

    **O zakresie regresji:** "Uruchamianie całej puli testów bywa zbyt drogie i czasochłonne. Planuję zakres regresji na podstawie analizy wpływu — wybieram tylko testy dla modułów, które stykają się ze zmodyfikowanym kodem, dodając do tego podstawowe happy paths dla najkrytyczniejszych funkcji systemu."
