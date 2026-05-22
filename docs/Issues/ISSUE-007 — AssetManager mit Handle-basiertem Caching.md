---
created: 2026-05-21T23:37
Last update: 2026-05-21T23:37
tags:
  - Phase-1-5
  - Assets
---
# ISSUE-007 — AssetManager mit Handle-basiertem Caching

## ID

ISSUE-007

## Typ

🙋 HITL — erfordert deinen aktiven Einsatz (Caching-Strategie und Handle-Design durchdenken)

## Phase

Phase 1.5 — AssetManager

## What to build

Ein zentraler `AssetManager` der Texturen über Integer-Handles verwaltet. Wird die gleiche Textur zweimal geladen, liefert der Manager denselben Handle zurück — kein doppelter GPU-Upload. Die Engine referenziert Texturen fortan nur über Handles, nie über rohe Vulkan-Objekte. Zusätzlich wird `nlohmann/json` für das spätere Tilemap-Parsen eingebunden.

## Acceptance criteria

- [ ] `AssetManager::loadTexture("path/to/texture.png")` gibt einen `TextureHandle` (Integer) zurück
- [ ] Zweifaches Laden der gleichen Textur liefert denselben Handle — kein doppelter GPU-Upload
- [ ] `AssetManager::getTexture(handle)` liefert die Vulkan-Textur-Daten zurück
- [ ] `Sprite`-Komponente nutzt `TextureHandle` statt direktem Vulkan-Objekt
- [ ] `nlohmann/json` ist via vcpkg eingebunden und ein simples JSON-Parsen funktioniert

## Blocked by

ISSUE-006 — ECS Integration mit entt

## Tasks

- [ ] **`AssetManager`-Klasse anlegen**
  - [ ] `engine/assets/AssetManager.h` / `AssetManager.cpp`
  - [ ] Internes `std::unordered_map<std::string, TextureHandle>` für Pfad → Handle Mapping
  - [ ] Internes `std::vector<TextureData>` für Handle → Vulkan-Objekte Mapping

- [ ] **Handle-basiertes Laden implementieren**
  - [ ] `loadTexture(path)`: Prüfen ob Pfad bereits gecacht → falls ja, bestehenden Handle zurückgeben
  - [ ] Falls neu: Textur laden (stb_image + Vulkan Upload), Handle vergeben, in Cache eintragen
  - [ ] `getTexture(handle)`: Direkter Array-Zugriff, O(1)

- [ ] **`Sprite`-Komponente anpassen**
  - [ ] `TextureHandle` statt direktem Vulkan-Objekt in `Sprite`-Komponente
  - [ ] `RenderSystem` holt Textur-Daten via `AssetManager::getTexture()` beim Rendern

- [ ] **nlohmann/json einbinden**
  - [ ] `nlohmann-json` in `vcpkg.json` hinzufügen
  - [ ] Test: Kleines JSON-Objekt parsen und Felder ausgeben

- [ ] **Testen**
  - [ ] Gleiche Textur zweimal laden → gleicher Handle, kein doppelter Upload (via Breakpoint prüfen)
  - [ ] 3 verschiedene Texturen laden → 3 verschiedene Handles

## Ressourcen

| Thema | Link |
|---|---|
| nlohmann/json | [github.com/nlohmann/json](https://github.com/nlohmann/json) |
| stb_image | [github.com/nothings/stb](https://github.com/nothings/stb) |

## Mentor-Hinweis

> Der AssetManager ist eine der wenigen Engine-Klassen die als Singleton sinnvoll sein kann — oder als Referenz die durch die Engine durchgereicht wird. Entscheide dich für einen Ansatz und bleibe dabei. Das Handle-Muster ist mächtiger als es aussieht: Handles können serialisiert, gespeichert und über das Netzwerk übertragen werden — rohe Pointer nicht.
