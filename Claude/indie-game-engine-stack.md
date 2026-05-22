---
created: 2026-05-21T23:00
Last update: 2026-05-21T23:00
---
# Indie Game Engine Stack — Entscheidungszusammenfassung

## Kontext

Ziel ist die Entwicklung einer eigenen Spielengine von Grund auf (Renderer, Game Loop, ECS), beginnend mit 2D-Indie-Spielen, später erweiterbar auf 3D. Tiefe und technisches Verständnis haben Vorrang vor schnellen Ergebnissen.

---

## Stack-Übersicht

| Bereich | Entscheidung |
|---|---|
| Sprache | C++23 |
| Grafik-API | Vulkan |
| Window / Input | GLFW |
| Engine-Architektur | ECS (entt) |
| Build-System | CMake + vcpkg |
| Audio Phase 1 | miniaudio |
| Audio Phase 2+ | FMOD |
| Texturen | stb_image |
| Tilemaps | Tiled + nlohmann/json |
| Fonts | stb_truetype |
| Debug-UI | Dear ImGui (Vulkan-Backend) |
| GPU-Debugging | RenderDoc + Vulkan Validation Layers |
| Profiling (Phase 2) | Tracy Profiler |

---

## Schritt 1 — Sprache: C++23

**Entscheidung:** C++23 gegenüber Rust bevorzugt, da das Rust-Ökosystem für Spieleentwicklung noch unreif ist und der Borrow-Checker bei Engine-typischen Patterns (ECS, Shared State) erheblichen Widerstand leistet.

