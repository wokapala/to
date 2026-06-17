# Blok 6 — Testy jednostkowe i integracyjne

## Scenariusz 6.1 — testy jednostkowe i ich struktura

> Dla wybranej logiki biznesowej zaprojektuj przypadki w strukturze AAA (przygotowanie–działanie–weryfikacja): poprawny, graniczny i obsługę błędu. Sparametryzuj je (testy sterowane danymi), zdecyduj, co właściwie weryfikujesz (wynik, stan czy komunikację), i zilustruj cykl Red–Green–Refactor na przykładzie zaprojektowanego testu.

### 1. Struktura AAA (Arrange–Act–Assert)

**Przykład:** funkcja `ObliczZniżkę(wiek)`.

- **Arrange (Przygotowanie):** ustawienie stanu początkowego — inicjalizacja klasy, dane wejściowe (`wiek = 20`)
- **Act (Działanie):** wywołanie testowanej metody — `wynik = ObliczZniżkę(wiek)`
- **Assert (Weryfikacja):** sprawdzenie rezultatu — `wynik == 0`

**Trzy przypadki:**

| Typ | Przykład |
|-----|----------|
| **Poprawny (Happy Path)** | Typowe dane — `20` lat (brak zniżki) |
| **Graniczny** | Punkt styku — `18` lat |
| **Obsługa błędu** | Nieprawidłowe dane — wiek `-5`, oczekiwany rzut wyjątkiem |

### 2. Parametryzacja (testy sterowane danymi)

Zamiast trzech osobnych testów — **jeden test sparametryzowany**. Wstrzykuje różne zestawy danych do tej samej struktury AAA:

```
[
  [20, 0],
  [18, 10%],
  [-5, Error]
]
```

### 3. Co właściwie weryfikujemy?

Podczas asercji możemy badać trzy rzeczy:

| Typ | Co sprawdzamy | Kiedy |
|-----|---------------|-------|
| **Wynik** | Wartość zwróconą przez metodę | Kalkulacja matematyczna |
| **Stan** | Czy zmieniła się właściwość obiektu | Flaga `IsActive` zmieniła się na `True` |
| **Komunikację** | Czy testowany moduł wywołał metodę z zewnętrznej zależności | Czy wywołano `ZapiszDoBazy()` — używamy **mocków** |

### 4. Cykl Red–Green–Refactor (TDD)

**Red (Czerwony)**

- Piszesz test (ujemny wiek rzuca wyjątek)
- Test nie przechodzi — brak implementacji

**Green (Zielony)**

- Dopisujesz **najprostszą możliwą linię kodu**:

  ```javascript
  if (wiek < 0) throw error;
  ```

- Test świeci się na zielono

**Refactor (Refaktoryzacja)**

- Poprawiasz jakość i czytelność kodu (lub samego testu)
- Wiesz, że działający test chroni Cię przed zepsuciem logiki

!!! quote "Jak to powiedzieć egzaminatorowi"
    **O AAA:** "W fazie Arrange ustawiam stan początkowy — inicjalizuję klasę i przygotowuję dane wejściowe. W fazie Act wywołuję testowaną metodę. W fazie Assert sprawdzam, czy rezultat jest zgodny z oczekiwaniami."

    **O trzech przypadkach:** "Pokazuję trzy: poprawny, czyli happy path z typowymi danymi; graniczny, czyli punkt styku — równe 18 lat; oraz obsługę błędu — nieprawidłowe dane, oczekiwany rzut wyjątkiem."

    **O parametryzacji:** "Zamiast pisać trzy osobne testy, tworzę jeden test sparametryzowany. Pozwala to na wstrzyknięcie różnych zestawów danych do tej samej struktury AAA."

    **O tym, co weryfikujemy:** "Podczas asercji mogę badać trzy rzeczy. Wynik — sprawdzam wartość zwróconą bezpośrednio przez metodę, np. kalkulację matematyczną. Stan — sprawdzam, czy zmieniła się właściwość obiektu po wykonaniu akcji. Komunikację — sprawdzam, czy testowany moduł wywołał metodę z zewnętrznej zależności, używając do tego mocków."

    **O TDD:** "Najpierw piszę test — np. ujemny wiek rzuca wyjątek. Test nie przechodzi, bo nie mam jeszcze implementacji — to faza Red. Następnie dopisuję najprostszą możliwą linię kodu, żeby test zaświecił się na zielono — faza Green. Na koniec poprawiam jakość i czytelność kodu lub samego testu — Refactor — wiedząc, że działający test chroni mnie przed zepsuciem logiki."

---

## Scenariusz 6.2 — izolacja, dublery testowe i jakość testów

> Wybierz styl testowania (klasyczny albo londyński) i odpowiedni dubler (mock, stub, fake). Zdecyduj, jak podejść do integracji z bazą czy brokerem wiadomości. Na koniec oceń jakość testów: pokrycie to nie to samo co jakość, więc omów też testowanie mutacyjne i niestabilność testów.

