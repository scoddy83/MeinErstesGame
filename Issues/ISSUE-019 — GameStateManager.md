---
created: 2026-05-21T23:37
Last update: 2026-05-21T23:37
tags:
  - Phase-1-8
  - Game
  - GameState
---
# ISSUE-019 — GameStateManager

## ID

ISSUE-019

## Typ

🙋 HITL — erfordert deinen aktiven Einsatz (State-Übergänge durchdenken, Menü gestalten)

## Phase

Phase 1.8 — Jump & Run Spiel

## What to build

Ein `GameStateManager` mit Enum-basierten Zuständen: `MainMenu`, `Playing`, `GameOver`, `LevelComplete`. Jeder Zustand hat ein eigenes Rendering und eigene Input-Verarbeitung. Das Hauptmenü erlaubt das Starten des Spiels. Game Over zeigt den Score und bietet Neustart. Level Complete lädt das nächste Level.

## Acceptance criteria

- [ ] Spiel startet im `MainMenu`-Zustand
- [ ] `MainMenu`: "Start" wechselt zu `Playing`
- [ ] `Playing`: Spieler fällt in Lücke → `GameOver`
- [ ] `Playing`: Spieler erreicht Ziel → `LevelComplete`
- [ ] `GameOver`: Score anzeigen, Neustart (zurück zu `MainMenu` oder direkt zu `Playing`)
- [ ] `LevelComplete`: nächstes Level laden (oder Ende-Bildschirm nach letztem Level)
- [ ] Level-Wechsel lädt neue Tilemap und setzt Spieler-Position zurück

## Blocked by

ISSUE-018 — Collectibles + Score-System

## Tasks

- [ ] **`GameState`-Enum definieren**
  - [ ] `enum class GameState { MainMenu, Playing, GameOver, LevelComplete }`
  - [ ] `GameStateManager`-Klasse oder einfache Variable in `Application`

- [ ] **State-Rendering**
  - [ ] `MainMenu`: Titel-Text + "Press Enter to Start" (via ImGui oder Sprite)
  - [ ] `Playing`: normales Spiel-Rendering
  - [ ] `GameOver`: "Game Over" + Score + "Press R to Restart"
  - [ ] `LevelComplete`: "Level Complete!" + Score + kurze Pause, dann nächstes Level

- [ ] **State-Übergänge**
  - [ ] `MainMenu` → `Playing`: Enter-Taste, Level 1 laden
  - [ ] `Playing` → `GameOver`: Spieler-Y-Position unter Level-Bounds (in Lücke gefallen)
  - [ ] `Playing` → `LevelComplete`: Spieler-AABB überschneidet `LevelGoal`-Entity
  - [ ] `GameOver` → `Playing`: R-Taste, Level neu laden, Score zurücksetzen
  - [ ] `LevelComplete` → `Playing`: Automatisch nach 2 Sekunden, nächstes Level laden

- [ ] **Level-Wechsel**
  - [ ] Level-Index (`currentLevel`) tracken
  - [ ] `loadLevel(index)`: Alle Level-Entities löschen, neue Tilemap laden, Spieler-Spawn setzen
  - [ ] Nach letztem Level: Ende-Bildschirm statt Absturz

- [ ] **Todeszone definieren**
  - [ ] Wenn `player.position.y < deathY` (unter dem Level) → GameOver auslösen

## Ressourcen

| Thema | Link |
|---|---|
| Game State Pattern | [gameprogrammingpatterns.com/state.html](https://gameprogrammingpatterns.com/state.html) |

## Mentor-Hinweis

> Fang mit dem einfachsten State-System an: ein Enum und eine Switch-Anweisung. Das reicht vollständig für 3 Level. Das State-Pattern aus Game Programming Patterns ist interessant als nächster Schritt — aber über-engineering hier kostet mehr Zeit als es spart.
