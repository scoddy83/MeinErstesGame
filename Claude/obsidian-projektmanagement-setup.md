---
created: 2026-05-21T22:59
Last update: 2026-05-21T23:45
---
# Obsidian Projekt-Management Setup — Mein erstes 2D Game

## Entscheidungen (Zusammenfassung)

| Thema | Entscheidung |
|---|---|
| Tool | Obsidian (Kanban Plugin bereits installiert) |
| Scope | Solo-Projekt, kein Kollaborations-Overhead |
| Board | Ein einziges Kanban Board für das gesamte Projekt |
| Spalten | Backlog → In Progress → Testing → Done |
| Hierarchie | PRD.md → Issues als Karten → Tasks als Checkboxen |
| Dokumentation | Eine zentrale `PRD.md` |
| Ordnerstruktur | `Projects/MeinErstesGame/` im Obsidian Vault |
| GitHub | Ein Repo für Spielcode + `docs/` Ordner für PRD & Issues |
| Sync | Obsidian Git Plugin (automatischer Push) |

---

## Ziel-Ordnerstruktur im Obsidian Vault

```
Projects/MeinErstesGame/
├── PRD.md                        ← Zentrale Anforderungen, gegliedert nach Feature-Bereichen
├── Kanban.md                     ← Das Board (Obsidian Kanban Plugin, nur lokal)
└── Issues/
    ├── ISSUE-01-Physik.md
    ├── ISSUE-02-Kollision.md
    ├── ISSUE-03-Kamera.md
    └── ...                       ← Je eine Datei pro Issue/Feature
```

## Ziel-Struktur auf GitHub

```
MeinErstesGame/                   ← GitHub Repo Root
├── src/                          ← Spielcode (Godot/Unity/etc.)
├── assets/                       ← Grafiken, Sounds, Tilemaps
├── docs/
│   ├── PRD.md                    ← Gespiegelt aus Obsidian Vault
│   └── Issues/
│       ├── ISSUE-01-Physik.md
│       ├── ISSUE-02-Kollision.md
│       └── ...
└── README.md                     ← Kurze Projektbeschreibung
```

> **Hinweis:** `Kanban.md` wird nicht auf GitHub gepusht — sie enthält Obsidian-spezifische Syntax und ist nur lokal sinnvoll. Füge sie zur `.gitignore` hinzu.

---

## Datei-Formate

### PRD.md — Struktur
```markdown
# PRD — Mein erstes 2D Jump & Run

## Überblick
Kurze Beschreibung des Spiels, Ziel-Plattform, Scope (3–5 Level).

## Feature-Bereiche
### Physik & Bewegung
- Gravitation, Sprung, Coyote Time, Jump Buffering

### Kollision
- AABB-Tile-Kollision, Plattform-Erkennung

### Kamera
- Scrollende Kamera, Player-Tracking

### Level-Design
- Tilemap-System, 3–5 Level

### Audio
- Sprung-Sound, Musik, Game Over

### UI & HUD
- Leben, Score, Menü
```

### Issues/ISSUE-XX-Feature.md — Struktur
```markdown
# ISSUE-01: Physik & Bewegung

**Bereich:** Engine / Gameplay  
**Priorität:** Hoch  
**Verknüpft mit:** [[PRD#Physik & Bewegung]]

## Beschreibung
Implementierung der Grundbewegung des Spielers inkl. Gravitation und Sprung.

## Akzeptanzkriterien
- [ ] Spieler fällt durch Gravitation nach unten
- [ ] Spieler kann springen (einmaliger Sprung)
- [ ] Coyote Time: Sprung noch kurz nach Abgrund möglich
- [ ] Jump Buffering: Sprung-Input kurz vor Landung wird gepuffert
- [ ] Bewegung links/rechts mit konfigurierbarer Geschwindigkeit

## Tasks
- [ ] Gravitations-Variable in PlayerController definieren
- [ ] Sprung-Kraft implementieren
- [ ] Coyote Time Timer einbauen (ca. 0.1–0.15s)
- [ ] Jump Buffer Timer einbauen (ca. 0.1s)
- [ ] Horizontale Bewegung mit Beschleunigung/Verzögerung

## Notizen
_Hier Learnings, Links zu Ressourcen oder Code-Snippets eintragen._
```

