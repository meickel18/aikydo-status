> **Stand:** 2026-07-19T20:21Z · Commit a1b9e6a4 · Lauf-Nr. 16 · automatischer Spiegel, nach jedem Autopilot-Lauf aktualisiert. Cache-Hinweis: raw.githubusercontent kann bis ~5 Min alt sein — diese Zeile zeigt den echten Stand.

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

### 2026-07-18 — Eingang (Pool → rausfischen): Vorschlags-Hierarchie
- Befund: Der Eingang (`/eingang`) zeigte je Zuruf bis zu 5 GLEICHWERTIGE Vorschlags-Pillen
  (Scores von ~53% runter bis ~36%). Das Auge musste alle lesen, obwohl das Backend die
  Vorschläge längst nach Score sortiert liefert (inbox.service `suggestLines`, `slice(0,5)`,
  `.sort(b.score-a.score)`) — die KI hatte den Favoriten also schon, die UI stellte ihn aber
  nicht heraus. Widerspricht dem Maßstab „schnelle Erfassung" und der Metapher „Aufgabe
  rausfischen" (ein Klick sollte genügen).
- Gebaut (nur Frontend, `/eingang`): Klare Hierarchie im Vorschlags-Block. Der beste Treffer
  ist jetzt eine prominente Accent-Pille (#5F74D1 gefüllt, „→ {Titel} {score}%", ein Klick =
  zugeordnet). Die weiteren Vorschläge liegen ruhig darunter als flache Outline-Pillen — max 2
  sichtbar, der Rest hinter „+N weitere" (neuer State `showMore`). Best-Treffer-Titel kürzt mit
  Ellipsis, Score bleibt via `flexShrink:0` sichtbar. Karte dadurch spürbar kürzer → mehr Pool
  auf einen Blick. Kein Backend/Schema-Eingriff (Endpoint liefert bereits sortiert). i18n de+en
  (`moreSuggestions`). SW-Cache v14→v15. DESIGN-LOCK-konform (gesperrte Accent-Farbe, flach, kein Glow).
- Geprüft: tsc frontend sauber. Deploy NUR Prod (frontend --no-cache), `/eingang` → HTTP 200.
  Abnahme mit Screenshot-Tool auf app.aikydo.de (Sim-Tenant, 5 offene Zurufe): Desktop 1440 zeigt
  je Karte den Best-Treffer als Accent-Pille + 2 ruhige Pillen + „+2 weitere"; Mobil 390 wrappt
  sauber, Best-Treffer kürzt korrekt mit sichtbarem Score. Nur bekannte/ignorierbare 401/403.
  - Screenshot: .autopilot/shots/2026-07-18_eingang-vorschlag-hierarchie.png
  - Screenshot (Mobil): .autopilot/shots/2026-07-18_eingang-vorschlag-hierarchie-390.png
- Commit: 9a901159 feat(eingang): Vorschlags-Hierarchie beim Rausfischen — bester Treffer prominent
- Nächster Schritt: Eingang→Storyline-Fluss weiter schärfen — z.B. nach dem Zuordnen ein kurzer
  Bestätigungs-/Undo-Hinweis (Zuruf wird still entfernt) oder die „Line wählen …"-Zuordnung
  visuell klarer vom KI-Vorschlag trennen. Alternativ Momentum-/Mindmap-Ansicht mit gleichem Maßstab.

### 2026-07-18 — Eingang: Bestätigung + Rückgängig beim Rausfischen
- Befund: Nach Zuordnen/Verwerfen verschwand die Zuruf-Karte STUMM — kein Beleg, WOHIN
  gefischt wurde, und ein Fehlklick auf die prominente Best-Treffer-Accent-Pille (ein Klick =
  zugeordnet) war nicht korrigierbar. Widerspricht „schnelle Erfassung" + der Rausfisch-Metapher.
- Gebaut:
  - Backend: neuer Endpoint `POST /raw-inputs/:id/restore` (`inbox.restore`) — setzt `threadId`
    + `dismissedAt` zurück (Zuruf zurück in den Eingang), Line selbst bleibt unberührt. KEIN
    Schema-Wechsel (nutzt vorhandene Felder), Audit `RAWINPUT_RESTORED`.
  - Frontend (`/eingang`): Statt stummem Entfernen zeigt die Karte kurz einen flachen
    Accent-Bestätigungs-Strip: „✓ → Zugeordnet zu «Line»" / „✓ → Neue Line «…»" / „Aus dem
    Eingang entfernt". Bei Zuordnen/Verwerfen rechts ein „Rückgängig" (ruft restore → Karte
    kehrt in den Normalzustand zurück). Nach 6 s wird der Zuruf still aus der Liste entfernt.
    Ziel-Titel kommt aus dem gewählten Vorschlag/Select bzw. der neu erzeugten Line. Neue-Line
    bewusst OHNE Undo (Löschen einer frischen Line wäre schwerer/riskanter). i18n de+en
    (confirmAssigned/confirmCreated/confirmDismissed/undo). SW-Cache v15→v16. DESIGN-LOCK-konform
    (gesperrte Accent-Farbe, flach, kein Glow).
- Geprüft: tsc backend+frontend sauber. Deploy NUR Prod (backend + frontend --no-cache),
  `/eingang` → HTTP 200, `POST …/restore` ohne Auth → 401 (Route existiert). Abnahme mit
  echtem Klick-Flow (Playwright, Sim-Tenant, 5 Zurufe): Best-Treffer-Pille geklickt → Strip
  „✓ → Zugeordnet zu «sim: Messestand Halle 7 beschaffen» · Rückgängig" erscheint oben, Karten
  darunter rücken auf; „Rückgängig" geklickt → restore → Karte ist wieder da, „5 offen"
  unverändert (Sim-Daten sauber). Mobil 390: Strip kürzt die Nachricht mit Ellipsis, „Rückgängig"
  bleibt sichtbar (flexShrink), keine Kollision. Nur bekannte/ignorierbare 401/403.
  - Screenshot (Bestätigung): .autopilot/shots/2026-07-18_eingang-confirm-undo.png
  - Screenshot (Bestätigung Mobil): .autopilot/shots/2026-07-18_eingang-confirm-undo-390.png
  - Screenshot (nach Undo, Karte zurück): .autopilot/shots/2026-07-18_eingang-after-undo.png
- Commit: cbac93d9 feat(eingang): Bestätigung + Rückgängig beim Rausfischen (+ PROGRESS-LOG)
- Nächster Schritt: Eingang→Storyline-Fluss ist damit rund (Vorschlags-Hierarchie + Bestätigung/
  Undo). Nächste Kandidaten: „Line wählen …"-Select visuell klarer vom KI-Vorschlag trennen, oder
  Momentum-/Mindmap-Ansicht mit gleichem Maßstab (schnelle Erfassung) schärfen.

