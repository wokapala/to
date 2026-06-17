# Notatki MkDocs - jak uruchomić

## Krok 1 - Instalacja

Potrzebujesz Pythona 3.8+. Sprawdź:

```bash
python --version
```

Zainstaluj MkDocs Material:

```bash
pip install mkdocs-material
```

## Krok 2 - Test lokalny

W katalogu projektu (tam gdzie jest `mkdocs.yml`):

```bash
mkdocs serve
```

Otwórz w przeglądarce: http://localhost:8000

Edytujesz pliki w `docs/`, strona sama się odświeża.

## Krok 3 - Wrzucenie na GitHub

1. Załóż konto na github.com (jeśli nie masz)
2. Stwórz nowe **puste** repo, np. `notatki` (publiczne lub prywatne — jak chcesz)
3. W terminalu, w katalogu projektu:

```bash
git init
git add .
git commit -m "init"
git branch -M main
git remote add origin https://github.com/TWOJ_NICK/notatki.git
git push -u origin main
```

(Zamień `TWOJ_NICK` na swoją nazwę użytkownika GitHub)

## Krok 4 - Deploy na GitHub Pages

Jedna komenda:

```bash
mkdocs gh-deploy
```

Buduje stronę i pushuje na branch `gh-pages`.

Po ~1 minucie strona jest dostępna pod:

```
https://TWOJ_NICK.github.io/notatki/
```

## Krok 5 - Włącz GitHub Pages (jeśli trzeba)

Jeśli strona nie działa, wejdź na github.com → swoje repo → **Settings** → **Pages** i ustaw:

- Source: **Deploy from a branch**
- Branch: **gh-pages** / **/ (root)**

Zapisz. Po minucie strona powinna działać.

## Aktualizacje

Po każdej zmianie notatek:

```bash
git add .
git commit -m "update"
git push
mkdocs gh-deploy
```

## Tipy pod egzamin

- **Skrócony URL** — jeśli nazwiesz repo jednoliterowo (`n`), URL będzie `TWOJ_NICK.github.io/n` — krócej do wpisania
- **Wyszukiwanie** — klawisz `/` lub ikonka lupy w prawym górnym rogu, przeszukuje wszystkie pliki naraz
- **Repo prywatne** — jeśli notatki mają być niepubliczne, użyj **Cloudflare Pages** zamiast GitHub Pages (też darmowe, można dodać hasło)

## Struktura projektu

```
notatki-mkdocs/
├── mkdocs.yml              # konfiguracja
├── README.md               # ten plik
└── docs/
    ├── index.md            # strona główna
    ├── kryteria-oceny.md
    ├── blok-01-podstawy.md
    ├── blok-02-statyczne.md
    ├── blok-03-cykl-zycia.md
    ├── blok-04-analiza-projektowanie.md
    ├── blok-05-zarzadzanie.md
    ├── blok-06-jednostkowe-integracyjne.md
    ├── blok-07-wydajnosciowe.md
    ├── blok-08-kontraktowe.md
    ├── blok-09-bezpieczenstwo.md
    └── blok-10-genai.md
```

Pliki edytujesz zwykłym edytorem tekstu (VS Code, Notepad++, cokolwiek). Składnia to **Markdown** — proste, dużo tutoriali w sieci.
