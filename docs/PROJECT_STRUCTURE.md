# Crystalborne | Struktura projektu

Poniżej znajduje się struktura folderów projektu wraz z opisem ich przeznaczenia.

```text

Content/
└── Crystalborne/
    │
    ├── AI/                             # Sztuczna inteligencja przeciwników
    │   ├── BehaviorTrees/              # Drzewa zachowań: patrol, pościg, poszukiwanie gracza
    │   ├── Blackboards/                # Pamięć AI: pozycja gracza, cel, ostatni dźwięk
    │   ├── Controllers/                # Kontrolery sterujące zachowaniem przeciwników
    │   ├── Decorators/                 # Warunki wykonywania poszczególnych zachowań
    │   ├── EQS/                        # Wyszukiwanie punktów patrolu, ukrycia i obserwacji
    │   ├── Perception/                 # Wykrywanie gracza poprzez wzrok i słuch
    │   ├── Services/                   # Aktualizowanie stanu i informacji AI
    │   └── Tasks/                      # Akcje AI: ruch, atak, szukanie, ucieczka
    │
    ├── Art/                            # Zasoby graficzne i efekty wizualne
    │   ├── Decals/                     # Ślady, pęknięcia, zabrudzenia i oznaczenia
    │   ├── Materials/                  # Materiały przypisywane modelom 3D
    │   │   ├── Functions/              # Funkcje wielokrotnego użytku w materiałach
    │   │   ├── Instances/              # Warianty materiałów o różnych parametrach
    │   │   └── MasterMaterials/        # Materiały bazowe dla skał, metalu i kryształów
    │   ├── Meshes/                     # Modele 3D wykorzystywane w grze
    │   │   ├── Environment/            # Skały, ściany, podłogi i konstrukcje kopalni
    │   │   ├── Equipment/              # Narzędzia, latarki i wyposażenie gracza
    │   │   ├── Interactables/          # Modele drzwi, terminali i skrzyń
    │   │   └── Props/                  # Beczki, przewody, skrzynki i dekoracje
    │   ├── PostProcess/                # Efekty obrazu, zakłócenia i halucynacje
    │   ├── Textures/                   # Tekstury powierzchni i elementów graficznych
    │   │   ├── Decals/                 # Tekstury śladów, napisów i zabrudzeń
    │   │   ├── Environment/            # Tekstury skał, piasku, metalu i ścian
    │   │   ├── Props/                  # Tekstury przedmiotów i wyposażenia
    │   │   └── UI/                     # Ikony, tła i elementy wizualne interfejsu
    │   └── VFX/                        # Efekty cząsteczkowe i wizualne
    │       ├── Corruption/             # Efekty skażenia i zniekształcenia rzeczywistości
    │       ├── Crystals/               # Świecenie, energia i aktywacja kryształów
    │       ├── Environment/            # Pył, mgła, para i efekty atmosferyczne
    │       └── Interactions/           # Efekty używania urządzeń i obiektów
    │
    ├── Audio/                          # Muzyka, dźwięki i konfiguracja audio
    │   ├── Ambience/                   # Atmosfera kopalni, szumy, wiatr i pogłosy
    │   ├── Attenuation/                # Zasięg i przestrzenne rozchodzenie się dźwięku
    │   ├── MetaSounds/                 # Proceduralne i dynamiczne systemy audio
    │   ├── Mixes/                      # Miksowanie i kontrola głośności
    │   ├── Music/                      # Muzyka menu, eksploracji i zagrożenia
    │   ├── SFX/                        # Efekty dźwiękowe rozgrywki
    │   │   ├── Characters/             # Kroki, oddech, obrażenia i ruch postaci
    │   │   ├── Environment/            # Maszyny, skały, instalacje i otoczenie
    │   │   ├── Interactions/           # Drzwi, terminale, kryształy i przedmioty
    │   │   └── UI/                     # Kliknięcia, powiadomienia i dźwięki menu
    │   └── Voices/                     # Dialogi, transmisje radiowe i szepty
    │
    ├── Characters/                     # Postacie gracza i przeciwników
    │   ├── Enemies/                    # Wszystkie zasoby związane z przeciwnikami
    │   │   ├── Animations/             # Animacje i Animation Blueprints przeciwników
    │   │   ├── Base/                   # Bazowe klasy Blueprint przeciwników
    │   │   ├── Components/             # Zdrowie, reakcje i komponenty przeciwników
    │   │   ├── Meshes/                 # Modele 3D i szkielety przeciwników
    │   │   └── Types/                  # Konkretne typy i warianty przeciwników
    │   └── Player/                     # Zasoby postaci sterowanej przez gracza
    │       ├── Animations/             # Animacje chodzenia, sprintu i interakcji
    │       ├── Blueprints/             # BP_PlayerCharacter i logika postaci
    │       ├── Components/             # Komponenty specyficzne dla postaci gracza
    │       ├── Meshes/                 # Model gracza i widoczne wyposażenie
    │       └── Skeletons/              # Szkielety wykorzystywane przez animacje
    │
    ├── Core/                           # Podstawowa architektura i konfiguracja gry
    │   ├── FunctionLibraries/          # Wspólne funkcje pomocnicze Blueprint
    │   ├── GameInstances/              # Dane i stan przechowywany między mapami
    │   ├── GameModes/                  # Zasady rozgrywki i konfiguracja trybów gry
    │   ├── Input/                      # System sterowania Enhanced Input
    │   │   ├── Actions/                # Akcje: ruch, sprint, interakcja, ekwipunek
    │   │   └── MappingContexts/        # Przypisania klawiszy i kontrolera do akcji
    │   ├── PlayerControllers/          # Sterowanie graczem, kursorem i interfejsem
    │   └── Subsystems/                 # Menedżery usług gry, jeśli będą potrzebne
    │
    ├── Data/                           # Wspólne struktury danych i konfiguracje
    │   ├── Curves/                     # Krzywe balansu, progresji i intensywności efektów
    │   ├── DataAssets/                 # Konfiguracje przedmiotów, mechanik i przeciwników
    │   ├── DataTables/                 # Tabele statystyk, parametrów i wartości
    │   ├── Enums/                      # Enumy: typ przedmiotu, stan AI, rodzaj interakcji
    │   ├── PrimaryDataAssets/          # Dane obsługiwane przez Asset Manager
    │   └── Structs/                    # Struktury: przedmiot, zadanie, wpis ekwipunku
    │
    ├── Interaction/                    # System interakcji z otoczeniem
    │   ├── Components/                 # Wykrywanie obiektów i wykonywanie interakcji
    │   ├── Interfaces/                 # Interfejsy Blueprint, np. BPI_Interactable
    │   ├── Interactables/              # Obiekty reagujące na działania gracza
    │   │   ├── Containers/             # Skrzynie, szafki i pojemniki
    │   │   ├── Doors/                  # Drzwi, zamki i mechanizmy otwierania
    │   │   ├── Elevators/              # Winda, sterowanie i przemieszczanie między sekcjami
    │   │   └── Terminals/              # Komputery, panele i urządzenia elektroniczne
    │   ├── Pickups/                    # Podnoszone przedmioty i zasoby
    │   └── Puzzles/                    # Zagadki środowiskowe
    │       ├── Logic/                  # Warunki rozwiązania i postęp zagadek
    │       ├── Mechanics/              # Sekwencje, przełączniki i układy zasilania
    │       └── Objects/                # Interaktywne elementy zagadek
    │
    ├── Levels/                         # Produkcyjne mapy i poziomy gry
    │   ├── MainMenu/                   # Mapa tła menu głównego
    │   ├── Mine/                       # Poziomy związane z opuszczoną kopalnią
    │   │   ├── Chambers/               # Komory, sale i specjalne pomieszczenia
    │   │   ├── Corridors/              # Korytarze i połączenia między obszarami
    │   │   └── Entrance/               # Wejście do kopalni i początek eksploracji
    │   ├── Prototype/                  # Wczesne prototypy układu poziomów
    │   └── VerticalSlice/              # Dopracowany fragment gry do prezentacji
    │
    ├── Narrative/                      # Fabuła, narracja i historia świata
    │   ├── Dialogues/                  # Dialogi, komunikaty i transmisje
    │   ├── Documents/                  # Notatki, raporty i dzienniki znalezione w grze
    │   ├── EnvironmentalStorytelling/  # Elementy otoczenia przekazujące historię
    │   ├── Memories/                   # Wspomnienia wywoływane przez kryształy
    │   ├── Sequences/                  # Sekwencje filmowe i wydarzenia fabularne
    │   └── StoryEvents/                # Triggery i logika postępu fabuły
    │
    ├── Systems/                        # Główne mechaniki i systemy rozgrywki
    │   ├── Combat/                     # Walka, obrażenia, broń i opcjonalny laser
    │   ├── Corruption/                 # Poziom Corruption, progi i efekty skażenia
    │   ├── Crystals/                   # Kryształy, aktywacja i mechanika checkpointów
    │   ├── Health/                     # Punkty zdrowia, obrażenia i śmierć gracza
    │   ├── Inventory/                  # Ekwipunek i zarządzanie przedmiotami
    │   │   ├── Components/             # Komponent przechowujący ekwipunek
    │   │   ├── Items/                  # Definicje i logika przedmiotów
    │   │   └── Logic/                  # Dodawanie, usuwanie i używanie przedmiotów
    │   ├── Objectives/                 # Cele misji, warunki i postęp zadań
    │   ├── Resources/                  # Zarządzanie zasobami i materiałami
    │   └── SaveSystem/                 # Zapis i odczyt gry, SaveGame i checkpointy
    │
    ├── Testing/                        # Środowisko testowe i debugowanie
    │   ├── Debug/                      # Narzędzia diagnostyczne i wizualizacja stanu
    │   ├── Maps/                       # L_Test_General i mapy testujące konkretne systemy
    │   ├── TestActors/                 # Tymczasowe Blueprinty do sprawdzania mechanik
    │   └── TestData/                   # Konfiguracje i dane wykorzystywane w testach
    │
    ├── UI/                             # Interfejs użytkownika i widgety
    │   ├── Common/                     # Wspólne przyciski, style i elementy wielokrotnego użytku
    │   ├── Corruption/                 # Wskaźniki Corruption i komunikaty o skażeniu
    │   ├── HUD/                        # HUD, podpowiedzi interakcji i informacje ekranowe
    │   ├── Inventory/                  # Okno ekwipunku, sloty i opisy przedmiotów
    │   ├── MainMenu/                   # Menu główne, przyciski i nawigacja
    │   ├── Objectives/                 # Lista celów i powiadomienia o postępach
    │   ├── PauseMenu/                  # Menu pauzy i wznowienia gry
    │   └── Settings/                   # Ustawienia obrazu, audio i sterowania
    │
    └── World/                          # Logika i elementy środowiska gry
        ├── Environment/                # Elementy budujące otoczenie planety
        │   ├── Cave/                   # Jaskinie i naturalne formacje skalne
        │   ├── Mine/                   # Konstrukcje, tunele i instalacje kopalniane
        │   └── Surface/                # Powierzchnia planety i zewnętrzne obszary
        ├── Hazards/                    # Zagrożenia środowiskowe, pułapki i niebezpieczne strefy
        ├── Lighting/                   # Lampy, migające światła i sterowanie oświetleniem
        ├── Props/                      # Blueprinty przedmiotów i elementów otoczenia
        ├── SetDressing/                # Zestawy dekoracji i kompozycje środowiskowe
        └── WorldActors/                # Pozostałe aktory związane ze światem gry

        ```