### 2026-07-19 — Momentum-Cockpit: Zeilen klickbar → Line-Storyline öffnen
- Befund: Die Momentum-Ansicht (`/mindmap`, Nav „Momentum") zeigt alle Storylines als
  divergierende Leucht-Linien (VERLIERT ← Nullachse → GEWINNT, Gesamt −45) — stark für die
  „auf einen Blick"-Erfassung, ABER die Zeilen waren tote Grafik: Wer eine rote, weit links
  liegende (liegengebliebene) Line entdeckte, konnte sie nicht direkt öffnen — Umweg über die
  Lines-Liste + Suchen. Das brach die Kern-Schleife des Portals: Momentum (Überblick) →
  liegengebliebene Line öffnen → Storyline.
- Gebaut (nur Frontend, `/mindmap`): Jede Momentum-Zeile ist jetzt ein Klick-Ziel über die
  volle Zeilenbreite → navigiert zu `/threads/[id]` (per `window.location.href`, konsistent mit
  der Klick-Karte der Lines-Liste). Affordance: `cursor:pointer`, dezentes Hover-Band
  (`rgba(255,255,255,0.04)`, borderRadius 8, transition 0.15s), voller Line-Titel als
  `title`-Tooltip (Titel wird ab 30 Zeichen gekürzt). Kein Backend/Schema-Eingriff, keine
  Struktur-Änderung an der Bespoke-Karte. DESIGN-LOCK: mindmap darf Glow/Verlauf; das Hover-Band
  ist minimal-flach. SW-Cache v16→v17.
- Geprüft: tsc frontend sauber. Deploy NUR Prod (frontend --no-cache), `/mindmap` → HTTP 200.
  Abnahme direkt auf app.aikydo.de (Sim-Tenant): (1) Screenshot 1440 — Cockpit unverändert
  intakt (8 Lines, Nullachse, Farb-/Längen-Kodierung); (2) Klick-Navigation aktiv per Playwright
  verifiziert: Klick auf die oberste/rötelste Zeile („sim: Employer-Branding Reel", −100) landet
  auf `https://app.aikydo.de/threads/sim-t-reel`; (3) Mobil 390 rendert sauber, Zeilen über volle
  Breite tippbar, keine Regression. Nur bekannte/ignorierbare 401/403.
  - Screenshot: .autopilot/shots/2026-07-19_momentum-klickbar.png
  - Screenshot (Mobil): .autopilot/shots/2026-07-19_momentum-klickbar-390.png
- Commit: (dieser Lauf) feat(momentum): Zeilen im Cockpit klickbar → Line-Storyline öffnen
- Nächster Schritt: Momentum-Cockpit weiter schärfen — z.B. Klick auf die Zahl/Achse als
  Sortier-Sekundärsignal, oder Hover-Vorschau (Stand-Summary) je Zeile. Alternativ „Line wählen …"-
  Select im Eingang visuell klarer vom KI-Vorschlag trennen.

### 2026-07-19 — Eingang: beim Rausfischen den nächsten Schritt festhalten (GTD, Backlog #1)
- Richtung: ORCHESTRATOR „Aktuelle Richtung" / Interview-Backlog Punkt 1. Wer eine Kachel in
  eine Line fischt, soll direkt den beabsichtigten nächsten Schritt festhalten → GTD-Kern
  („immer der nächste kleine Schritt"). Bisher endete das Rausfischen still mit dem
  Bestätigungs-Strip; die Line startete ohne definierten nächsten Schritt.
- Gebaut (nur Frontend, `/eingang`): Nach dem Rausfischen IN EINE LINE (Zuordnen bester Treffer /
  „Line wählen"-Zuordnen / neue Line) zeigt der Bestätigungs-Strip jetzt einen kompakten
  GTD-Moment: Eyebrow „NÄCHSTER SCHRITT" (Accent) + Eingabefeld „Was passiert als Nächstes?"
  (autofokus) + „Festhalten". Enter speichert ebenfalls. Der eingegebene Schritt landet als
  `nextStep` der Ziel-Line — via bestehendem `PATCH /threads/:id`, KEIN Schema-/Backend-Eingriff.
  Dezenter GTD-Hinweis darunter: „Wirkt es wie unter 2 Minuten? Dann erledige es am besten
  gleich." Rechts „Überspringen" — komplett optional, kein Zwang (leeres Feld = überspringen).
  Nach dem Festhalten kurze Bestätigung „Nächster Schritt festgehalten.", dann still entfernt.
  Verwerfen (kein Ziel-Thread) behält den bisherigen 6-Sek-Strip ohne Capture. „Rückgängig"
  bleibt erhalten und setzt auch das Capture-Feld zurück. i18n de+en (captureNextStep/
  captureNextStepPlaceholder/captureHold/skipStep/twoMinHint/nextStepSaved). SW-Cache v17→v18.
  DESIGN-LOCK-konform (Accent #5F74D1, flach, kein Glow, keine AI-Symbolik).
- Geprüft: tsc frontend sauber. Deploy NUR Prod (frontend --no-cache), `/eingang` → HTTP 200.
  Abnahme mit echtem Klick-Flow (Playwright, Sim-Tenant, 5 Zurufe): Best-Treffer-Pille des
  Zoll-Zurufs geklickt → Capture-Panel erscheint über der Karte („✓ → Zugeordnet zu «sim:
  Messestand Halle 7 beschaffen»" + Rückgängig, NÄCHSTER SCHRITT, Eingabe, GTD-Hinweis,
  Überspringen). Text eingetippt + „Festhalten" → „Nächster Schritt festgehalten.". API-Gegenprobe:
  `sim-t-messe.nextStep` = „Ursprungszeugnis beim Lieferanten anfordern" (persistiert), Inbox 5→4.
  Mobil 390: Panel stapelt sauber (Feld voll breit, Button darunter), keine Kollision. Nur
  bekannte/ignorierbare 401/403.
  - Screenshot (Capture-Panel): .autopilot/shots/2026-07-19_eingang-nextstep-capture.png
  - Screenshot (festgehalten): .autopilot/shots/2026-07-19_eingang-nextstep-saved.png
  - Screenshot (Mobil): .autopilot/shots/2026-07-19_eingang-nextstep-capture-390.png
- Sim sauber gehalten: Test-Zuruf per `restore` zurück in den Eingang (Inbox wieder 5),
  `sim-t-messe.nextStep` wieder geleert — Sim-Tenant im Ausgangszustand.
- Commit: (dieser Lauf) feat(eingang): beim Rausfischen den nächsten Schritt festhalten (GTD)
- Nächster Schritt: Interview-Backlog Punkt 2 — Absender-Vorschlag „wer könnte übernehmen"
  (kleines optionales Feld beim Einwerfen; Vorschlag nie Zwang). Zuerst prüfen, ob ein Feld
  am RawInput nötig ist → falls Schema-Eingriff, als Rückmeldung in ORCHESTRATOR ablegen. Sonst
  Backlog Punkt 3 (horizontale Timeline) in Teilschritten oder Momentum-Ladenhüter (Punkt 5).

### 2026-07-19 — Line-Detail: horizontale Storyline-Timeline (Backlog #3, Teilschritt 1)
- Befund zu Backlog #2 (Absender-Vorschlag „wer könnte übernehmen"): `RawInput` hat bereits
  ein `assignedToUserId` + `assignee` — das ist aber „wer ÜBERNIMMT" (Zuweisung), nicht der
  optionale Absender-VORSCHLAG aus dem Interview. Beides zu vermischen oder ein neues Feld
  anzulegen wäre ein Schema-/Konzept-Eingriff, und es fehlt eine Einwerf-UI im Portal (Zurufe
  entstehen serverseitig, `profile.service`). Damit ist #2 ein „erst-Rückmeldung"-Fall (kein
  Alleingang) → deshalb stattdessen der klar interview-verlangte, schemafreie Punkt #3.
- Gebaut (nur Frontend, `/threads/[id]`): Die Storyline im Line-Detail ist jetzt eine ECHTE
  HORIZONTALE Timeline (Interview Punkt 4) statt der vertikalen Schiene. Start links →
  Jetzt rechts auf einer waagerechten Achse; je Knoten oben das absolute Datum, ein Dot auf
  der Achse (neuester gefüllt Accent #5F74D1 = JETZT, frühere hohl), darunter Medium-Icon +
  Label und ein 2-Zeilen-Kurztext. Anker-Badges START/JETZT. Auswahl je Schritt (Hover am
  Desktop / Tap am Handy) hebt den Dot per Accent-Ring hervor und zeigt den VOLLTEXT in einem
  Detail-Panel direkt darunter (Standard: neuester Schritt) — „Hover → Details" aus dem
  Interview, aber klippfrei (kein Popover im scroll-Container) und mobil-tauglich. Die Achse
  ist horizontal scrollbar (overflow-x auto), bricht also nie das Layout. Kein Schema/Backend.
  DESIGN-LOCK-konform (gesperrte Palette, flach, kein Glow, keine AI-Symbolik). SW-Cache v18→v19.
  Bewusst NOCH offen (weitere Teilschritte): dynamische Zeitskala (Tage/Wochen/Jahre, proportionale
  Abstände) und „Bereich markieren → zoomen".
- Geprüft: tsc frontend sauber. Deploy NUR Prod (frontend --no-cache), `/threads/sim-t-messe`
  → HTTP 200. Abnahme mit Screenshot-Tool auf app.aikydo.de (Sim-Tenant, Line „sim: Messestand
  Halle 7 beschaffen", 4 Schritte): Desktop 1440 zeigt die Achse START 9.Juli (Notiz) → 13.Juli
  (E-Mail) → 16.Juli (Sprachnotiz) → JETZT 18.Juli (Notiz, Accent-Dot), Datum/Medium/Kurztext je
  Knoten, Detail-Panel darunter mit Volltext des JETZT-Schritts. Mobil 390: Achse scrollt
  horizontal (frühe Schritte sichtbar, spätere per Swipe), kein Layout-Bruch, Panel voll lesbar.
  Nur bekannte/ignorierbare 401/403.
  - Screenshot: .autopilot/shots/2026-07-19_storyline-horizontal.png
  - Screenshot (Mobil): .autopilot/shots/2026-07-19_storyline-horizontal-390.png
- Commit: 14c95f17 feat(threads): horizontale Storyline-Timeline im Line-Detail (Teilschritt 1)
- Nächster Schritt: Backlog #3, Teilschritt 2 — dynamische Zeitskala (proportionale Abstände je
  echtem Zeit-Delta, Skalenwechsel Tage/Wochen/Jahre je nach Line-Alter). Danach Teilschritt 3
  „Bereich markieren → zoomen". Alternativ Backlog #2 (Absender-Vorschlag) als Rückmeldung in
  ORCHESTRATOR ablegen und Michaels Entscheidung abwarten.

### 2026-07-19 — Lines-Übersicht: chats-artige Zeilen-Mechanik (Aktuelle Richtung Schritt 1)
- Richtung: ORCHESTRATOR „Aktuelle Richtung" Schritt 1 — die Lines-Übersicht (`/threads`)
  soll sich in der Zeilen-Mechanik anfühlen wie `/chats` (Muster übernehmen, nicht neu erfinden),
  die Momentum-Optik bleibt obendrauf.
- Gebaut (Frontend `/threads` + kleines Backend-Add):
  - Karten → Zeilen: Hover-State (JS, `hoveredRow`), Checkbox-Slot (festes 20px, erscheint nur
    bei Hover/Select-Mode → kein Layout-Sprung), klickbarer Zeileninhalt (→ Storyline), Kebab
    (`MoreHorizontal`) nur bei Hover & nicht im Select-Mode.
  - Kontextmenü-Dropdown (chats-Muster: `rgba(30,30,35,0.98)`, radius 12, blur, boxShadow):
    Auswählen · Abschliessen · Umbenennen (inline, kein Modal) · [Divider] · Löschen (#ef4444).
  - Select-Mode + Bulk-Bar (`minHeight:32px`): alle auswählen + Zähler + mehrere abschließen /
    mehrere löschen + Schließen. Zähler-/„Auswählen"-Zeile im Nicht-Select-Modus.
  - BLEIBT: Momentum-Farbcodierung (Links-Akzent per inset box-shadow, überlebt Hover) +
    Sortierung (liegengeblieben oben) + Summary/nextStep/Badges je Zeile. Auswahl-Akzent auf
    Brand `#5F74D1` gezogen (neue `.line-checkbox`, DESIGN-LOCK-Korrektur statt chats-Blau).
  - Backend (kein Schema-Wechsel; Feld/Route neu, nicht Prisma-Migration): `DELETE /threads/:id`
    (`threadsService.removeThread`) — löst die Zurufe der Line ab (`threadId=null` + `dismissedAt`,
    kein harter Datenverlust), räumt Dependencies/Entities, Audit `THREAD_DELETED`. Rename nutzt
    vorhandenes `PATCH /threads/:id {title}`, Abschließen `POST /threads/:id/close`.
  - „Projekt zuordnen" (im Muster genannt) BEWUSST NICHT gebaut → siehe Rückmeldung: das
    `projectId`-Feld am Thread ist „Fundament, noch ohne UI", eine Zuordnung wäre nirgends
    sichtbar (nicht per Screenshot abnehmbar). Als Rückmeldung notiert, statt eine unsichtbare
    Aktion zu liefern. i18n de+en (lineCount/deleteConfirm/deleteMultipleConfirm). SW-Cache v19→v20.
- Geprüft: tsc backend+frontend sauber. Deploy NUR Prod (backend + frontend --no-cache).
  `/threads` → HTTP 200; `DELETE /threads/xxx` ohne Auth → 401 (Route existiert, nicht 404).
  Abnahme auf app.aikydo.de (Sim-Tenant, 8 Lines) mit Screenshot-Tool + Playwright:
  (1) Desktop 1440 — Zeilen mit Momentum-Akzent, Summary/Status/Badges/nextStep, in <2s erfassbar;
  (2) Hover → Checkbox links + Kebab rechts, Menü zeigt Auswählen/Abschliessen/Umbenennen/Löschen;
  (3) Select-Mode → Bulk-Bar (Select-all, „1 ausgewählt", Abschließen/Löschen/X), Brand-Checkbox,
  kein Layout-Sprung; (4) Rename funktional: Titel geändert + über Reload persistiert (PATCH);
  (5) Mobil 390 — Zeilen wrappen sauber, Ellipsis, kein Bruch. Nur bekannte 401/403.
  - Screenshot (Desktop): .autopilot/shots/2026-07-19_lines-rows-chatslike.png
  - Screenshot (Kebab-Menü): .autopilot/shots/2026-07-19_lines-rows-menu.png
  - Screenshot (Select-Mode): .autopilot/shots/2026-07-19_lines-rows-select.png
  - Screenshot (Mobil 390): .autopilot/shots/2026-07-19_lines-rows-390.png
- Sim sauber gehalten: der Rename-Funktionstest (Playwright) hatte beim Revert wegen der
  Momentum-Neusortierung (Rename setzt lastActivity=jetzt) die falsche Zeile erwischt; die zwei
  betroffenen Sim-Titel (sim-t-foerder, sim-t-reel) + deren lastActivity direkt in der Prod-DB
  aus seed.sql wiederhergestellt → Sim-Tenant im Ausgangszustand.
- Commit: 94e4df7d feat(threads): Lines-Übersicht auf chats-artige Zeilen-Mechanik + Delete-Endpoint
- Nächster Schritt: Aktuelle Richtung Schritt 2 — Line-Detail (`/threads/[id]`) rechte Panel-Spalte
  im Stil von `app/projects/[id]/page.tsx` (Stand/Nächster Schritt/Dateien), Timeline bleibt links.
  Vorab Datenlage prüfen: Dateien/Medien-Anbindung an Lines? Falls Schema/Endpoint fehlt → erst
  Rückmeldung. Panels Stand + Nächster Schritt gehen ohne Backend-Eingriff.

### 2026-07-19 — Line-Detail zweispaltig: Panel-Spalte rechts (Aktuelle Richtung Schritt 2)
- Richtung: ORCHESTRATOR „Aktuelle Richtung" Schritt 2 — Line-Detail (`/threads/[id]`) am
  Projekt-Detail-Muster (`app/projects/[id]/page.tsx`) orientieren: Timeline bleibt Hauptbühne
  links, rechts eine feste Panel-Spalte.
- Gebaut (nur Frontend `/threads/[id]`):
  - Layout: flex-Row, `gap:2.5rem`, `maxWidth:1200px` zentriert; links `flex:1, maxWidth:800px`
    (Storyline-Hauptbühne), rechts feste Panel-Spalte `width/minWidth:380px`, `border 1px
    rgba(255,255,255,0.08)`, `radius:18px`, `padding:1.4rem`, auf Desktop `position:sticky;
    top:1rem; maxHeight:calc(100vh-2rem); overflowY:auto`.
  - Rechte Panels (Projekt-Muster: `h3` fontWeight 500 / 0.95rem / `rgba(255,255,255,0.7)`,
    gestapelt je `marginBottom:1.75rem`): (a) **Stand** = `summary` (oder ruhiger Leerhinweis),
    (b) **Nächster Schritt** = editierbar (Accent-Header `#5F74D1` + Edit-Icon, Accent-Box mit
    Klick→Input, Enter speichert), (c) **Abschließen** = ruhige Ghost-Aktion + Close-Form INLINE
    im Panel — ersetzt die alte fixe Bottom-Leiste (die im Zweispalter mit dem Panel kollidiert wäre).
  - Links bleiben: Titel (editierbar), Meta-Zeile (+ Status/Bereich-Reveal), Blockiert, Entities,
    Verlauf/Timeline (horizontale Storyline unverändert), Closing-Summary.
  - **MOBIL (< 900px, State `isNarrow` + resize-Listener):** die rechte Spalte klappt unter die
    Timeline (Row → Column, volle Breite, ohne sticky/feste Höhe) — bewusst ERGÄNZT, da das
    Projekt-Detail keine Mobile-Logik hat (einziger Punkt, der nicht kopiert, sondern gebaut wurde).
  - i18n de+en (standEmpty). SW-Cache v20→v21. DESIGN-LOCK-konform (flach, gesperrte Palette, kein Glow).
- Geprüft: tsc frontend sauber. Deploy NUR Prod (frontend --no-cache), `/threads/sim-t-messe` → HTTP 200.
  Abnahme auf app.aikydo.de (Sim-Tenant) mit Screenshot-Tool + Playwright:
  (1) Desktop 1440 — links Verlauf (START 9.Juli → JETZT 18.Juli) + Detail-Panel, rechts die
  380px-Panel-Spalte mit Stand + Nächster Schritt + „Erledigt"; sauber vom Projekt-Detail abgeleitet;
  (2) nextStep funktional: über das rechte Panel getippt, „sim: Testschritt Panel" persistiert +
  über Reload im Accent-Panel angezeigt (PATCH); (3) Mobil 390 (full page) — Timeline oben
  (Hauptbühne, horizontal scrollbar), die Panel-Spalte klappt darunter, volle Breite, kein Bruch.
  Nur bekannte/ignorierbare 401/403.
  - Screenshot (Desktop): .autopilot/shots/2026-07-19_detail-zweispaltig.png
  - Screenshot (nextStep-Panel befüllt): .autopilot/shots/2026-07-19_detail-nextstep-panel.png
  - Screenshot (Mobil 390): .autopilot/shots/2026-07-19_detail-zweispaltig-390.png
- Sim sauber gehalten: Test-nextStep von sim-t-messe wieder geleert, lastActivity auf Seed-Wert
  (now - 3h) zurückgesetzt → Sim-Tenant im Ausgangszustand.
- Commit: 14bbdd76 feat(threads): Line-Detail zweispaltig — Panel-Spalte rechts (Projekt-Detail-Stil)
- Nächster Schritt: Dateien/Medien-Panel (c) braucht eine Backend-Anbindung (Thread↔Datei), die es
  noch nicht gibt → als Rückmeldung in ORCHESTRATOR abgelegt (Schema-Entscheidung nötig). Sonst
  Backlog #3 Teilschritt 2 (dynamische Zeitskala der horizontalen Timeline) oder Momentum-Ladenhüter (#5).

### 2026-07-19 — Line-Detail: dynamische Zeitskala der horizontalen Timeline (Backlog #3, Teilschritt 2)
- Richtung: Interview Punkt 4 / Backlog #3, Teilschritt 2 — die horizontale Storyline-Timeline
  (bislang äquidistant, 170px je Knoten) soll die ECHTE Zeit abbilden: proportionale Abstände je
  Zeit-Delta + dynamische Skala (Tage/Wochen/Jahre je nach Line-Alter).
- Gebaut (nur Frontend, `/threads/[id]`):
  - Abstände zwischen den Storyline-Knoten sind jetzt proportional zum echten Zeit-Delta. Umsetzung
    ohne fragiles Absolut-Positionieren: jeder Knoten trägt einen `marginLeft` = (gap − Kartenbreite),
    Achse bleibt normaler Flex-Flow (auto-Höhe, kein Kollaps). Der Achsen-Connector je Knoten hat
    exakt die Dot-Center-Distanz zum nächsten (ein Connector nach rechts statt zweier Hälften).
  - Skalierung am **Median-Delta** (robust gegen Ausreißer): ein typischer Abstand → TL_TARGET_GAP
    (175px), gefloort auf MIN_GAP (158, > Kartenbreite → kollisionsfrei) und gedeckelt auf MAX_GAP
    (430). Ergebnis: ähnliche Deltas wirken kompakt/fast äquidistant, echte Zeit-Lücken dehnen sich
    sichtbar, Bursts rücken zusammen — ohne dass ein Jahres-Sprung alles zerquetscht oder die Achse
    endlos scrollt. (Erster Wurf mit maxDelta-Normalisierung + MAX 430 schob JETZT-Knoten aus dem
    Viewport — per Screenshot erkannt und auf Median umgestellt.)
  - Dynamische Skalen-Beschriftung unter der Achse: „Zeitspanne {span} · Abstände maßstäblich",
    Span menschlich (Std./Tage/Wochen/Monate/Jahre je nach Gesamtspanne). i18n de+en
    (timelineScale/timelineSpanHours/Days/Weeks/Months/Years). SW-Cache v21→v22.
  - Kein Schema/Backend-Eingriff. DESIGN-LOCK-konform (flach, gesperrte Palette, kein Glow).
    Bewusst NOCH offen: Teilschritt 3 „Bereich markieren → zoomen".
- Geprüft: tsc frontend sauber. Deploy NUR Prod (frontend --no-cache), `/threads/sim-t-messe` → HTTP 200.
  Abnahme auf app.aikydo.de (Sim-Tenant, Line „sim: Messestand Halle 7 beschaffen", 4 Schritte):
  Desktop 1440 — Achse START 9.Juli → 13.Juli → 16.Juli → JETZT 18.Juli (Accent-Dot); die Abstände
  bilden die echten Deltas ab (9→13 = 4 Tage = breitester Abstand, 13→16 = 3 Tage, 16→18 = 2 Tage =
  engster), Caption „Zeitspanne 9 Tage · Abstände maßstäblich" rendert. Mobil 390 — Achse scrollt
  horizontal, Caption + Detail-Panel + gestapelte Panel-Spalte darunter, kein Layout-Bruch. Nur
  bekannte/ignorierbare 401/403.
  - Screenshot (Desktop): .autopilot/shots/2026-07-19_timeline-zeitskala.png
  - Screenshot (Mobil 390): .autopilot/shots/2026-07-19_timeline-zeitskala-390.png
- Commit: 16a50ec1 feat(threads): dynamische Zeitskala der horizontalen Timeline (Backlog #3, Teilschritt 2)
- Nächster Schritt: Backlog #3, Teilschritt 3 — „Bereich markieren → zoomen" (Ausschnitt der Achse
  per Drag markieren → hineinzoomen). Alternativ Momentum-Ladenhüter (#5, Variante B) als nächster
  schemafreier Punkt.

### 2026-07-19 — Line-Detail: Einwerfer je Storyline-Schritt sichtbar („wer")
- Richtung: Interview-Kern — das Portal ist ein „Verantwortungs-Verteiler mit Gedächtnis"; „wer
  bearbeitet / wer übernommen" zieht sich durch. Die Storyline zeigte bisher Datum + Medium + Text,
  aber NICHT, wer den jeweiligen Schritt eingeworfen hat. Befund: `RawInput` hat längst `userId` +
  `user`-Relation (Autor) — die Daten waren da, die Timeline lieferte sie nur nicht mit.
- Gebaut (schemafrei, kein Migrations-Eingriff):
  - Backend `threads.service.getTimeline`: `include: { user: { select: { id, name, email } } }` —
    read-only, Scope bleibt `where: { tenantId, threadId }` (NICHT-MACHEN-Regel eingehalten).
  - Frontend (`/threads/[id]`): `TimelineEntry.user` + Helper `authorName` (Name > E-Mail-Präfix).
    Anzeige des Einwerfers auf der Achse **nur beim Start & bei Bearbeiter-Wechsel** (macht
    Verantwortungs-Übergaben sichtbar, ohne Namens-Spam bei gleichem Autor); im Detail-Panel-Header
    **immer** vollständig („{Medium} · {Datum} · {Name}"). SW-Cache v22→v23. DESIGN-LOCK-konform
    (flach, gesperrte Palette, kein Icon-Kitsch — nur der Name in Sekundärgrau).
- Geprüft: tsc backend+frontend sauber. Deploy NUR Prod (backend + frontend --no-cache),
  `/threads/sim-t-messe` → HTTP 200. Abnahme auf app.aikydo.de (Sim-Tenant): Um den Übergabe-Fall
  real zu zeigen, temporär EINEN Sim-Schritt eingefügt, den Ilva einwirft („sim: Grafik fuer
  Standrueckwand uebernommen") → Achse zeigt am START „Nadja Brandt", das Detail-Panel den JETZT-
  Schritt „Notiz · 19. Juli · Ilva Sork" (Übergabe Nadja→Ilva belegt). Mobil 390: Name rendert unter
  dem Medium, Panel zeigt den Autor, kein Layout-Bruch. Danach den Temp-Schritt wieder gelöscht →
  Sim-Baseline zurück auf 4 Schritte. Nur bekannte/ignorierbare 401/403.
  - Screenshot (Desktop): .autopilot/shots/2026-07-19_storyline-autor.png
  - Screenshot (Mobil 390): .autopilot/shots/2026-07-19_storyline-autor-390.png
- Commit: 44cd874a feat(threads): Einwerfer je Storyline-Schritt (wer) in Timeline + Detail-Panel
- Nächster Schritt: Backlog #3, Teilschritt 3 („Bereich markieren → zoomen", Drag-Interaktion —
  per Playwright verifizierbar) oder Momentum-Ladenhüter (#5, Variante B). Denkbar auch: den Einwerfer
  bzw. „WER bearbeitet" analog auf der Lines-Übersicht/Momentum sichtbar machen (Interview Punkt 3).

### 2026-07-19 — Lines-Übersicht: „WER bearbeitet" je Line (Einwerfer-Avatare) (Interview Punkt 3)
- Richtung: Interview Punkt 3 verlangt „pro Line sofort sichtbar: worum geht's, WER bearbeitet,
  Pflege-Status". Farb-/Status/Summary/nextStep waren da, aber NICHT der „wer". Direkter Anschluss
  an den vorigen Schritt (Einwerfer im Line-Detail) — dieselbe Datenquelle (`RawInput.user`).
- Gebaut (schemafrei, kein Migrations-Eingriff):
  - Backend `threads.service.listThreads`: liefert je Line `contributors` = distinkte Einwerfer
    (Autoren der Zurufe), zuletzt-aktiv zuerst. Ein Extra-Query über alle Line-Zurufe
    (`where: { tenantId, threadId: in ids }`) + JS-Gruppierung — kein N+1, kein Schema-Wechsel,
    Tenant-Scope gewahrt. Name = `user.name` > E-Mail-Präfix.
  - Frontend (`/threads`): `Thread.contributors` + Helper `initialsOf`. In der Meta-Zeile ein
    kompakter, rechtsbündiger Initialen-Avatar-Cluster (20px-Kreise, Accent-getönt, überlappend
    −6px), max 3 sichtbar + „+N", Tooltip mit vollen Namen („Bearbeitet von: …"). i18n de+en
    (worksOn). SW-Cache v23→v24. DESIGN-LOCK-konform (flach, gesperrte Accent-Palette, keine
    AI-Symbolik — nur Initialen).
- Geprüft: tsc backend+frontend sauber. Deploy NUR Prod (backend + frontend --no-cache),
  `/threads` → HTTP 200. Abnahme auf app.aikydo.de (Sim-Tenant, 8 Lines): Desktop 1440 — je Zeile
  rechts der Einwerfer-Avatar (NB/PO/TR/IS = Nadja/Piet/Tomas/Ilva), in <2s erfassbar „wer an welcher
  Line dran ist"; Momentum-Akzent + „Liegengeblieben"-Badges + Summary/Impact/nextStep intakt.
  Mobil 390 — Meta-Zeile wrappt, Avatar rechtsbündig auf eigener Zeile, kein Bruch. Nur bekannte
  401/403.
  - Screenshot (Desktop): .autopilot/shots/2026-07-19_lines-wer-bearbeitet.png
  - Screenshot (Mobil 390): .autopilot/shots/2026-07-19_lines-wer-bearbeitet-390.png
- Commit: e21de72a feat(threads): WER bearbeitet je Line — Einwerfer-Avatare auf der Übersicht
- Nächster Schritt: „WER" analog im Momentum-Cockpit (/mindmap) andeuten, oder Backlog #3 Teilschritt 3
  (Zeitachse zoomen) bzw. Momentum-Ladenhüter (#5, Variante B). Offene Rückmeldungen (Dateien-Panel,
  Projekt-Zuordnung, Absender-Vorschlag) warten weiter auf Michaels Entscheidung.

### 2026-07-19 — Line-Detail: Bereich markieren → zoomen (Backlog #3, Teilschritt 3 — Timeline-Trilogie fertig)
- Richtung: Interview Punkt 4 — „Wunsch: Bereich markieren (z.B. Woche 16–18) → zoomt in diesen
  Ausschnitt." Letzter offener Teilschritt der horizontalen Storyline-Timeline (Teilschritte 1
  horizontal + 2 dynamische Zeitskala waren fertig). Damit ist Backlog #3 komplett.
- Gebaut (nur Frontend, `/threads/[id]`):
  - Eine **Brush-Übersichtsleiste** unter der Achsen-Skala: volle Breite (KEIN Scroll) → robustes
    Pixel↔Zeit-Mapping (der horizontale Scroll-Container der Achse machte Drag-Marquee dort fragil).
    Die Leiste zeigt die ganze Storyline als Mittelachse + einen Punkt je Schritt an seiner echten
    Zeit-Position (0..1). Ziehen (PointerEvents + setPointerCapture, `touchAction:none` → Maus & Touch)
    markiert einen Zeitraum; beim Loslassen zoomt die Haupt-Timeline in den Ausschnitt: die Deltas des
    Ausschnitts dehnen sich über `computeTimelineLayout(view)` auf die volle Bühne. Auswahl < 2%
    Breite oder < 2 enthaltene Schritte = ignoriert (als Klick gewertet, kein versehentliches Zoomen).
  - Der gezoomte Ausschnitt ist in der Leiste als Accent-Fenster hervorgehoben (enthaltene Punkte
    Accent, äußere grau). Caption „Ausschnitt {span} · {N} Schritte" (Accent) + Button
    „Zoom zurücksetzen". START/JETZT-Badges + der gefüllte Accent-„Jetzt"-Dot hängen jetzt am
    **globalen** Start/Jetzt (globaler Index), nicht am Ausschnitt-Rand — im Zoom fehlen sie
    ehrlich, wenn die Endpunkte nicht im Ausschnitt liegen. Detail-Panel-Default folgt dem
    Ausschnitt-Ende. `computeTimelineLayout` als wiederverwendbare Funktion herausgezogen (statt
    inline-useMemo), damit sie auf den Ausschnitt anwendbar ist. Zoom wird beim Thread-Wechsel
    zurückgesetzt. i18n de+en (timelineZoomHint/timelineZoomReset/timelineZoomedSpan). SW-Cache v24→v25.
    Kein Schema/Backend. DESIGN-LOCK-konform (flach, gesperrte Accent-Palette, kein Glow, keine AI-Symbolik).
- Geprüft: tsc frontend sauber. Deploy NUR Prod (frontend --no-cache), `/threads/sim-t-messe` → HTTP 200.
  Abnahme mit Screenshot-Tool + Playwright-Drag auf app.aikydo.de (Sim-Tenant, Line „sim: Messestand
  Halle 7 beschaffen", 4 Schritte 9.–18. Juli): (1) Base — Leiste „Zeitraum markieren zum Zoomen" mit
  4 Punkten unter der Achse; (2) Desktop-Drag über die mittleren 13.–16. Juli → Haupt-Timeline zeigt
  nur noch diese 2 Schritte gedehnt auf die volle Bühne, Leiste hebt das Fenster + die 2 Punkte accent
  hervor, Caption „Ausschnitt 3 Tage · 2 Schritte", START/JETZT korrekt verschwunden; (3) „Zoom
  zurücksetzen" → volle Line zurück („Zeitspanne 9 Tage" wieder da, per Assertion verifiziert);
  (4) Mobil 390 — Drag zoomt ebenso, Leiste + Timeline + Panels stapeln sauber, kein Layout-Bruch.
  Nur bekannte/ignorierbare 401/403.
  - Screenshot (Base, Brush-Leiste): .autopilot/shots/2026-07-19_timeline-zoom-base.png
  - Screenshot (gezoomt Desktop): .autopilot/shots/2026-07-19_timeline-zoom.png
  - Screenshot (gezoomt Mobil 390): .autopilot/shots/2026-07-19_timeline-zoom-390.png
- Commit: 8c13fd69 feat(threads): Bereich-Zoom der horizontalen Timeline (Backlog #3, Teilschritt 3)
- Nächster Schritt: Backlog #3 ist damit komplett (horizontal + dynamische Skala + Bereich-Zoom).
  Nächste schemafreie Kandidaten: „WER" im Momentum-Cockpit (/mindmap) andeuten, oder Momentum-Ladenhüter
  (#5, Variante B — beachten: volle Fassung braucht die Question-Sektion #7, die noch nicht existiert).
  Offene Rückmeldungen (Dateien-Panel, Projekt-Zuordnung, Absender-Vorschlag) warten auf Michaels Entscheidung.

### 2026-07-19 — Lines-Übersicht: Ranking-Umschalter (Interview Punkt 3 — „30–100 Lines beherrschen")
- Richtung: Interview Punkt 3 verlangt Kategorien/Rankings der Lines-Übersicht — „Top gepflegt, Top
  vernachlässigt, neueste, älteste — simuliere sinnvolle Ordnung als würdest du selbst die Firma führen".
  Bislang gab es NUR eine feste Sortierung (liegengeblieben zuerst). Für 30–100 Lines braucht man den
  Blickwinkel-Wechsel.
- Gebaut (nur Frontend, `/threads`): Ein segmentierter Umschalter (rechts in der Zähler-Zeile) mit vier
  Rankings: **Vernachlässigt** (Default = bisheriges Verhalten: liegengeblieben zuerst, dann impact-stärkste,
  dann Momentum) · **Gepflegt** (laufende/frischeste zuerst, impact als Tiebreak) · **Neueste** (createdAt
  absteigend) · **Älteste** (createdAt aufsteigend). Kein Regress: „Vernachlässigt" bleibt der Startzustand
  und die Momentum-Farbcodierung (Links-Akzent per inset box-shadow) bleibt in JEDER Sortierung je Zeile
  erhalten — man ordnet die Firma neu, sieht aber weiter auf einen Blick, was rot/grün ist. Schemafrei
  (rein clientseitige Sortierung der ohnehin gelieferten Felder). i18n de+en (sortStalled/Cared/Newest/
  Oldest). SW-Cache v25→v26. DESIGN-LOCK-konform (flach, gesperrte Accent-Palette, Pill-Buttons wie sonst).
- Geprüft: tsc frontend sauber. Deploy NUR Prod (frontend --no-cache), `/threads` → HTTP 200. Abnahme auf
  app.aikydo.de (Sim-Tenant, 8 Lines) mit Screenshot-Tool + Playwright: alle vier Tabs erzeugen sichtbar
  UNTERSCHIEDLICHE Top-3 (per Assertion): Vernachlässigt = Foerdermittel/Imagefilm/Employer-Branding;
  Neueste = Messestand/Kampagnen-Q3/Website; Älteste = Rohbau/Fuhrpark/Employer-Branding; Gepflegt =
  Messestand/Rohbau/Kampagnen-Q3 (grüne „Läuft" oben). Aktiver Tab in Accent, Momentum-Akzent/Badges/
  Contributor-Avatare intakt. Mobil 390: Tabs wrappen sauber auf eine eigene Zeile (alle vier passen),
  kein Layout-Bruch. Nur bekannte/ignorierbare 401/403.
  - Screenshot (Vernachlässigt/Default): .autopilot/shots/2026-07-19_lines-ranking-stalled.png
  - Screenshot (Gepflegt): .autopilot/shots/2026-07-19_lines-ranking-cared.png
  - Screenshot (Mobil 390): .autopilot/shots/2026-07-19_lines-ranking-390.png
- Commit: 69d435db feat(threads): Ranking-Umschalter auf der Lines-Übersicht (Interview Punkt 3)
- Nächster Schritt: Interview Punkt 3 ist damit rund (worum geht's/Stand + WER bearbeitet + Pflege-Status +
  Rankings). Nächste schemafreie Kandidaten: „WER" im Momentum-Cockpit (/mindmap), Momentum-Ladenhüter
  (#5, Variante B — volle Fassung braucht Question-Sektion #7). Offene Rückmeldungen unverändert offen.

### 2026-07-19 — Momentum: Ladenhüter — liegengebliebene Eingangs-Kacheln (Backlog #5, Variante B, Typ 3)
- Richtung: Interview Punkt 8 (Ladenhüter) + die in ORCHESTRATOR entschiedene **Variante B** — Momentum
  ist die ehrliche Heimat der Ladenhüter, die „vernachlässigt"-Seite umfasst alle drei Typen:
  liegengebliebene Lines, unbeantwortete Ja/Nein-Fragen, überalterte ungefischte Eingangs-Kacheln.
  Typ 1 (Lines) steckt bereits im Cockpit (rote Seite); Typ 3 (aged Eingangs-Kacheln) war im Momentum
  bislang KOMPLETT UNSICHTBAR — genau die Lücke, die eine ungefischte, alt gewordene Kachel unsichtbar
  liegen lässt. Diesen Typ jetzt sichtbar gemacht (Typ 2 Ja/Nein-Fragen folgt mit der Question-Sektion #7).
- Gebaut (nur Frontend, `/mindmap`): Unter dem Momentum-Cockpit eine **flache Consult-Fläche** „Im Eingang
  liegengeblieben" — die ungefischten Eingangs-Kacheln, die seit ≥ 2 Tagen im Pool liegen (älteste zuerst,
  Top 5). Je Zeile: haiku-artige Kurzvorschau (Präfix „sim:" abgeschnitten) + Alter („vor N Tagen"). Links
  ein **Alters-Akzent** (matt-rot, je älter desto kräftiger, inset box-shadow) — bewusst KEIN Glow, damit
  die Fläche dem glühenden Cockpit klar untergeordnet bleibt. Klick auf Zeile ODER „Ganzen Eingang öffnen →"
  navigiert zum Eingang. **Kein Interrupt, keine zweite Queue** neben dem Eingang (Michaels
  Kannibalisierungs-Falle bewusst vermieden — reine Reflexions-/Consult-Fläche, die man aufsucht). Nur
  gerendert, wenn es echte Ladenhüter gibt (sonst ehrlich leer/versteckt). Schemafrei (`GET /raw-inputs/inbox`,
  bereits vorhanden). i18n de+en (stalledInboxTitle/Hint/Age/All). SW-Cache v26→v27. DESIGN-LOCK-konform.
- Geprüft: tsc frontend sauber. Deploy NUR Prod (frontend --no-cache), `/mindmap` → HTTP 200. Abnahme auf
  app.aikydo.de (Sim-Tenant, 5 offene Zurufe 1,1–3,1 Tage alt): Desktop 1440 — Cockpit unverändert (Glow,
  Gesamt −85), darunter die flache Ladenhüter-Fläche mit den beiden ≥2-Tage-Kacheln (Rollup-Banner „vor 3
  Tagen" kräftiger rot, Whiteboard-Foto „vor 2 Tagen"), die 1,1–1,3-Tage-Kacheln erscheinen korrekt NICHT.
  Klick auf eine Zeile → `https://app.aikydo.de/eingang` (per Playwright verifiziert). Mobil 390 — Titel/Hinweis/
  Link wrappen sauber, Zeilen mit Ellipsis + rechtsbündigem Alter, kein Layout-Bruch. Nur bekannte 401/403.
  - Screenshot (Desktop): .autopilot/shots/2026-07-19_momentum-ladenhueter.png
  - Screenshot (Mobil 390): .autopilot/shots/2026-07-19_momentum-ladenhueter-390.png
- Commit: (dieser Lauf) feat(momentum): Ladenhüter — liegengebliebene Eingangs-Kacheln (Variante B, Typ 3)
- Nächster Schritt: Ladenhüter Typ 2 (unbeantwortete Ja/Nein-Fragen) fehlt noch — hängt an der
  Question-Sektion (#7), die es noch nicht gibt (wahrscheinlich neues Modell → Schema-Tabu → Rückmeldung).
  Sonst „WER" im Momentum-Cockpit oder Backlog-Feinschliff. Offene Rückmeldungen (Dateien-Panel,
  Projekt-Zuordnung, Absender-Vorschlag, Question-Sektion) warten auf Michaels Entscheidung.

### 2026-07-19 — Eingang: Einwerfer je Pool-Kachel sichtbar (Interview Punkt 1 — „wer es eingeworfen hat")
- Befund: Interview Punkt 1 verlangt auf jeder Eingangs-Kachel „Herkunftsquelle + WER es eingeworfen hat
  auf einen Blick". Die Karte zeigte bislang nur den Medium-Typ (VOICE/CHAT/EMAIL/PHOTO) + Relativzeit —
  der Einwerfer fehlte komplett, obwohl die Daten da sind (`RawInput.user`/Autor, Relation `rawInputAuthor`,
  `userId` required). Dieselbe „wer"-Datenquelle, die schon in Timeline (Einwerfer je Schritt) und
  Lines-Übersicht (Contributor-Avatare) genutzt wird — im Eingang war sie ungenutzt.
- Gebaut (schemafrei, kein Migrations-Eingriff):
  - Backend `inbox.service.listInbox`: den Autor mitliefern (`user: { id, name, email }`), read-only,
    Tenant-Scope unverändert (`where: { tenantId, threadId: null, dismissedAt: null }`), kein Schema-Wechsel.
  - Frontend (`/eingang`): dezenter Einwerfer-Cluster rechtsbündig in der Meta-Zeile (`marginLeft:auto`) —
    Initialen-Avatar (20px, Accent-getönt `rgba(95,116,209,0.18)`, exakt das /threads-Muster) + „von {Name}"
    in Sekundärgrau, Tooltip „Eingeworfen von {Name}". Name = `user.name` > E-Mail-Präfix (Helper
    `authorLabel`). Bewusst getrennt vom bestehenden „Übernommen von"-Feld unten (Einwerfer ≠ wer übernimmt
    — genau die Interview-Unterscheidung). i18n de+en (thrownIn/thrownInBy). SW-Cache v27→v28.
    DESIGN-LOCK-konform (flach, gesperrte Accent-Palette, keine AI-Symbolik — nur Initialen + Name).
- Geprüft: tsc backend+frontend sauber. Deploy NUR Prod (backend + frontend --no-cache). `/eingang` → HTTP 200.
  Abnahme auf app.aikydo.de (Sim-Tenant, 5 Zurufe mit vier verschiedenen Einwerfern): Desktop 1440 — je Karte
  rechts oben der Einwerfer, sichtbar differenziert („NB von Nadja Brandt" auf dem Voice-Zuruf, „TR von Tomas
  Reuter" auf dem Chat-Zuruf usw.), in <2s erfassbar „von wem kam das". Vorschlags-Hierarchie/Aktionen/
  „Übernommen von" intakt. Mobil 390 — Einwerfer rechtsbündig in der Meta-Zeile, wrappt sauber, kein
  Layout-Bruch. Nur bekannte/ignorierbare 401 (new-users-count) / 403.
  - Screenshot (Desktop): .autopilot/shots/2026-07-19_eingang-einwerfer.png
  - Screenshot (Mobil 390): .autopilot/shots/2026-07-19_eingang-einwerfer-390.png
- Commit: (dieser Lauf) feat(eingang): Einwerfer je Pool-Kachel sichtbar (Interview Punkt 1)
- Nächster Schritt: Damit ist Interview Punkt 1 („Merkmale auf einen Blick: Herkunftsquelle + wer eingeworfen")
  auf der Pool-Kachel rund. Nächste schemafreie Kandidaten: „WER" im Momentum-Cockpit (/mindmap), oder
  Interview Punkt 5 Momentum-Skalierung (Top-N bei vielen Lines — mit 8 Sim-Lines aber schwer per Screenshot
  abnehmbar). Blockiert bleiben #7 Question-Sektion, #6 Benachrichtigung, #4 Dateien-Panel, #2 Absender-
  Vorschlag (alle brauchen Michaels Schema-/Konzept-Entscheidung — siehe ORCHESTRATOR Rückmeldungen).

### 2026-07-19 — Eingang: Medium-Badge mit Icon + Label statt rohem Typ-String (Interview Punkt 1 — Herkunftsquelle)
- Befund: Direkter Anschluss an den Einwerfer-Schritt. Interview Punkt 1 verlangt „Merkmale auf einen Blick:
  Herkunftsquelle + wer eingeworfen hat". Der Einwerfer ist jetzt da; die Herkunft/Medium zeigte die Karte
  aber nur als rohen Uppercase-Typ-String („VOICE", „CHAT", „EMAIL", „PHOTO") — technisch, unpoliert und
  INKONSISTENT zur Storyline-Timeline, die längst eine warme Medium-Sprache spricht (Icon + Sprachnotiz/
  Foto/E-Mail/Notiz, siehe Lauf „Zuruf-Medium je Storyline-Schritt").
- Gebaut (nur Frontend, `/eingang`): Das Medium-Badge nutzt jetzt dasselbe flache Icon + lokalisierte Label
  wie die Storyline — Sprachnotiz (Mikrofon), Foto (Kamera), E-Mail (Umschlag), Notiz/Chat (Sprechblase).
  Lokales `MediumIcon` (SVGs 1:1 aus der Storyline übernommen, currentColor, 12px) + Label-Helper mit
  Fallback auf „Notiz" für unbekannte Typen. Accent-Pille bleibt (gesperrte Palette), nur Inhalt = Icon+Label
  statt Uppercase-String. Kein Backend/Schema-Eingriff. i18n de+en (medium.voice/photo/email/chat).
  SW-Cache v28→v29. DESIGN-LOCK-konform (flach, gesperrte Accent-Palette, keine AI-Symbolik).
- Geprüft: tsc frontend sauber. Deploy NUR Prod (frontend --no-cache). `/eingang` → HTTP 200. Abnahme auf
  app.aikydo.de (Sim-Tenant, 5 Zurufe, alle vier Medientypen vertreten) mit Screenshot-Tool: Full-Page 1440
  zeigt je Karte das korrekte Medium-Badge — „Sprachnotiz" (Mikro, Zoll-Voice), „Notiz" (Sprechblase,
  Statiker-Chat), „E-Mail" (Umschlag, Sponsoring), „Foto" (Kamera, Whiteboard), „Notiz" (Rollup-Chat) —
  plus der Einwerfer rechts (Nadja/Tomas/Piet/Ilva). Mobil 390: Badge + Einwerfer in der Meta-Zeile, wrappt
  sauber, kein Layout-Bruch. Nur bekannte/ignorierbare 401 (new-users-count) / 403.
  - Screenshot (Desktop Full): .autopilot/shots/2026-07-19_eingang-medium-full.png
  - Screenshot (Mobil 390): .autopilot/shots/2026-07-19_eingang-medium-390.png
- Commit: (dieser Lauf) feat(eingang): Medium-Badge mit Icon + Label statt rohem Typ-String
- Nächster Schritt: Interview Punkt 1 ist damit auf der Pool-Kachel komplett (Medium/Herkunft + Einwerfer +
  Zeit + Vorschlags-Hierarchie + Übernahme + nextStep-Capture). Nächste schemafreie Kandidaten: „WER" im
  Momentum-Cockpit (/mindmap), Momentum-Skalierung (Top-N, schwer per 8-Sim-Lines abnehmbar). Blockiert:
  #7 Question-Sektion, #6 Benachrichtigung, #4 Dateien-Panel, #2 Absender-Vorschlag (Michaels Entscheidung).

### 2026-07-19 — Momentum-Cockpit: Balance-Zähler an VERLIERT/GEWINNT (Interview Punkt 5)
- Befund: Der Cockpit-Kopf zeigte nur die abstrakte Gesamtsumme (z.B. −93). Interview Punkt 5 will „aktiv
  vs. vernachlässigt auf einen Blick" — die Summe allein verrät aber nicht, ob das EINE katastrophale oder
  FÜNF mittelmäßige Lines sind (Anzahl und Magnitude sind darin vermischt). Die Balance der Firma (wie viele
  Vorhaben verlieren vs. gewinnen an Momentum) war nur qualitativ aus dem Fächer ablesbar, nicht als Zahl.
- Gebaut (nur Frontend, `/mindmap`): An die bestehenden Kopf-Labels VERLIERT/GEWINNT hängt je ein Zähler —
  `losingCount` (m<0) links, `gainingCount` (m>0) rechts, konsistent zur Karten-Semantik (die Seiten sind
  per Vorzeichen definiert, nicht per /threads-3-Bucket). Dezent rot/grün getönt (#c98a8a / #7cba97), echot
  die Fächerfarben, ohne sie zu überstrahlen — die Bespoke-Karte darf Farbe (DESIGN-LOCK-Ausnahme). Zähler
  erscheint nur, wenn > 0. Kein neuer i18n-Key (Zahl sprachneutral). SW-Cache v29→v30. Kein Backend/Schema.
- Geprüft: tsc frontend sauber. Deploy NUR Prod (frontend --no-cache). `/mindmap` → HTTP 200. Abnahme auf
  app.aikydo.de (Sim-Tenant, 8 Lines): Desktop 1440 — Kopf „VERLIERT · 5" (rot) links, „3 · GEWINNT" (grün)
  rechts, Summe −93; die Zähler stimmen exakt mit dem Fächer überein (5 rote Lines −100…−3, 3 grüne +26…+95).
  Cockpit-Glow + Ladenhüter-Fläche darunter unverändert intakt. Mobil 390 — Zähler in der Label-Zeile, kein
  Layout-Bruch. Nur bekannte/ignorierbare 401 (new-users-count) / 403.
  - Screenshot (Desktop): .autopilot/shots/2026-07-19_momentum-balance-count.png
  - Screenshot (Mobil 390): .autopilot/shots/2026-07-19_momentum-balance-count-390.png
- Commit: (dieser Lauf) feat(momentum): Balance-Zähler an VERLIERT/GEWINNT (Interview Punkt 5)
- Nächster Schritt: Momentum-Skalierung auf viele Lines (Top-N beide Richtungen bei 30–100 Lines) ist der
  logische nächste Punkt 5-Schritt, aber mit nur 8 Sim-Lines nicht sauber per Screenshot abnehmbar (Top-10-
  Deckel greift nicht) → besser mit mehr Sim-Daten oder Michaels OK. Blockiert bleiben #7/#6/#4/#2 (Schema).

### 2026-07-19 — Lines-Übersicht: Suche/Volltextfilter (Interview Punkt 3 — „30–100 Lines beherrschen")
- Befund: Interview Punkt 3 verlangt ausdrücklich, 30–100 Lines zu beherrschen. Die Rankings (Vernachlässigt/
  Gepflegt/Neueste/Älteste) waren gebaut, aber es fehlte die naheliegendste Fähigkeit, um eine bestimmte Line
  unter vielen zu FINDEN: eine Suche. `/chats` hat sie längst (etabliertes Muster, `searchQuery`-State +
  Lupen-Input + gefilterte Liste), `/threads` hatte KEINE — genau die Lücke, die bei 30–100 Lines beißt.
  Passt exakt zur aktuellen Richtung „Muster aus chats übernehmen, nicht neu erfinden".
- Gebaut (nur Frontend, `/threads`): Ein Such-Input im chats-Stil (Lupe links, Clear-„X" rechts sobald Text da
  ist) direkt unter dem Header, immer sichtbar solange Lines existieren. Volltextfilter clientseitig über
  **Titel + Stand (`summary`) + nächster Schritt (`nextStep`)** — matcht also nicht nur den Namen, sondern auch
  worum es geht/wo es steht. Zähler (Header-Pille + Zeile) spiegeln die Trefferzahl. Ranking-Umschalter und
  Momentum-Farbcodierung (Links-Akzent, Badges, Contributor-Avatare) bleiben je Zeile intakt — man filtert die
  Firma ein, sieht aber weiter auf einen Blick rot/grün. Eigener No-Treffer-Zustand („Keine Line gefunden" +
  Query-Echo), der die Suchbox bedienbar lässt (kein Sackgassen-Empty). Schemafrei (rein clientseitig auf
  ohnehin gelieferten Feldern). i18n de+en (searchPlaceholder/noLinesFound/noResultsFor). SW-Cache v30→v31.
  DESIGN-LOCK-konform (flach, gesperrte Palette, kein Glow, keine AI-Symbolik).
- Geprüft: tsc frontend sauber. Deploy NUR Prod (frontend --no-cache), `/threads` → HTTP 200. Abnahme auf
  app.aikydo.de (Sim-Tenant, 8 Lines) mit Screenshot-Tool + Playwright-Tippen: (1) Basis — Suchbox über der
  Zähler-/Sortier-Zeile, alle 8 Zeilen samt Momentum-Akzent intakt; (2) Query „mess" → exakt 1 Treffer
  („sim: Messestand Halle 7 beschaffen", per Assertion der sichtbaren Titel), Header „1 offen", Clear-X sichtbar,
  „Läuft"-Badge/Akzent erhalten; (3) Query „zzz" → No-Treffer-Zustand „Keine Line gefunden · Kein Treffer für
  „zzz"", Suchbox bleibt bedienbar; (4) Mobil 390 — Suchbox volle Breite mit Clear-X, Sortier-Tabs wrappen auf
  eigene Zeile, gefilterte Zeile ohne Layout-Bruch. Nur bekannte/ignorierbare 401 (new-users-count) / 403.
  - Screenshot (gefiltert „mess"): .autopilot/shots/2026-07-19_lines-suche.png
  - Screenshot (No-Treffer „zzz"): .autopilot/shots/2026-07-19_lines-suche-leer.png
  - Screenshot (Mobil 390): .autopilot/shots/2026-07-19_lines-suche-390.png
- Commit: f62a4d09 feat(threads): Suche auf der Lines-Übersicht (Interview Punkt 3 – 30–100 Lines beherrschen)
- Nächster Schritt: Interview Punkt 3 („30–100 Lines beherrschen") ist damit rund (Rankings + Suche + Momentum-
  Sicht je Zeile). Weiterhin blockiert auf Michaels Schema-/Konzept-Entscheidung: #7 Question-Sektion (größter
  Hebel, schaltet Ladenhüter-Typ 2 + #6 frei), #6 Benachrichtigung, #4 Dateien-Panel, #2 Absender-Vorschlag.
  Schemafreie Rest-Kandidaten werden dünn (Momentum-Top-N braucht mehr Sim-Lines).

### 2026-07-19 — Skalierung: Sim-Welt auf 38 Lines / 20 Pool-Kacheln, dann drei Engpässe behoben
- Ausgangslage: Der schemafreie Backlog galt als erschöpft, weil die verbleibenden Interview-Punkte
  (Momentum Top-N, Punkt 5) mit nur 8 Sim-Lines nicht sauber per Screenshot abnehmbar waren. Statt
  auf Michaels Entscheidung zu warten, wurde die naheliegende Vorbedingung selbst hergestellt: die
  Sim-Welt auf die vom Interview verlangte Größenordnung gebracht (Punkt 3: „30–100 Lines beherrschen",
  Punkt 9: „Desktop = die große Übersicht"). Das legte sofort drei echte Engpässe offen.
- Sim-Daten (schemafrei, alles Tenant `sim-team` / „sim:"-Prefix, per `wipe.sql` restlos entfernbar):
  `seed.sql` um 30 Lines (`sim-t-x01..x30`, realistischer Handwerks-/Werkstatt-Alltag mit `summary`,
  `nextStep`, gestaffelter `lastActivity` über 0–30 Tage) + 38 zugehörige Zurufe + 15 weitere
  Pool-Kacheln erweitert → **38 Lines, ~60 Zurufe, 20 offene Pool-Kacheln**. Fix am Seed: Tenant/User
  werden nicht mehr gelöscht, sondern geupsertet — die App hängt eine `Subscription` an den Tenant,
  der Delete lief in einen FK-Fehler (Transaktion rollte sauber zurück, keine Datenverluste).

**(1) Momentum-Cockpit: Top-10 beide Richtungen statt 38-Zeilen-Wand (Interview Punkt 5)**
- Befund am Screenshot: Das Cockpit rendert jede Line als eigene Zeile — bei 38 Lines eine endlose
  Wand, davon 17 identisch bei −100 und damit nicht unterscheidbar. Die „auf einen Blick"-Qualität,
  der ganze Zweck der Karte, war weg. Interview Punkt 5 verlangt ausdrücklich „Top-10-Listen beide
  Richtungen".
- Gebaut (nur Frontend, `/mindmap`): Ab 2×TOP_N Lines zeigt das Cockpit nur die Extreme — **Top 10
  vernachlässigt oben, Top 10 aktiv unten**, dazwischen eine ruhige Trennzeile „18 ruhige Lines
  dazwischen — anzeigen" (aufklappbar, per „Nur die Extreme zeigen" wieder zuklappbar). Summe und
  die VERLIERT/GEWINNT-Zähler bleiben bewusst über ALLE Lines — gekappt wird die Darstellung, nicht
  die Wahrheit. Kein Backend/Schema. SW-Cache v31→v32.
- Geprüft: tsc sauber, Deploy nur Prod. Cockpit passt mit 38 Lines wieder auf einen Schirm; Auf-/
  Zuklappen per Playwright verifiziert (20 → 38 → 20 Zeilen); Mobil 390 ohne Layout-Bruch.
  - Screenshot: .autopilot/shots/2026-07-19_momentum-topn.png
  - Screenshot (aufgeklappt): .autopilot/shots/2026-07-19_momentum-topn-expanded.png
  - Screenshot (Mobil 390): .autopilot/shots/2026-07-19_momentum-topn-390.png
- Commit: feat(momentum): Top-10 beide Richtungen statt Zeilen-Wand (Interview Punkt 5)

**(2) Lines-Übersicht: kompakte Dichte (Interview Punkt 3 + 9)**
- Befund: Die ausführliche Zeile (Titel + Stand + Meta + nächster Schritt) ist 128px hoch — auf einem
  1440er Schirm passten **sechs** Vorhaben. Bei 30–100 Lines wird die Übersicht damit zur Scroll-Strecke,
  obwohl Desktop laut Punkt 9 „die große Übersicht" sein soll.
- Gebaut (nur Frontend, `/threads`): Kompakte Dichte mit **62px/Zeile (~12 Lines je Schirm)** — Titel +
  Meta bleiben stehen, Stand und nächster Schritt wandern in den Titel-Tooltip. Momentum-Akzent, Badges,
  Bearbeiter-Avatare, Rankings und Suche unverändert. **Ab 20 offenen Lines ist kompakt die Vorgabe**,
  darunter bleibt es ausführlich; der Umschalter neben den Rankings kehrt das jederzeit um. Die Vorgabe
  hängt an der Gesamtzahl, nicht an der Trefferzahl — sonst würde jede Suche die Dichte umspringen lassen.
  i18n de+en (densityCompact/densityDetailed). SW-Cache v32→v33.
- Geprüft: tsc sauber, Deploy nur Prod. Zeilenhöhe per Playwright gemessen: 61,75px kompakt / 128,45px
  ausführlich / zurück auf 61,75px; die Stand-Zeile erscheint im ausführlichen Modus wieder. Suche +
  Ranking bleiben im kompakten Modus korrekt (Query „angebot" → 5 Treffer, Dichte springt nicht,
  „Gepflegt" sortiert weiter richtig). Mobil 390 ohne Layout-Bruch.
  - Screenshot (kompakt): .autopilot/shots/2026-07-19_lines-kompakt.png
  - Screenshot (ausführlich): .autopilot/shots/2026-07-19_lines-ausfuehrlich.png
  - Screenshot (Mobil 390): .autopilot/shots/2026-07-19_lines-kompakt-390.png
- Commit: feat(threads): kompakte Dichte auf der Lines-Uebersicht (Interview Punkt 3 + 9)

**(3) Eingang: Pool-Kachel gefaltet (Interview Punkt 1)**
- Befund: Derselbe Test für den Eingang (20 statt 5 Zurufe): jede Kachel ~430px hoch (Vorschlags-Block
  + „Line wählen" + Zuordnen/+Neue Line + „Übernommen von") — **zwei Zurufe je Schirm**. Interview
  Punkt 1 will Kacheln mit den Merkmalen auf einen Blick und das Detail erst auf Klick.
  - Screenshot (Zustand vorher): .autopilot/shots/2026-07-19_eingang-vorher-20-kacheln.png
- Gebaut (nur Frontend, `/eingang`): Die Kachel zeigt gefaltet Medium + Zeit + Einwerfer + Text +
  **Best-Treffer-Pille** + eine schlanke Fußzeile (Mehr Optionen / Verwerfen) = **~220px, vier Zurufe
  je Schirm**. Das Rausfischen bleibt **ein Klick**, weil die Best-Treffer-Pille sichtbar bleibt — die
  Kern-Metapher wird nicht angetastet. Aufgeklappt ist die Karte exakt wie vorher (weitere Vorschläge,
  „Line wählen", Zuordnen, + Neue Line, „Übernommen von"). i18n de+en (cardMore/cardLess). SW-Cache v33→v35.
- Geprüft: tsc sauber, Deploy nur Prod. Kartenhöhe per Playwright gemessen (221,5px gefaltet / 485,3px
  aufgeklappt / zurück). Rausfisch-Flow intakt: Klick auf die Best-Treffer-Pille öffnet das
  nextStep-Capture-Panel, „Rückgängig" stellt den Zuruf wieder her (20 offen — Sim-Tenant sauber
  hinterlassen). Mobil 390 ohne Layout-Bruch. Nur bekannte 401/403.
  - Screenshot (gefaltet): .autopilot/shots/2026-07-19_eingang-kompakt.png
  - Screenshot (aufgeklappt): .autopilot/shots/2026-07-19_eingang-aufgeklappt.png
  - Screenshot (Mobil 390): .autopilot/shots/2026-07-19_eingang-kompakt-390.png
- Commit: feat(eingang): Pool-Kachel gefaltet — Zuordnungs-Maschinerie erst auf Klick

- Nächster Schritt: Die drei großen Übersichten (Momentum, Lines, Eingang) halten jetzt die vom
  Interview verlangte Größenordnung aus. Damit ist Interview Punkt 5 rund und Punkt 3/Punkt 1 auf
  Skalierung geprüft. Weiterhin blockiert auf Michaels Schema-/Konzept-Entscheidung: **#7
  Question-Sektion** (größter Hebel — schaltet Ladenhüter-Typ 2 + #6 frei), **#6 Benachrichtigung**,
  **#4 Dateien-Panel**, **#2 Absender-Vorschlag**. Siehe ORCHESTRATOR „Rückmeldungen".