### 1. Style testowania i dublery

**Styl klasyczny (Detroit)**

- Izolujesz **testy od siebie**, nie poszczególne klasy
- Pozwalasz na rzeczywiste zależności (inne klasy), o ile nie są wolne (np. baza danych)
- Dublery: **Stub** (zwracają sztywne dane), **Fake** (uproszczone, w pełni działające implementacje)

**Styl londyński (Mockist)**

- Izolujesz testowaną jednostkę (klasę) od **absolutnie wszystkich** zależności
- Do każdej zależności tworzysz **Mock** — weryfikujesz **komunikację** (czy klasa A poprawnie wywołała metodę z klasy B)

### 2. Integracja z bazą i brokerem wiadomości

!!! warning "Kluczowe"
    W testach jednostkowych **nigdy nie łączymy się z rzeczywistą bazą czy brokerem**. Zewnętrzne zależności są wolne i niestabilne — testy stają się wolne i często fałszywie wykazują błędy z powodu np. chwilowej niedostępności usługi.

**Rozwiązanie:**

- W testach jednostkowych bazę zastępujesz przez **Fake** (np. lekka baza in-memory)
- Testowanie **rzeczywistej komunikacji** z bazą i brokerem zostawiasz dla wyższej warstwy — **testów integracyjnych**

### 3. Ocena jakości testów

**Pokrycie ≠ jakość**

- Osiągnięcie 100% pokrycia instrukcji **nie gwarantuje**, że kod działa poprawnie
- Test może przejść przez wszystkie linijki i nie wykryć problemu, który "wybuchnie" na produkcji (np. wstrzyknięcie zera → dzielenie przez zero)
- Testy pisane tylko pod statystyki bywają **puste w środku** (brak mocnych asercji)

**Testowanie mutacyjne** — technika mierząca jakość testów

- Narzędzie celowo modyfikuje kod źródłowy → **mutanty** (np. zmienia `+` na `-`)
- Dobry test powinien **"zabić mutanta"** — zaświecić się na czerwono
- Jeśli test nadal przechodzi dla zepsutego kodu, jego asercje są **bezwartościowe**

**Niestabilność testów (Flakiness)**

- Testy dające **losowe wyniki** (raz przechodzą, raz nie) pomimo braku zmian w kodzie
- Przyczyna: testowanie na żywym, niestabilnym API dostawcy
- Skutek: **całkowita utrata zaufania** — zespół zaczyna ignorować błędy, zakładając "ten test tak ma"

!!! quote "Jak to powiedzieć egzaminatorowi"
    **O stylu klasycznym (Detroit):** "Izoluję testy od siebie, a nie poszczególne klasy. Pozwalam na korzystanie z rzeczywistych zależności — innych klas — o ile nie są wolne, jak baza danych. Stosuję głównie Stub, który zwraca sztywne, przygotowane dane, lub Fake, czyli uproszczoną w pełni działającą implementację."

    **O stylu londyńskim (Mockist):** "Izoluję testowaną jednostkę od absolutnie wszystkich jej zależności. Do każdej zależności tworzę Mock, za pomocą którego weryfikuję komunikację — np. sprawdzam, czy klasa A poprawnie wywołała metodę z klasy B."

    **O bazie i brokerze:** "W testach jednostkowych nigdy nie łączymy się z rzeczywistą bazą czy brokerem. Zewnętrzne zależności są wolne i niestabilne, co sprawia, że testy stają się wolne i często fałszywie wykazują błędy z powodu np. chwilowej niedostępności usługi. W testach jednostkowych bazę zastępuję za pomocą Fake, np. lekkiej bazy in-memory. Testowanie rzeczywistej komunikacji zostawiam wyłącznie dla wyższej warstwy — testów integracyjnych."

    **O pokryciu vs jakości:** "Osiągnięcie 100% pokrycia instrukcji nie daje gwarancji, że kod działa poprawnie. Test może przejść przez wszystkie linijki i nie wykryć problemu, który w rzeczywistości wybuchnie na produkcji — np. wstrzyknięcie zera skutkujące dzieleniem przez zero. Testy pisane tylko pod statystyki pokrycia bywają puste w środku — brak mocnych asercji."

    **O testowaniu mutacyjnym:** "To technika mierząca jakość testów. Narzędzie celowo modyfikuje kod źródłowy — tworzy mutanty, np. zmienia operator plus na minus. Dobry test powinien zabić mutanta, czyli zaświecić się na czerwono. Jeśli test nadal przechodzi dla zepsutego kodu, oznacza to, że jego asercje są bezwartościowe."

    **O flakiness:** "Są to testy, które dają losowe wyniki — raz przechodzą, raz nie — pomimo braku zmian w kodzie. Skutkuje to całkowitą utratą zaufania — zespół zaczyna ignorować błędy, zakładając, że ten test tak ma."
