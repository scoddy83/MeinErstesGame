---
created: 2026-05-21T23:37
Last update: 2026-05-21T23:37
tags:
  - Phase-1-8
  - Game
  - Physik
---
# ISSUE-013 — Sprung + Physik-System

## ID

ISSUE-013

## Typ

🙋 HITL — erfordert deinen aktiven Einsatz (Gravitations- und Sprungwerte tunen bis es sich richtig anfühlt)

## Phase

Phase 1.8 — Jump & Run Spiel

## What to build

`PhysicsBody`-Komponente und `PhysicsSystem` die Gravitation und Velocity-Integration übernehmen. Der Spieler springt mit der Leertaste, wird durch Gravitation nach unten gezogen, und das `isGrounded`-Flag gibt an ob er auf dem Boden steht. Coyote Time und Jump Buffering kommen in ISSUE-015 — hier wird erst die Grundlage gelegt.

## Acceptance criteria

- [ ] `PhysicsBody`-Komponente ist vollständig definiert
- [ ] `PhysicsSystem` wendet Gravitation pro Fixed-Timestep-Tick an
- [ ] Velocity wird in Position integriert: `position += velocity × dt`
- [ ] Leertaste löst Sprung aus (Velocity.y = jumpForce) — nur wenn `isGrounded == true`
- [ ] Spieler fällt nach dem Sprung durch Gravitation wieder nach unten
- [ ] Sprung fühlt sich physikalisch korrekt an (nicht zu schwimmend, nicht zu steif)

## Blocked by

ISSUE-012 — Input-System + horizontale Spielerbewegung

## Tasks

- [ ] **`PhysicsBody`-Komponente definieren**
  - [ ] `PhysicsBody { glm::vec2 velocity, glm::vec2 acceleration, bool isGrounded, float coyoteTimer, float jumpBufferTimer }`
  - [ ] `coyoteTimer` und `jumpBufferTimer` werden hier definiert, aber erst in ISSUE-015 genutzt

- [ ] **`PhysicsSystem` implementieren**
  - [ ] Gravitation anwenden: `velocity.y += gravity × dt` (gravity ist negativ, z. B. `-980.0f` Pixel/s²)
  - [ ] Velocity integrieren: `position += velocity × dt`
  - [ ] Maximale Fallgeschwindigkeit clampen (Terminal Velocity)

- [ ] **Sprung in `PlayerSystem`**
  - [ ] `Input::isKeyPressed(Key::Space)` prüfen
  - [ ] Wenn `isGrounded == true`: `velocity.y = jumpForce`
  - [ ] `isGrounded = false` nach dem Sprung setzen

- [ ] **Werte tunen**
  - [ ] `gravity`, `jumpForce`, Terminal Velocity mit ImGui live anpassen
  - [ ] Sprung soll sich kräftig und responsiv anfühlen — kein "Mondsprung"

## Ressourcen

| Thema | Link |
|---|---|
| Platformer Physik | [Gamedev.net — Platformer Physics](https://www.gamedev.net/tutorials/programming/general-and-gameplay-programming/classic-platformer-physics-explained-r5168/) |
| Jump Feel | [The Art of Screenshake — Jan Willem Nijman](https://www.youtube.com/watch?v=AJdEqssNZ-U) |

## Mentor-Hinweis

> `gravity = -9.8` ist viel zu schwach für ein Spiel — denke in Pixel/s², nicht in m/s². Starte mit `gravity = -800` bis `-1200` und `jumpForce = +400` bis `+600` und tune von dort. Nutze ImGui um den Sweet Spot live zu finden.
