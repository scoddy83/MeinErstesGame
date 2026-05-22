---
created: 2026-05-21T23:37
Last update: 2026-05-21T23:37
tags:
  - Phase-1-3
  - Renderer
  - Performance
---
# ISSUE-005 — Sprite Batching

## ID

ISSUE-005

## Typ

🙋 HITL — erfordert deinen aktiven Einsatz (Renderer-Architektur durchdenken, Vertex-Buffer-Strategie wählen)

## Phase

Phase 1.3 — Eigener 2D-Renderer

## What to build

Statt jeden Sprite in einem eigenen Draw Call zu rendern, werden mehrere Sprites in einem einzigen Draw Call gebündelt. Der Renderer sammelt alle `drawSprite()`-Aufrufe pro Frame in einem dynamischen Vertex Buffer, und schickt alles mit einem einzigen `vkCmdDrawIndexed`. Das verhindert, dass die Framerate mit der Sprite-Anzahl kollabiert.

## Acceptance criteria

- [ ] 100+ Sprites werden in einem einzigen Draw Call gerendert
- [ ] Kein messbarer Performance-Einbruch gegenüber 10 Sprites
- [ ] `drawSprite()` API bleibt nach außen identisch — kein Breaking Change
- [ ] `beginBatch()` / `endBatch()` (oder äquivalentes Flush-Muster) ist klar strukturiert
- [ ] Keine Vulkan Validation Errors

## Blocked by

ISSUE-004 — Texturierter Sprite-Renderer + orthografische Kamera

## Tasks

- [ ] **Dynamischen Vertex Buffer aufsetzen**
  - [ ] Host-visible Buffer der pro Frame mit Sprite-Daten befüllt wird
  - [ ] Buffer groß genug für z. B. 10.000 Sprites dimensionieren
  - [ ] Vertices werden CPU-seitig gesammelt, einmal gemappt und in den Buffer geschrieben

- [ ] **Batch-Logik implementieren**
  - [ ] `beginBatch()`: Buffer zurücksetzen, Schreibzeiger auf Anfang
  - [ ] `drawSprite()`: Quad-Vertices in den Batch-Buffer schreiben (kein Draw Call)
  - [ ] `endBatch()` / `flush()`: Einen einzigen `vkCmdDrawIndexed` für alle gesammelten Sprites

- [ ] **Index Buffer für Quads**
  - [ ] Statischer Index Buffer für max. N Quads vorab generieren (0,1,2,2,3,0 Muster)

- [ ] **Testen**
  - [ ] 100 Sprites rendern und Frame Time messen (ImGui FPS Counter noch nicht da — Konsolen-Output reicht)
  - [ ] Vergleich: Draw Calls vorher vs. nachher via RenderDoc verifizieren

## Ressourcen

| Thema | Link |
|---|---|
| Batching-Konzept | [The Cherno — Batch Rendering](https://www.youtube.com/watch?v=Th4huqR77rI) |
| Brendan Galea Playlist | [Vulkan Game Engine Tutorials](https://www.youtube.com/playlist?list=PL8327DO66nu9qYVKLDmdLW_84-yE4auCR) |

## Mentor-Hinweis

> Batching ist einer der größten Performance-Hebel in 2D-Spielen. Eine Tilemap mit 500 Tiles ohne Batching bedeutet 500 Draw Calls pro Frame — das tötet die GPU. Mit Batching ist es einer. Dieses Issue ist der Grund, warum Tilemap-Rendering in Phase 1.5 ohne Performance-Probleme funktionieren wird.
