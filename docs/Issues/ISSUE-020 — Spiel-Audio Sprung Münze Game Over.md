---
created: 2026-05-21T23:37
Last update: 2026-05-21T23:37
tags:
  - Phase-1-8
  - Game
  - Audio
---
# ISSUE-020 — Spiel-Audio: Sprung, Münze, Game Over

## ID

ISSUE-020

## Typ

🙋 HITL — erfordert deinen aktiven Einsatz (Sounds beschaffen oder erstellen, Volume-Balance tunen)

## Phase

Phase 1.8 — Jump & Run Spiel

## What to build

Sound-Effekte für die wichtigsten Spielereignisse: Springen, Münze einsammeln, Game Over. Hintergrundmusik im Loop während `Playing`. Die Sounds werden über den `AudioManager` (ISSUE-010) abgespielt und sind an die Game-State-Übergänge gebunden.

## Acceptance criteria

- [ ] Sprung-Sound spielt beim Springen ab
- [ ] Münze-Sound spielt beim Einsammeln einer Münze ab
- [ ] Game-Over-Sound spielt beim Übergang zu `GameOver` ab
- [ ] Hintergrundmusik läuft in `Playing` im Loop und stoppt bei `GameOver` / `LevelComplete`
- [ ] Musik startet neu wenn ein neues Level geladen wird
- [ ] Volume ist ausgewogen — kein Sound überwältigt andere

## Blocked by

ISSUE-019 — GameStateManager
ISSUE-010 — Audio-System mit miniaudio

## Tasks

- [ ] **Sound-Assets beschaffen**
  - [ ] Sprung-Sound (WAV/MP3) — kurz, knackig
  - [ ] Münze-Sound (WAV/MP3) — helles "pling"
  - [ ] Game-Over-Sound (WAV/MP3) — kurz dramatisch
  - [ ] Hintergrundmusik (MP3/OGG) — loopbar, passt zum Jump & Run Stil
  - [ ] Freie Quellen: [freesound.org](https://freesound.org), [opengameart.org](https://opengameart.org)

- [ ] **Sounds via AssetManager vorladen**
  - [ ] Alle Sounds beim Level-Start via `AudioManager::loadSound()` laden
  - [ ] Handles in Game-Klasse oder eigenem `SoundLibrary`-Struct speichern

- [ ] **Sound-Auslöser einbauen**
  - [ ] `PlayerSystem`: Sprung ausgelöst → `audioManager.playSound(jumpSound)`
  - [ ] `CollisionSystem`: Münze eingesammelt → `audioManager.playSound(coinSound)`
  - [ ] `GameStateManager`: Übergang zu `GameOver` → `audioManager.playSound(gameOverSound)` + `audioManager.stopMusic()`
  - [ ] `GameStateManager`: Übergang zu `Playing` → `audioManager.playMusic(bgMusic, true)`

- [ ] **Volume-Balance**
  - [ ] Musik lauter als SFX (Hintergrund)
  - [ ] SFX klar hörbar aber nicht aufdringlich
  - [ ] Volume-Werte mit ImGui live anpassen

## Ressourcen

| Thema | Link |
|---|---|
| Freie Sounds | [freesound.org](https://freesound.org) |
| Freie Game Assets | [opengameart.org](https://opengameart.org) |
| miniaudio | [miniaud.io](https://miniaud.io) |

## Mentor-Hinweis

> Audio ist das Feature das am meisten unterschätzt wird — ein Spiel ohne Sound fühlt sich leer an, selbst wenn Grafik und Gameplay gut sind. Starte mit kostenlosen Assets von freesound.org und opengameart.org. Sound-Design kann man später verbessern; kein Sound ist fast immer schlechter als ein provisorischer Platzhalter.
