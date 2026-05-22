---
created: 2026-05-21T22:53
Last update: 2026-05-21T22:53
---
# PRD — Meine 2D Game Engine: Phase 0 & Phase 1

> **Projekt:** Eigene 2D Spielengine in C++23 + Vulkan  
> **Zieldatum Phase 1:** ~3–6 Monate  
> **Stack:** C++23 · Vulkan · GLFW · entt · CMake + vcpkg · miniaudio · stb · Dear ImGui  
> **Erstes Spiel:** Jump & Run Platformer (Phase 1.8)

---

## Problem Statement

Als angehender Spieleentwickler mit C++-Erfahrung möchte ich eine eigene 2D Game Engine von Grund auf bauen — nicht wegen einer schnellen Lösung, sondern um tief zu verstehen, wie moderne Spieletechnologie funktioniert. Fertige Engines wie Unity oder Godot verbergen die Rendering-Pipeline, den Game Loop und das ECS-System hinter Abstraktionsschichten, die das Lernen behindern. Das Ziel ist technische Tiefe, kein schnelles Produkt.

Das konkrete Lernproblem: Es fehlt ein strukturierter, aufbauender Plan, der von der Entwicklungsumgebung über Vulkan-Grundlagen bis hin zu einem fertigen, spielbaren Jump & Run führt — mit klaren Meilensteinen und Erfolgskriterien pro Phase.

---

## Solution

Eine selbst entwickelte 2D Spielengine in C++23 mit Vulkan als Grafik-API, die in klar abgegrenzten Lernphasen aufgebaut wird. Jede Phase liefert ein sichtbares Ergebnis (ein Fenster, ein Dreieck, ein Sprite, ein spielbares Level) und baut auf der vorherigen auf.

Das erste spielbare Spiel — ein kurzes Jump & Run — dient als konkretes Ziel, das alle Engine-Systeme herausfordert: Physik, Kollision, Tilemap-Rendering, Input, Kamera und Audio.

---

## User Stories

### Phase 0 — Setup

1. Als Entwickler möchte ich CLion mit allen nötigen Plugins eingerichtet haben, damit ich sofort produktiv Code schreiben kann ohne Tooling-Probleme.
2. Als Entwickler möchte ich das Vulkan SDK installiert und mit `vulkaninfo` verifiziert haben, damit ich sicher weiß, dass Vulkan auf meiner Hardware funktioniert.
3. Als Entwickler möchte ich RenderDoc installiert haben, damit ich später GPU-Frame-Debugging einsetzen kann ohne erneut Setup-Zeit zu verlieren.
4. Als Entwickler möchte ich vcpkg im Manifest-Modus eingerichtet haben, damit ich Dependencies per `vcpkg.json` deklarativ verwalten kann.
5. Als Entwickler möchte ich ein GitHub-Repository mit sauberer Branch-Strategie (`main` / `dev` / Feature-Branches), damit stabile Stände immer erreichbar sind.
6. Als Entwickler möchte ich eine definierte Ordnerstruktur (`engine/` / `game/` / `shaders/` / `assets/`), damit Engine-Code und Spiel-Code klar getrennt sind und das Projekt skaliert.
7. Als Entwickler möchte ich ein Root-CMakeLists.txt mit C++23-Standard und zwei Subprojekten (`engine` + `game`), damit der Build-Prozess von Anfang an sauber strukturiert ist.
8. Als Entwickler möchte ich CMake Presets für Debug und Release, damit ich schnell zwischen beiden Konfigurationen wechseln kann.

### Phase 1.1 — Modernes C++

9. Als Entwickler möchte ich RAII und Smart Pointer (`unique_ptr`, `shared_ptr`) sicher anwenden können, damit ich Memory Leaks in der Engine von Anfang an vermeide.
10. Als Entwickler möchte ich Move Semantics (`&&`, `std::move`) verstehen und anwenden können, damit Engine-Ressourcen effizient übertragen werden ohne unnötige Kopien.
11. Als Entwickler möchte ich `constexpr`, `auto` und Range-based for sicher nutzen, damit mein Code idiomatisches modernes C++23 spricht.
12. Als Entwickler möchte ich eine eigene RAII-Klasse mit Rule of Five geschrieben haben, damit ich das Konzept nicht nur verstehe, sondern angewendet habe.

### Phase 1.2 — GLFW Window + Vulkan Setup

