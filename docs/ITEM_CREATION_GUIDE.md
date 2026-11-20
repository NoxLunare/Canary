# Crystalborne | Dodawanie nowego typu przedmiotu

**Task:** CRY-0679  
**Zakres:** tworzenie i weryfikacja definicji przedmiotu przy użyciu istniejących Blueprintów i Data Assets.  
**Środowisko testowe:** `Lvl_Test`.

## 1. Utwórz Data Asset

1. W Unreal Engine otwórz `Content/Crystalborne/Data/DataAssets/`.
2. Wybierz **Add / Right Click → Miscellaneous → Data Asset**.
3. Jako klasę wybierz istniejący `PDA_ItemBase`.
4. Nazwij asset zgodnie z konwencją `DA_<ItemName>`, np. `DA_Battery`.
5. Nie twórz osobnej klasy PrimaryDataAsset dla każdego nowego przedmiotu.

## 2. Uzupełnij ItemDefinition

Otwórz nowy asset i w polu `ItemDefinition` (`F_ItemDefinition`) ustaw:

| Pole | Zasada |
|---|---|
| `ItemID` | Unikalny identyfikator **typu** przedmiotu (`Name`), np. `battery_basic`; nie może być `None` |
| `DisplayName` | Nazwa widoczna dla gracza, np. `Battery` |
| `Description` | Krótki opis przeznaczenia |
| `Category` | Odpowiednia wartość `E_ItemCategory` |
| `Width`, `Height` | Dodatnie rozmiary przedmiotu; minimum `1 × 1` |
| `bCanRotate` | Czy przedmiot może być obracany w ekwipunku |
| `bIsQuestItem` | Czy przedmiot jest fabularny / zadaniowy |
| `bCanDrop` | Czy gracz może go upuścić |
| `ItemIcon` | Przypisz teksturę ikony (`Texture2D`); `None` jest zgłaszane przez walidator |

**Istotne:** `ItemID` identyfikuje *typ*. Każdy egzemplarz przedmiotu w świecie otrzymuje osobny `InstanceID` (`Guid`) w `BP_WorldItem`. Nie wpisuj GUID do `ItemID`.

Jeśli `PDA_ItemBase` zawiera starsze, powielone pola obok `ItemDefinition`, nie traktuj ich jako drugiej niezależnej definicji. Dla nowych danych korzystaj z `ItemDefinition`; zgodność pól wykorzystywanych przez inne systemy sprawdź przed ich usunięciem.

## 3. Uruchom walidację danych

1. Otwórz mapę `Lvl_Test`.
2. Zaznacz istniejącą instancję `BP_ItemDataValidator`.
3. Dodaj nowy asset do tablicy `ItemsToValidate`.
4. Uruchom **Play** i sprawdź komunikaty:
   - brak Data Asset,
   - pusty lub zduplikowany `ItemID`,
   - brak `ItemIcon`,
   - `Width <= 0` lub `Height <= 0`.
5. Popraw zgłoszone błędy i ponów test.

**Ograniczenie:** sprawdzanie, czy przedmiot jest większy od całej siatki ekwipunku, pozostaje poza zakresem bieżącej walidacji do czasu wdrożenia siatki (CRY-0676, Blocked).

## 4. Sprawdź przedmiot w świecie

1. Na `Lvl_Test` zaznacz `BP_TestItemSpawner`.
2. Przypisz nowy asset do `ItemToSpawn`.
3. Uruchom **Play**.
4. Potwierdź, że `BP_WorldItem` powstaje, a podpowiedź interakcji pokazuje `Pick up <DisplayName>`.
5. Jeśli uruchomiona jest walidacja po spawnie, sprawdź komunikat `ITEM DATA VALID`.
6. W razie testowania kilku egzemplarzy jednego typu sprawdź, że mają wspólny `ItemID`, ale różne `InstanceID`.

## 5. Zapisz zmiany

1. Przywróć tymczasowo zmienione dane testowe i usuń przypadkowe duplikaty z `ItemsToValidate`.
2. Kliknij **Save All**.
3. Sprawdź zmiany w GitHub Desktop i dołącz nowy asset do commita.

## Kryteria ukończenia

- [ ] Nowy `DA_<ItemName>` korzysta z `PDA_ItemBase`.
- [ ] `ItemDefinition` zawiera poprawny, unikalny `ItemID` i wymagane metadane.
- [ ] `BP_ItemDataValidator` nie zgłasza błędów dla nowego typu.
- [ ] `BP_TestItemSpawner` tworzy przedmiot z właściwą nazwą interakcji.
- [ ] Zmiany są zapisane i gotowe do commita.
