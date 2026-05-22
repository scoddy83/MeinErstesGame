---
created: 2026-05-21T22:59
Last update: 2026-05-21T22:59
---
# Projektplan — Meine 2D Game Engine

> **Ziel:** Eigene 2D Spielengine von Grund auf in C++23 + Vulkan entwickeln.
> Lerntiefe hat Vorrang vor Geschwindigkeit. Kein fester Deadline-Druck.
>
> **Stack:** C++23 · Vulkan · GLFW · entt (ECS) · CMake + vcpkg · miniaudio · stb · Dear ImGui

---

## Überblick

| Phase | Inhalt | Geschätzter Zeitaufwand |
|---|---|---|
| **Phase 0** | Setup: CLion, GitHub, Ordnerstruktur, Tools | 1–2 Wochen |
| **Phase 1.1** | Modernes C++ auffrischen + CMake/vcpkg verstehen | 2–3 Wochen |
| **Phase 1.2** | GLFW Window + Vulkan Setup | 3–5 Wochen |
| **Phase 1.3** | Eigener 2D-Renderer | 4–6 Wochen |
| **Phase 1.4** | ECS mit entt | 2–3 Wochen |
| **Phase 1.5** | AssetManager (stb_image, Tiled) | 2–3 Wochen |
| **Phase 1.6** | Game Loop + miniaudio | 1–2 Wochen |
| **Phase 1.7** | ImGui Debug-UI | 1–2 Wochen |
| **Phase 1.8** | Erstes spielbares 2D-Spiel 🎉 | 2–4 Wochen |
| **Gesamt Phase 1** | | **~3–6 Monate** |

---

## Phase 0 — Entwicklungsumgebung Setup

> Ziel: Einmal sauber aufsetzen, danach nie mehr nachdenken müssen.

### 0.1 CLion einrichten

**Aufgaben:**
- [ ] CLion installieren (JetBrains Website oder Toolbox App)
- [ ] Plugin installieren: **CMake** (meist vorinstalliert)
- [ ] Plugin installieren: **Clangd** als Language Server aktivieren (bessere Code-Completion für C++)
- [ ] Clang-Tidy in den Settings aktivieren (`Editor > Inspections > C/C++`)
- [ ] Farbschema nach Wunsch anpassen (z. B. Darcula oder One Dark)
- [ ] Keymap prüfen — Standard reicht für den Anfang

**Wichtige CLion-Shortcuts zum Merken:**
| Shortcut | Funktion |
|---|---|
| `Shift + Shift` | Alles suchen (Dateien, Symbole, Aktionen) |
| `Ctrl + B` | Zur Definition springen |
| `Alt + Enter` | Quick Fix / Intent Actions |
| `Ctrl + Shift + B` | CMake Re-Build |
| `Ctrl + Alt + L` | Code formatieren |

