---
created: 2026-05-21T23:37
Last update: 2026-05-21T23:37
tags:
  - Phase-1-2
  - Vulkan
  - GLFW
---
# ISSUE-003 — GLFW Fenster + erstes Vulkan Dreieck

## ID

ISSUE-003

## Typ

🙋 HITL — erfordert deinen aktiven Einsatz (vulkan-tutorial.com Schritt für Schritt durcharbeiten)

## Phase

Phase 1.2 — GLFW Window + Vulkan Setup

## What to build

Ein GLFW-Fenster das sich öffnet, und ein buntes Dreieck das via Vulkan auf dem Bildschirm gerendert wird. Das ist das schwierigste Kapitel des gesamten Projekts — Vulkan ist bewusst verbose. Jede Zeile soll verstanden, nicht kopiert werden. Am Ende steht eine saubere `Window`-Klasse und eine funktionierende Vulkan-Pipeline mit aktivierten Validation Layers.

## Acceptance criteria

- [ ] GLFW-Fenster öffnet sich und schließt sich sauber (kein Crash beim Beenden)
- [ ] Vulkan Validation Layers sind aktiv (Debug-Build) — keine Validation-Errors in der Konsole
- [ ] Vulkan-Instanz, Physical Device, Logical Device und Swapchain sind aufgesetzt
- [ ] Render Pass, Graphics Pipeline und Command Buffers sind konfiguriert
- [ ] Ein buntes Dreieck wird auf dem Bildschirm gerendert 🎉
- [ ] `Window`-Klasse kapselt GLFW in `engine/core/Window.h` / `Window.cpp`

## Blocked by

ISSUE-002 — Modernes C++ Grundlagen üben

## Tasks

- [ ] **Dependencies einrichten**
  - [ ] `glfw3` und `vulkan` in `vcpkg.json` hinzufügen
  - [ ] CMake: `find_package(Vulkan)` und `find_package(glfw3)` in `engine/CMakeLists.txt`
  - [ ] Build läuft durch mit neuen Dependencies

- [ ] **vulkan-tutorial.com durcharbeiten** (Abschnitt *Introduction* bis *Drawing a triangle*)
  - [ ] Window Surface
  - [ ] Swap Chain
  - [ ] Image Views
  - [ ] Render Pass
  - [ ] Graphics Pipeline (Shader Module, Fixed Functions, Pipeline Layout)
  - [ ] Framebuffers
  - [ ] Command Buffers
  - [ ] Rendering & Presentation (Semaphores, Fences)

- [ ] **`Window`-Klasse anlegen**
  - [ ] `engine/core/Window.h` / `Window.cpp` — GLFW hinter Engine-API kapseln
  - [ ] Fenster-Größe, Titel konfigurierbar
  - [ ] Event-Loop (sollFensterGeschlossen) sauber getrennt

- [ ] **Validation Layers aktivieren**
  - [ ] `VK_LAYER_KHRONOS_validation` im Debug-Build aktiv
  - [ ] Debug Messenger für Validation-Output einrichten

- [ ] **Shader kompilieren**
  - [ ] Einfacher Vertex Shader (`vert.glsl`) — Hardcoded Dreieck-Koordinaten
  - [ ] Einfacher Fragment Shader (`frag.glsl`) — Bunte Ausgabe
  - [ ] SPIR-V kompilieren mit `glslc`

## Ressourcen

| Thema | Link |
|---|---|
| Hauptressource | [vulkan-tutorial.com](https://vulkan-tutorial.com) |
| Vulkan Engine YouTube | [Brendan Galea Playlist](https://www.youtube.com/playlist?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR) |
| GLFW Dokumentation | [glfw.org/docs/latest](https://www.glfw.org/docs/latest/) |
| Vulkan Beispiele | [github.com/SaschaWillems/Vulkan](https://github.com/SaschaWillems/Vulkan) |

## Mentor-Hinweis

> Wenn etwas nicht funktioniert: **zuerst die Validation Layer Messages lesen** — sie sagen in 90% der Fälle exakt was falsch ist und wo. RenderDoc kannst du schon jetzt öffnen und einen Frame capturen, um die Pipeline visuell zu inspizieren. Diese Phase dauert länger als erwartet — das ist normal und wertvoll.
