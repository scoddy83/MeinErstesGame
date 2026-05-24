---
created: 2026-05-21T23:37
Last update: 2026-05-21T23:37
tags:
  - Setup
  - Phase-0
---
# ISSUE-001 — Entwicklungsumgebung aufsetzen

## ID

ISSUE-001

## Typ

🙋 HITL — erfordert deinen aktiven Einsatz (Installation, Konfiguration, Entscheidungen)

## Phase

Phase 0 — Setup

## What to build

Eine vollständig eingerichtete, fehlerfreie Entwicklungsumgebung für das Projekt. Das Ergebnis ist kein Code, sondern das Fundament, auf dem alle weiteren Phasen aufbauen: IDE, Grafik-API, Paketverwaltung, Versionskontrolle und Build-System sind eingerichtet und verifiziert.

Konkret: CLion mit C++-Toolchain, Vulkan SDK mit aktiven Validation Layers, vcpkg im Manifest-Modus, ein GitHub-Repository mit sauberer Ordnerstruktur und ein funktionsfähiges CMake-Projekt, das in Debug und Release baut — ohne Fehler.

## Acceptance criteria

- [x] CLion öffnet ein leeres CMake-Projekt ohne Fehler oder rote Markierungen
- [x] `vulkaninfo` gibt GPU-Informationen aus (kein Fehler, kein "Vulkan not found")
- [x] `vcpkg version` gibt eine Versionsnummer aus
- [x] GitHub-Repository existiert mit Branch-Strategie (`main` / `dev`)
- [x] Ordnerstruktur (`engine/`, `game/`, `shaders/`, `assets/`, `third_party/`, `docs/`) liegt auf GitHub
- [x] `cmake --build build` läuft durch (auch ohne eigentlichen Spielcode)
- [x] CMake Presets für Debug und Release sind konfiguriert
- [x] Erster Commit ist auf `main` gepusht

## Blocked by

Nichts — kann sofort gestartet werden.

## Tasks

- [x] **CLion einrichten**
  - [x] CLion installieren (JetBrains Toolbox App empfohlen)
  - [x] Clangd als Language Server aktivieren (`Settings > Languages & Frameworks > C/C++ > Clangd`)
  - [x] Clang-Tidy aktivieren (`Settings > Editor > Inspections > C/C++`)
  - [x] Shortcut-Cheat-Sheet anlegen: `Shift+Shift`, `Ctrl+B`, `Alt+Enter`, `Ctrl+Alt+L`

- [x] **Vulkan SDK installieren**
  - [x] SDK von [vulkan.lunarg.com](https://vulkan.lunarg.com) herunterladen
  - [x] Bei Installation: *Validation Layers* und *SPIR-V Tools* auswählen
  - [x] `vulkaninfo` im Terminal ausführen und Output prüfen
  - [x] Umgebungsvariable `VULKAN_SDK` ist gesetzt (wird automatisch gesetzt, trotzdem prüfen)
  - [x] RenderDoc von [renderdoc.org](https://renderdoc.org) herunterladen (wird in Phase 1.2 gebraucht)

- [x] **vcpkg einrichten**
  - [x] vcpkg nach `C:/dev/vcpkg` klonen: `git clone https://github.com/microsoft/vcpkg.git C:/dev/vcpkg`
  - [x] Bootstrap ausführen: `cd C:/dev/vcpkg && bootstrap-vcpkg.bat`
  - [x] Umgebungsvariable `VCPKG_ROOT` auf `C:/dev/vcpkg` setzen
  - [x] `vcpkg version` gibt Versionsnummer aus

- [x] **GitHub Repository anlegen**
  - [x] Neues privates Repository anlegen: z. B. `my-2d-engine`
  - [x] `.gitignore` für C++, CMake, CLion generieren (via [gitignore.io](https://www.toptal.com/developers/gitignore))
  - [x] Repository lokal klonen
  - [x] Branch `dev` anlegen (`git checkout -b dev`)
  - [x] Git-Integration in CLion prüfen (`VCS > Enable Version Control`)

- [x] **Ordnerstruktur anlegen**
  - [x] Verzeichnisse anlegen: `engine/src/core/`, `engine/src/renderer/`, `engine/src/ecs/`, `engine/src/assets/`, `engine/src/audio/`, `engine/include/engine/`, `game/src/`, `shaders/`, `assets/textures/`, `assets/sounds/`, `assets/maps/`, `third_party/`, `docs/`
  - [x] Platzhalter `CMakeLists.txt` in `engine/` und `game/` anlegen (können leer sein)
  - [x] `README.md` mit Kurzbeschreibung anlegen

- [x] **Basis-CMake aufsetzen**
  - [x] Root `CMakeLists.txt` mit C++23-Standard, `add_subdirectory(engine)`, `add_subdirectory(game)` anlegen
  - [x] `CMakePresets.json` mit Debug- und Release-Preset erstellen
  - [x] Leere `vcpkg.json` anlegen (Dependencies kommen in späteren Phasen)
  - [x] CLion: CMake neu laden — grüner Haken muss erscheinen

- [x] **Alles committen und pushen**
  - [x] `git add .` + `git commit -m "chore: initial project setup"` auf `dev`
  - [x] `git push origin dev`
  - [x] Merge zu `main`: `git checkout main && git merge dev && git push origin main`

## Ressourcen

| Thema | Link |
|---|---|
| CLion Quickstart | [jetbrains.com/help/clion/clion-quick-start-guide.html](https://www.jetbrains.com/help/clion/clion-quick-start-guide.html) |
| CLion + CMake Tutorial | [jetbrains.com/help/clion/quick-cmake-tutorial.html](https://www.jetbrains.com/help/clion/quick-cmake-tutorial.html) |
| vcpkg Getting Started | [vcpkg.io/en/getting-started](https://vcpkg.io/en/getting-started) |
| vcpkg Manifest Mode | [vcpkg.io/en/docs/manifests.html](https://vcpkg.io/en/docs/manifests.html) |
| Modern CMake | [cliutils.gitlab.io/modern-cmake/](https://cliutils.gitlab.io/modern-cmake/) |
| CMake Basics (YouTube) | [vector-of-bool CMake Playlist](https://www.youtube.com/playlist?list=PLK6MXr8gasrGmIiSuVQXpfFuE1uR19Hos) |
| .gitignore Generator | [toptal.com/developers/gitignore](https://www.toptal.com/developers/gitignore) |

## Mentor-Hinweis

> Diese Phase fühlt sich nicht nach "echtem" Programmieren an — und das ist völlig normal. Ein sauberes Setup jetzt spart dir Stunden an Debugging-Zeit in späteren Phasen. Besonders wichtig: Vulkan Validation Layers **von Anfang an aktiv** lassen. Sie sind dein wichtigstes Debugging-Werkzeug für alles, was danach kommt.
