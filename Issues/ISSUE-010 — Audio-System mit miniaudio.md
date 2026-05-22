---
created: 2026-05-21T23:37
Last update: 2026-05-21T23:37
tags:
  - Phase-1-6
  - Audio
---
# ISSUE-010 — Audio-System mit miniaudio

## ID

ISSUE-010

## Typ

🙋 HITL — erfordert deinen aktiven Einsatz (miniaudio API kennenlernen, AudioManager API entwerfen)

## Phase

Phase 1.6 — miniaudio

## What to build

Einen `AudioManager` der Sound-Effekte und Hintergrundmusik über `miniaudio` abspielt. miniaudio wird als Single-Header direkt eingebunden — kein vcpkg nötig. Die API ist simpel: `playSound(handle)`, `playMusic(handle, loop)`, `stopMusic()`. Ein Sound-Effekt und Hintergrundmusik im Loop laufen am Ende dieses Issues.

## Acceptance criteria

- [ ] `miniaudio.h` ist als Single-Header in `third_party/` eingebunden
- [ ] `AudioManager` kann WAV oder MP3 Sound-Effekte laden und abspielen
- [ ] `AudioManager` kann Hintergrundmusik im Loop abspielen und stoppen
- [ ] Ein Test-Sound wird beim Drücken einer Taste abgespielt
- [ ] Hintergrundmusik läuft beim App-Start automatisch in Loop

## Blocked by

ISSUE-009 — Fixed Timestep Game Loop

## Tasks

- [ ] **miniaudio einbinden**
  - [ ] `miniaudio.h` von [github.com/mackron/miniaudio](https://github.com/mackron/miniaudio) herunterladen
  - [ ] In `third_party/miniaudio/` ablegen
  - [ ] `#define MINIAUDIO_IMPLEMENTATION` in genau einer `.cpp`-Datei

- [ ] **`AudioManager`-Klasse anlegen**
  - [ ] `engine/audio/AudioManager.h` / `AudioManager.cpp`
  - [ ] `ma_engine` initialisieren und beim Shutdown sauber freigeben (RAII)
  - [ ] `loadSound(path)` → gibt `SoundHandle` zurück
  - [ ] `playSound(handle)` → spielt den Sound einmalig ab
  - [ ] `loadMusic(path)` → gibt `MusicHandle` zurück
  - [ ] `playMusic(handle, loop)` → spielt Musik ab (geloopt oder einmalig)
  - [ ] `stopMusic()` → stoppt die laufende Musik

- [ ] **Test-Integration**
  - [ ] Test-Sound (WAV) beim Drücken von `Space` abspielen
  - [ ] Hintergrundmusik beim App-Start starten
  - [ ] App-Shutdown: Kein Crash, alle ma_-Objekte korrekt freigegeben

## Ressourcen

| Thema | Link |
|---|---|
| miniaudio Dokumentation | [miniaud.io](https://miniaud.io) |
| miniaudio Repository | [github.com/mackron/miniaudio](https://github.com/mackron/miniaudio) |

## Mentor-Hinweis

> miniaudio ist eine der angenehmsten Audio-Bibliotheken die es gibt — Single-Header, kein Build-System, funktioniert auf Windows/Mac/Linux. Die `ma_engine` High-Level-API reicht für Phase 1 vollständig aus. Für Phase 2 (FMOD) hast du dann einen Vergleichspunkt.
