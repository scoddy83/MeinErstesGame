---
created: 2026-05-21T23:37
Last update: 2026-05-21T23:37
tags:
  - Phase-1-8
  - Game
  - Physik
  - GameFeel
---
# ISSUE-015 — Coyote Time + Jump Buffering

## ID

ISSUE-015

## Typ

🙋 HITL — erfordert deinen aktiven Einsatz (Timer-Werte tunen bis es sich "richtig" anfühlt)

## Phase

Phase 1.8 — Jump & Run Spiel

## What to build

Zwei kleine Features die den größten Unterschied zwischen einem trägen und einem responsiven Plattformer machen. Coyote Time: Der Spieler darf noch kurz nach dem Verlassen einer Plattformkante springen. Jump Buffering: Ein Sprung-Input kurz vor dem Landen wird gepuffert und beim Aufkommen automatisch ausgeführt. Beide Timer sind bereits in `PhysicsBody` definiert (ISSUE-013) — hier werden sie aktiviert.

## Acceptance criteria

- [ ] Spieler kann noch ca. 100–150 ms nach dem Verlassen einer Plattform springen (Coyote Time)
- [ ] Sprung-Input wird ca. 100–150 ms vor dem Landen gepuffert und beim Aufkommen ausgeführt
- [ ] Beide Features fühlen sich natürlich an — der Spieler merkt sie nicht, aber vermisst sie wenn sie fehlen
- [ ] Kein "doppelter Sprung" durch Coyote Time (nur wenn Spieler von Plattform läuft, nicht nach Sprung)

## Blocked by

ISSUE-014 — AABB-Kollision mit Tilemap

## Tasks

- [ ] **Coyote Time implementieren**
  - [ ] `PhysicsBody.coyoteTimer` starten wenn `isGrounded` von `true` auf `false` wechselt (und kein Sprung ausgelöst wurde)
  - [ ] Timer läuft herunter: `coyoteTimer -= dt`
  - [ ] Sprung erlaubt wenn: `isGrounded == true` ODER `coyoteTimer > 0`
  - [ ] Nach Sprung: `coyoteTimer = 0` setzen (kein zweites Mal nutzen)

- [ ] **Jump Buffering implementieren**
  - [ ] `PhysicsBody.jumpBufferTimer` setzen wenn Sprung-Input erkannt wird: `jumpBufferTimer = jumpBufferWindow` (z. B. 0.12f)
  - [ ] Timer läuft herunter: `jumpBufferTimer -= dt`
  - [ ] Bei Landung (`isGrounded` wird `true`): wenn `jumpBufferTimer > 0` → Sprung sofort ausführen, Timer zurücksetzen

- [ ] **Timer-Werte tunen**
  - [ ] Coyote Time Window: mit ImGui zwischen 0.05f und 0.20f testen
  - [ ] Jump Buffer Window: mit ImGui zwischen 0.05f und 0.20f testen
  - [ ] Zielwert: ~0.10f–0.15f für beide (je nach Spielgefühl)

- [ ] **Testen**
  - [ ] Bewusst knapp an Plattformkante laufen → Sprung funktioniert noch
  - [ ] Sprung-Taste kurz vor Landung drücken → Spieler springt sofort beim Aufkommen

## Ressourcen

| Thema | Link |
|---|---|
| Coyote Time Erklärung | [gamedev.stackexchange.com — Coyote Time](https://gamedev.stackexchange.com/questions/108660/implementing-coyote-time-in-a-platformer) |
| Jump Buffering | [@MaddyThorson auf Twitter/X — Celeste Mechanics](https://twitter.com/MaddyThorson) |

## Mentor-Hinweis

> Diese zwei Features machen den Unterschied zwischen einem Spiel das sich "billig" anfühlt und einem das sich "polished" anfühlt. Celeste — eines der besten Plattformer ever — hat sehr großzügige Werte für beide. Teste dein Spiel zuerst ohne, dann mit — du wirst den Unterschied sofort spüren.
