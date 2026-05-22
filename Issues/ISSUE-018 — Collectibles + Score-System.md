---
created: 2026-05-21T23:37
Last update: 2026-05-21T23:37
tags:
  - Phase-1-8
  - Game
  - Score
---
# ISSUE-018 — Collectibles + Score-System

## ID

ISSUE-018

## Typ

🙋 HITL — erfordert deinen aktiven Einsatz (Score-Anzeige gestalten, Collectible-Platzierung im Level testen)

## Phase

Phase 1.8 — Jump & Run Spiel

## What to build

Münzen (Collectibles) die der Spieler einsammeln kann, ein Score-Counter der hochzählt, und eine Anzeige des aktuellen Scores. Collectibles sind Trigger-Kollisionen (keine Positions-Korrektur). Beim Einsammeln wird die Entity zerstört und der Score erhöht. Der Score wird als ImGui-Overlay oder einfache Sprite-Zahl angezeigt.

## Acceptance criteria

- [ ] Münzen sind als Entities im Level vorhanden (aus Objekt-Layer geladen, ISSUE-017)
- [ ] Spieler läuft durch Münze → Münze verschwindet, Score +1
- [ ] Score wird sichtbar angezeigt (ImGui-Overlay oder Sprite-Text)
- [ ] Score wird korrekt zurückgesetzt beim Level-Neustart
- [ ] Keine Position-Korrektur bei Münz-Kollision (reine Trigger-Kollision)

## Blocked by

ISSUE-014 — AABB-Kollision mit Tilemap
ISSUE-017 — Level-Design: 2–3 Levels im Tiled Editor

## Tasks

- [ ] **`Collectible`-Komponente definieren**
  - [ ] `Collectible { int value }` (Standard: 1 pro Münze)

- [ ] **Trigger-Kollision in `CollisionSystem`**
  - [ ] Spieler-AABB gegen alle Entities mit `Collectible`-Komponente prüfen
  - [ ] Bei Überschneidung: Entity zur Löschliste hinzufügen, Score erhöhen
  - [ ] Entities erst nach dem System-Durchlauf löschen (nicht während Iteration)

- [ ] **Score-System**
  - [ ] Einfache `int score`-Variable im Game-State oder als eigene Komponente
  - [ ] `score += collectible.value` beim Einsammeln

- [ ] **Score-Anzeige**
  - [ ] Option A (einfach): ImGui Overlay `ImGui::Text("Score: %d", score)`
  - [ ] Option B (polished): Sprite-basierte Ziffernanzeige
  - [ ] Score-Anzeige bleibt sichtbar während des Spielens

- [ ] **Zurücksetzen beim Neustart**
  - [ ] `score = 0` wenn Level neu geladen wird

## Ressourcen

| Thema | Link |
|---|---|
| entt Entity Destroy | [github.com/skypjack/entt](https://github.com/skypjack/entt) — Registry::destroy() |

## Mentor-Hinweis

> Lösche Entities nie während du über sie iterierst — das korrumpiert den entt-Iterator. Sammle zu löschende Entities in einem `std::vector<entt::entity>` während der Iteration und rufe `registry.destroy()` danach auf. Das ist ein klassischer Anfängerfehler mit ECS.
