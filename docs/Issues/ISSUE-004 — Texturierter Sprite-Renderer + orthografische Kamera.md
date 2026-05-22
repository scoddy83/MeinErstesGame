---
created: 2026-05-21T23:37
Last update: 2026-05-21T23:37
tags:
  - Phase-1-3
  - Renderer
---
# ISSUE-004 — Texturierter Sprite-Renderer + orthografische Kamera

## ID

ISSUE-004

## Typ

🙋 HITL — erfordert deinen aktiven Einsatz (Vulkan Descriptor Sets, Shader anpassen, Kamera-Mathe verstehen)

## Phase

Phase 1.3 — Eigener 2D-Renderer

## What to build

Erweiterung des Vulkan-Dreiecks zu einem vollständigen 2D-Sprite-Renderer: PNG-Texturen laden via `stb_image`, als Vulkan Texture Sampler binden, und einen texturierten Quad (Rechteck mit Bild) auf dem Bildschirm anzeigen. Dazu eine orthografische Kamera, die über einen Uniform Buffer an die Shader übergeben wird — die Grundlage für alle weiteren 2D-Darstellungen.

## Acceptance criteria

- [ ] Ein PNG wird via `stb_image` geladen und als Vulkan-Textur auf der GPU gespeichert
- [ ] Ein texturierter Quad (Sprite) wird korrekt auf dem Bildschirm gerendert
- [ ] Orthografische Kamera ist implementiert — Welt-Koordinaten werden korrekt auf Screen-Koordinaten abgebildet
- [ ] Kamera-Position kann verändert werden und der Sprite bewegt sich entsprechend
- [ ] `Renderer`-Klasse in `engine/renderer/` kapselt Vulkan-Interna hinter `drawSprite()`
- [ ] Keine Vulkan Validation Errors

## Blocked by

ISSUE-003 — GLFW Fenster + erstes Vulkan Dreieck

## Tasks

- [ ] **stb_image einbinden**
  - [ ] `stb_image` via vcpkg oder direkt als Single-Header einbinden
  - [ ] PNG laden, Pixel-Daten in Vulkan Image + Image View + Sampler übertragen
  - [ ] Staging Buffer für den GPU-Upload nutzen

- [ ] **Descriptor Sets aufsetzen**
  - [ ] Descriptor Set Layout: Uniform Buffer (Kamera) + Combined Image Sampler (Textur)
  - [ ] Descriptor Pool anlegen
  - [ ] Descriptor Sets pro Swapchain-Frame schreiben

- [ ] **Shader anpassen**
  - [ ] Vertex Shader: Position + UV als Input, MVP-Matrix aus Uniform Buffer
  - [ ] Fragment Shader: `texture(sampler, uv)` für Textur-Farbe

- [ ] **Quad-Geometrie**
  - [ ] Vertex Buffer mit 4 Vertices (Position + UV)
  - [ ] Index Buffer mit 6 Indices (2 Dreiecke = 1 Quad)

- [ ] **Orthografische Kamera**
  - [ ] `glm::ortho()` Projektion (links, rechts, oben, unten, near, far)
  - [ ] Kamera-Position als View-Matrix
  - [ ] MVP-Matrix (Model × View × Projection) via Uniform Buffer an Shader übergeben

- [ ] **`Renderer`-Klasse anlegen**
  - [ ] `engine/renderer/Renderer.h` / `Renderer.cpp`
  - [ ] Öffentliche API: `drawSprite(texture, position, size)`
  - [ ] Vulkan-Interna (Pipeline, Command Buffers) bleiben privat

## Ressourcen

| Thema | Link |
|---|---|
| Hauptressource | [Brendan Galea Playlist](https://www.youtube.com/playlist?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR) |
| stb_image | [github.com/nothings/stb](https://github.com/nothings/stb) |
| Vulkan Textur-Beispiel | [vulkan-tutorial.com/Texture mapping](https://vulkan-tutorial.com/Texture_mapping/Images) |
| GLM Dokumentation | [github.com/g-truc/glm](https://github.com/g-truc/glm) |
| Vulkan Beispiele (SaschaWillems) | [github.com/SaschaWillems/Vulkan](https://github.com/SaschaWillems/Vulkan) |

## Mentor-Hinweis

> Descriptor Sets sind das Konzept, das viele Vulkan-Einsteiger am längsten aufhält. Nimm dir Zeit, das Layout wirklich zu verstehen — du wirst es für jede Textur und jeden Uniform Buffer brauchen. Die Investition hier zahlt sich ab ISSUE-005 sofort aus.
