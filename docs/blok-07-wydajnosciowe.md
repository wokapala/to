# Blok 7 — Testy wydajnościowe

## Scenariusz 7.1 — projektowanie scenariuszy wydajnościowych

> Na przykładzie serwisu sprzedaży biletów lub platformy wyprzedażowej zaprojektuj kilka profili obciążenia: obciążeniowy, stresowy, skokowy i wytrzymałościowy. Do każdego podaj warunki uruchomienia, kryteria oceny i sposób wykrywania degradacji wydajności oraz wycieków zasobów narastających w czasie.

**Przykład:** system sprzedaży biletów na duży festiwal muzyczny.

### 1. Testy obciążeniowe (Load Testing)

- **Warunki uruchomienia:** symulacja normalnego, przewidywanego wysokiego ruchu — np. standardowy dzień przedsprzedaży
- **Kryteria oceny:** system działa stabilnie, czasy odpowiedzi w normie
- **Ważne:** mierz nie tylko **średnią**, ale przede wszystkim **percentyle** (np. czas odpowiedzi dla 99% najwolniejszych żądań)

### 2. Testy stresowe (Stress Testing)

- **Warunki uruchomienia:** stopniowe zwiększanie ruchu **"schodkami"** daleko poza przewidywane limity, aż do całkowitej awarii
- **Kryteria oceny:**
  - wyznaczenie **maksymalnej pojemności** serwera
  - sprawdzenie, **w jaki sposób system umiera** — z gracją (przyjazny komunikat o błędzie) czy rzuca surowe błędy / całkowicie zamarza

### 3. Testy skokowe (Spike Testing)

- **Warunki uruchomienia:** nagłe uderzenie potężnego obciążenia w jednej chwili — np. równo o 12:00, gdy startuje główna pula biletów
- **Kryteria oceny:** czy system obsłuży nagłą "bombę" ruchu i czy odpowiednio **opóźnia lub kolejkuje żądania**, zamiast je gubić / odrzucać

### 4. Testy wytrzymałościowe (Endurance Testing) i wycieki zasobów

- **Warunki uruchomienia:** system biegnie **maraton** — ciągły, umiarkowany lub wysoki ruch przez bardzo długi czas (wiele godzin / dni)
- **Wykrywanie degradacji:**
  - stałe monitorowanie metryk sprzętowych (RAM, CPU) — narzędzia typu **Grafana**
  - jeśli mimo stałego ruchu zużycie pamięci nieustannie rośnie i nie jest czyszczone → **wyciek pamięci (memory leak)**, który spowodowałby awarię na produkcji

---

## Scenariusz 7.2 — cele i metryki wydajności

> Zdefiniuj cele (SLO, SLI, SLA) i dobierz właściwe metryki. Zaproponuj, jak zdiagnozujesz wąskie gardło i jak podejdziesz do planowania "budżetu błędów".

### 1. SLI, SLO i SLA

| Skrót | Co to | Przykład |
|-------|-------|----------|
| **SLI** (Service Level Indicator — wskaźnik) | Konkretna, mierzalna wartość z działającego systemu | Rzeczywisty czas odpowiedzi serwera, dostępność usługi |
| **SLO** (Service Level Objective — cel) | Nasz wewnętrzny, założony cel dla SLI | "99% zapytań obsłużonych w czasie poniżej 200 ms" |
| **SLA** (Service Level Agreement — umowa) | Zewnętrzny, formalny kontrakt z klientem — najczęściej z karami finansowymi | Gwarantowana dostępność z karami za niedotrzymanie |

!!! warning "Wskazówka — mocno akcentowane na wykładzie"
    **SLA nigdy nie może wynosić 100%** — nieuniknione są awarie poza naszą kontrolą. Nawet najwięksi dostawcy chmur dają np. 99,95%.

### 2. Dobór właściwych metryk

!!! danger "Najważniejsza zasada: średnia kłamie"
    Średni czas odpowiedzi ukrywa to, że część klientów może doświadczać ogromnych opóźnień.

Zamiast średniej:

- **Percentyle** (p90, p95, p99) — zachowanie dla najwolniejszych żądań
- **Przepustowość** (requesty na sekundę)
- **Błędy** (np. 500)

### 3. Diagnozowanie wąskiego gardła

Narzędzia monitorowania w czasie rzeczywistym (**Grafana**, **Prometheus**).

- Wykres pokazuje, że percentyl dla najwolniejszych żądań (np. top 1%) **drastycznie rośnie** (np. z 2 do 5 s) → identyfikacja anomalii
- Częsta przyczyna **dławienia się** — operacje na bazie danych (brak odpowiednich indeksów)

### 4. Budżet błędów (Error Budget)

**Definicja:** dopuszczalny margines awarii — różnica między 100% a Twoim SLO.

- Przykład: SLO dostępności = 99% → budżet błędów = **1%**

**Podejście:**

- Budżet daje zespołowi **swobodę we wdrażaniu nowości**
- Dopóki masz zapas → możesz ryzykować szybkie wdrożenia
- Po wyczerpaniu budżetu (zbyt duża awaryjność) → **wstrzymujesz wydawanie nowych funkcji** i cały zespół zajmuje się stabilizacją i naprawą długu technologicznego, by nie złamać SLA
