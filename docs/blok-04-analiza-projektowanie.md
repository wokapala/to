# Blok 4 — Analiza i projektowanie testów

## Scenariusz 4.1 — podział na klasy równoważności

> Na przykładzie formularza lub walidatora wyznacz klasy poprawne i niepoprawne, a potem podaj minimalny zestaw przypadków dający 100% pokrycia klas.

### 1. Walidator i klasy

**Przykład:** walidator wieku przy zakładaniu konta bankowego (wymagane 18 lat).

System potraktuje każdy element wewnątrz danej klasy **dokładnie tak samo** — pozwala drastycznie zredukować liczbę testów. **Klasy muszą być rozłączne.**

**Klasy poprawne:**

- **K1:** osoby pełnoletnie (wiek 18–150)

**Klasy niepoprawne** (warto rozdzielić na znaczeniowe i składniowe):

- **K2:** osoby niepełnoletnie (0–17)
- **K3:** wiek ujemny (< 0)
- **K4:** błędy składniowe (litery, znaki specjalne, ułamki — np. `"osiemnaście"`, `"20,5"`)

### 2. Minimalny zestaw dla 100% pokrycia

**Wzór:** `(Liczba przetestowanych klas / Liczba zdefiniowanych klas) * 100%`

Po jednym reprezentancie z każdej klasy → **4 przypadki = 100%**:

| PT | Klasa | Wartość | Oczekiwany rezultat |
|----|-------|---------|---------------------|
| PT1 | K1 | 25 | Walidacja OK |
| PT2 | K2 | 15 | Odrzucenie (komunikat o niepełnoletności) |
| PT3 | K3 | -5 | Błąd walidacji (wiek nie może być ujemny) |
| PT4 | K4 | "ABC" | Błąd formatu (wymagana liczba całkowita) |

!!! tip "Wskazówka na egzamin"
    Doceniana jest umiejętność rozróżnienia w klasach niepoprawnych:
    - błąd **logiczny/znaczeniowy** (np. za młody)
    - błąd **składniowy** (litery w polu numerycznym)

!!! quote "Jak to powiedzieć egzaminatorowi"
    **O zasadzie klas równoważności:** "Zasada tej techniki mówi, że system potraktuje każdy element wewnątrz danej klasy dokładnie tak samo, co pozwala drastycznie zredukować liczbę testów. Klasy muszą być rozłączne."

    **O pokryciu:** "Aby osiągnąć 100% pokrycia klas, potrzebuję stworzyć testy, które wezmą dokładnie po jednym reprezentancie z każdej zdefiniowanej klasy. Pokrycie liczę jako liczba przetestowanych klas dzielona przez liczbę zdefiniowanych klas razy 100%."

    **O różnicy błędów:** "W klasach niepoprawnych warto rozdzielić błąd logiczny lub znaczeniowy — np. ktoś jest za młody, czyli zła wartość — od błędu składniowego, gdy ktoś wpisał litery zamiast liczby w pole numeryczne. To są dwa różne typy walidacji i system musi sobie z każdym poradzić inaczej."

---

## Scenariusz 4.2 — analiza wartości brzegowych (2- i 3-punktowa)

> Mając reguły progowe, zaprojektuj testy granic. Policz też, jakie pokrycie wartości brzegowych osiąga już istniejący zestaw przypadków.

### 1. Klasy i granice

**Przykład:** zniżka dla osób od 18 do 65 lat.

- **Klasy:** K1 (0–17), K2 (18–65), K3 (66+)
- **Granica:** punkt styku — dla pełnoletności **18**

### 2. Podejście 2-punktowe vs 3-punktowe

**2-punktowe** — sama wartość brzegowa + najbliższa wartość z sąsiedniej klasy

- Dla progu 18: **17, 18**

**3-punktowe** — sama wartość brzegowa + najbliżsi sąsiedzi z **obu stron**

- Dla progu 18: **17, 18, 19**

!!! warning "Dlaczego 3-punktowe jest bezpieczniejsze"
    2-punktowe może nie wykryć błędu, jeśli programista użył **złego operatora relacyjnego** (np. `< 10` zamiast `<= 10`). 3-punktowe na pewno to wyłapie.

### 3. Obliczanie pokrycia dla istniejącego zestawu

**Wzór:**