**Ressourcen:**
- 🌐 [CLion Quickstart Guide](https://www.jetbrains.com/help/clion/clion-quick-start-guide.html)
- 🌐 [CLion + CMake Tutorial](https://www.jetbrains.com/help/clion/quick-cmake-tutorial.html)

**Erfolgskriterium:** CLion öffnet ein leeres CMake-Projekt ohne Fehler.

---

### 0.2 Vulkan SDK + Tools installieren

**Aufgaben:**
- [ ] **Vulkan SDK** von [vulkan.lunarg.com](https://vulkan.lunarg.com) herunterladen und installieren
  - Wichtig: *Validation Layers* und *SPIR-V Tools* bei der Installation mit auswählen
- [ ] Installation prüfen: `vulkaninfo` im Terminal ausführen
- [ ] **RenderDoc** von [renderdoc.org](https://renderdoc.org) herunterladen (wird später gebraucht)
- [ ] Umgebungsvariable `VULKAN_SDK` prüfen (wird automatisch gesetzt)

**Erfolgskriterium:** `vulkaninfo` gibt GPU-Informationen aus, kein Fehler.

---

### 0.3 vcpkg einrichten

**Aufgaben:**
- [ ] vcpkg klonen:
  ```bash
  git clone https://github.com/microsoft/vcpkg.git C:/dev/vcpkg
  cd C:/dev/vcpkg
  bootstrap-vcpkg.bat
  ```
- [ ] Umgebungsvariable `VCPKG_ROOT` setzen (z. B. `C:/dev/vcpkg`)
- [ ] vcpkg im `manifest mode` verwenden (empfohlen) — `vcpkg.json` pro Projekt

**Ressourcen:**
- 🌐 [vcpkg Getting Started](https://vcpkg.io/en/getting-started)
- 🌐 [vcpkg Manifest Mode Docs](https://vcpkg.io/en/docs/manifests.html)

**Erfolgskriterium:** `vcpkg version` gibt eine Versionsnummer aus.

---

### 0.4 GitHub Repository anlegen

**Aufgaben:**
- [ ] Neues **privates** Repository auf GitHub anlegen: z. B. `my-2d-engine`
- [ ] `.gitignore` für C++ und CMake generieren (via [gitignore.io](https://www.toptal.com/developers/gitignore) mit `C++`, `CMake`, `CLion`)
- [ ] Repository lokal klonen:
  ```bash
  git clone https://github.com/DEIN_USERNAME/my-2d-engine.git
  ```
- [ ] In CLion: Git-Integration prüfen (`VCS > Enable Version Control`)
- [ ] Ersten leeren Commit pushen

**Branch-Strategie (simpel für Soloprojekte):**
- `main` → stabiler Stand, läuft immer durch
- `dev` → aktuelle Entwicklung
- Feature-Branches bei größeren Änderungen: z. B. `feature/vulkan-renderer`

**Erfolgskriterium:** Leeres Projekt liegt auf GitHub, CLion erkennt das Git-Repository.

---

### 0.5 Ordnerstruktur anlegen

**Empfohlene Projektstruktur:**
```
my-2d-engine/
├── CMakeLists.txt          ← Root CMake
├── CMakePresets.json       ← Build-Presets (Debug/Release)
├── vcpkg.json              ← vcpkg Manifest (Dependencies)
├── .gitignore
├── README.md
│
├── engine/                 ← Engine-Code (Bibliothek)
│   ├── CMakeLists.txt
│   ├── src/
│   │   ├── core/           ← Window, Application, Game Loop
│   │   ├── renderer/       ← Vulkan Renderer
│   │   ├── ecs/            ← ECS Wrapper (entt)
│   │   ├── assets/         ← Asset Manager
│   │   └── audio/          ← miniaudio Integration
│   └── include/
│       └── engine/         ← Public Header Files
│
├── game/                   ← Erstes Spiel (nutzt Engine)
│   ├── CMakeLists.txt
│   └── src/
│       └── main.cpp
│
├── shaders/                ← GLSL Shader Files
│   ├── vert.glsl
│   └── frag.glsl
│
├── assets/                 ← Texturen, Sounds, Tilemaps
│   ├── textures/
│   ├── sounds/
│   └── maps/
│
├── third_party/            ← Manuelle Abhängigkeiten (falls nicht via vcpkg)
│
└── docs/                   ← Notizen, Diagramme, Entscheidungen
    └── indie-game-engine-stack.md
```

**Aufgaben:**
- [ ] Verzeichnisse anlegen (per Terminal oder CLion)
- [ ] Platzhalter-`CMakeLists.txt` in `engine/` und `game/` anlegen
- [ ] `README.md` mit Kurzbeschreibung anlegen
- [ ] Struktur committen und pushen

**Erfolgskriterium:** Struktur liegt auf GitHub, CLion öffnet das Projekt ohne Fehler.

---

### 0.6 Basis-CMake aufsetzen

**Aufgaben:**
- [ ] Root `CMakeLists.txt` erstellen:
  ```cmake
  cmake_minimum_required(VERSION 3.25)
  project(MyEngine VERSION 0.1.0 LANGUAGES CXX)
  
  set(CMAKE_CXX_STANDARD 23)
  set(CMAKE_CXX_STANDARD_REQUIRED ON)
  
  add_subdirectory(engine)
  add_subdirectory(game)
  ```
- [ ] `CMakePresets.json` für Debug und Release anlegen
- [ ] `vcpkg.json` mit ersten Dependencies anlegen (erstmal leer oder mit `glfw3`)
- [ ] Sicherstellen, dass CMake in CLion das Projekt konfiguriert (grüner Haken)

**Ressourcen:**
- 📺 [vector-of-bool — CMake Basics](https://www.youtube.com/playlist?list=PLK6MXr8gasrGmIiSuVQXpfFuE1uR19Hos)
- 🌐 [An Introduction to Modern CMake](https://cliutils.gitlab.io/modern-cmake/)

**Erfolgskriterium:** `cmake --build build` läuft durch, auch wenn noch kein Code existiert.

---

### ✅ Phase 0 Abschluss-Checkliste

- [ ] CLion läuft, Plugins konfiguriert
- [ ] Vulkan SDK installiert, `vulkaninfo` funktioniert
- [ ] vcpkg eingerichtet
- [ ] GitHub Repository existiert mit sauberer Ordnerstruktur
- [ ] CMake konfiguriert, Projekt baut ohne Fehler
- [ ] Erster Commit auf `main` gepusht

---

---

## Phase 1 — 2D Engine

---

### Phase 1.1 — Modernes C++ auffrischen

> **Warum zuerst?** C++23 mit RAII, Move Semantics und Templates ist der Baustein für alles Weitere. Besser eine Woche hier investieren, als später ständig stolpern.

**Lernziele:**
- RAII und Smart Pointer (`unique_ptr`, `shared_ptr`) verstehen
- Move Semantics (&&, `std::move`) anwenden können
- Range-based for, `auto`, `constexpr` sicher nutzen
- Grundlegendes Template-Verständnis

**Aufgaben:**
- [ ] *Effective Modern C++* — Scott Meyers: Kapitel 1–4 (Items 1–23) lesen
- [ ] Kleine Übungsprogramme schreiben: eigene RAII-Klasse, Move-Konstruktor implementieren
- [ ] cppreference.com als tägliches Nachschlagewerk einrichten

**Ressourcen:**
- 📖 *Effective Modern C++* — Scott Meyers (Pflichtlektüre!)
- 📺 [The Cherno — C++ Series](https://www.youtube.com/@TheCherno) — besonders: RAII, Smart Pointers, Move Semantics
- 🌐 [cppreference.com](https://en.cppreference.com) — immer griffbereit halten

**Erfolgskriterium:** Du kannst eine Klasse mit RAII, Rule of Five und `unique_ptr` schreiben, ohne nachzuschlagen.

---

### Phase 1.2 — GLFW Window + Vulkan Setup

> Das schwierigste Kapitel. Vulkan ist bewusst verbose — das ist der Punkt.
> Folge `vulkan-tutorial.com` Schritt für Schritt und versuche, **jede Zeile** zu verstehen.

**Lernziele:**
- GLFW-Fenster erstellen und Event-Loop verstehen
- Vulkan-Instanz, Physical Device, Logical Device aufsetzen
- Swapchain erstellen
- Erstes Dreieck auf dem Bildschirm rendern

**Aufgaben:**
- [ ] `glfw3` und `vulkan` via vcpkg hinzufügen (`vcpkg.json` updaten)
- [ ] [vulkan-tutorial.com](https://vulkan-tutorial.com) — Abschnitte: *Introduction* bis *Drawing a triangle* durcharbeiten
- [ ] GLFW-Fenster in `core/Window.h` / `core/Window.cpp` kapseln
- [ ] Vulkan Validation Layers von Anfang an aktivieren
- [ ] Erstes Dreieck rendert sich auf dem Bildschirm 🎉

**Ressourcen:**
- 🌐 [vulkan-tutorial.com](https://vulkan-tutorial.com) ← **Hauptressource**
- 📺 [Brendan Galea — Vulkan Game Engine Tutorials](https://www.youtube.com/playlist?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR)
- 🌐 [GLFW Docs](https://www.glfw.org/docs/latest/)

> 💡 **Mentor-Tipp:** Wenn etwas unklar ist, schau dir zuerst die Vulkan Validation Layer Messages an — die sind extrem hilfreich und genau dafür da.

**Erfolgskriterium:** Ein GLFW-Fenster öffnet sich, ein buntes Dreieck wird via Vulkan gerendert.

---

### Phase 1.3 — Eigener 2D-Renderer

> Jetzt wird der Vulkan-Code in eine saubere Engine-Architektur überführt.

**Lernziele:**
- Vertex Buffer, Index Buffer, Uniform Buffers verstehen
- Texturen laden und samplen (mit `stb_image`)
- 2D Sprites und Batching implementieren
- Kamera-System (orthografisch) aufbauen

**Aufgaben:**
- [ ] `stb_image` via vcpkg oder direkt einbinden
- [ ] `Renderer`-Klasse in `renderer/` anlegen
- [ ] Textur-Support implementieren: PNG laden, Vulkan Descriptor Set binden
- [ ] Sprite-Rendering: Quad mit Textur zeichnen
- [ ] Orthografische Kamera implementieren
- [ ] Basis-Batching: mehrere Sprites in einem Draw Call

**Ressourcen:**
- 📺 [Brendan Galea — Vulkan Game Engine Tutorials](https://www.youtube.com/playlist?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR) ← **Hauptressource**
- 🌐 [github.com/nothings/stb](https://github.com/nothings/stb) — stb_image Dokumentation
- 🌐 [github.com/SaschaWillems/Vulkan](https://github.com/SaschaWillems/Vulkan) — Beispiele für Texturen, Descriptor Sets

**Erfolgskriterium:** Ein texturierter Sprite wird auf dem Bildschirm gerendert, Kamera lässt sich bewegen.

---

### Phase 1.4 — ECS mit entt

> Das Herzstück der Engine-Architektur.

**Lernziele:**
- ECS-Konzept (Entity, Component, System) verstehen
- `entt` Registry, Views und Groups anwenden
- Bestehende Renderer-Logik ins ECS-Paradigma überführen

**Aufgaben:**
- [ ] `entt` via vcpkg hinzufügen
- [ ] Basis-Komponenten anlegen: `Transform`, `Sprite`, `Velocity`
- [ ] `RenderSystem`: iteriert über alle Entities mit `Sprite` + `Transform` und rendert sie
- [ ] `MovementSystem`: aktualisiert Positionen anhand `Velocity`
- [ ] Entity-Erstellung und -Zerstörung im Spiel-Code testen
- [ ] Bereits an spätere Komponenten denken (noch nicht bauen): `PhysicsBody`, `PlayerController`, `Collider` — das Spiel wird ein Jump & Run, diese kommen in Phase 1.8

**Ressourcen:**
- 📺 [The Cherno — Intro to EnTT ECS](https://www.youtube.com/watch?v=D4hz0wEB978)
- 🌐 [github.com/skypjack/entt](https://github.com/skypjack/entt)
- 🌐 [EnTT in Action (Wiki)](https://github.com/skypjack/entt/wiki/EnTT-in-Action)

> 💡 **Mentor-Tipp:** Für das Hintergrundwissen lohnt sich der Artikel von [Austin Morlan — ECS in C++](https://austinmorlan.com/posts/entity_component_system/), der zeigt wie man ein ECS von Null baut. Das hilft enorm beim Verständnis, warum entt so designt ist.

**Erfolgskriterium:** 50+ Entities mit `Transform` + `Sprite` werden ohne Leistungsprobleme gerendert.

---

### Phase 1.5 — AssetManager (Texturen + Tilemaps)

**Lernziele:**
- Zentralen AssetManager mit Caching aufbauen
- Tilemaps aus dem Tiled Editor laden und rendern

**Aufgaben:**
- [ ] `AssetManager`-Klasse in `assets/` anlegen
  - Texturen per Handle verwalten (kein doppeltes Laden)
  - Texturen laden, cachen, freigeben
- [ ] `nlohmann/json` via vcpkg einbinden
- [ ] Tiled Editor installieren und eine Test-Map erstellen (JSON-Export)
- [ ] `TilemapLoader`: JSON-Datei parsen, Tilemap rendern
- [ ] Tilemap-Entities ins ECS einfügen

**Ressourcen:**
- 🌐 [Tiled Editor](https://www.mapeditor.org)
- 🌐 [github.com/nlohmann/json](https://github.com/nlohmann/json)
- 🌐 [github.com/nothings/stb](https://github.com/nothings/stb)

**Erfolgskriterium:** Eine Tiled-Map wird aus einer JSON-Datei geladen und korrekt gerendert.

---

### Phase 1.6 — Game Loop + miniaudio

**Lernziele:**
- Sauberen, zeitbasierten Game Loop implementieren (Fixed Timestep)
- Sounds und Musik abspielen

**Aufgaben:**
- [ ] Game Loop in `core/Application.cpp` überarbeiten: Fixed Timestep + variable Render Rate
- [ ] `miniaudio.h` als single-header einbinden (direkt von GitHub)
- [ ] `AudioManager`-Klasse in `audio/` anlegen
- [ ] Sound-Effekt laden und bei Event abspielen
- [ ] Hintergrundmusik in Loop abspielen

**Ressourcen:**
- 🌐 [miniaud.io — Dokumentation](https://miniaud.io)
- 🌐 [github.com/mackron/miniaudio](https://github.com/mackron/miniaudio)
- 🌐 [Fix Your Timestep!](https://gafferongames.com/post/fix_your_timestep/) — Klassischer Artikel über Game Loop Design

**Erfolgskriterium:** Game Loop läuft stabil mit Fixed Timestep, ein Sound-Effekt und Hintergrundmusik spielen ab.

---

### Phase 1.7 — ImGui Debug-UI

**Lernziele:**
- Dear ImGui mit Vulkan-Backend einbinden
- Entity Inspector und Performance-Anzeige bauen

**Aufgaben:**
- [ ] `imgui` via vcpkg einbinden (mit `imgui[vulkan-binding]` Feature)
- [ ] ImGui in den bestehenden Vulkan Render Pass integrieren
- [ ] Debug-Overlay bauen:
  - [ ] FPS-Counter
  - [ ] Entity-Inspector: ausgewählte Entity mit allen Komponenten anzeigen
  - [ ] einfache Asset-Statistik (geladene Texturen, Sounds)
- [ ] Debug-UI mit einer Taste (z. B. `F1`) ein-/ausblenden

**Ressourcen:**
- 🌐 [github.com/ocornut/imgui](https://github.com/ocornut/imgui) — besonders `backends/imgui_impl_vulkan.cpp`
- 📺 [The Cherno — ImGui in C++ Tutorial](https://www.youtube.com/watch?v=nVaQuNXueFw)

**Erfolgskriterium:** ImGui-Overlay zeigt FPS und Entity-Daten live an.

---

### Phase 1.8 — Erstes spielbares 2D-Spiel: Jump & Run 🎉

> Jetzt kommt alles zusammen. Das Ziel ist kein perfektes Spiel — ein **fertiges** Spiel.
> Kein Metroidvania, kein Mega Man. Denk: 3–5 Levels, ein Spieler, Springen, Fallen, Ziel erreichen.

**Spielkonzept (Vorschlag):**
- Spieler läuft nach rechts, springt über Hindernisse und Lücken
- Tilemap-basierte Level (erstellt im Tiled Editor)
- Münzen oder Sterne einsammeln (Score)
- Fallen in Lücken = Game Over
- Level-Abschluss beim Erreichen einer Flagge / eines Portals

---

**Aufgaben — Input & Bewegung:**
- [ ] Input-System in `core/Input.h` sauber kapseln (Keyboard, später Gamepad vorbereiten)
- [ ] `PlayerController`-Komponente anlegen
- [ ] Horizontale Bewegung mit Beschleunigung und Reibung (fühlt sich besser an als direkte Geschwindigkeit)
- [ ] Sprung implementieren: Sprung-Kraft + Gravitation als konstante Beschleunigung

**Aufgaben — Physik:**
- [ ] `PhysicsBody`-Komponente: Velocity, Acceleration, `isGrounded`-Flag
- [ ] `PhysicsSystem`: Gravitation pro Frame anwenden, Velocity integrieren
- [ ] Coyote Time implementieren (Spieler kann noch kurz nach der Plattformkante springen) — kleines Detail, großer Spielgefühl-Unterschied
- [ ] Jump Buffering implementieren (Sprung-Input kurz vor dem Landen wird gepuffert)

**Aufgaben — Kollision:**
- [ ] AABB-Kollision zwischen Spieler und Tilemap-Tiles
- [ ] Kollisionsantwort: Spieler wird korrekt auf Tiles gestoppt (oben, unten, links, rechts getrennt behandeln)
- [ ] `isGrounded` korrekt setzen (nur wenn von oben auf Tile getroffen)
- [ ] Kollision mit Münzen / Collectibles (Trigger, kein Stopp)
- [ ] Kollision mit Gegnern oder Fallen → Game Over

**Aufgaben — Level & Kamera:**
- [ ] 2–3 Test-Level im Tiled Editor erstellen (Tileset, Kollisions-Layer, Objekt-Layer für Spawnpoints)
- [ ] Scrolling-Kamera: folgt dem Spieler horizontal, bleibt innerhalb des Level-Bounds
- [ ] Level-Wechsel: Nächstes Level laden wenn Ziel erreicht

**Aufgaben — Spielzustände:**
- [ ] `GameStateManager` oder einfacher Enum: `MainMenu`, `Playing`, `GameOver`, `LevelComplete`
- [ ] Einfaches Hauptmenü (Start-Button via ImGui oder gerenderte Sprites)
- [ ] Game Over Screen mit Restart-Option
- [ ] Score-Anzeige (gesammelte Münzen)

**Aufgaben — Audio & Polish:**
- [ ] Sprung-Sound
- [ ] Münze-einsammeln Sound
- [ ] Game Over Sound
- [ ] Hintergrundmusik in Loop
- [ ] Einfache Partikel oder Sprite-Animation beim Einsammeln (optional, aber motivierend)

**Aufgaben — Release:**
- [ ] Release-Build konfigurieren (CMake Release Preset)
- [ ] Assets-Pfade relativ zum Executable machen (nicht hardcoded)
- [ ] Spiel startet als `.exe` ohne CLion

---

> 💡 **Mentor-Tipp — Coyote Time & Jump Buffering:** Diese zwei kleinen Features machen den größten Unterschied zwischen einem Spiel das sich *träge* anfühlt und einem das sich *gut* anfühlt. Baue sie von Anfang an ein, nicht als Nachbesserung.
>
> 💡 **Tilemap-Tipp:** Im Tiled Editor einen separaten Layer für Kollisions-Tiles anlegen (z. B. `collision`) und einen für Dekoration (`background`). So kann der Renderer dekorative Tiles zeichnen, ohne dass die Physik sie als Hindernis behandelt.

**Erfolgskriterium:** Du kannst das Spiel einer anderen Person in die Hand geben. Sie versteht ohne Erklärung was zu tun ist, spielt 2–3 Minuten, kommt zum Game Over oder Level-Abschluss.

---

## Allgemeine Lernhinweise

**Was tue, wenn du feststeckst:**
1. Vulkan Validation Layer Messages lesen — sie sagen dir meist genau, was falsch ist
2. [gamedev.stackexchange.com](https://gamedev.stackexchange.com) und [reddit.com/r/vulkan](https://www.reddit.com/r/vulkan/) fragen
3. Den relevanten Abschnitt in vulkan-tutorial.com nochmal lesen
4. RenderDoc aufmachen und den Frame analysieren

**Commit-Disziplin:**
- Nach jedem abgeschlossenen Meilenstein committen
- Commit-Messages klar halten: `feat: add texture loading via stb_image`
- Lieber zu oft als zu selten pushen

**Bücher parallel lesen:**
- *Game Engine Architecture* — Jason Gregory: Kapitel zur aktuellen Phase lesen
- *Effective Modern C++* — bei neuen C++ Patterns nachschlagen

---

## Ressourcen-Übersicht

| Thema | Ressource |
|---|---|
| C++23 Referenz | [cppreference.com](https://en.cppreference.com) |
| Vulkan Tutorial | [vulkan-tutorial.com](https://vulkan-tutorial.com) |
| Vulkan Engine YouTube | [Brendan Galea Playlist](https://www.youtube.com/playlist?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR) |
| Vollständige Engine YouTube | [Travis Vroman — Kohi Engine](https://www.youtube.com/playlist?list=PLv8Ddw9K0JPg1BEO-RS-0MYs423cvLamx) |
| C++ YouTube | [The Cherno](https://www.youtube.com/@TheCherno) |
| ECS (entt) | [github.com/skypjack/entt](https://github.com/skypjack/entt) |
| CMake | [An Introduction to Modern CMake](https://cliutils.gitlab.io/modern-cmake/) |
| vcpkg | [vcpkg.io](https://vcpkg.io) |
| Community Q&A | [gamedev.stackexchange.com](https://gamedev.stackexchange.com) |
| Vulkan Community | [reddit.com/r/vulkan](https://www.reddit.com/r/vulkan/) |
