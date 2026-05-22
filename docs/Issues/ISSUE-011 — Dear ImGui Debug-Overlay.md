---
created: 2026-05-21T23:37
Last update: 2026-05-21T23:37
tags:
  - Phase-1-7
  - ImGui
  - Debug
---
# ISSUE-011 — Dear ImGui Debug-Overlay

## ID

ISSUE-011

## Typ

🙋 HITL — erfordert deinen aktiven Einsatz (ImGui Vulkan-Backend integrieren, Render Pass anpassen)

## Phase

Phase 1.7 — ImGui Debug-UI

## What to build

Dear ImGui mit Vulkan-Backend in den bestehenden Render Pass integrieren. Das Debug-Overlay zeigt FPS, Entity-Daten und Asset-Statistiken live an — ohne separates Fenster, direkt über dem Spiel. Mit `F1` ein- und ausblendbar. Das Overlay wird das primäre Debugging-Werkzeug für alle folgenden Phasen.

## Acceptance criteria

- [ ] ImGui rendert korrekt über dem Spiel-Content (kein Z-Fighting, korrekte Render-Reihenfolge)
- [ ] FPS-Counter zeigt aktuelle Framerate live an
- [ ] Entity Inspector zeigt alle Komponenten einer ausgewählten Entity an
- [ ] Asset-Statistik: Anzahl geladener Texturen wird angezeigt
- [ ] `F1` blendet das Overlay ein und aus
- [ ] Keine Vulkan Validation Errors durch ImGui-Integration

## Blocked by

ISSUE-009 — Fixed Timestep Game Loop

## Tasks

- [ ] **ImGui via vcpkg einbinden**
  - [ ] `imgui` mit `imgui[vulkan-binding]` Feature in `vcpkg.json`
  - [ ] `imgui_impl_glfw.cpp` und `imgui_impl_vulkan.cpp` in Build einbinden

- [ ] **ImGui in Vulkan-Render-Pass integrieren**
  - [ ] `ImGui::CreateContext()` beim App-Start
  - [ ] `ImGui_ImplGlfw_InitForVulkan()` und `ImGui_ImplVulkan_Init()` mit korrekten Vulkan-Objekten
  - [ ] Descriptor Pool für ImGui anlegen
  - [ ] ImGui-Draw-Commands in Command Buffer nach Spiel-Rendering einfügen
  - [ ] `ImGui::DestroyContext()` beim Shutdown

- [ ] **Debug-Panels implementieren**
  - [ ] FPS-Counter: `ImGui::Text("FPS: %.1f", fps)`
  - [ ] Entity Inspector: `registry.view<Transform>()` iterieren, ausgewählte Entity anzeigen
  - [ ] Komponenten-Werte live editierbar machen (Position, Velocity)
  - [ ] Asset-Statistik: `AssetManager::getTextureCount()` anzeigen

- [ ] **F1-Toggle**
  - [ ] `Input`-System um `isKeyPressed(F1)` erweitern
  - [ ] Bool `showDebugOverlay` togglen, ImGui-Rendering überspringen wenn false

## Ressourcen

| Thema | Link |
|---|---|
| ImGui Repository | [github.com/ocornut/imgui](https://github.com/ocornut/imgui) |
| Vulkan Backend | `imgui/backends/imgui_impl_vulkan.cpp` im ImGui-Repo |
| The Cherno — ImGui | [ImGui in C++ Tutorial](https://www.youtube.com/watch?v=nVaQuNXueFw) |

## Mentor-Hinweis

> Das ImGui Vulkan-Backend ist technisch das fummeligste an dieser Integration — der Descriptor Pool muss separat für ImGui angelegt werden. Schaue dir `examples/example_glfw_vulkan/main.cpp` im ImGui-Repository an, das ist die beste Referenz. Sobald es läuft, wirst du ImGui in jedem zukünftigen Projekt von Tag 1 einbinden wollen.
