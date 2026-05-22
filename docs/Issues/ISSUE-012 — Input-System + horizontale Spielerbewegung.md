---
created: 2026-05-21T23:37
Last update: 2026-05-21T23:37
tags:
  - Phase-1-8
  - Game
  - Input
---
# ISSUE-012 — Input-System + horizontale Spielerbewegung

## ID

ISSUE-012

## Typ

🙋 HITL — erfordert deinen aktiven Einsatz (Feel der Bewegung tunen — Beschleunigung, Reibung, Geschwindigkeit)

## Phase

Phase 1.8 — Jump & Run Spiel

## What to build

Ein sauberes `Input`-System in `core/Input.h` das GLFW-Tastatur-Events hinter einer Engine-API kapselt. Darauf aufbauend: horizontale Spielerbewegung mit Beschleunigung und Reibung (fühlt sich besser an als direkte Geschwindigkeitszuweisung). Der Spieler-Sprite bewegt sich links und rechts auf dem Bildschirm.

## Acceptance criteria

- [ ] `Input::isKeyDown(Key::Left)` / `Key::Right` / `Key::Space` funktionieren
- [ ] `PlayerController`-Komponente mit `moveSpeed` und `jumpForce` ist definiert
- [ ] `PlayerSystem` liest Input und setzt `Velocity.x` basierend auf Beschleunigung und Reibung
- [ ] Spieler beschleunigt beim Drücken, verzögert beim Loslassen (kein abruptes Stoppen)
- [ ] Maximale Bewegungsgeschwindigkeit ist begrenzt

## Blocked by

ISSUE-009 — Fixed Timestep Game Loop

## Tasks

- [ ] **`Input`-System anlegen**
  - [ ] `engine/core/Input.h` / `Input.cpp`
  - [ ] `Input::isKeyDown(Key)` — Taste ist gerade gedrückt
  - [ ] `Input::isKeyPressed(Key)` — Taste wurde in diesem Frame gedrückt (für Sprung-Input)
  - [ ] GLFW Key-Callback an Input-System binden
  - [ ] `Key`-Enum: `Left`, `Right`, `Space`, `F1`, `Escape`

- [ ] **`PlayerController`-Komponente definieren**
  - [ ] `PlayerController { float moveSpeed, float jumpForce, float acceleration, float friction }`

- [ ] **`PlayerSystem` implementieren**
  - [ ] `registry.view<PlayerController, PhysicsBody>()` iterieren
  - [ ] Links/Rechts: `velocity.x += acceleration × direction × dt`
  - [ ] Loslassen: `velocity.x *= (1.0f - friction × dt)` (Reibung)
  - [ ] Geschwindigkeit clampen: `velocity.x = clamp(velocity.x, -moveSpeed, moveSpeed)`

- [ ] **Spieler-Entity anlegen**
  - [ ] Entity mit `Transform`, `Sprite`, `PhysicsBody`, `PlayerController` anlegen
  - [ ] Spieler-Textur laden und zuweisen

- [ ] **Werte tunen**
  - [ ] `moveSpeed`, `acceleration`, `friction` so einstellen, dass Bewegung sich natürlich anfühlt
  - [ ] ImGui nutzen um Werte live zu verändern

## Ressourcen

| Thema | Link |
|---|---|
| GLFW Input Docs | [glfw.org/docs/latest/input_guide.html](https://www.glfw.org/docs/latest/input_guide.html) |
| Game Feel — Bewegung | [Game Feel (Buch) — Steve Swink](https://www.google.com/search?q=game+feel+steve+swink) |

## Mentor-Hinweis

> Beschleunigung und Reibung statt direkter Geschwindigkeit machen den größten Unterschied für das Spielgefühl. Nutze ImGui (ISSUE-011) um `acceleration` und `friction` live zu tunen — das spart dir Dutzende von Neustarts.
