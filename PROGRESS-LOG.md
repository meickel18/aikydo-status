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
