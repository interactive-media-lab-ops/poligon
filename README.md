# Poligon · hackathon package

Decision trainer for personnel-support reform. Pitch edition v0.9.1, build 2026-11-05.
Cultural Forces (ГО Культурний Десант) · Максим Поляков · m.poliakov@culturalforces.org

## Contents

| File | What it is |
| --- | --- |
| `poligon-pitch.html` | The app. One file, no server. Open in Chrome or Edge. |
| `demo-final-state.json` | Board state at the end of the demo (72.8 % · 3/7). Open it with the «↑» button if the moves did not go through. |
| `SUBMISSION.md` | Project description for the hackathon form: problem, solution, model, evidence policy, roadmap, ready-to-paste short answers. |
| `DEMO-SCRIPT.ru.md` | 4-minute demo script for the presenter (Russian): preparation checklist, timing, exact lines, plan B, jury Q&A. |
| `screenshots/` | 1920×1080 captures of the key screens for the form and the deck. |

## Run

Double-click `poligon-pitch.html`. Allow pop-ups for the page if you want the second screen («display» on the left rail).
Standalone projector mode without an operator: open the file with `#display&p=now` appended to the address.

## Changes in v0.9.1 (2026-11-05)

- Keys 3–9 no longer throw an error on screens hidden in the pitch edition.
- Esc closes the crash-test journal from inside the text field.
- About 120 strings that still rendered in Ukrainian now render in English: move card, rule titles, block menus, link notes, toasts, tour, display window.
- Tested in headless Chromium: all variants, all decisions, every block menu and system rule, drag and drop, undo/redo, link round-trip, standalone display. Zero page errors.
