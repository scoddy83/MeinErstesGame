---
created: 2026-05-21T23:37
Last update: 2026-05-21T23:37
tags:
  - Phase-1-8
  - Game
  - LevelDesign
---
# ISSUE-017 — Level-Design: 2–3 Levels im Tiled Editor

## ID

ISSUE-017

## Typ

🙋 HITL — erfordert deinen aktiven Einsatz (Level designen, testen, iterieren)

## Phase

Phase 1.8 — Jump & Run Spiel

## What to build

2–3 spielbare Level im Tiled Editor erstellen: mit Plattformen, Lücken, Münzen, einem Spawnpoint und einem Ziel (Flagge / Portal). Die Level steigen in Schwierigkeit. Jedes Level ist in sich abgeschlossen und testet die Spielmechaniken: Springen, Collectibles einsammeln, Ziel erreichen.

## Acceptance criteria

- [ ] Level 1: Tutorial-Level — einfache Plattformen, wenige Lücken, leicht zu beenden
- [ ] Level 2: Mittelschwer — Lücken erfordern präzises Springen, mehr Münzen
- [ ] Level 3 (optional): Schwerer — enge Sprünge, Coyote Time wird benötigt
- [ ] Jedes Level hat: Spawnpoint (Objekt-Layer), Ziel (Flagge/Portal), Münzen (Objekt-Layer)
- [ ] Alle Level exportiert als JSON und vom `TilemapLoader` korrekt geladen
- [ ] Layer-Konvention eingehalten: `collision`, `background`, `objects`

## Blocked by

ISSUE-008 — Tiled Tilemap laden und rendern

## Tasks

- [ ] **Tileset finalisieren**
  - [ ] Tileset-PNG mit Plattform-Tiles, Hintergrund-Tiles, Dekorations-Tiles
  - [ ] Tileset in Tiled registrieren

- [ ] **Level 1 erstellen**
  - [ ] Layer: `background` (Dekoration), `collision` (solide Tiles), `objects` (Spawnpoint, Flagge, Münzen)
  - [ ] Breite: ca. 40–60 Tiles, Höhe: ca. 15–20 Tiles
  - [ ] Spawnpoint-Objekt platzieren
  - [ ] Ziel-Objekt (Flagge / Portal) am rechten Ende
  - [ ] 5–10 Münzen verteilen
  - [ ] Als JSON exportieren: `assets/maps/level1.json`

- [ ] **Level 2 erstellen**
  - [ ] Schwieriger als Level 1: Lücken, höhere Plattformen
  - [ ] Als JSON exportieren: `assets/maps/level2.json`

- [ ] **Level 3 erstellen (optional)**
  - [ ] Coyote Time wird benötigt um bestimmte Sprünge zu schaffen
  - [ ] Als JSON exportieren: `assets/maps/level3.json`

- [ ] **Objekt-Layer in `TilemapLoader` einlesen**
  - [ ] Spawnpoint → Spieler-Spawn-Position setzen
  - [ ] Flagge/Portal → Ziel-Entity mit `LevelGoal`-Komponente anlegen
  - [ ] Münzen → Collectible-Entities anlegen

## Ressourcen

| Thema | Link |
|---|---|
| Tiled Editor Docs | [doc.mapeditor.org](https://doc.mapeditor.org/en/stable/) |
| Tiled JSON Format | [Tiled JSON Reference](https://doc.mapeditor.org/en/stable/reference/json-map-format/) |
| Freie Tilesets | [itch.io — Free Tilesets](https://itch.io/game-assets/free/tag-tileset) |

## Mentor-Hinweis

> Weniger ist mehr für das erste Spiel. Level 1 sollte in unter 2 Minuten zu beenden sein und dem Spieler alle Mechaniken zeigen. Lass jemanden Level 1 spielen bevor du Level 2 designst — du wirst überrascht sein was Menschen schwierig finden. Freie Tilesets gibt es zuhauf auf itch.io.
