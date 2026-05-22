---
created: 2026-05-21T23:37
Last update: 2026-05-21T23:37
tags:
  - Phase-1-1
  - CPP
---
# ISSUE-002 — Modernes C++ Grundlagen üben

## ID

ISSUE-002

## Typ

🙋 HITL — erfordert deinen aktiven Einsatz (Lesen, Üben, eigene Klassen schreiben)

## Phase

Phase 1.1 — Modernes C++ auffrischen

## What to build

Kein Produktionscode — sondern ein solides Fundament in modernem C++23, das für alle folgenden Phasen trägt. Du schreibst kleine Übungsprogramme: eine eigene RAII-Klasse mit Rule of Five, Move-Konstruktor, Smart Pointer. Das Ziel ist, dass du diese Konzepte ohne Nachschlagen anwenden kannst, wenn du in Phase 1.2 mit Vulkan-Ressourcen arbeitest.

## Acceptance criteria

- [ ] Du kannst RAII erklären und eine eigene RAII-Klasse schreiben (kein Copy, nur Move)
- [ ] Du hast Rule of Five vollständig implementiert: Destruktor, Copy-Konstruktor, Copy-Assignment, Move-Konstruktor, Move-Assignment
- [ ] Du kannst `unique_ptr` und `shared_ptr` situationsgerecht einsetzen
- [ ] Du verstehst `std::move` und wann es sinnvoll ist
- [ ] Du nutzt `auto`, `constexpr` und Range-based for sicher im Code
- [ ] *Effective Modern C++* Kapitel 1–4 (Items 1–23) gelesen

## Blocked by

ISSUE-001 — Entwicklungsumgebung aufsetzen

## Tasks

- [ ] **Lektüre**
  - [ ] *Effective Modern C++* — Kapitel 1: Deducing Types (Items 1–4)
  - [ ] *Effective Modern C++* — Kapitel 2: auto (Items 5–6)
  - [ ] *Effective Modern C++* — Kapitel 3: Moving to Modern C++ (Items 7–17)
  - [ ] *Effective Modern C++* — Kapitel 4: Smart Pointers (Items 18–22)

- [ ] **Übungsprogramme schreiben**
  - [ ] Eigene RAII-Klasse `ResourceHandle` implementieren (verwaltet eine Ressource, gibt sie im Destruktor frei)
  - [ ] Rule of Five vollständig implementieren und in `main.cpp` testen
  - [ ] `unique_ptr`-Beispiel: Ressource anlegen, in Funktion übergeben (move), kein raw delete
  - [ ] `shared_ptr`-Beispiel: Ressource zwischen zwei Objekten teilen
  - [ ] Move-Semantik demonstrieren: Performance-Unterschied zwischen Copy und Move messen

- [ ] **Referenz einrichten**
  - [ ] cppreference.com als Bookmark anlegen
  - [ ] CLion so einrichten, dass Hover-Dokumentation funktioniert

## Ressourcen

| Thema | Link |
|---|---|
| Pflichtlektüre | *Effective Modern C++* — Scott Meyers |
| C++ YouTube-Serie | [The Cherno — C++ Series](https://www.youtube.com/@TheCherno) |
| RAII & Smart Pointer | [The Cherno — Smart Pointers](https://www.youtube.com/watch?v=UOB7-B2MfwA) |
| Move Semantics | [The Cherno — Move Semantics](https://www.youtube.com/watch?v=ehMg6zvXuMY) |
| C++ Referenz | [cppreference.com](https://en.cppreference.com) |

## Mentor-Hinweis

> Vulkan verlangt präzises Ressourcen-Management — Objekte müssen in exakt der richtigen Reihenfolge erstellt und zerstört werden. Wer RAII und Smart Pointer jetzt verinnerlicht, wird in Phase 1.2 viel weniger kämpfen. Investiere hier eine Woche, nicht einen Tag.
