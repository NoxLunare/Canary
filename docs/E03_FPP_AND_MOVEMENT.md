# Crystalborne | E03: FPP i movement

> Dokumentacja techniczna epika E03  
> Projekt: **Crystalborne** | Silnik: **Unreal Engine 5.8** | Platforma: **Windows**  
> Zakres backlogu: **CRY-0019 do CRY-0042** | Priorytet: **MUST**  
> Status na dzień **15.11.2026**: **20 Done / 4 Blocked**  
> Technologia: **Unreal Engine Blueprints, bez własnego kodu C++**


## 1. Architektura rozwiązania

### 1.1. Istotne zasoby i lokalizacje

Zgodnie z `PROJECT_STRUCTURE.md` następujące foldery są przeznaczone na zasoby systemu. To **miejsca docelowe w strukturze.**

| Katalog | Zawartość / rola |
| --- | --- |
| `Content/Crystalborne/Characters/Player/Blueprints/` | Postać `BP_FirstPersonCharacter` i logika poruszania |
| `Content/Crystalborne/Characters/Player/Components/` | Komponenty specyficzne dla postaci, jeśli zostaną wydzielone |
| `Content/Crystalborne/Characters/Player/Animations/` | Animacje ruchu postaci |
| `Content/Crystalborne/Core/Input/Actions/` | `IA_Move`, `IA_Look`, `IA_MouseLook`, `IA_Jump`, `IA_Sprint`, `IA_Crouch` |
| `Content/Crystalborne/Core/Input/MappingContexts/` | `IMC_Crystalborne` |
| `Content/Crystalborne/Data/Enums/` | `E_PlayerMovementState` |
| `Content/Crystalborne/Audio/SFX/Characters/` | Przyszłe dźwięki kroków zależne od stanu |
| `Content/Crystalborne/Testing/Maps/` | Mapy do testów ruchu i kolizji |
| `Content/Crystalborne/Testing/Debug/` | Pomocnicza diagnostyka stanów, prędkości i hałasu |

**Ustalony kontekst:** projekt bazuje na szablonie First Person. Inputy pierwotne to `IA_Jump`, `IA_Look`, `IA_MouseLook`, `IA_Move`. W pracach dodano `IA_Sprint`, `IA_Crouch` i kontekst `IMC_Crystalborne`

### 1.2. Przepływ sterowania

```text
Enhanced Input (IMC_Crystalborne)
    |
    +--> IA_Move ------------------> Add Movement Input
    +--> IA_Look / IA_MouseLook ---> obrót kontrolera / kamery
    +--> IA_Jump ------------------> Jump / Stop Jumping
    +--> IA_Sprint ----------------> aktualizacja żądania sprintu
    +--> IA_Crouch ----------------> Crouch / Un Crouch
                                          |
                                          v
                                  BP_FirstPersonCharacter
                                          |
                      +-------------------+------------------+
                      |                   |                  |
                  CharacterMovement   CapsuleComponent   CameraBobPivot
                      |                   |                  |
                      v                   v                  v
                prędkość ruchu      wysokość postaci    camera bob
                      |
                      v
               E_PlayerMovementState
                      |
                      +--> aktualna prędkość
                      +--> NoiseLevel (logika / dane dla AI)
                      +--> parametry Camera Bob
```

Schemat przedstawia logiczne zależności systemów. Nie jest dosłownym eksportem grafu Blueprint.

## 2. Zasady stanu ruchu i powiązania

Przy implementacji enuma i konfiguracji ruchu przyjęto następującą hierarchię logiczną:

1. Jeżeli gracz faktycznie kuca, priorytet ma `Crouching`, nawet przy próbie sprintu.
2. Jeżeli gracz stoi, porusza się i sprint jest aktywny oraz dozwolony, stanem jest `Sprinting`.
3. Jeżeli gracz stoi i porusza się bez sprintu, stanem jest `Walking`.
4. W przeciwnym razie stanem jest `Idle`.

**Uwaga implementacyjna:** jest to opis ustalonych reguł, nie wierny zapis konkretnego grafu. `Idle` nie musi oznaczać ustawienia `Max Walk Speed` na zero. Stan w powietrzu nie ma osobnej pozycji w opisanym enumie; ruch skoku jest obsługiwany przez `CharacterMovement` i powinien zostać uwzględniony przy efektach zależnych od kontaktu z podłożem.

### 2.1. Wspólna tabela stanów