```
(Liczba przetestowanych wartości brzegowych / Łączna liczba zdefiniowanych wartości brzegowych) * 100%
```

!!! note "Usuwaj duplikaty"
    Wartość 18 pokrywa granicę wspólną dla dwóch klas — to **jedna** wartość brzegowa.

    **Przykład:** system ma 5 wartości brzegowych (0, 17, 18, 65, 66), stary zestaw sprawdza 17 i 18 → pokrycie = (2/5) * 100% = **40%**

!!! quote "Jak to powiedzieć egzaminatorowi"
    **O granicach:** "Skupiam się na punktach styku między klasami. Dla granicy pełnoletności progiem styku jest 18."

    **O 2-punktowej:** "Testuję samą wartość brzegową oraz najbliższą jej wartość z sąsiedniej klasy. Dla progu 18 to 17 oraz 18."

    **O 3-punktowej:** "Testuję samą wartość brzegową oraz jej najbliższych sąsiadów z obu stron — dla progu 18 to 17, 18 oraz 19. Analiza 3-punktowa jest bezpieczniejsza, bo podejście 2-punktowe może nie wykryć błędu, jeśli programista użył złego operatora relacyjnego — np. wpisał *mniejsze niż 10* zamiast *mniejsze równe 10*. 3-punktowa na pewno to wyłapie."

    **O duplikatach przy liczeniu pokrycia:** "Jeśli mam test dla wartości 18, to pokrywa on granicę wspólną dla dwóch klas — traktuję to jako jedną przetestowaną wartość brzegową."

---

## Scenariusz 4.3 — tablice decyzyjne

> Dla złożonej logiki biznesowej (np. naliczanie rabatów albo akceptacja kredytu) zbuduj tablicę decyzyjną, zminimalizuj reguły i wskaż tę, która opisuje sytuację niemożliwą.

### 1. Budowa tablicy decyzyjnej

Tablica składa się z:

- **Warunków** (dane wejściowe)
- **Akcji** (operacje systemu)
- **Reguł** (unikalne kombinacje tych dwóch)

**Konstrukcja:**

- 2–3 warunki wejściowe jako Prawda/Fałsz (T/F)
- Możliwe akcje (zgoda, odrzucenie, przekazanie do ręcznej analizy)
- Przykładowa reguła — pokaż 1–2 kolumny "happy path" (wszystkie T → akceptacja)

### 2. Minimalizacja reguł (uogólnianie)

**Problem:** przy dużej liczbie warunków tablice stają się ogromne i nieczytelne.

**Rozwiązanie:** szukasz warunku **krytycznego**, który determinuje akcję bez względu na pozostałe parametry.

- Przykład: klient nie ma stałych dochodów → odrzucenie kredytu (reszta nieistotna)

**Zapis:** decydujący warunek wpisujesz (T/F), pozostałe zastępujesz znakiem `-`. Redukuje liczbę testów.

### 3. Sytuacja niemożliwa

Logika, która z biznesowego punktu widzenia **wzajemnie się wyklucza** — wykreślasz z analizy.

**Przykład:** kombinacja "klient jest u nas całkowicie nowy" (W4=T) **i jednocześnie** "ma wewnętrzną dobrą historię kredytową" (W3=T) → biznesowo niemożliwa, nie tracimy na nią czasu.

!!! quote "Jak to powiedzieć egzaminatorowi"
    **O tablicy:** "Tablica decyzyjna składa się z warunków, które są danymi wejściowymi, akcji, które są operacjami systemu, oraz reguł, czyli unikalnych kombinacji tych dwóch elementów."

    **O minimalizacji:** "Przy dużej liczbie warunków tablice stają się ogromne i nieczytelne, co utrudnia ich użycie. Szukam w systemie warunku, który jest na tyle krytyczny, że determinuje ostateczną akcję bez względu na pozostałe parametry — np. klient nie ma stałych dochodów przy decyzji kredytowej. Dla takiej reguły wpisuję ten decydujący warunek, a pozostałe, nieistotne dla tej ścieżki, zastępuję znakiem myślnika. Pozwala to zredukować liczbę testów."

    **O sytuacji niemożliwej:** "Szukam logiki, która z biznesowego punktu widzenia wzajemnie się wyklucza. Pokażę tym, że tester myśli krytycznie i nie projektuje testów dla zdarzeń, które fizycznie nie mogą zajść. Wyobraźmy sobie, że dodajemy do tablicy warunek 'Czy klient jest u nas całkowicie nowy?'. Kombinacja, w której klient jest nowy i jednocześnie ma u nas wewnętrzną, dobrą historię kredytową, jest biznesowo niemożliwa. Taki przypadek testowy wykreślam i nie tracę na niego czasu."

