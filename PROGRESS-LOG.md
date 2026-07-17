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
