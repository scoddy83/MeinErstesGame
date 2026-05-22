---
created: 2026-05-21T23:37
Last update: 2026-05-21T23:37
tags:
  - Phase-1-6
  - GameLoop
---
# ISSUE-009 — Fixed Timestep Game Loop

## ID

ISSUE-009

## Typ

🙋 HITL — erfordert deinen aktiven Einsatz (Artikel lesen, Loop-Architektur durchdenken)

## Phase

Phase 1.6 — Game Loop

## What to build

Den bestehenden Game Loop in `core/Application.cpp` auf Fixed Timestep umstellen: Physik- und Logik-Updates laufen immer in gleichmäßigen Zeitschritten (z. B. 60 Hz), unabhängig von der Framerate. Das Rendering läuft so schnell wie möglich mit Interpolation. Das verhindert, dass physikalisches Verhalten sich je nach Framerate unterschiedlich anfühlt.

## Acceptance criteria

- [ ] Physik-Update läuft mit fixem Timestep (z. B. `1/60` Sekunden)
- [ ] Rendering läuft entkoppelt von der Physik-Rate (so schnell wie möglich)
- [ ] Bei 30 FPS und bei 120 FPS verhält sich ein fallender Sprite physikalisch identisch
- [ ] Akkumulator-Muster ist korrekt implementiert (kein Spiral of Death)
- [ ] Interpolation zwischen Physik-Schritten ist vorbereitet (kann später verfeinert werden)

## Blocked by

ISSUE-006 — ECS Integration mit entt

## Tasks

- [ ] **"Fix Your Timestep!" lesen**
  - [ ] Artikel von Glenn Fiedler vollständig lesen und Akkumulator-Muster verstehen
  - [ ] Unterschied zwischen `update(deltaTime)` und fixem Timestep klar sein

- [ ] **`Application`-Game-Loop umschreiben**
  - [ ] `accumulator`-Variable einführen
  - [ ] Pro Frame: `accumulator += frameTime`
  - [ ] Solange `accumulator >= fixedTimestep`: Physik-Update ausführen, `accumulator -= fixedTimestep`
  - [ ] Render-Aufruf mit Interpolations-Alpha: `alpha = accumulator / fixedTimestep`

- [ ] **Systeme anpassen**
  - [ ] `PhysicsSystem` und `MovementSystem` erhalten fixen `dt` statt variablen `deltaTime`
  - [ ] `RenderSystem` bleibt frame-basiert

- [ ] **Testen**
  - [ ] Framerate künstlich drosseln (Sleep einbauen) → Sprite-Bewegung bleibt gleich
  - [ ] Frame-Time-Spike simulieren → kein Spiral of Death (accumulator wird gecappt)

## Ressourcen

| Thema | Link |
|---|---|
| Pflichtlektüre | [Fix Your Timestep! — Gaffer on Games](https://gafferongames.com/post/fix_your_timestep/) |
| Game Loop Patterns | [gameprogrammingpatterns.com/game-loop.html](https://gameprogrammingpatterns.com/game-loop.html) |

## Mentor-Hinweis

> Ohne Fixed Timestep wird dein Spieler bei 30 FPS langsamer springen als bei 120 FPS — das ist nicht akzeptabel. Dieser Artikel von Glenn Fiedler ist Pflichtlektüre für jeden Spieleentwickler. Lies ihn zweimal.
