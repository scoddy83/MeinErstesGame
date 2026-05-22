---
created: 2026-05-21T23:37
Last update: 2026-05-21T23:37
tags:
  - Phase-1-4
  - ECS
---
# ISSUE-006 — ECS Integration mit entt

## ID

ISSUE-006

## Typ

🙋 HITL — erfordert deinen aktiven Einsatz (ECS-Konzept verstehen, Architektur-Entscheidungen treffen)

## Phase

Phase 1.4 — ECS mit entt

## What to build

Das Herzstück der Engine-Architektur: Entity Component System via `entt`. Bestehende Renderer-Logik wird ins ECS-Paradigma überführt. Spielobjekte werden nicht mehr durch Klassen-Vererbung, sondern durch Daten-Komponenten beschrieben. Systeme iterieren über Entities und enthalten die gesamte Logik. 50+ Entities werden ohne Performance-Einbußen gerendert.

## Acceptance criteria

- [ ] `entt` via vcpkg eingebunden und im Projekt verfügbar
- [ ] Basis-Komponenten `Transform`, `Sprite`, `Velocity` als reine Datenstrukturen definiert
- [ ] `RenderSystem` iteriert über alle Entities mit `Sprite` + `Transform` und rendert sie via `Renderer`
- [ ] `MovementSystem` aktualisiert Positionen anhand `Velocity` pro Frame
- [ ] 50+ Entities werden ohne merkliche Performance-Einbußen gerendert
- [ ] `PhysicsBody`, `PlayerController`, `Collider` sind konzeptuell durchdacht (noch nicht implementiert)

## Blocked by

ISSUE-005 — Sprite Batching

## Tasks

- [ ] **entt einbinden**
  - [ ] `entt` in `vcpkg.json` hinzufügen
  - [ ] `entt::registry` in `Application` oder einem zentralen `Scene`-Objekt anlegen

- [ ] **Komponenten definieren** (reine Datenstrukturen, keine Logik)
  - [ ] `Transform` { `glm::vec2 position`, `glm::vec2 scale`, `float rotation` }
  - [ ] `Sprite` { `TextureHandle textureHandle`, `int layer` }
  - [ ] `Velocity` { `float x`, `float y` }

- [ ] **Systeme implementieren**
  - [ ] `RenderSystem`: `registry.view<Transform, Sprite>()` iterieren → `renderer.drawSprite()` aufrufen
  - [ ] `MovementSystem`: `registry.view<Transform, Velocity>()` iterieren → Position += Velocity × deltaTime

- [ ] **Test-Szene aufbauen**
  - [ ] 50+ Entities mit `Transform`, `Sprite`, `Velocity` anlegen
  - [ ] Entities bewegen sich und werden korrekt gerendert
  - [ ] Performance messen (Framerate bei 50+ Entities stabil)

- [ ] **Konzept-Planung für Phase 1.8**
  - [ ] `PhysicsBody` Komponenten-Felder skizzieren: velocity, acceleration, isGrounded, coyoteTimer, jumpBufferTimer
  - [ ] `PlayerController` Felder skizzieren: moveSpeed, jumpForce
  - [ ] `Collider` Felder skizzieren: AABB bounds
  - [ ] Kurze Notiz in `docs/` ablegen — noch kein Code

## Ressourcen

| Thema | Link |
|---|---|
| entt Repository | [github.com/skypjack/entt](https://github.com/skypjack/entt) |
| EnTT in Action (Wiki) | [EnTT in Action](https://github.com/skypjack/entt/wiki/EnTT-in-Action) |
| The Cherno — EnTT | [Intro to EnTT ECS](https://www.youtube.com/watch?v=D4hz0wEB978) |
| ECS Konzept | [Austin Morlan — ECS in C++](https://austinmorlan.com/posts/entity_component_system/) |

## Mentor-Hinweis

> Lies den Artikel von Austin Morlan bevor du mit entt startest — er zeigt wie man ein ECS von Null baut, und das Verständnis dahinter macht entt's Design-Entscheidungen sofort nachvollziehbar. Komponenten enthalten keine Logik, Systeme enthalten keine Daten — das ist die einzige Regel die du einhalten musst.
