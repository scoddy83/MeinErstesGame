---
created: 2026-05-21T23:37
Last update: 2026-05-21T23:37
tags:
  - Phase-1-8
  - Game
  - Release
---
# ISSUE-021 — Release-Build + .exe Distribution

## ID

ISSUE-021

## Typ

🙋 HITL — erfordert deinen aktiven Einsatz (Release-Build testen, auf fremdem PC verifizieren)

## Phase

Phase 1.8 — Jump & Run Spiel

## What to build

Das Spiel als standalone `.exe` die ohne CLion, ohne installierten Compiler und ohne Entwicklungsumgebung startet. Asset-Pfade sind relativ zum Executable. Vulkan Validation Layers sind im Release-Build deaktiviert. Das fertige ZIP-Archiv kann einer anderen Person gegeben werden — sie öffnet es, startet die `.exe`, und spielt.

## Acceptance criteria

- [ ] CMake Release Preset baut das Projekt ohne Debug-Symbole und mit Optimierungen
- [ ] Vulkan Validation Layers sind im Release-Build deaktiviert
- [ ] Alle Asset-Pfade sind relativ zum Executable (`./assets/...` statt absoluten Pfaden)
- [ ] Spiel startet auf einem Rechner ohne CLion / ohne MSVC Developer Console
- [ ] Alle nötigen DLLs sind im Release-Ordner enthalten
- [ ] Das Spiel kann einer anderen Person übergeben werden und läuft ohne Erklärung

## Blocked by

ISSUE-020 — Spiel-Audio: Sprung, Münze, Game Over

## Tasks

- [ ] **CMake Release Preset konfigurieren**
  - [ ] `CMakePresets.json`: Release-Preset mit `CMAKE_BUILD_TYPE=Release`, `-O2` oder `-O3`
  - [ ] Debug-Makros (`NDEBUG`) setzen damit Validation Layers nicht aktiv sind
  - [ ] Release-Build durchführen: `cmake --build build --config Release`

- [ ] **Asset-Pfade relativ machen**
  - [ ] Alle hardcodierten absoluten Pfade durch relative ersetzen: `"assets/textures/player.png"` statt `"C:/dev/..."`)
  - [ ] Executable-Pfad als Basis nutzen: `std::filesystem::current_path()` oder `argv[0]`-basiert

- [ ] **Validation Layers im Release deaktivieren**
  - [ ] `#ifdef NDEBUG` um Validation Layer Setup
  - [ ] Release-Build hat keine Validation-Layer-Overhead

- [ ] **DLL-Dependencies einsammeln**
  - [ ] Benötigte DLLs identifizieren (GLFW, Vulkan Runtime, ggf. MSVC Runtime)
  - [ ] DLLs in Release-Ordner kopieren
  - [ ] Optional: `windeployqt`-ähnliches Tool oder manuell via `dumpbin /dependents`

- [ ] **Finales Testing**
  - [ ] Release-Ordner auf einen anderen PC oder eine VM kopieren
  - [ ] `.exe` starten ohne Entwicklungsumgebung — Spiel läuft
  - [ ] Alle 3 Level spielbar, Audio funktioniert, kein Crash
  - [ ] Spiel an eine andere Person zum Testen geben 🎉

- [ ] **ZIP-Archiv erstellen**
  - [ ] `my-2d-engine-v1.0.zip` mit `.exe`, DLLs, `assets/`-Ordner
  - [ ] README mit Steuerung beilegen: "Arrow Keys / WASD: Bewegen, Space: Springen"

## Ressourcen

| Thema | Link |
|---|---|
| CMake Install | [cmake.org/cmake/help/latest/command/install.html](https://cmake.org/cmake/help/latest/command/install.html) |
| DLL-Abhängigkeiten | `dumpbin /dependents MyGame.exe` im Developer Command Prompt |
| std::filesystem | [cppreference.com/w/cpp/filesystem](https://en.cppreference.com/w/cpp/filesystem) |

## Mentor-Hinweis

> Das ist der Moment wo das Projekt vom "Lernprojekt" zum "fertigen Spiel" wird. Gib es tatsächlich an jemanden — ein Familienmitglied, einen Freund — und schau ihnen beim Spielen zu ohne etwas zu erklären. Was sie nicht verstehen oder wo sie hängen bleiben, das ist dein Feedback für das nächste Spiel. Herzlichen Glückwunsch — Phase 1 ist abgeschlossen! 🎉
