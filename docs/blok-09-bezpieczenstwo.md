# Blok 9 — Ryzyka bezpieczeństwa i RODO

## Scenariusz 9.1 — modelowanie zagrożeń i ryzyko bezpieczeństwa

> Dla aplikacji zidentyfikuj i wyceń aktywa, przeprowadź modelowanie zagrożeń i oszacuj ryzyko.

### 1. Identyfikacja i wycena aktywów

**Typy aktywów:**

- **Cyfrowe** — dane osobowe klientów
- **Fizyczne** — serwery
- **Niematerialne** — wizerunek marki, zaufanie

**Wycena oparta na:**

- utraconych przyszłych przychodach
- koszcie odtworzenia
- wartości dla konkurencji
- **potencjalnych karach finansowych** (np. za złamanie RODO)

### 2. Modelowanie zagrożeń — metoda STRIDE

Szukasz luk w **sześciu kategoriach**:

| Litera | Pełna nazwa | Co to |
|--------|-------------|-------|
| **S** | **Spoofing** | Podszywanie się pod innego użytkownika |
| **T** | **Tampering** | Nieuprawniona modyfikacja danych (np. kwoty przelewu) |
| **R** | **Repudiation** | Zaprzeczalność — brak dowodów, że użytkownik wykonał akcję |
| **I** | **Information Disclosure** | Wyciek poufnych informacji |
| **D** | **Denial of Service** | Odmowa usługi (np. atak DDoS) |
| **E** | **Elevation of Privilege** | Nieuprawnione podniesienie uprawnień do poziomu administratora |

### 3. Szacowanie ryzyka

**Wzór:**

```
Poziom ryzyka = Prawdopodobieństwo ataku × Wpływ (skutek)
```

Na podstawie równania i stworzonej macierzy przypisujesz zagrożenia do kategorii: **Low / Medium / High / Critical**.

---

## Scenariusz 9.2 — podatności i testy bezpieczeństwa

> Dla aplikacji internetowej zaplanuj testy wybranych kategorii podatności z OWASP Top 10 (m.in. wstrzyknięcie SQL), ustal priorytety i przygotuj raport zawierający między innymi opis, wpływ i rekomendację.

### 1. Plan testów podatności

**SQL Injection**

- Testuj pola wejściowe formularzy, wstrzykując fragmenty zapytań SQL (np. z `UNION SELECT`)
- Cel: czy system zwróci **nieuprawnione dane** ze struktury bazy

**Cross-Site Scripting (XSS)**

- Wprowadź złośliwy kod JavaScript (np. `<script>alert(1)</script>`) w pola wejściowe
- Cel: czy aplikacja wykona go w przeglądarkach innych użytkowników (np. **kradzież cookies**)

**Podatności uwierzytelniania**

- Ataki **brute-force** z słownikami popularnych haseł
- Badaj zachowanie: brak blokady po serii błędnych prób, zróżnicowany czas odpowiedzi serwera (ułatwia zgadywanie loginów)

### 2. Ustalenie priorytetów

**Macierz ryzyka:**

```
Priorytet = Prawdopodobieństwo × Wpływ na biznes
```

- **SQLi, XSS** → niemal zawsze **Critical** lub **High** — umożliwiają masowy wyciek danych / przejęcie kontroli

### 3. Struktura raportu (na przykładzie SQLi)

**Opis**

- Brak walidacji i bezpośrednie przekazywanie danych użytkownika do bazy w module logowania
- Dokładne kroki do reprodukcji i miejsce w aplikacji

**Wpływ**

- Całkowita kompromitacja zasobów cyfrowych
- Atakujący może zrzucić całą strukturę bazy i wyciągnąć dane klientów
- Konsekwencje dla biznesu i użytkownika

**Rekomendacja**

- **Zapytania sparametryzowane** po stronie serwera
- Ścisła **walidacja i sanityzacja** danych wejściowych

---

## 4. Pełna lista OWASP Top 10

| Kod | Nazwa | Co testować |
|-----|-------|-------------|
| **A01** | **Broken Access Control** (brak kontroli dostępu) | Próba dostępu do funkcji administratora ze zwykłego konta, manipulacja parametrami URL w celu wyświetlenia danych innych osób |
| **A02** | **Security Misconfiguration** (błędy konfiguracji) | Czy system korzysta z domyślnych haseł, czy błędy aplikacji ujawniają zrzuty pamięci (**stack traces**) |
| **A03** | **Software Supply Chain Failures** (awarie łańcucha dostaw) | Czy zewnętrzne biblioteki są aktualne, wolne od podatności, pobierane z zaufanych źródeł |
| **A04** | **Cryptographic Failures** (błędy kryptograficzne) | Czy poufne dane przesyłane są w czystym tekście, czy używane są przestarzałe algorytmy szyfrujące |
| **A05** | **Injection** (wstrzyknięcia — SQL, XSS) | Wprowadzanie złośliwego kodu w polach formularza w celu ominięcia zabezpieczeń i wykonania nieautoryzowanych operacji |
| **A06** | **Insecure Design** (niezabezpieczony projekt) | Luki w architekturze i logice biznesowej — etap, na którym zapomniano wdrożyć mechanizmy bezpieczeństwa |
| **A07** | **Authentication Failures** (błędy uwierzytelniania) | Brute-force, prawidłowe wygasanie sesji po wylogowaniu |
| **A08** | **Software or Data Integrity Failures** (błędy integralności) | Weryfikacja infrastruktury CI/CD, automatycznych aktualizacji pod kątem złośliwego kodu |
| **A09** | **Logging & Alerting Failures** (brak logowania i monitorowania) | Czy system rejestruje krytyczne zdarzenia (błędne próby logowania) i powiadamia administratorów |
| **A10** | **Mishandling of Exceptional Conditions** (niewłaściwa obsługa wyjątków) | Jak aplikacja reaguje na nietypowe awarie — czy nie załamuje się i nie ujawnia luk przez błędną obsługę wyjątków |

!!! note "Lista OWASP Top 10"
    Zgodnie z kryteriami oceny — **nie wymaga się pamięciowej znajomości wszystkich pozycji**. Możesz korzystać z notatek lub komputera. Oceniana jest **umiejętność trafnego dobrania i zastosowania** informacji.
