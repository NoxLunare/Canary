# Crystalborne | E04: Interakcje i trace

> Dokumentacja techniczna epika E04  
> Projekt: **Crystalborne** | Silnik: **Unreal Engine 5.8** | Platforma: **Windows**  
> Zakres backlogu: **CRY-0043 do CRY-0060** | Priorytet: **MUST**  
> Status na dzień **15.11.2026**: **17 Done / 1 Blocked**  
> Technologia: **Unreal Engine Blueprints, bez własnego kodu C++**

## 1. Architektura systemu

### 1.1. Lokalizacje zgodne ze strukturą projektu

Poniższe ścieżki są **docelowym przeznaczeniem folderów** według `PROJECT_STRUCTURE.md`. Bez bezpośredniego odczytu gałęzi projektu nie można potwierdzić, że konkretne assety już istnieją dokładnie w tych miejscach.

| Folder | Przeznaczenie |
| --- | --- |
| `Content/Crystalborne/Interaction/Interfaces/` | Interfejs interakcji, np. Blueprint Interface `BPI_Interactable` |
| `Content/Crystalborne/Interaction/Components/` | Komponent śledzenia i obsługi celu interakcji |
| `Content/Crystalborne/Interaction/Interactables/` | Klasy i Blueprinty obiektów reagujących na interakcje |
| `Content/Crystalborne/Interaction/Interactables/Doors/` | Przyszły docelowy Blueprint drzwi i test animacji |
| `Content/Crystalborne/Interaction/Pickups/` | Podnoszone przedmioty i zasoby |
| `Content/Crystalborne/Interaction/Puzzles/` | Obiekty i logika zagadek |
| `Content/Crystalborne/Core/Input/Actions/` | Input Action dla Interact |
| `Content/Crystalborne/Core/Input/MappingContexts/` | Kontekst sterowania z E02/E03 |
| `Content/Crystalborne/Data/Enums/` | Identyfikatory lub enumy typu interakcji, o ile taki typ został zastosowany |
| `Content/Crystalborne/UI/HUD/` | Widget wskazówki interakcji |
| `Content/Crystalborne/Characters/Player/Blueprints/` | Integracja systemu z postacią FPP |
| `Content/Crystalborne/Testing/Maps/` | Scenariusze testowe interakcji |

### 1.2. Ogólny przepływ działania

```text
Kamera FPP
   |
   v
Line Trace (kierunek patrzenia, ograniczony zasięg)
   |
   +--> brak trafienia lub obiekt nieinteraktywny
   |         |
   |         v
   |      ExitFocus poprzedniego celu
   |      ukrycie promptu / usunięcie podświetlenia
   |
   +--> trafienie obiektu obsługującego interfejs
             |
             v
       porównanie z aktualnym celem
             |
             +--> zmiana celu: ExitFocus(stary), EnterFocus(nowy)
             |
             v
       CanInteract + powód odmowy
             |
             v
       aktualizacja UI promptu i podświetlenia
             |
             v
       wejście Interact (Enhanced Input)
             |
             v
       ponowna walidacja celu, zasięgu i dostępności
             |
             v
       zabezpieczenie przed powtórnym wykonaniem
             |
             v
       wywołanie interakcji na docelowym obiekcie
```

Jest to **model przepływu**, a nie potwierdzenie, że każda kontrola jest wykonana w identycznej kolejności lub tymi samymi węzłami. Ważną zasadą jest oddzielenie *wykrycia i wyświetlenia promptu* od *autoryzacji faktycznego wykonania interakcji*.

### 1.3. Kontrakty między modułami

- **Postać FPP (E03):** udostępnia kamerę i źródło sterowania. Stan ruchu i postawa nie powinny powodować utraty synchronizacji wybranego celu.
- **Komponent interakcji:** odpowiada za trace, aktualny cel, walidację i przekazanie żądania do interfejsu. Sam w sobie nie definiuje zachowania każdego rodzaju obiektu.
- **Interfejs obiektu:** umożliwia ujednolicone pytania o dostępność interakcji oraz wywołanie jej bez uzależnienia gracza od konkretnej klasy drzwi, terminala czy przedmiotu.
- **UI HUD:** wyświetla komunikat na podstawie aktualnego celu, identyfikatora typu akcji i wyniku walidacji. Nie powinien samodzielnie podejmować decyzji, czy interakcję wykonać.
- **Wizualny focus:** służy do oznaczania aktualnie wskazanego obiektu; musi być czyszczony po jego zmianie, zniknięciu lub utracie zasięgu.


## 2. Reguły działania i sytuacje brzegowe

