---
created: 2026-05-21T23:37
Last update: 2026-05-22T06:33
tags:
  - Setup
  - Phase-0
---
# ISSUE-000 — Projekt aufsetzen

## ID

ISSUE-001

## Typ

🙋 HITL — erfordert deinen aktiven Einsatz (Installation, Konfiguration, Entscheidungen)

## Phase

Phase 0 — Setup

## What to build

Obsidian als Projektmanagement und Dokumentationstool aufsetzten. 

Guthub als Zentrales Versionierungs und Sicherung aufsetzten.

## Acceptance criteria

- [x] Kanban Board mit ersten Issues
- [x] Inital Issues
- [x] Github Repo MeinErstesGame
- [x] README File angelgt inkl. Beschreibung
- [x] Ordnerstruktur (`src/`, `assets/`, `docs/`) liegt auf GitHub
- [x] Github Plugin in Obsidan installiert und getested

## Blocked by

Nichts — kann sofort gestartet werden.

## Tasks

### Phase 1 — Obsidian Setup
- [x] **Task 1:** Ordner `Projects/MeinErstesGame/` und `Issues/` im Obsidian Vault anlegen
- [x] **Task 2:** `PRD.md` erstellen — Abschnitte pro Feature-Bereich befüllen
- [x] **Task 3:** `Issues/ISSUE-01-Physik.md` erstellen (Akzeptanzkriterien + Tasks als Checkboxen)
- [x] **Task 4:** `Issues/ISSUE-02-Kollision.md` erstellen
- [x] **Task 5:** `Issues/ISSUE-03-Kamera.md` erstellen
- [x] **Task 6:** `Issues/ISSUE-04-Level.md` erstellen
- [x] **Task 7:** `Issues/ISSUE-05-Audio.md` erstellen
- [x] **Task 8:** `Issues/ISSUE-06-UI.md` erstellen
- [x] **Task 9:** `Kanban.md` erstellen — alle Issues als verlinkte Karten im Backlog einpflegen
- [x] **Task 10:** Im Obsidian Kanban Plugin prüfen, ob das Board korrekt gerendert wird

### Phase 2 — GitHub Setup
- [x] **Task 11:** Neues GitHub Repository `MeinErstesGame` erstellen (Public oder Private)
- [x] **Task 12:** `README.md` mit Kurzbeschreibung des Projekts anlegen
- [x] **Task 13:** `.gitignore` erstellen — `Kanban.md` und Obsidian-Systemdateien (`.obsidian/`) ausschliessen
- [x] **Task 14:** `docs/` Ordner im Repo anlegen und `PRD.md` + `Issues/` hineinkopieren
- [x] **Task 15:** Obsidian Git Plugin installieren und konfigurieren (Auto-Commit alle 30 Min. oder beim Schliessen)
- [x] **Task 16:** Obsidian Vault mit GitHub Repo verbinden (Remote URL setzen)
- [x] **Task 17:** Ersten Commit pushen und auf GitHub prüfen

## Ressourcen


## Mentor-Hinweis

> Diese Phase fühlt sich nicht nach "echtem" Programmieren an — und das ist völlig normal. Ein sauberes Setup jetzt spart dir Stunden an Debugging-Zeit in späteren Phasen. Besonders wichtig: Vulkan Validation Layers **von Anfang an aktiv** lassen. Sie sind dein wichtigstes Debugging-Werkzeug für alles, was danach kommt.
