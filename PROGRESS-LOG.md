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

### 2026-07-18 — Verlaufszusammenfassung je Line auf der Übersicht (/threads)
- Befund vorab: Ein echtes „nextStep"-Feld existiert NIRGENDS (nicht im Prisma-Schema,
  nicht im Code) — die Annahme im letzten Log war ungenau; die Detail-Timeline ist nur
  eine chronologische Zuruf-Liste. Ein dediziertes GTD-nextStep-Feld wäre ein
  Prisma-Schema-Wechsel → laut ORCHESTRATOR Tabu im Alleingang, deshalb als Vorschlag
  unter „## Rückmeldungen" abgelegt (nicht gebaut).
- Gebaut: Das bereits vorhandene, aber bislang NIRGENDS angezeigte `summary`-Feld
  (die „Haiku-Verlaufszusammenfassung" aus dem ZIEL) je Line auf der Momentum-Übersicht
  sichtbar gemacht — einzeilig unter dem Titel, gekürzt mit Ellipsis + title-Tooltip,
  gesperrte Sekundärfarbe (#a1a1aa), flat (DESIGN-LOCK-konform). Damit ist auf einen
  Blick erfassbar, worum eine Line geht und wo sie steht, ohne sie zu öffnen. Kein
  Schema-/Backend-Eingriff nötig (Feld wird von /threads bereits geliefert). SW-Cache v8→v9.
- Geprüft: tsc sauber. Test-Deploy (frontend-test), Screenshot 1440 + 390 mit Sim-Daten
  (sim-team, sim-user-nadja). Zusammenfassungen rendern sauber unter jedem Titel, Momentum-
  Farbcodierung + „Liegengeblieben"-Badges intakt, Hierarchie Titel→Summary→Status stimmt.
  Mobil kürzt die Summary korrekt mit Ellipsis. Nur bekannte/ignorierbare 401/403-Fehler.
  Prod nachgezogen (frontend, app.aikydo.de/threads → HTTP 200).
  - Screenshot: .autopilot/shots/2026-07-18_threads-summary.png
  - Screenshot (Mobil): .autopilot/shots/2026-07-18_threads-summary-390.png
- Commit: c4294f15 feat(threads): Verlaufszusammenfassung je Line auf der Übersicht sichtbar
- Nächster Schritt: Entscheidung zum „nextStep"-Feld abwarten (siehe ORCHESTRATOR
  Rückmeldungen). Parallel dazu ORCHESTRATOR Punkt 3 möglich: Storyline-Timenline im
  Detail (aktuell newest-first, DESC) auf das ZIEL-Bild „links Start → rechts nächster
  Schritt" ausrichten (chronologisch aufsteigend) — reine Frontend-Lesbarkeit, kein Schema.

### 2026-07-18 — Storyline-Detail chronologisch (Start→Jetzt) mit Timeline-Schiene
- Gebaut: ORCHESTRATOR Punkt 3 umgesetzt. Die Timeline im Line-Detail
  (`/threads/[id]`) ist jetzt eine echte Storyline: chronologisch AUFSTEIGEND
  sortiert (vorher newest-first/DESC) — Start oben, nächster Schritt/„Jetzt" unten.
  Neu: flache vertikale Timeline-Schiene (2px Linie `#3d444d` + Dots je Schritt;
  frühere Dots hohl mit Rand `#4d555e`, der neueste gefüllt Accent `#5F74D1`).
  Statt nur Relativzeit steht jetzt das absolute Datum je Schritt (locale-aware,
  Jahr nur wenn abweichend). „Start"-/„Jetzt"-Badges ankern die Leserichtung.
  Section-Label „Aktivitäten"→„Verlauf" (de) / „Storyline" (en). Die schmale
  Detailspalte (max 800px, mobil 390) macht die vertikale Schiene zur getreuen
  Umsetzung der „Start→nächster Schritt"-Metapher (horizontaler Scroll für
  Text-je-Schritt wäre schlechter erfassbar). Nur gesperrte Palette, kein Glow
  (DESIGN-LOCK-konform, flat). i18n de+en. SW-Cache v9→v10.