| Przypadek | Oczekiwane zachowanie systemu |
| --- | --- |
| Trace nie trafia w żaden obiekt | Brak aktualnego celu i ukryty prompt |
| Trace trafia w zwykły obiekt bez interfejsu | Brak interakcji; poprzedni focus zostaje zakończony |
| Kamera nadal wskazuje ten sam obiekt | Focus pozostaje stabilny, bez ponawiania EnterFocus w każdej klatce |
| Kamera przechodzi z obiektu A na B | ExitFocus(A), następnie EnterFocus(B) |
| `CanInteract` zwraca odmowę | Nie wykonuje się akcja; dostępny jest powód odmowy |
| Gracz wychodzi poza zasięg | Brak możliwości Interact; cel i UI nie mogą pozostać nieaktualne |
| Obiekt zostaje usunięty / zniszczony | Referencja musi zostać bezpiecznie unieważniona (scenariusz do weryfikacji) |
| Szybki ruch kamery | Nie dochodzi do interakcji ze starym celem |
| Dwa szybkie żądania Interact | Brak niezamierzonego podwójnego skutku |
| Interact podczas animacji drzwi | **Niezweryfikowane: CRY-0060 Blocked** |

Przypadki z tej tabeli wyznaczają zamierzony kontrakt jakościowy. Nie wszystkie są osobno potwierdzonymi testami wykonanymi na aktualnym buildzie.

## 3. Testy akceptacyjne i regresyjne

| Test | Powiązane zadania | Scenariusz | Oczekiwany rezultat | Status wg backlogu |
| --- | --- | --- | --- | --- |
| T-E04-01 | 0043, 0046 | Interaktywny i zwykły aktor | Tylko aktor implementujący interfejs jest celem | Done |
| T-E04-02 | 0044, 0045 | Cel blisko i poza promieniem | Ograniczony zasięg wykrywania | Done |
| T-E04-03 | 0047, 0048 | Wskazanie A, B, a potem pustej przestrzeni | Prawidłowe przejścia focusu | Done |
| T-E04-04 | 0049, 0050 | Użycie różnych testowych obiektów | Wspólne wejście i komponent delegują interakcję | Done |
| T-E04-05 | 0051, 0052, 0053 | Cel z niedostępną akcją | Odmowa i jej powód, bez wykonania efektu | Done |
| T-E04-06 | 0054, 0055 | Patrzenie na cel i użycie wejścia | Prompt odpowiada aktualnemu celowi i akcji | Done |
| T-E04-07 | 0056 | Szybka zmiana fokusu | Podświetlony wyłącznie aktualny obiekt | Done |
| T-E04-08 | 0057 | Dwa szybkie wywołania | Brak podwójnego wykonania | Done |
| T-E04-09 | 0058 | Wyjście poza zasięg po wcześniejszym focusie | Brak wykonania interakcji | Done |
| T-E04-10 | 0059 | Szybkie obracanie kamerą pomiędzy celami | Brak starego celu / promptu / highlightu | Done |
| T-E04-11 | 0060 | Ponowna interakcja w trakcie animacji drzwi | Kontrolowane zachowanie stanu i animacji | **Blocked: brak assetu drzwi** |

## 4. Zależności z innymi epikami

- **E02, Repozytorium i infrastruktura:** struktura `Content/Crystalborne/`, mapa testowa, konfiguracja Enhanced Input i GameMode.
- **E03, FPP i movement:** system pobiera punkt widzenia z kamery FPP. Kucanie i camera bob mogą zmieniać miejsce, z którego wykonywany jest trace; należy zachować stabilne wskazanie celu. Powiązany test CRY-0042 (zmiana postawy podczas interakcji) pozostawał `Blocked` w przekazanym backlogu E03.
- **Drzwi i level design:** udostępnienie obiektu drzwi umożliwi test CRY-0060 oraz wcześniejszy test kapsuły z drzwiami (CRY-0038).
- **UI:** HUD prezentuje dane otrzymywane z logiki interakcji, nie przechowuje osobnego niezależnego celu.
- **Puzzles i inventory:** przyszłe obiekty wykorzystują interfejs i walidację `CanInteract` bez kopiowania całego systemu trace.

## 5. Ryzyka, ograniczenia i dalsze kroki

| Obszar | Ryzyko / uwaga | Następny krok |
| --- | --- | --- |
| CRY-0060 | Brak assetu drzwi uniemożliwia test podczas animacji | Dodać drzwi z animacją i ponowić scenariusz |
| Synchronizacja danych | Wynik trace może się zdezaktualizować między odczytem a naciśnięciem | Zweryfikować walidację w momencie Interact |
| UI i focus | Zmiana celu może zostawić stary prompt lub podświetlenie | Utrzymać test A → B → brak celu w regresji |
| Obiekty niszczone | Referencje mogą stać się niepoprawne | Przetestować usunięcie celu w trakcie fokusu |
| Wiele komponentów mesh | Highlight może dotyczyć tylko fragmentu obiektu | Zweryfikować rzeczywiste obiekty produkcyjne |
| E03 / E04 | Zmiana postawy podczas interakcji nadal wymaga testu integracyjnego | Wrócić do CRY-0042 po udostępnieniu odpowiedniej sceny |