13. Als Entwickler möchte ich ein GLFW-Fenster in einer `Window`-Klasse gekapselt haben, damit das Fenster-Management sauber von der Render-Logik getrennt ist.
14. Als Entwickler möchte ich Vulkan Validation Layers von Beginn an aktiviert haben, damit API-Fehler sofort als verständliche Meldungen sichtbar werden.
15. Als Entwickler möchte ich eine Vulkan-Instanz, Physical Device, Logical Device und Swapchain aufgesetzt haben, damit die Grundlage für das gesamte Rendering steht.
16. Als Entwickler möchte ich ein buntes Dreieck auf dem Bildschirm rendern, damit ich weiß, dass die gesamte Vulkan-Pipeline von Vertex bis Fragment-Shader funktioniert.

### Phase 1.3 — 2D-Renderer

17. Als Entwickler möchte ich Texturen (PNG) via `stb_image` laden und als Vulkan Texture Sampler binden können, damit Sprites mit echten Bildern gerendert werden.
18. Als Entwickler möchte ich einen texturierten Quad (Sprite) auf dem Bildschirm rendern, damit das erste sichtbare Spielobjekt entsteht.
19. Als Entwickler möchte ich eine orthografische Kamera implementiert haben, damit die 2D-Welt korrekt ohne Perspektiv-Verzerrung dargestellt wird.
20. Als Entwickler möchte ich Basis-Batching implementieren, damit mehrere Sprites in einem einzigen Draw Call gerendert werden und die Performance nicht mit der Sprite-Anzahl kollabiert.
21. Als Entwickler möchte ich eine `Renderer`-Klasse in `engine/renderer/`, damit Vulkan-Implementierungsdetails hinter einer sauberen API verborgen sind.

### Phase 1.4 — ECS mit entt

22. Als Entwickler möchte ich das ECS-Konzept (Entity, Component, System) praktisch verstanden haben, damit ich die Architektur-Entscheidungen hinter `entt` nachvollziehen kann.
23. Als Entwickler möchte ich Basis-Komponenten `Transform`, `Sprite` und `Velocity` definiert haben, damit Spielobjekte über Daten und nicht über Klassenvererbung beschrieben werden.
24. Als Entwickler möchte ich ein `RenderSystem` haben, das alle Entities mit `Sprite` + `Transform` iteriert und rendert, damit das Rendering system-getrieben und erweiterbar ist.
25. Als Entwickler möchte ich ein `MovementSystem` haben, das Positionen anhand `Velocity` aktualisiert, damit Bewegung über das ECS-Paradigma läuft.
26. Als Entwickler möchte ich 50+ Entities ohne merkliche Performance-Einbußen rendern, damit das ECS-Design seine Cache-Lokalität-Vorteile nachweist.
27. Als Entwickler möchte ich bereits die späteren Komponenten `PhysicsBody`, `PlayerController` und `Collider` konzeptuell eingeplant haben, damit das ECS-Design das Jump & Run nicht nachträglich erfordert.

### Phase 1.5 — AssetManager

28. Als Entwickler möchte ich einen `AssetManager` mit Handle-basiertem Caching, damit jede Textur nur einmal in den GPU-Speicher geladen wird — egal wie viele Entities sie nutzen.
29. Als Entwickler möchte ich den Tiled Editor installiert und eine Test-Map erstellt haben, damit ich den Workflow Level-Design → JSON → Engine praktisch kenne.
30. Als Entwickler möchte ich einen `TilemapLoader` der Tiled-JSON-Dateien parst und die Tiles als Entities ins ECS einfügt, damit Level aus Dateien geladen und nicht hardgecodet werden.
31. Als Entwickler möchte ich zwischen Kollisions-Layer und Dekorations-Layer in der Tilemap unterscheiden, damit der Renderer dekorative Tiles zeichnet, ohne dass die Physik sie als Hindernis behandelt.

### Phase 1.6 — Game Loop + Audio

32. Als Entwickler möchte ich einen Fixed-Timestep Game Loop in `core/Application.cpp`, damit Physik-Updates immer in gleichen Zeitschritten laufen — unabhängig von der Framerate.
33. Als Entwickler möchte ich `miniaudio` als single-header eingebunden haben, damit Audio ohne externe Build-Abhängigkeiten funktioniert.
34. Als Entwickler möchte ich einen `AudioManager` der Sound-Effekte und Hintergrundmusik abspielt, damit Audio-Anforderungen der Engine über eine einfache API zugänglich sind.
35. Als Entwickler möchte ich Hintergrundmusik im Loop abspielen, damit das Spiel eine Audio-Atmosphäre hat.

### Phase 1.7 — ImGui Debug-UI

