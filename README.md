# CRYSTALBORNE

**A Sci-Fi Survival Horror Experience**

*Explore the unknown. Uncover forgotten memories. Resist the corruption.*

---

## O projekcie

**Crystalborne** to pierwszoosobowy survival horror science fiction, którego akcja rozgrywa się na tajemniczej planecie z opuszczonym kompleksem górniczym.

Gracz wciela się w samotnego odkrywcę, który odbiera nieznany sygnał alarmowy i postanawia zbadać jego źródło. Eksplorując kopalnię, odkrywa tajemnicze kryształy zdolne do przechowywania wspomnień oraz ślady obecności nieznanej istoty.

Rozgrywka łączy eksplorację, zagadki środowiskowe, zarządzanie zasobami i unikanie zagrożeń. Istotnym elementem projektu jest mechanika **Corruption**, która wprowadza dodatkowe ryzyko i wpływa na doświadczenie gracza.

Projekt powstaje w ramach studiów podyplomowych z projektowania gier komputerowych.

## Technologie

- **Silnik:** Unreal Engine 5
- **Programowanie:** Blueprint Visual Scripting
- **Kontrola wersji:** Git i GitHub
- **Platforma docelowa:** PC

## Getting Started

### 1. Wymagania

Przed rozpoczęciem pracy zainstaluj:

- **Unreal Engine 5** w wersji zgodnej z projektem (sprawdź plik `.uproject` i ustalenia zespołu).
- **Git** do klonowania i synchronizowania repozytorium.
- **GitHub Desktop** (opcjonalnie), do wygodniejszej obsługi Gita.

Projekt powstaje w **Blueprint Visual Scripting**, dlatego Visual Studio nie jest wymagane.

### 2. Klonowanie repozytorium

Otwórz terminal w wybranym katalogu i wykonaj:

```bash
git clone <repository-url>
cd <repository-folder>
```

Możesz również sklonować projekt w GitHub Desktop przez **File > Clone Repository**.

### 3. Uruchomienie projektu

1. Otwórz folder sklonowanego repozytorium.
2. Znajdź plik `Crystalborne.uproject`.
3. Otwórz go w odpowiedniej wersji Unreal Engine.
4. Poczekaj na załadowanie projektu i przygotowanie shaderów.
5. Otwórz mapę projektu w **Content Browser**.

Przy pierwszym uruchomieniu przygotowanie projektu może potrwać dłużej.

### 4. Rozpoczęcie pracy

Przed rozpoczęciem nowego zadania zaktualizuj główną gałąź:

```bash
git switch main
git pull
```

Utwórz osobny branch:

```bash
git switch -c feature/nazwa-funkcjonalnosci
```

Po zakończeniu pracy zapisz wszystkie zmiany w Unreal Engine, a następnie wykonaj:

```bash
git add .
git commit -m "feat: opis wprowadzonych zmian"
git push -u origin feature/nazwa-funkcjonalnosci
```

Na GitHubie utwórz **Pull Request** do głównej gałęzi.

### 5. Zasady współpracy

- Rozwijaj nowe funkcjonalności na osobnych branchach.
- Regularnie synchronizuj lokalne repozytorium.
- Uzgadniaj z zespołem edycję współdzielonych Blueprintów i map.
- Umieszczaj assety zgodnie z ustaloną strukturą katalogów.
- Nie commituj katalogów generowanych lokalnie: `Binaries/`, `Intermediate/`, `Saved/` i `DerivedDataCache/` (upewnij się, że obejmuje je `.gitignore`).
- Przed utworzeniem Pull Requesta sprawdź, czy projekt uruchamia się poprawnie.

> **Ważne:** Blueprinty (`.uasset`) i mapy (`.umap`) są plikami binarnymi. Git nie scala ich automatycznie tak jak zwykłych plików tekstowych, dlatego nie należy równolegle edytować tych samych zasobów bez uzgodnienia.

## Dokumentacja

Szczegółowy opis organizacji plików i zasobów znajdziesz tutaj:

**[Project Structure](docs/PROJECT_STRUCTURE.md)**

Dokumentacja będzie uzupełniana wraz z rozwojem projektu.

## Status projektu

**W trakcie rozwoju | Preprodukcja / Prototypowanie**

Projekt znajduje się na wczesnym etapie rozwoju. Zakres mechanik i funkcjonalności może się zmieniać w kolejnych iteracjach.