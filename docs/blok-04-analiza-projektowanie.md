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