36. Als Entwickler möchte ich Dear ImGui mit Vulkan-Backend in den bestehenden Render Pass integriert haben, damit Debug-Informationen ohne separates Fenster angezeigt werden.
37. Als Entwickler möchte ich einen FPS-Counter im Debug-Overlay, damit ich Leistungsprobleme sofort erkenne.
38. Als Entwickler möchte ich einen Entity Inspector, der alle Komponenten einer ausgewählten Entity live anzeigt, damit ich Engine-Zustände ohne Debugger-Breakpoints inspizieren kann.
39. Als Entwickler möchte ich das Debug-Overlay mit `F1` ein- und ausblenden, damit es im Spielbetrieb nicht sichtbar ist.

### Phase 1.8 — Jump & Run Spiel

40. Als Spieler möchte ich meinen Charakter mit Tastatur (links/rechts) steuern können, damit ich die Grundbewegung intuitiv beherrsche.
41. Als Spieler möchte ich springen können (Leertaste), damit ich Hindernisse und Lücken überwinden kann.
42. Als Spieler möchte ich durch Gravitation nach unten gezogen werden, damit sich Sprünge physikalisch korrekt anfühlen.
43. Als Spieler möchte ich Coyote Time erleben (kurz nach Plattformkante noch springen dürfen), damit sich das Sprungsystem nicht unfair träge anfühlt.
44. Als Spieler möchte ich Jump Buffering (Sprung-Input kurz vor dem Landen wird gepuffert), damit das Spiel auf meine Inputs reagiert, auch wenn das Timing knapp ist.
45. Als Spieler möchte ich nicht durch Tile-Wände und den Boden fallen, damit AABB-Kollision mit der Tilemap korrekt funktioniert.
46. Als Spieler möchte ich Münzen oder Sterne einsammeln können, damit ich ein Ziel und einen Score habe.
47. Als Spieler möchte ich bei einem Sturz in eine Lücke ein Game Over erhalten, damit das Spiel eine Herausforderung hat.
48. Als Spieler möchte ich beim Erreichen der Ziel-Flagge / des Portals das Level abschließen, damit jedes Level ein klares Ende hat.
49. Als Spieler möchte ich zwischen 2–3 Levels wechseln, damit das Spiel ein Gefühl von Fortschritt vermittelt.
50. Als Spieler möchte ich eine scrollende Kamera, die meinem Charakter folgt, damit große Level vollständig erkundet werden können.
51. Als Spieler möchte ich ein einfaches Hauptmenü, damit ich das Spiel starten und neu starten kann.
52. Als Spieler möchte ich meinen aktuellen Score sehen, damit ich weiß, wie viele Münzen ich gesammelt habe.
53. Als Spieler möchte ich Sprung-, Münz- und Game-Over-Sounds hören, damit das Spiel auditives Feedback gibt.
54. Als Spieler möchte ich das Spiel als `.exe` starten können ohne CLion, damit andere es ausprobieren können.
55. Als Entwickler möchte ich das Spiel einer anderen Person in die Hand geben können, die ohne Erklärung versteht was zu tun ist und 2–3 Minuten spielt.

---

## Implementation Decisions

### Module die gebaut werden

**Phase 0 — Infrastruktur**
- `CMakeLists.txt` (Root + Submodule `engine/` und `game/`) mit C++23-Standard und CMake Presets für Debug/Release.
- `vcpkg.json` Manifest für alle Dependencies. vcpkg läuft im Manifest-Modus — keine globale Installation.
- GitHub-Repository mit Branch-Strategie: `main` (stabil), `dev` (aktuelle Entwicklung), Feature-Branches nach Bedarf.