| Stan | Prędkość w ustaleniach | `NoiseLevel` | Amplituda bob | Tempo bob |
| --- | ---: | ---: | ---: | ---: |
| `Idle` | 0 cm/s (ruch rzeczywisty) | 0.0 | 0 cm | 0 |
| `Walking` | 250 cm/s | 0.4 | 1.2 cm | 8 |
| `Sprinting` | 450 cm/s | 1.0 | 2.2 cm | 12 |
| `Crouching` | 120 cm/s | 0.1 | 0.5 cm | 5 |

To zestawienie łączy **wartości omawiane przy realizacji E03**. Wartości należy porównać z aktualnymi assetami przed wykorzystaniem dokumentu jako źródła do balansu.

## 3. Testy akceptacyjne

| Test | Scenariusz | Oczekiwany rezultat | Stan wg backlogu |
| --- | --- | --- | --- |
| T-E03-01 | Uruchomienie jako postać FPP | Kamera i sterowanie działają | Done (CRY-0019–0022) |
| T-E03-02 | Ruszanie i hamowanie | Płynne przyspieszenie i zatrzymanie | Done (CRY-0023–0024) |
| T-E03-03 | Sprint na otwartej przestrzeni | Właściwa prędkość i stan | Done (CRY-0025) |
| T-E03-04 | Wejście w kucanie i ruch | Zmiana wysokości kapsuły oraz ograniczona prędkość | Done (CRY-0026–0027) |
| T-E03-05 | Próba wstania pod stropem | Kapsuła i kamera pozostają w stanie kucania | Done (CRY-0028–0030, 0040) |
| T-E03-06 | Jednokrotny i wielokrotny skok | Skok z podłoża; brak air-jump | Done (CRY-0031–0032) |
| T-E03-07 | Zmiana stanu w ruchu | Prędkość i NoiseLevel odzwierciedlają stan | Done (CRY-0033–0035) |
| T-E03-08 | Ruch i zatrzymanie z camera bob | Kołysanie zależne od ruchu, wygaszane po zatrzymaniu | Done (CRY-0037) |
| T-E03-09 | Sprint podczas kucania | Brak przejścia do Sprinting | Done (CRY-0041) |
| T-E03-10 | Dźwięki kroków | Kroki zależne od ruchu, cisza w idle | Blocked (CRY-0036) |
| T-E03-11 | Kolizja z drzwiami | Brak przenikania i zakleszczania | Blocked (CRY-0038) |
| T-E03-12 | Ruch na schodach | Stabilny ruch i zachowanie kamery | Blocked (CRY-0039) |
| T-E03-13 | Interakcja i zmiana postawy | Brak sprzecznych stanów i błędów interakcji | Blocked (CRY-0042) |

Tabela opisuje zakres sprawdzenia na podstawie backlogu i ustalonych reguł. Nie stanowi raportu z automatycznych testów ani potwierdzenia każdej opisanej obserwacji.

## 4. Zadania zablokowane i dalsze kroki

| ID | Przedmiot | Co jest potrzebne do zamknięcia |
| --- | --- | --- |
| **CRY-0036** | Audio kroków | Wybór zasobów i sposobu wyzwalania dźwięku; test różnych stanów ruchu |
| **CRY-0038** | Kapsuła i drzwi | Docelowy obiekt drzwi oraz test wariantów kolizji |
| **CRY-0039** | Schody | Gotowy moduł schodów / scena testowa i test parametrów ruchu |
| **CRY-0042** | Postawa i interakcja | Uzgodnienie reguł z E04 i test integracyjny |

**Ważne:** podane wymagania opisują naturalne następne kroki. Backlog nie wskazuje jednoznacznych przyczyn statusu Blocked, dlatego nie należy traktować ich jako potwierdzonych powodów blokad.

## 5. Zależności między epikami

- **E02, Repozytorium i infrastruktura:** bazowy projekt, struktura katalogów, mapy testowe, Enhanced Input, GameMode i PlayerController.
- **E04, Interakcje i trace:** wpływ wysokości kamery, postawy i ruchu na wybór celu oraz wykonywanie interakcji; szczególnie CRY-0042.
- **AI i percepcja:** `NoiseLevel` może być źródłem dla przyszłych bodźców słuchowych, ale sama zmienna nie wdraża detekcji przeciwników.
- **Audio:** CRY-0036 jest naturalnym punktem integracji ruchu z efektami kroków.
- **Level design:** testy niskich przejść, drzwi, schodów oraz kolizji środowiska muszą być powtarzane na mapach produkcyjnych.
