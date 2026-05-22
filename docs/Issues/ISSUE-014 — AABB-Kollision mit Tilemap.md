---
created: 2026-05-21T23:37
Last update: 2026-05-21T23:37
tags:
  - Phase-1-8
  - Game
  - Kollision
  - Physik
---
# ISSUE-014 — AABB-Kollision mit Tilemap

## ID

ISSUE-014

## Typ

🙋 HITL — erfordert deinen aktiven Einsatz (Kollisionsantwort debuggen, Kantenfälle testen)

## Phase

Phase 1.8 — Jump & Run Spiel

## What to build

AABB-Kollisionserkennung und -antwort zwischen Spieler-Collider und soliden Tilemap-Tiles (`TileCollider { isSolid: true }`). Der Spieler fällt nicht durch den Boden, wird von Wänden gestoppt und `isGrounded` wird korrekt gesetzt wenn er auf einem Tile landet. Reihenfolge: erst X-Achse auflösen, dann Y-Achse.

## Acceptance criteria

- [ ] Spieler fällt nicht durch Boden-Tiles
- [ ] Spieler wird von seitlichen Wand-Tiles gestoppt (links und rechts)
- [ ] Spieler wird von Decken-Tiles nach unten gedrückt (Kopf-Kollision)
- [ ] `isGrounded` wird `true` wenn Spieler von oben auf Tile trifft
- [ ] `isGrounded` wird `false` wenn Spieler von Tile weg springt oder fällt
- [ ] Trigger-Kollision (Collectibles) löst keine Positions-Korrektur aus

## Blocked by

ISSUE-013 — Sprung + Physik-System
ISSUE-008 — Tiled Tilemap laden und rendern

## Tasks

- [ ] **`Collider`-Komponente definieren**
  - [ ] `Collider { glm::vec2 offset, glm::vec2 size }` — AABB relativ zur Transform-Position

- [ ] **`CollisionSystem` implementieren**
  - [ ] Spieler-AABB berechnen: `position + collider.offset`, `collider.size`
  - [ ] Alle Tiles mit `TileCollider { isSolid: true }` in der Nähe finden (Broad Phase: nur nahe Tiles prüfen)
  - [ ] Für jeden soliden Tile: AABB-Überschneidung prüfen (`rectIntersect`)
  - [ ] Kollisionsantwort: erst X auflösen (horizontale Überschneidung korrigieren), dann Y

- [ ] **`isGrounded` korrekt setzen**
  - [ ] `isGrounded = true` wenn Spieler von oben auf Tile trifft (Y-Korrektur nach oben)
  - [ ] `isGrounded = false` am Anfang jedes Physik-Ticks (muss jedes Frame neu bestätigt werden)

- [ ] **Trigger-Kollision für Collectibles**
  - [ ] `CollectibleSystem`: Überschneidung prüfen, aber Position nicht korrigieren
  - [ ] Event oder direkte Logik: Collectible zerstören, Score erhöhen

- [ ] **Kantenfälle testen**
  - [ ] Spieler in Ecke: keine Klemmfehler
  - [ ] Schmale Lücke (1 Tile breit): Spieler passt durch oder wird korrekt blockiert
  - [ ] Sehr schnelle Bewegung: kein Tunneling (Spieler fliegt durch Tile)

## Ressourcen

| Thema | Link |
|---|---|
| AABB Kollision | [gamedev.stackexchange.com/questions/98262](https://gamedev.stackexchange.com/questions/98262) |
| Platformer Kollision | [higherorderfun.com — Platformer Collision](https://higherorderfun.com/blog/2012/05/20/the-guide-to-implementing-2d-platformers/) |

## Mentor-Hinweis

> Die Reihenfolge "erst X, dann Y auflösen" ist entscheidend. Wenn du beide Achsen gleichzeitig auflöst, bekommst du seltsame Diagonalkollisionen wo der Spieler an Wänden klebt. Teste ausgiebig mit Ecken und engen Gängen — das sind die klassischen Fehlerquellen.
