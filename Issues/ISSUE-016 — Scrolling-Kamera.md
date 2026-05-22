---
created: 2026-05-21T23:37
Last update: 2026-05-21T23:37
tags:
  - Phase-1-8
  - Game
  - Kamera
---
# ISSUE-016 — Scrolling-Kamera

## ID

ISSUE-016

## Typ

🙋 HITL — erfordert deinen aktiven Einsatz (Kamera-Smoothing und Bounds tunen)

## Phase

Phase 1.8 — Jump & Run Spiel

## What to build

Eine Kamera die dem Spieler horizontal folgt und innerhalb der Level-Bounds bleibt. Die Kamera scrollt sanft mit dem Spieler mit (optional: leichtes Smoothing damit sie nicht abrupt springt) und klemmt am linken und rechten Level-Rand.

## Acceptance criteria

- [ ] Kamera folgt dem Spieler horizontal
- [ ] Kamera bleibt innerhalb der Level-Bounds (klemmt links und rechts)
- [ ] Kamera zeigt keinen Bereich außerhalb der Tilemap
- [ ] Kamera-Position wird korrekt als View-Matrix an den Renderer übergeben
- [ ] Optional: leichtes Smoothing (Kamera folgt mit kleiner Verzögerung)

## Blocked by

ISSUE-012 — Input-System + horizontale Spielerbewegung

## Tasks

- [ ] **Kamera-Logik in `PlayerSystem` oder eigenem `CameraSystem`**
  - [ ] Kamera-Position = Spieler-Position, horizontal zentriert
  - [ ] Level-Bounds aus Tilemap-Größe berechnen (Breite × Tile-Größe)
  - [ ] Kamera clampen: `cameraX = clamp(playerX - screenWidth/2, 0, levelWidth - screenWidth)`

- [ ] **Kamera-Position an Renderer übergeben**
  - [ ] `Renderer::setCameraPosition(x, y)` oder View-Matrix direkt aktualisieren
  - [ ] Uniform Buffer mit neuer View-Matrix pro Frame updaten

- [ ] **Smoothing (optional)**
  - [ ] `cameraX = lerp(cameraX, targetX, smoothFactor × dt)`
  - [ ] `smoothFactor` mit ImGui tunen (0 = sofort, 1 = sehr träge)

- [ ] **Testen**
  - [ ] Spieler läuft zum rechten Level-Rand → Kamera klemmt, zeigt keine leere Fläche
  - [ ] Spieler läuft zum linken Level-Rand → gleich
  - [ ] Spieler springt → Kamera folgt oder bleibt horizontal (vertikales Folgen ist optional)

## Ressourcen

| Thema | Link |
|---|---|
| Kamera-Smoothing | [gamedev.stackexchange.com — Smooth Camera](https://gamedev.stackexchange.com/questions/152465/how-do-i-implement-smooth-camera-tracking-in-a-platformer) |

## Mentor-Hinweis

> Starte mit harter Kamera (kein Smoothing) und füge Smoothing als letzten Schritt hinzu. Zu viel Smoothing fühlt sich träge an und kann Spieler desorientieren. Viele professionelle Plattformer haben eine "Kamera-Box" (Dead Zone) in der sich der Spieler frei bewegen kann ohne die Kamera zu bewegen — das ist ein guter nächster Schritt für Phase 2.
