# PROGRESS-LOG — Autopilot-Verlauf

Chronologisch, neueste Einträge **unten anhängen**. Je Lauf: Datum, was gebaut,
Screenshot-Pfad, wie geprüft, „nächster Schritt". Nicht wiederholen, was hier steht.

Format je Eintrag:
```
### YYYY-MM-DD — <kurzer Titel>
- Gebaut: …
- Geprüft: … (Screenshot: .autopilot/…)
- Commit: <hash / Message>
- Nächster Schritt: …
```

---

### 2026-07-17 — Autopilot-Setup (Baseline)
- Gebaut: Autopilot-Gerüst steht — `.autopilot/run.sh` (STOP-Schalter, Single-Run-Lock,
  Branch-Guard, 30-Min-Timeout), `.autopilot/PROMPT.md`, Sim-Daten-Skripte
  (`sim/sim-seed.sh`, `sim/sim-wipe.sh`), ORCHESTRATOR.md + dieser Log.
- Sim-Daten: in `kydo_test` geseedet (Tenant `sim-team`, 4 Personas, 8 Lines, ~19 Zurufe).
  Sim-User für Screenshots: `sim-user-nadja` / `nadja@sim.aikydo.local`.
- Baseline-Screenshots (Breite 1440, Test-Umgebung, Sim-Daten):
  - `.autopilot/baseline/eingang.png`   — Eingang, 5 offene Zurufe
  - `.autopilot/baseline/threads.png`   — Lines-Übersicht, 8 Storylines + Momentum-Scores
  - `.autopilot/baseline/thread-messe.png` — Storyline „Messestand Halle 7"
  - `.autopilot/baseline/mindmap.png`
  - `.autopilot/baseline/dashboard.png` — leitet für Sim-User auf /onboarding um (erwartbar)
- Geprüft: alle Views rendern echte Sim-Daten; nur die bekannten/ignorierbaren
  Fehler (401 new-users-count, 403 admin/stats) in der Konsole.
- Nächster Schritt: siehe ORCHESTRATOR.md „Aktuelle Richtung" — Momentum-Übersicht schärfen.

### 2026-07-17 — Momentum-Farbcodierung + Sortierung auf Lines-Übersicht (/threads)
- Gebaut: Liegengebliebene Lines stechen jetzt heraus. Momentum-Score aus letzter
  Aktivität (heute=+100 … 14T=-100), 3 Buckets: liegengeblieben (≥7T, rot), neutral
  (4–6T, grau), läuft (≤3T, grün). Umgesetzt als (a) Links-Akzent per inset box-shadow
  (überlebt Hover), (b) Label „Liegengeblieben"/„Läuft" neben dem Status, (c) Sortierung:
  liegengeblieben oben, innerhalb der Gruppe impact-stärkste zuerst, frischeste unten.
  Nur gesperrte Palette, kein Glow/Verlauf (DESIGN-LOCK-konform, flat). i18n de+en.
  SW-Cache v7→v8.
- Geprüft: Test-Deploy (frontend-test), Screenshot 1440 + 390 mit Sim-Daten (sim-team,
  sim-user-nadja). Ergebnis stimmt: 4 rote „Liegengeblieben"-Lines oben, ruhige neutrale
  Mitte, grüne „Läuft"-Lines unten — in <2 Sek. erfassbar. Nur bekannte/ignorierbare
  401/403-Konsolenfehler. Prod nachgezogen (frontend, app.aikydo.de/threads → HTTP 200).
  - Screenshot: .autopilot/shots/2026-07-17_threads-momentum.png
  - Screenshot (Mobil): .autopilot/shots/2026-07-17_threads-momentum-390.png
- Commit: e50abf3d feat(threads): Momentum-Farbcodierung + Sortierung auf Lines-Übersicht
- Nächster Schritt: ORCHESTRATOR-Richtung Punkt 2 — der nächste kleine Schritt je Line
  (GTD-Geist) auf der Übersicht sichtbar machen. Dafür braucht die /threads-Liste ein
  „nextStep"-Feld pro Line (aktuell nur in der Timeline/Detailansicht vorhanden) — vor
  Backend-Erweiterung prüfen, ob ein Feld schon existiert oder abgeleitet werden kann.