- Geprüft: tsc sauber. Test-Deploy (frontend-test), Screenshot 1440 + 390 mit
  Sim-Daten (sim-team, Line „sim: Messestand Halle 7 beschaffen", 4 Zurufe).
  Die Storyline liest sich top-down: START 8.Juli → 12.Juli → 15.Juli → JETZT 17.Juli
  (Accent-Dot); der letzte Schritt zeigt sogar den GTD-Nächsten-Schritt („Naechster
  Schritt: Grafik fuer Standrueckwand …"). Schiene + Dots verbinden sauber, Mobil
  wrappt korrekt. Nur bekannte/ignorierbare 403-Fehler. Prod nachgezogen
  (frontend, app.aikydo.de/threads/sim-t-messe → HTTP 200).
  - Screenshot: .autopilot/shots/2026-07-18_storyline-detail.png
  - Screenshot (Mobil): .autopilot/shots/2026-07-18_storyline-detail-390.png
- Commit: (dieser Lauf) feat(threads): Storyline-Detail chronologisch (Start→Jetzt)
- Nächster Schritt: ORCHESTRATOR Punkt 1–3 sind damit adressiert (Farbcodierung,
  Summary auf Übersicht, Storyline-Leserichtung). Offen bleibt die Entscheidung zum
  dedizierten „nextStep"-Feld (ORCHESTRATOR Rückmeldungen). Ohne diese Entscheidung:
  Storyline weiter schärfen (z.B. Sprachnotiz-Wiedergabe je Schritt, falls Audio-URL
  am RawInput vorhanden) oder Momentum-/mindmap-Ansicht prüfen.

### 2026-07-18 — Zuruf-Medium je Storyline-Schritt (Icon + Label)
- Befund vorab: Es gibt KEIN Audio-URL-Feld am `RawInput` — Sprachnotizen werden zu
  Text transkribiert (`type: "voice"`, `content` = Transkript, `metadata` nur
  Confidence/Dauer). Echte Sprachnotiz-Wiedergabe ist ohne Audio-Storage nicht
  möglich (aus letztem Log als Option genannt → hiermit geklärt/verworfen).
- Gebaut: Jeder Storyline-Schritt zeigt statt des rohen Typ-Strings („voice · deepgram")
  jetzt ein flaches Icon + lokalisiertes Label je Medium — Sprachnotiz (Mikrofon),
  Foto (Kamera), E-Mail (Umschlag), Notiz/Chat (Sprechblase). Damit ist die Herkunft
  je Schritt auf einen Blick erfassbar (ZIEL: „Sprachnotiz/Text je Schritt"). Die
  technische `source` (deepgram/ocr) wandert in den title-Tooltip statt sichtbar zu
  stören. Reines Frontend, kein Schema/Backend-Eingriff. DESIGN-LOCK-konform: flach,
  gesperrte Palette, Icons in currentColor (Sekundärgrau). i18n de+en. SW-Cache v10→v11.
- Geprüft: tsc sauber. Test-Deploy (frontend-test), Screenshot 1440 + 390 mit Sim-Daten
  (sim-team, Line „sim: Messestand Halle 7 beschaffen"). Die Storyline liest sich
  top-down mit klaren Medien: START Notiz (8.7.) → E-Mail (12.7.) → Sprachnotiz (15.7.)
  → JETZT Notiz (17.7.). Icons subtil/flach, Schiene + START/JETZT-Badges intakt, Mobil
  wrappt sauber ohne Kollision. Nur bekannte/ignorierbare 401/403-Fehler. Prod nachgezogen
  (frontend, app.aikydo.de/threads/sim-t-messe → HTTP 200).
  - Screenshot: .autopilot/shots/2026-07-18_storyline-medium.png
  - Screenshot (Mobil): .autopilot/shots/2026-07-18_storyline-medium-390.png
- Commit: 2397bd48 feat(threads): Zuruf-Medium je Storyline-Schritt mit Icon + Label
- Nächster Schritt: Offen bleibt die Entscheidung zum dedizierten „nextStep"-Feld
  (ORCHESTRATOR Rückmeldungen). Das Storyline-Detail ist damit in Leserichtung,
  Zeitangabe UND Medium ausgereift. Nächste Kandidaten: Momentum-Übersicht (/momentum)
  bzw. /mindmap-Ansicht mit gleichem Maßstab schärfen, oder Eingang→Storyline-Fluss
  (Zuruf aus dem Pool rausfischen) auf schnelle Erfassung prüfen.

### 2026-07-18 — nextStep-Feld je Line (GTD-Nächster-Schritt) + Umstellung „nur noch Prod"
- Entscheidung: Michael hat das dedizierte `nextStep`-Feld freigegeben (war offene Rückmeldung).
- Gebaut: `nextStep String?` auf `Thread` (nullable, kein Default). Backend: update-thread.dto,
  thread-response.dto, threads.service (Update-Mapping); GET/PATCH liefern/setzen das Feld.
  Frontend Detail (/threads/[id]): editierbare Accent-Box „Nächster Schritt:" (Klick→Input,
  Enter speichert, Escape verwirft) nach dem blockedBy-Muster. Frontend Übersicht (/threads):
  „→ nächster Schritt" als eine Accent-Zeile (Ellipsis + Tooltip) unter der Meta-Zeile.
  i18n de+en (nextStepLabel/Placeholder/Short). SW-Cache v11→v12. DESIGN-LOCK-konform (Accent
  #5F74D1, flach, kein Glow).
- Migration `20260718_add_thread_nextstep`: zuerst auf Test verifiziert (kydo_test, Spalte
  text/nullable), dann Prod (kydo) via `prisma migrate deploy`. Beide grün.
- Geprüft: tsc backend+frontend sauber. Deploy NUR Prod (backend+frontend --no-cache).
  Abnahme mit Screenshot-Tool direkt auf app.aikydo.de (Prod-Tenant, echte Daten): PATCH
  über die Prod-API persistiert; Detail zeigt die Accent-Box (Desktop+Mobil 390 wrappt sauber);
  Übersicht zeigt die „→ nächster Schritt"-Zeile auf zwei Lines. Nur bekannte 401/403.
  - Screenshot (Detail): .autopilot/shots/2026-07-18_nextstep-detail.png
  - Screenshot (Detail Mobil): .autopilot/shots/2026-07-18_nextstep-detail-390.png
  - Screenshot (Liste full): .autopilot/shots/2026-07-18_nextstep-liste-full.png
- Arbeitsweise umgestellt: ab jetzt wird DIREKT auf Prod gebaut/geprüft/deployed; Test-Umgebung
  nicht mehr mitbespielt (Ausnahme: DB-Migration erst Test, dann Prod). Eingetragen in
  ORCHESTRATOR.md (aktuelle Richtung/Tabu) und PROMPT.md (harte Grenzen, Deploy-, Screenshot-Schritt).
- Hinweis: Als Abnahme-Beleg wurden zwei echte Lines mit einem nextStep befüllt
  („Steuererklärung 2025", „AiKydo MVP bauen") — jede Bearbeitung setzt lastActivity=jetzt,
  daher gelten sie nun als „läuft". Können in der UI jederzeit geleert werden.
- Nächster Schritt: nextStep optional auch aus dem Storyline-Detail/AI ableitbar machen
  (Vorschlag statt manuell). Sonst Momentum-/Mindmap-Ansicht mit gleichem Maßstab schärfen.

### 2026-07-18 — Line-Detailseite neu gedacht: die Seite IST die Storyline
- Richtung (Michael): Detailseite ist Alt-Mechanik, komplett neu denkbar. Pillen-Reihen
  (Status/Bereich) dominierten, obwohl sie Verwaltung sind. Ziel: sofort Verlauf + Stand +
  nächster Schritt sehen; Status/Bereich beiläufig; leere Lines mit sinnvollem Einstieg.
- Gebaut (nur Frontend, /threads/[id]):
  - Status/Bereich → **eine kompakte Meta-Zeile** unter dem Titel: farbiger Stand-Dot +
    Label · Bereich · „seit {Datum}" · „ändern". Die volle Verwaltung (Status-/Bereich-Chips)
    erscheint nur nach Klick auf „ändern" (einklappbar). Kein Dauer-Rauschen mehr.
  - **Nächster Schritt** als Hauptdarsteller: prominente Accent-Box mit Uppercase-Eyebrow
    „Nächster Schritt" + großem Wert (0.95rem), direkt unter der Meta-Zeile.
  - **Verlauf/Storyline** bleibt der Kern (bestehende Zeitschiene START→JETZT).
  - Leere Line: statt „Noch keine Aktivitäten" jetzt ein Einstiegs-Zustand „Die Storyline
    startet hier" + Erklärtext + Button **„Ersten Schritt festhalten"** (öffnet + scrollt zum
    nextStep-Feld).
  - i18n de+en (metaSince/metaEdit/metaDone/nextStepHeading/emptyStoryline*/captureFirstStep).
    SW-Cache v12→v13. DESIGN-LOCK-konform (gesperrte Palette, flach, kein Glow).
- Geprüft: tsc frontend sauber. Deploy NUR Prod (frontend --no-cache). Abnahme direkt auf
  app.aikydo.de (Prod-Tenant): (1) Line mit voller Demo-Storyline (Notiz→E-Mail→Sprachnotiz)
  zeigt Meta-Zeile + prominenten nextStep + Verlauf als Hauptdarsteller; (2) leere Line zeigt
  den neuen Einstiegs-Zustand; Mobil 390 wrappt sauber. Nur bekannte 401/403/404.
  - Screenshot (Storyline): .autopilot/shots/2026-07-18_detail-redesign-storyline.png
  - Screenshot (Storyline Mobil): .autopilot/shots/2026-07-18_detail-redesign-storyline-390.png
  - Screenshot (leere Line): .autopilot/shots/2026-07-18_detail-redesign-empty.png
- Demo-Daten auf Prod (löschbar): 3 Zurufe `demo-ri-1..3` auf „Steuererklärung 2025"
  (`DELETE FROM "RawInput" WHERE id LIKE 'demo-ri-%';`); nextStep auf zwei echten Lines gesetzt.
- Nächster Schritt: nextStep aus der Storyline/AI vorschlagen; ggf. „ändern"-Reveal auf einen
  echten Zustandswechsel (Statuswechsel direkt aus der Meta-Zeile) verschlanken.

### 2026-07-18 — „Stand" (Verlaufszusammenfassung) im Line-Detail sichtbar
- Befund: Die Haiku-Verlaufszusammenfassung (`summary`) war nur auf der /threads-Übersicht
  sichtbar, im Line-Detail NICHT — obwohl das ZIEL sie als Teil der Storyline führt („sofort
  sehen: den Verlauf, den Stand und den nächsten Schritt"). Der „Stand" fehlte also im Detail.
- Gebaut (nur Frontend, /threads/[id]): Eine beiläufige „Stand"-Zeile zwischen Meta-Zeile und
  nächstem Schritt — tertiärer Uppercase-Eyebrow „STAND" (de) / „WHERE IT STANDS" (en) + Summary
  in ruhiger Sekundärfarbe (#a1a1aa, 0.9rem). Nur wenn `summary` vorhanden. Leseordnung jetzt:
  Titel → Meta → Stand (Rückblick, ruhig) → Nächster Schritt (Accent-Hero) → Verlauf (Zeitschiene).
  Kein Schema-/Backend-Eingriff (Feld wird von GET /threads/:id bereits geliefert). i18n de+en
  (standHeading). SW-Cache v13→v14. DESIGN-LOCK-konform (flach, gesperrte Palette, kein Glow).
- Infra-Befund + Fix: Der Sim-Tenant `sim-team` existierte nur in `kydo_test`, NICHT auf Prod (kydo)
  — Screenshots auf Prod mit dem Sim-User schlugen daher mit „Tenant not found" fehl. Sim-Daten
  per `.autopilot/sim/seed.sql` jetzt auch in die Prod-DB geseedet (idempotent, Tenant `sim-team`,
  „sim:"-Prefix, per `.autopilot/sim/wipe.sql` restlos entfernbar). Damit funktioniert die
  PROMPT-Abnahme (Screenshot auf Prod mit Sim-Tenant) ab jetzt auch auf Prod.
- Geprüft: tsc frontend sauber. Deploy NUR Prod (frontend --no-cache). Abnahme direkt auf
  app.aikydo.de (Line „sim: Messestand Halle 7 beschaffen"): Die „STAND"-Zeile rendert ruhig
  zwischen Meta und nächstem Schritt, Summary „Standbau fuer die Herbstmesse; Auftrag vergeben.";
  Hierarchie + Storyline-Schiene intakt; Mobil 390 wrappt sauber. Nur bekannte 401/403.
  - Screenshot: .autopilot/shots/2026-07-18_stand-detail.png
  - Screenshot (Mobil): .autopilot/shots/2026-07-18_stand-detail-390.png
- Commit: (dieser Lauf) feat(threads): Stand-Zusammenfassung (Haiku) im Line-Detail sichtbar
- Nächster Schritt: Damit sind im Detail alle drei ZIEL-Elemente sichtbar (Verlauf, Stand,
  nächster Schritt). Offen: nextStep/Stand aus der Storyline per AI vorschlagen (Backend, Rate-Limit
  beachten) — sonst Momentum-/Mindmap-Ansicht bzw. Eingang→Storyline-Fluss mit gleichem Maßstab schärfen.