### Kanban.md — Struktur
```markdown
---
kanban-plugin: basic
---

## Backlog

- [ ] [[Issues/ISSUE-01-Physik|ISSUE-01: Physik & Bewegung]]
- [ ] [[Issues/ISSUE-02-Kollision|ISSUE-02: Kollision]]
- [ ] [[Issues/ISSUE-03-Kamera|ISSUE-03: Kamera]]
- [ ] [[Issues/ISSUE-04-Level|ISSUE-04: Level-Design]]
- [ ] [[Issues/ISSUE-05-Audio|ISSUE-05: Audio]]
- [ ] [[Issues/ISSUE-06-UI|ISSUE-06: UI & HUD]]

## In Progress

## Testing

## Done
```

---

## Setup-Tasks für Claude

Diesen Abschnitt kannst du direkt an Claude übergeben, um das Setup durchzuführen.

### Prompt für Claude:
> Ich möchte mein Obsidian Projekt-Management für mein 2D Jump & Run Spiel aufsetzen.
> Bitte erstelle folgende Dateien in meinem Obsidian Vault unter `[PFAD ZUM VAULT]/Projects/MeinErstesGame/`:
>
> 1. `PRD.md` — mit Abschnitten für: Physik & Bewegung, Kollision, Kamera, Level-Design, Audio, UI & HUD
> 2. `Kanban.md` — Obsidian Kanban Board mit 4 Spalten: Backlog, In Progress, Testing, Done. Issues als verlinkte Karten im Backlog.
> 3. `Issues/ISSUE-01-Physik.md` bis `Issues/ISSUE-06-UI.md` — je eine Datei pro Feature-Bereich mit Beschreibung, Akzeptanzkriterien und Tasks als Checkboxen.
>
> Vorlage und Struktur: siehe `obsidian-projektmanagement-setup.md` im Game-Projektordner.

---

## Einzelne Setup-Tasks (Schritt für Schritt)

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
- [ ] **Task 11:** Neues GitHub Repository `MeinErstesGame` erstellen (Public oder Private)
- [ ] **Task 12:** `README.md` mit Kurzbeschreibung des Projekts anlegen
- [ ] **Task 13:** `.gitignore` erstellen — `Kanban.md` und Obsidian-Systemdateien (`.obsidian/`) ausschliessen
- [ ] **Task 14:** `docs/` Ordner im Repo anlegen und `PRD.md` + `Issues/` hineinkopieren
- [ ] **Task 15:** Obsidian Git Plugin installieren und konfigurieren (Auto-Commit alle 30 Min. oder beim Schliessen)
- [ ] **Task 16:** Obsidian Vault mit GitHub Repo verbinden (Remote URL setzen)
- [ ] **Task 17:** Ersten Commit pushen und auf GitHub prüfen

---

## GitHub `.gitignore` Vorlage

```gitignore
# Obsidian lokale Dateien (nicht auf GitHub)
.obsidian/workspace
.obsidian/workspace.json
.obsidian/plugins/obsidian-git/

# Kanban Board (Obsidian-spezifisch, nicht auf GitHub sinnvoll)
**/Kanban.md

# Betriebssystem
.DS_Store
Thumbs.db
```

---

## Obsidian Git Plugin — Empfohlene Einstellungen

| Einstellung | Wert |
|---|---|
| Auto-Commit Intervall | 30 Minuten |
| Auto-Push | Ein |
| Commit Message | `vault backup: {{date}}` |
| Pull beim Start | Ein |

---

## Workflow-Regeln (für den Alltag)

- **WIP-Limit:** Maximal 2–3 Karten gleichzeitig in "In Progress"
- **Testing:** Eine Karte erst auf "Done" ziehen, wenn das Feature **im laufenden Spiel** getestet wurde
- **Neue Issues:** Immer zuerst als `ISSUE-XX.md` anlegen, dann als Karte ins Backlog
- **PRD-Update:** Wenn sich Anforderungen ändern, zuerst `PRD.md` anpassen, dann das Issue aktualisieren
- **GitHub:** `docs/` wird automatisch via Obsidian Git synchronisiert — kein manuelles Kopieren nötig
