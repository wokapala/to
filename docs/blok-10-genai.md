# Blok 10 — Testowanie generatywnej AI

## Scenariusz 10.1 — ewaluacja a test; projekt środowiska ewaluacyjnego

> Zaprojektuj zestaw ewaluacyjny agenta: strategię doboru przypadków ewaluacyjnych, ewaluatory i model oceniający. Opisz, jak porównasz warianty agenta za pomocą testów A/B.

### 1. Ewaluacja a test (różnica)

**Ewaluacje (evals)**

- Weryfikują **kompetencje i możliwości modelu**
- Odpowiadają na pytanie: *czy potrafi wykonać zadanie?*
- Sprawdzają wierność źródłom, ton itp.

**Testy A/B**

- Sprawdzają **rzeczywistą wartość dla użytkownika** i wpływ na biznes
- Mierzą: retencja, konwersja, wskaźnik odrzuceń
- Na **żywym ruchu**

### 2. Projekt zestawu ewaluacyjnego (Golden Dataset)

**Strategia doboru przypadków:**

- Zestaw zwany **"Golden Dataset"** musi być **reprezentatywny** dla środowiska produkcyjnego
- Różnorodny: ścieżki pozytywne, **przypadki brzegowe**, **ataki bezpieczeństwa** (np. **jailbreaki**)
- Musi być **"odkażony" (decontaminated)** — nie może pokrywać się z danymi treningowymi modelu
- Służy jako filtr do **testów regresyjnych** w potoku CI/CD

**Ewaluatory**

- Automatyczne sprawdziany oceniające konkretne wskaźniki:
  - stopień **halucynacji**
  - zgodność z dostarczonym kontekstem (**faithfulness**)
  - trafność

**Model oceniający (LLM-as-a-Judge)**

- Potężny model (np. GPT 5.4 / Opus 4.8 / Fable 5 / Mythos) ocenia subiektywne kryteria odpowiedzi: spójność, pomocność
- Skaluje się lepiej niż ocena ludzka
- !!! warning "Ograniczenia"
    Wbudowane uprzedzenia (np. **faworyzowanie własnego stylu wypowiedzi**). Ostateczna weryfikacja zawsze wymaga **nadzoru ekspertów domenowych**.

### 3. Porównanie wariantów — testy A/B

Trudniejsze niż w klasycznym oprogramowaniu — z uwagi na **niedeterministyczne (losowe) odpowiedzi modeli**.

**Rozmiar próby i metryki:**

- Bardzo duże próbki — często **ponad 10 000 interakcji na wariant**
- Cel: odróżnić **rzeczywistą poprawę jakości od losowego szumu**
- Ocena **wielowymiarowa**: trafność, koszty, czas odpowiedzi (latencja)

**Wstrzykiwanie opóźnień (Artificial Latency Injection)**

- Większe i mądrzejsze modele działają wolniej
- By rzetelnie ocenić jakość odpowiedzi bez zafałszowania wyników przez **irytację użytkownika długim czasem ładowania**
- **Celowo spowalniasz szybszy wariant kontrolny**

**Alternatywa — wieloręki bandyta (Multi-Armed Bandits)**

- Do szybkich iteracji na promptach
- Algorytm **automatycznie przekierowuje ruch** do lepiej działającej wersji
- Minimalizuje straty biznesowe

---

## Scenariusz 10.2 — jakość, ryzyka i bezpieczeństwo generatywnej AI

> Zaplanuj wykrywanie i ograniczanie halucynacji, uprzedzeń i błędów rozumowania.

### 1. Halucynacje (konfabulacje)

**Wykrywanie**

- Zautomatyzowane techniki (np. **SelfCheckGPT**) — mierzą częstotliwość zmyślonych twierdzeń przez badanie **spójności wielu generowanych odpowiedzi**

**Ograniczanie**

- Wbudowane techniki **fact-checkingu** — weryfikacja dokładności informacji, zwłaszcza z wielu źródeł
- **Ugruntowanie modelu** na zaufanych danych bazowych — np. **Retrieval-Augmented Generation (RAG)**

### 2. Uprzedzenia (Harmful Bias i stereotypy)

**Wykrywanie**

- Odpowiednie benchmarki (**Bias Benchmark Questions**, **Winogender Schemas**)
- **Fairness assessments** — pomiar wydajności systemu w przekroju różnych grup demograficznych

**Ograniczanie**

- Przegląd i ewaluacja **źródeł danych treningowych** pod kątem reprezentatywności i balansu
- Kontrolowanie **proporcji danych syntetycznych do rzeczywistych**
- Cel: uniknąć **homogenizacji** i **zjawiska zapaści modelu (model collapse)**

### 3. Błędy rozumowania i nieprzewidziane zachowania

**Wykrywanie**

- Zaawansowane, symulowane ataki:
  - **GAI red-teaming**
  - **Adversarial role-playing** (odgrywanie ról)
  - **Chaos testing**
- Cel: identyfikacja **ukrytych i nietypowych trybów awarii**

**Ograniczanie**

- **Human-in-the-Loop** — nadzór ludzki i eksperckie weryfikacje
- Techniki **objaśnialnego AI (XAI):**
  - analiza **osadzeń (embeddings)**
  - testowanie za pomocą **promptów kontrfaktycznych** — zrozumienie procesu decyzyjnego modelu