---

## Scenariusz 4.4 — testowanie przejść między stanami

> Dla maszyny stanów, np. cyklu życia zamówienia albo dokumentu, wyznacz minimalny zestaw testów pokrywający poprawne przejścia.

### 1. Identyfikacja stanów i przejść

Model obrazuje **cykl życia obiektu** w systemie.

**Przykład — zamówienie:**

- **Stany:** Nowe, Opłacone, Wysłane
- **Przejścia:** akcje wywołujące zmianę stanu (np. zaksięgowanie wpłaty)

### 2. Minimalny zestaw testów

**Cel:** pokryć wszystkie **poprawne przejścia** (happy paths).

!!! tip "Klucz na egzamin"
    **Nie tworzysz osobnego testu dla każdego pojedynczego przejścia.** Łączysz przejścia w dłuższe, logiczne ścieżki od stanu początkowego do końcowego.

**Schemat — cykl życia dokumentu:**

```
Szkic → Weryfikacja → Opublikowany / Odrzucony
```

Zamiast testować każdy krok osobno, tworzysz optymalne ścieżki:

- **Test 1:** Szkic → (zgłoszenie) → Weryfikacja → (akceptacja) → **Opublikowany**
- **Test 2:** Szkic → (zgłoszenie) → Weryfikacja → (odrzucenie) → **Odrzucony**

Dwoma testami pokrywasz wszystkie możliwe prawidłowe rozgałęzienia.

!!! quote "Jak to powiedzieć egzaminatorowi"
    **O modelu:** "Model maszyny stanów obrazuje cykl życia obiektu w systemie. Wypisuję dostępne stany — np. Nowe, Opłacone, Wysłane — oraz przejścia, czyli akcje wywołujące zmianę stanu."

    **O minimalizacji testów:** "Nie tworzę osobnego testu dla każdego pojedynczego przejścia. Aby zminimalizować ich liczbę, łączę przejścia w dłuższe, logiczne ścieżki od stanu początkowego do końcowego. Dzięki temu zaledwie dwoma testami pokrywam wszystkie możliwe, prawidłowe rozgałęzienia w cyklu życia obiektu."

---

## Scenariusz 4.5 — pokrycie instrukcji a pokrycie gałęzi

> Dla fragmentu kodu dobierz przypadki dające 100% pokrycia instrukcji i osobno 100% pokrycia gałęzi, a potem wyjaśnij, czym ta różnica skutkuje.

### 1. Fragment kodu

```javascript
if (wiek >= 18) {
    status = "dorosły";
}
return status;
```

### 2. 100% pokrycia instrukcji

**Zestaw testów:** wystarczy **jeden** przypadek, gdzie warunek jest prawdziwy — np. `wiek = 20`.

System wejdzie w blok `if` i wykona każdą linijkę kodu → **100% pokrycia instrukcji**, ale **pokrycie gałęzi tylko 50%** (nigdy nie sprawdziłeś zachowania dla fałszu).

### 3. 100% pokrycia gałęzi

**Zestaw testów:** dwa przypadki:

- `wiek = 20` (True)
- `wiek = 15` (False)

Wymusza przejście przez **obie ścieżki algorytmu** — automatycznie gwarantuje też wysokie pokrycie instrukcji.

### 4. Czym skutkuje ta różnica

!!! warning "Kluczowy wniosek"
    **100% pokrycia instrukcji daje fałszywe poczucie bezpieczeństwa.** Jeśli poprzestaniesz tylko na pokryciu instrukcji (jeden test), ścieżka dla "fałszu" pozostanie całkowicie w ciemno.

    Na produkcji może to skutkować np. **Null Reference Exception**, jeśli programista zapomniał ustawić domyślny status dla osób poniżej 18.

    **Celuj przede wszystkim w pokrycie gałęzi.**

