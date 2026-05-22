---
created: 2026-05-21T23:37
Last update: 2026-05-21T23:37
tags:
  - Phase-1-5
  - Assets
  - Tilemap
---
# ISSUE-008 — Tiled Tilemap laden und rendern

## ID

ISSUE-008

## Typ

🙋 HITL — erfordert deinen aktiven Einsatz (Tiled Editor lernen, JSON-Struktur verstehen, Layer-Konzept definieren)

## Phase

Phase 1.5 — AssetManager (Tilemaps)

## What to build

Ein `TilemapLoader` der Tiled JSON-Dateien parst und die Tiles als ECS-Entities ins System einfügt. Zwei Layer-Typen werden unterschieden: `collision` (physikalisch relevant, wird von der Physik berücksichtigt) und `background` (nur visuell, kein Einfluss auf Kollision). Eine Test-Map aus dem Tiled Editor wird geladen und vollständig gerendert.

## Acceptance criteria

- [ ] Tiled Editor ist installiert und eine Test-Map wurde erstellt (mind. 20×15 Tiles, 2 Layer)
- [ ] Map wird als JSON exportiert und vom `TilemapLoader` korrekt geparst
- [ ] Tiles werden als ECS-Entities mit `Transform`, `Sprite` und optional `TileCollider` angelegt
- [ ] Kollisions-Layer-Tiles erhalten `TileCollider { isSolid: true }`
- [ ] Dekorations-Layer-Tiles erhalten keinen `TileCollider`
- [ ] Die gerenderte Map sieht korrekt aus (kein falsch positionierter Tile)

## Blocked by

ISSUE-007 — AssetManager mit Handle-basiertem Caching

## Tasks

- [ ] **Tiled Editor einrichten**
  - [ ] Tiled von [mapeditor.org](https://www.mapeditor.org) installieren
  - [ ] Tileset anlegen (PNG mit Tile-Raster)
  - [ ] Test-Map erstellen: Layer `background` (Dekoration) + Layer `collision` (solide Tiles)
  - [ ] Map als JSON exportieren (`File > Export As > JSON`)

- [ ] **`TilemapLoader` implementieren**
  - [ ] `engine/assets/TilemapLoader.h` / `TilemapLoader.cpp`
  - [ ] JSON-Datei mit `nlohmann/json` einlesen
  - [ ] Tile-Layer iterieren, Tile-ID → Tileset-UV-Koordinaten berechnen
  - [ ] Pro Tile: `entt::entity` anlegen mit `Transform` (Welt-Position) und `Sprite` (Tileset-UV)

- [ ] **`TileCollider`-Komponente anlegen**
  - [ ] `TileCollider { bool isSolid }` als Komponente definieren
  - [ ] Kollisions-Layer-Tiles bekommen `TileCollider { isSolid: true }`
  - [ ] Dekorations-Layer-Tiles bekommen keinen `TileCollider`

- [ ] **Layer-Namen aus JSON lesen**
  - [ ] Layer-Name aus JSON-Feld `"name"` auslesen
  - [ ] `"collision"` → Tiles mit `TileCollider` anlegen; alle anderen → ohne

- [ ] **Testen**
  - [ ] Test-Map laden und rendern — sieht korrekt aus
  - [ ] Kollisions-Tiles via Debugger prüfen: haben `TileCollider`-Komponente

## Ressourcen

| Thema | Link |
|---|---|
| Tiled Editor | [mapeditor.org](https://www.mapeditor.org) |
| Tiled JSON Format | [doc.mapeditor.org/en/stable/reference/json-map-format](https://doc.mapeditor.org/en/stable/reference/json-map-format/) |
| nlohmann/json | [github.com/nlohmann/json](https://github.com/nlohmann/json) |

## Mentor-Hinweis

> Lege im Tiled Editor von Anfang an zwei Layer an — `collision` und `background` — und halte dich konsequent daran. Diese Konvention entscheidet später in ISSUE-014, welche Tiles die Physik stoppt und welche nur aussehen. Falsch angelegte Layer bedeuten unsichtbare Wände oder Lücken im Boden.