### Quellen
- 📖 **Buch:** *Effective Modern C++* — Scott Meyers (O'Reilly) — Moderne C++-Idiome, RAII, Move Semantics
- 📖 **Buch:** *C++ Concurrency in Action* — Anthony Williams — Wichtig für Game Loop / Threading
- 🌐 **Web:** [isocpp.org](https://isocpp.org) — Offizielle C++ Core Guidelines
- 🌐 **Web:** [cppreference.com](https://en.cppreference.com) — Sprachreferenz C++23
- 📺 **YouTube:** [The Cherno — C++ Series](https://www.youtube.com/@TheCherno) — Umfassende C++-Tutorials von einem Ex-EA-Entwickler

---

## Schritt 2 — Grafik-API: Vulkan

**Entscheidung:** Vulkan statt OpenGL oder DirectX. Plattformübergreifend (Windows/Linux/macOS via MoltenVK), modern, zukunftssicher. Hohe Verbose ist gewollt — passt zur "von Grund auf"-Philosophie.

### Quellen
- 🌐 **Tutorial:** [vulkan-tutorial.com](https://vulkan-tutorial.com) — **Der** Einstiegspunkt für Vulkan mit C++ und GLFW. Vollständige, schrittweise Anleitung vom Fenster bis zum 3D-Modell.
- 🌐 **Offiziell:** [vulkan.org/learn](https://www.vulkan.org/learn) — Khronos-eigene Lernressourcen, Tutorials und Spezifikationen
- 🌐 **GitHub:** [github.com/SaschaWillems/Vulkan](https://github.com/SaschaWillems/Vulkan) — Umfangreiche C++ Beispielsammlung für Vulkan von Sascha Willems
- 🌐 **Blog:** [edw.is/learning-vulkan](https://edw.is/learning-vulkan/) — Erfahrungsbericht: Vulkan lernen und eine kleine Engine schreiben (2024)
- 🌐 **Blog:** [jeremyong.com — How to Learn Vulkan](https://www.jeremyong.com/c++/vulkan/graphics/rendering/2018/03/26/how-to-learn-vulkan/) — Lernstrategie und mentale Modelle für Vulkan
- 📺 **YouTube:** [Brendan Galea — Vulkan Game Engine Tutorials](https://www.youtube.com/playlist?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR) — Schritt-für-Schritt Vulkan Engine in C++ (sehr empfohlen)
- 📺 **YouTube:** [Travis Vroman — Kohi Game Engine](https://www.youtube.com/playlist?list=PLv8Ddw9K0JPg1BEO-RS-0MYs423cvLamx) — Vollständige Engine von Null, inkl. Vulkan-Renderer

---

## Schritt 3 — Window / Input: GLFW

**Entscheidung:** GLFW statt SDL3, da es schlanker ist, sich nahtlos mit Vulkan integriert und von praktisch allen Vulkan-Tutorials verwendet wird.

### Quellen
- 🌐 **Offizielle Doku:** [glfw.org/docs](https://www.glfw.org/docs/latest/) — Vollständige API-Dokumentation
- 🌐 **vcpkg:** `vcpkg install glfw3` — Direkt verfügbar
- 🌐 **Integration:** Wird in [vulkan-tutorial.com](https://vulkan-tutorial.com) als Standard verwendet — keine separaten Ressourcen nötig

---

## Schritt 4 — Engine-Architektur: ECS mit entt

**Entscheidung:** Entity-Component-System (ECS) statt klassischem GameObject/OOP-Ansatz. Performant durch Cache-Lokalität (Data-Oriented Design). `entt` ist die de-facto Standard-Bibliothek für C++.

### Quellen
- 🌐 **GitHub:** [github.com/skypjack/entt](https://github.com/skypjack/entt) — Offizielle entt-Repository mit Dokumentation und Wiki
- 🌐 **Wiki:** [entt — EnTT in Action](https://github.com/skypjack/entt/wiki/EnTT-in-Action) — Liste von Projekten, Tutorials und Artikeln die entt nutzen
- 📺 **YouTube:** [The Cherno — Intro to EnTT ECS](https://www.youtube.com/watch?v=D4hz0wEB978) — Hands-on Einführung im Kontext einer C++ Game Engine
- 📺 **YouTube:** [GameFromScratch — EnTT ECS Library](https://www.youtube.com/watch?v=jjGY7EyaTr0) — Überblick und Demo der entt-Bibliothek
- 🌐 **Blog:** [gamefromscratch.com — EnTT Library](https://gamefromscratch.com/entt-entity-component-system-gaming-library/) — Einführungsartikel mit Codebeispielen
- 🌐 **Artikel:** [austinmorlan.com — ECS in C++](https://austinmorlan.com/posts/entity_component_system/) — Detaillierte Erklärung wie man ein eigenes ECS in C++ baut (vertiefendes Hintergrundwissen)

---

## Schritt 5 — Build-System: CMake + vcpkg

**Entscheidung:** CMake mit Presets + vcpkg als Package-Manager. Bereits bekannte Infrastruktur. Standard in der Vulkan/Games-Community.

### Quellen
- 🌐 **Offizielle Doku:** [cmake.org/documentation](https://cmake.org/documentation/) — CMake Referenz
- 🌐 **vcpkg:** [vcpkg.io](https://vcpkg.io) — Microsoft Package Manager für C++
- 📺 **YouTube:** [vector-of-bool — CMake Basics](https://www.youtube.com/playlist?list=PLK6MXr8gasrGmIiSuVQXpfFuE1uR19Hos) — Moderne CMake-Konzepte verständlich erklärt
- 🌐 **Buch (online):** [An Introduction to Modern CMake](https://cliutils.gitlab.io/modern-cmake/) — Kostenloses Online-Buch zu modernem CMake

---

## Schritt 6 — Audio: miniaudio (Phase 1) → FMOD (Phase 2)

**Entscheidung:** Phase 1 mit miniaudio starten (header-only, zero dependencies), in Phase 2 bei Bedarf auf FMOD upgraden.

### Quellen
- 🌐 **Offizielle Seite:** [miniaud.io](https://miniaud.io) — Dokumentation und Beispiele
- 🌐 **GitHub:** [github.com/mackron/miniaudio](https://github.com/mackron/miniaudio) — Single-header Audio-Bibliothek
- 🌐 **FMOD:** [fmod.com/docs](https://www.fmod.com/docs/2.02/) — API-Dokumentation (kostenlos für Indie bis $200k Umsatz)

---

## Schritt 7 — Asset-Management

**Entscheidung:** Bewährte header-only Bibliotheken für I/O, eigener AssetManager für Caching und Handles.

| Asset-Typ | Bibliothek |
|---|---|
| Texturen (PNG/JPG) | stb_image |
| Tilemaps | Tiled Editor + nlohmann/json |
| Fonts | stb_truetype |
| Konfiguration | nlohmann/json |

### Quellen
- 🌐 **GitHub:** [github.com/nothings/stb](https://github.com/nothings/stb) — stb_image, stb_truetype und weitere header-only Libs
- 🌐 **Tiled Editor:** [mapeditor.org](https://www.mapeditor.org) — Standard 2D Tilemap-Editor, exportiert JSON
- 🌐 **GitHub:** [github.com/nlohmann/json](https://github.com/nlohmann/json) — JSON für C++, header-only

---

## Schritt 8 — Debugging & Profiling

**Entscheidung:** Mehrschichtige Debug-Strategie: Validation Layers + ImGui von Tag 1, RenderDoc extern, Tracy in Phase 2.

| Tool | Zweck |
|---|---|
| Vulkan Validation Layers | API-Misuse erkennen (im Vulkan SDK enthalten) |
| Dear ImGui | In-Engine Debug-UI (Entity Inspector, Stats) |
| RenderDoc | GPU Frame Debugger |
| Tracy Profiler | CPU/GPU Profiling (Phase 2) |

### Quellen
- 🌐 **GitHub:** [github.com/ocornut/imgui](https://github.com/ocornut/imgui) — Dear ImGui mit Vulkan-Backend Beispielen
- 🌐 **RenderDoc:** [renderdoc.org/docs](https://renderdoc.org/docs/index.html) — Offizielle Dokumentation
- 🌐 **Tracy:** [github.com/wolfpld/tracy](https://github.com/wolfpld/tracy) — Tracy Profiler GitHub
- 🌐 **Vulkan Tools:** [vulkan.org/tools](https://www.vulkan.org/tools) — Übersicht aller offiziell empfohlenen Vulkan-Debugging-Tools inkl. Tracy und RenderDoc

---

## Empfohlene Lernreihenfolge

```
Phase 1 — 2D Engine
1.  GLFW Window + Vulkan Setup       → vulkan-tutorial.com
2.  Eigener 2D-Renderer              → Brendan Galea YouTube
3.  ECS-Integration (entt)           → The Cherno YouTube
4.  AssetManager (stb_image, Tiled)  → stb GitHub + Tiled Doku
5.  Game Loop + miniaudio            → miniaud.io
6.  ImGui Debug-UI                   → imgui GitHub
7.  Erstes spielbares 2D-Spiel       → 🎉

Phase 2 — 3D Engine
8.  3D-Renderer (Meshes, Kamera)     → SaschaWillems Vulkan Examples
9.  FMOD Integration                 → fmod.com/docs
10. Tracy Profiler einbinden         → wolfpld/tracy
11. Erstes spielbares 3D-Spiel       → 🎉
```

---

## Pflichtlektüre (Bücher)

| Buch | Autor | Thema |
|---|---|---|
| *Game Engine Architecture* (3rd Ed.) | Jason Gregory | Theoretisches Fundament für alle Engine-Subsysteme |
| *Real-Time Rendering* (4th Ed.) | Akenine-Möller et al. | Tiefer Einblick in Rendering-Algorithmen |
| *Effective Modern C++* | Scott Meyers | Moderne C++ Idiome die in einer Engine täglich gebraucht werden |

- 🌐 **gameenginebook.com** — Offizielle Seite zu *Game Engine Architecture* von Jason Gregory

---

## Nützliche Community-Ressourcen

- 🌐 [gamedev.stackexchange.com](https://gamedev.stackexchange.com) — Q&A für Spieleentwicklung
- 🌐 [reddit.com/r/vulkan](https://www.reddit.com/r/vulkan/) — Vulkan Community
- 🌐 [reddit.com/r/gameenginedev](https://www.reddit.com/r/gameenginedev/) — Engine-Entwicklung Community
- 💬 **Discord:** Vulkan Discord (via vulkan.org) — Direkter Kontakt zur Community
- 📺 **YouTube:** [The Cherno](https://www.youtube.com/@TheCherno) — C++ und Game Engine Development
- 📺 **YouTube:** [GameFromScratch](https://www.youtube.com/@gamefromscratch) — Game Dev Tooling und Bibliotheken