!!! quote "Jak to powiedzieć egzaminatorowi"
    **O 100% instrukcji:** "Wystarczy jeden przypadek testowy, w którym warunek jest prawdziwy, np. wiek równy 20. System wejdzie w blok if i wykona dosłownie każdą linijkę napisanego kodu. Osiągam 100% pokrycia instrukcji, ale pokrycie gałęzi wynosi tu tylko 50%, ponieważ nigdy nie sprawdziłem, jak kod zachowa się, gdy warunek nie zostanie spełniony."

    **O 100% gałęzi:** "Aby pokryć gałęzie, muszę sprawdzić wszystkie możliwe ścieżki decyzyjne układu logicznego. Potrzebuję dwóch testów — wiek 20 dla True i wiek 15 dla False. Taki zestaw wymusza przejście przez obie ścieżki algorytmu, co automatycznie gwarantuje też wysokie pokrycie instrukcji."

    **O konsekwencjach:** "100% pokrycia instrukcji daje fałszywe poczucie bezpieczeństwa i nie gwarantuje poprawnego przetestowania logiki biznesowej. Jeśli poprzestanę tylko na pokryciu instrukcji, ścieżka dla fałszu pozostanie całkowicie w ciemno. W praktyce może to skutkować tym, że aplikacja na produkcji zwróci błąd — np. Null Reference Exception — jeśli programista zapomniał ustawić domyślny status dla osób poniżej 18 roku życia. Właśnie dlatego powinno się celować przede wszystkim w pokrycie gałęzi."

---

## Scenariusz 4.6 — techniki oparte na doświadczeniu

> Zastosuj zgadywanie błędów, zaplanuj sesję eksploracyjną (cel, zakres, czego szukasz) albo ocenę według listy kontrolnej, a potem uzasadnij, dlaczego w tym kontekście wybierasz właśnie tę technikę.

### 1. Wybór techniki

| Technika | Kiedy wybrać |
|----------|--------------|
| **Zgadywanie błędów** | Świetnie znam domenę i wiem, gdzie programiści najczęściej się mylą |
| **Testowanie eksploracyjne** | Brakuje dokumentacji (lub czasu), system jest dla mnie nowy i muszę się go nauczyć |
| **Lista kontrolna** | Ustrukturyzowane sprawdzenie standardów (UX, bezpieczeństwo) |

### 2. Rozpisanie techniki

**A. Zgadywanie błędów (Error Guessing)**

- **Akcja:** projektuję ataki na usterki (np. znaki specjalne w polu numerycznym), bazując na intuicji
- **Zasada:** narzucam **ścisły limit czasu** (np. 1,5 h), by nie szukać w nieskończoność
- Jeśli znajdę dużo błędów → zgłaszam potrzebę głębszej analizy klastra

**B. Sesja eksploracyjna**

- **Cel:** poznanie zachowania nowej funkcji + wyłapanie krytycznych awarii
- **Zakres:** ograniczam się do jednego konkretnego modułu
- **Podejście (iteracyjne):**

  ```
  obserwuję system → uczę się → projektuję test w locie → analizuję wynik
  ```

- Zostawiam notatki (np. plik Markdown) dla reszty zespołu

**C. Ocena według listy kontrolnej (Checklist)**

- **Cel:** pewność, że żaden standardowy element nie został pominięty
- **Przykład:** UX z **Heurystykami Nielsena** (status systemu, możliwość cofnięcia akcji, itd.)
- **Zasada:** lista musi być "żywa" i ewoluować z projektem, by nie stała się bezużyteczna

!!! quote "Jak to powiedzieć egzaminatorowi"
    **O wyborze zgadywania błędów:** "Wybieram, kiedy świetnie znam domenę i wiem, gdzie programiści najczęściej się mylą. Projektuję ataki na usterki — np. wpisanie znaków specjalnych w pole numeryczne — bazując na intuicji. Narzucam sobie ścisły limit czasu, np. 1,5 godziny, aby nie szukać w nieskończoność. Jeśli znajdę dużo błędów, zgłaszam potrzebę głębszej analizy tego klastra."

    **O eksploracji:** "Wybieram, kiedy brakuje dokumentacji lub czasu, a system jest dla mnie nowy i muszę się go dopiero nauczyć. Wykonuję proces iteracyjny: obserwuję system, uczę się, projektuję test w locie, analizuję wynik. Ograniczam się tylko do jednego, konkretnego modułu i zostawiam po sobie notatki dla reszty zespołu."

    **O liście kontrolnej:** "Wybieram, kiedy zależy mi na ustrukturyzowanym sprawdzeniu standardów — np. użyteczności lub bezpieczeństwa. Badam interfejs pod kątem użyteczności, stosując Heurystyki Nielsena — odhaczam między innymi czy użytkownik widzi status systemu, czy może łatwo cofnąć akcję. Lista musi być żywa i ewoluować wraz z rozwojem projektu, aby nie stała się bezużyteczna."