**`engine/core/` — Window, Application, Game Loop, Input**
- `Window`: GLFW-Fenster-Wrapper. Versteckt GLFW-API hinter einer Engine-eigenen Schnittstelle.
- `Application`: Haupt-Einstiegspunkt, hält den Game Loop. Implementiert Fixed Timestep nach dem Muster von [Fix Your Timestep! (Gaffer on Games)](https://gafferongames.com/post/fix_your_timestep/).
- `Input`: Kapselung des GLFW-Input-Systems (Keyboard). Bereit für spätere Gamepad-Unterstützung.

**`engine/renderer/` — Vulkan 2D Renderer**
- Vulkan-Setup: Instanz, Physical Device, Logical Device, Swapchain, Render Pass, Command Buffers.
- `Renderer`-Klasse: Öffentliche API für `drawSprite()` und `drawTilemap()`. Vulkan-Interna bleiben privat.
- Textur-Pipeline: Vulkan Descriptor Sets, Sampler. Laden via `stb_image`.
- Orthografische Kamera mit Uniform Buffer.
- Basis-Batching: Mehrere Sprites in einem Draw Call.

**`engine/ecs/` — Entity Component System**
- Dünner Wrapper um `entt`. Direkte Nutzung der `entt::registry` ist akzeptabel, wenn kein Framework-Lock-in entsteht.
- Komponenten (reine Datenstrukturen, keine Logik):
  - `Transform` { position, scale, rotation }
  - `Sprite` { textureHandle, layer }
  - `Velocity` { x, y }
  - `PhysicsBody` { velocity, acceleration, isGrounded, coyoteTimer, jumpBufferTimer }
  - `PlayerController` { moveSpeed, jumpForce }
  - `Collider` { bounds (AABB) }
  - `Collectible` { value }
  - `TileCollider` { isSolid }
- Systeme (iterieren über Komponenten, enthalten die Logik):
  - `RenderSystem`
  - `MovementSystem`
  - `PhysicsSystem` (Gravitation, Velocity-Integration)
  - `CollisionSystem` (AABB Tilemap-Kollision, Trigger-Kollision)
  - `PlayerSystem` (Input → PhysicsBody, Coyote Time, Jump Buffering)

**`engine/assets/` — AssetManager**
- Handle-basiertes Caching: Texturen werden durch einen Integer-Handle referenziert.
- Keine doppelten GPU-Uploads für die gleiche Datei.
- `TilemapLoader`: Parst Tiled JSON-Export (via `nlohmann/json`). Erzeugt ECS-Entities für Tiles mit `Transform`, `Sprite` und optional `TileCollider`.
- Zwei Tilemap-Layer: `collision` (physikalisch relevant) und `background` (nur visuell).

**`engine/audio/` — AudioManager**
- Wrapper um `miniaudio.h` (single-header, kein vcpkg nötig — direkte Einbindung aus GitHub).
- API: `playSound(handle)`, `playMusic(handle, loop)`, `stopMusic()`.

**`game/` — Jump & Run Spiel**
- `GameStateManager`: Enum-basierte Zustände `MainMenu`, `Playing`, `GameOver`, `LevelComplete`.
- Level-Dateien: Tiled JSON, werden aus `assets/maps/` zur Laufzeit geladen.
- Scrolling-Kamera: Folgt dem Spieler horizontal, klemmt an Level-Bounds.
- Score-System: Münzen-Counter, wird im ImGui-Overlay oder als Sprite-Text angezeigt.

### Architektur-Entscheidungen

**Engine/Spiel-Trennung:** `engine/` wird als statische Bibliothek gebaut. `game/` linkt dagegen. Diese Trennung hält die Engine wiederverwendbar für spätere Spiele und erzwingt eine saubere API-Grenze.

**Vulkan Validation Layers:** Von Tag 1 aktiv — nur im Debug-Build. Sie geben präzise Fehlermeldungen und sind die primäre Debugging-Ressource für Vulkan-Fehler.

**Coyote Time und Jump Buffering:** Werden in Phase 1.8 von Beginn an eingebaut, nicht nachgerüstet. Coyote Time: `PhysicsBody.coyoteTimer` — ein Float-Timer der nach dem Verlassen einer Plattform startet; Sprung ist gültig solange Timer > 0. Jump Buffering: `PhysicsBody.jumpBufferTimer` — Sprung-Input wird als Timer gespeichert; bei der nächsten Landung wird er konsumiert.

**AABB-Kollision:** Spieler-Kollider gegen Tilemap. Kollisionsantwort behandelt die vier Seiten getrennt: Oben/Unten setzen `isGrounded`, Links/Rechts stoppen horizontale Bewegung. Reihenfolge: erst X, dann Y auflösen.

**Fixed Timestep:** Physik und Logik laufen auf einem festen Timestep (z.B. 60 Hz), Rendering so schnell wie möglich mit Interpolation. Verhindert physikalisches Verhalten das sich je nach Framerate unterscheidet.

**Release-Build:** Asset-Pfade sind relativ zum Executable. CMake-Release-Preset schaltet Optimierungen ein und deaktiviert Validation Layers.

---

## Testing Decisions

**Was macht einen guten Test hier aus:** Tests prüfen beobachtbares Verhalten (Output, Zustandsänderungen) — nicht interne Implementierungsdetails wie private Funktionen oder Zwischenwerte. Da die Engine Grafik-Hardware voraussetzt, werden reine Logik-Module (ECS, Physik, Asset-Parsing) isoliert getestet.

**Module die getestet werden:**

- **Physik-Logik** (`PhysicsSystem`): Gravitation, Coyote Time Timer, Jump Buffering. Kein Rendering nötig — Komponenten-Werte können direkt geprüft werden. Test: Entity fällt durch Gravitation, Timer läuft korrekt ab, gepufferter Sprung wird bei Landung konsumiert.
- **AABB-Kollision** (`CollisionSystem`): Spieler kollidiert korrekt mit solidem Tile von oben/unten/links/rechts. `isGrounded` wird korrekt gesetzt und zurückgesetzt. Trigger-Kollision löst keine Position-Korrektur aus.
- **AssetManager Caching**: Zweifaches Laden der gleichen Textur liefert den gleichen Handle zurück. Keine doppelten Einträge im Cache.
- **TilemapLoader**: Parst eine bekannte Test-JSON-Datei und erzeugt die korrekte Anzahl Entities mit den richtigen Komponenten.
- **GameStateManager**: Übergänge zwischen Zuständen (`Playing` → `GameOver`, `LevelComplete` → nächstes Level) folgen der erwarteten Logik.

**Testansatz:** Unit Tests mit einem leichtgewichtigen C++ Test-Framework (z.B. Catch2 via vcpkg). Tests laufen ohne Fenster und ohne GPU (kein Vulkan-Kontext nötig).

---

## Out of Scope

- **3D-Rendering:** Phase 2 — nicht in diesem PRD.
- **FMOD-Integration:** Phase 2 — miniaudio deckt Phase 1 vollständig ab.
- **Tracy Profiler:** Phase 2.
- **Netzwerk / Multiplayer:** Nicht geplant.
- **Mobile / Konsolen:** Kein Ziel.
- **Eigene Scripting-Sprache oder Visual Scripting:** Zu komplex für Phase 1.
- **Physik-Engine (Box2D o.ä.):** Eigene einfache AABB-Physik ist ausreichend und lehrreicher.
- **Eigener Font-Renderer (stb_truetype):** UI-Text kann über ImGui angezeigt werden; eigenes Font-Rendering ist Phase 2.
- **Editor:** Kein In-Engine Level-Editor — Tiled wird als externer Editor genutzt.
- **Gamepad-Support:** Input-Architektur wird darauf vorbereitet, Implementierung ist Phase 2.
- **Mehr als 3–5 Level:** Das erste Spiel ist bewusst klein — Qualität vor Quantität.

---

## Further Notes

**Lernreihenfolge ist bewusst:** C++ auffrischen → Vulkan (das Schwierigste zuerst) → Renderer → ECS → Assets → Audio → ImGui → Spiel. Jede Phase liefert ein sichtbares Ergebnis, das Motivation erzeugt.

**Bücher als Begleitlektüre:** *Game Engine Architecture* (Jason Gregory) zum theoretischen Fundament, *Effective Modern C++* (Scott Meyers) zum täglichen Nachschlagen.

**Primäre Lernressourcen:**
- Vulkan: [vulkan-tutorial.com](https://vulkan-tutorial.com) + [Brendan Galea YouTube-Playlist](https://www.youtube.com/playlist?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR)
- C++: [The Cherno YouTube](https://www.youtube.com/@TheCherno)
- ECS/entt: [EnTT in Action Wiki](https://github.com/skypjack/entt/wiki/EnTT-in-Action) + [Austin Morlan — ECS in C++](https://austinmorlan.com/posts/entity_component_system/)
- CMake: [An Introduction to Modern CMake](https://cliutils.gitlab.io/modern-cmake/)

**Erfolgskriterium Phase 1 gesamt:** Das Spiel kann einer anderen Person ohne Erklärung in die Hand gegeben werden. Sie spielt 2–3 Minuten, versteht intuitiv was zu tun ist, und kommt entweder zu einem Game Over oder einem Level-Abschluss.

**Commit-Disziplin:** Nach jedem abgeschlossenen Meilenstein committen. Konvention: `feat: add texture loading via stb_image`. Lieber zu oft als zu selten pushen.

**Wenn du feststeckst:** Zuerst Vulkan Validation Layer Messages lesen — sie sagen in den meisten Fällen exakt was falsch ist. Dann: [gamedev.stackexchange.com](https://gamedev.stackexchange.com), [r/vulkan](https://www.reddit.com/r/vulkan/), RenderDoc für Frame-Analyse.