---

## Scenariusz 4.7 — podejście oparte na współpracy: historyjki użytkownika, kryteria akceptacji i ATDD

> Z historyjki wyprowadź testy akceptacyjne, zarówno ścieżkę pozytywną, jak i przypadki negatywne. Samą historyjkę oceń według kryteriów INVEST, a kryteria akceptacji osobno, pod kątem testowalności i jednoznaczności.

### 1. Wyprowadzenie testów akceptacyjnych (Given-When-Then / BDD)

**Ścieżka pozytywna (Happy Path):** poprawny przepływ realizujący główny cel historyjki

- *Przykład:* posiadając produkt na stanie, po kliknięciu "dodaj" ilość w koszyku zwiększa się o 1

**Przypadki negatywne:** zachowanie systemu, gdy coś idzie nie tak

- *Przykład:* próba dodania do koszyka produktu o stanie magazynowym 0 → oczekiwany komunikat o błędzie

### 2. Ocena historyjki — kryteria INVEST

| Litera | Pełna nazwa | Pytanie |
|--------|-------------|---------|
| **I** | Independent (Niezależna) | Czy można wdrożyć i przetestować oddzielnie, bez blokad? |
| **N** | Negotiable (Negocjowalna) | Czy określa "co", zostawiając zespołowi dyskusję "jak"? |
| **V** | Valuable (Wartościowa) | Czy dostarcza realną korzyść biznesowi lub użytkownikowi? |
| **E** | Estimable (Szacowalna) | Czy zespół potrafi oszacować czas wykonania? |
| **S** | Small (Mała) | Czy zmieści się w jednej iteracji (sprincie)? |
| **T** | Testable (Testowalna) | Czy da się napisać scenariusze potwierdzające jej ukończenie? |

### 3. Ocena kryteriów akceptacji

**Jednoznaczność:**

- Wskaż "miękkie" sformułowania (*"system działa szybko"*, *"ładny interfejs"*) i skrytykuj je
- Kryterium musi być **mierzalne** — zamiast "szybko" → "poniżej 30 sekund"

**Testowalność:**

- Kryterium na tyle konkretne, że pozwala na **zero-jedynkowy test** weryfikujący warunek (np. test automatyczny)

!!! quote "Jak to powiedzieć egzaminatorowi"
    **O happy path:** "Skupiam się na poprawnym, bezbłędnym przepływie, który realizuje główny cel historyjki. Posiadając produkt na stanie, po kliknięciu *dodaj* ilość w koszyku zwiększa się o 1."

    **O ścieżkach negatywnych:** "Sprawdzam zachowanie systemu, gdy coś idzie nie tak. Próba dodania do koszyka produktu o stanie magazynowym 0 — oczekiwany komunikat o błędzie."

    **O INVEST:** "Pytam się po kolei: czy historyjka jest niezależna i można ją wdrożyć oddzielnie bez blokad? Czy jest negocjowalna — określa co chcemy osiągnąć, zostawiając zespołowi pole do dyskusji jak to zrobić? Czy jest wartościowa — dostarcza realną korzyść dla biznesu lub użytkownika? Czy jest szacowalna — zespół potrafi oszacować czas? Czy jest mała — zmieści się w jednej iteracji? I czy jest testowalna — da się napisać scenariusze potwierdzające jej ukończenie?"

    **O jednoznaczności kryteriów akceptacji:** "Wskazuję miękkie sformułowania — *system działa szybko*, *ładny interfejs* — i krytykuję je. Kryterium musi być mierzalne. Zamiast *szybko* powinno być *poniżej 30 sekund*. Sprawdzam też, czy kryterium jest na tyle konkretne, że pozwala na zbudowanie zero-jedynkowego testu weryfikującego dany warunek, np. testu automatycznego."
