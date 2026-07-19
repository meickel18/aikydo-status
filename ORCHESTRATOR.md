> **Stand:** 2026-07-19T12:27Z · Commit f808a0f8 · Lauf-Nr. 12 · automatischer Spiegel, nach jedem Autopilot-Lauf aktualisiert. Cache-Hinweis: raw.githubusercontent kann bis ~5 Min alt sein — diese Zeile zeigt den echten Stand.

# ORCHESTRATOR — Steuerung des AiKydo-Autopilot

Diese Datei hat **Vorrang vor allem anderen**. Der Autopilot liest sie zu Beginn
jedes Laufs. Michael (und Fable) schreiben hier Richtungen rein; der Autopilot
schreibt Rückmeldungen unter „## Rückmeldungen".

Kurzregel: **Oben steht, was zu tun ist. Unten meldet der Autopilot zurück.**

---

## GRUNDLAGE ALLER UI-ARBEIT — Interview-Destillat (Michael, 2026-07-19)

Dies **ersetzt alle Vermutungen** über das Produkt. Ab jetzt ist es die verbindliche
Grundlage jeder UI-Entscheidung. Der verbatim-Text steht weiter unten unvermindert;
zuerst der Kern und die daraus abgeleitete Arbeitsreihenfolge.

**DER KERN:** Fast jeder Task endet mit „Wer kümmert sich drum?" Das Portal ist ein
**Verantwortungs-Verteiler mit Gedächtnis.** Maßstab überall: **schnellstes Erfassen,
einfachste Bedienung.** Soll-Workflow (unverrückbar): **fischen → vorantreiben →
Ergebnis → archivieren.** Kein ständiges Aufpoppen, kein zweiter Posteingang.

**Plattform-Priorität:** Handy = Einwerfen (Teilen-Fluss) + schnelle Fortschritte
unterwegs. Desktop = die große Übersicht, das eigentliche Arbeiten. Übersichten
(Lines, Momentum) für Desktop optimieren, Einwerfen/Nachtragen für Mobil.

**Design (hart, DESIGN-LOCK gilt):** Anthracit/Grau, matt, minimalistisch,
Claude-inspiriert. **KEINE** typische AI-Symbolsprache — keine Roboter-Icons, keine
grünen Häkchen/roten Kreuze, kein Emoji-Kitsch.

### Entscheidung Ladenhüter-Darstellung (Punkt 8 — Simulationsauftrag beantwortet)

**Empfehlung: Momentum ist die ehrliche Heimat der Ladenhüter — plus ein dezenter,
rein persönlicher Hinweis. KEINE eigene „Akut"-Sektion.**

Durchgespielte Varianten und Bewertung (mit Blick auf Aufmerksamkeits-Ablenkung und
Workflow-Kannibalisierung):

- **A) Eigene vierte Sektion „Akut" — VERWORFEN.** Genau Michaels befürchtete Falle:
  eine zweite Warteschlange neben dem Eingang. Die Gruppe arbeitet dann die lauteste
  Liste ab, der gemeinsame Eingangs-Pool (die ausdrückliche Stärke) verwaist. Ein
  Dauer-Alarm zieht Aufmerksamkeit vom Soll-Workflow ab.
- **B) Ladenhüter prominent in Momentum — EMPFOHLEN (Basis).** Momentum ist ohnehin
  der „Zustand der Firma"-Blick (aktiv vs. vernachlässigt, Top-10 in beide Richtungen).
  Liegengebliebenes gehört genau dorthin: eine **reflektierende Fläche, die man
  aufsucht**, kein Interrupt, der einen unterbricht. Erweiterung: die
  „vernachlässigt"-Seite umfasst **alle drei Typen** — liegengebliebene Lines,
  unbeantwortete Ja/Nein-Fragen, **und überalterte, ungefischte Eingangs-Kacheln**.
- **C) Dezenter, persönlicher Hinweis — EMPFOHLEN (Ergänzung).** Nur wenn etwas
  Gealtertes **genau mich** braucht (eine Frage an mich, etwas für mich zum
  Drüberschauen), ein leiser Indikator im Stil Punkt 6 (Instagram-Herz-mit-Punkt).
  Niemals eine geteilte To-do-Liste, niemals Spam.
- **D) Mischform B+C — das ist die Empfehlung.** Momentum trägt den kollektiven Blick
  aufs Liegengebliebene; der persönliche Indikator trägt nur das, was individuell an
  mir hängt. Beide konkurrieren nicht mit dem Eingang, weil keiner eine abzuarbeitende
  Queue neben dem Pool aufmacht.

**Begründung in einem Satz:** Ladenhüter sichtbar machen ohne den Workflow zu
kannibalisieren gelingt, indem man sie in eine **Consult-Fläche (Momentum)** legt
statt in eine **Interrupt-Fläche (Akut-Spalte)** — und das rein Persönliche über den
leisen Kanal aus Punkt 6 zustellt.

**Namensgebung:** NICHT „Probleme". „Ladenhüter" trägt (mercantil, warm); innerhalb
Momentum passt „Liegengeblieben" / „vernachlässigt". Alarmwörter meiden.

### Delta Interview ↔ aktueller Stand (Wo als Nächstes gebaut wird)

Was schon steht (nicht neu bauen, nur im Sinne des Interviews schärfen):
- **Eingang** als Kachel-Pool mit Herkunft/Einwerfer, Rausfischen = 1 Klick, Bestätigung
  + Rückgängig, Vorschlags-Hierarchie (bester Line-Treffer prominent).
- **Lines-Übersicht** mit Momentum-Farbcodierung (liegengeblieben/läuft), Sortierung,
  Verlaufszusammenfassung (`summary`) und `nextStep` je Line.
- **Line-Detail = Storyline** (chronologisch), Stand-Zusammenfassung, Zuruf-Medium je
  Schritt, editierbares `nextStep`, leerer Einstieg „Ersten Schritt festhalten".
- **Momentum-Cockpit** (`/mindmap`) groß, vorzeichenfarbig, Zeilen klickbar → Line.

Vom Interview neu/schärfer verlangt (Backlog, grob nach Wert × Aufwand):
1. **Eingang — beim Rausfischen den nächsten Schritt festhalten** + „unter 2 Min →
   sofort erledigen"-Hinweis (GTD). Schema-frei (`nextStep` existiert). **← nächster Schritt.**
2. **Eingang — Absender-Vorschlag „wer könnte übernehmen"** (kleines optionales Feld,
   Vorschlag nie Zwang; beim Handy-Teilen evtl. nur im Portal). Prüfen, ob Feld nötig →
   ggf. Rückmeldung (Schema-Tabu).
3. **Line-Detail — HORIZONTALE Timeline** (links Initial, nach rechts Schritte mit Datum,
   Hover → Details), **dynamische Skala** (Tage/Wochen/Jahre), **Bereich markieren → zoomen**.
   Größerer Umbau; ersetzt die aktuelle vertikale Schiene. In sinnvolle Teilschritte zerlegen.
4. **Line-Detail — Dokumente/Files-Feld** unter der Zusammenfassung (WhatsApp-Medien-Stil),
   **Upload/Teilen in fremde Lines** (jeder darf Material in jede Line teilen).
5. **Momentum — Ladenhüter integrieren** (Variante B: alle drei Typen auf der
   vernachlässigt-Seite; Top-10 beide Richtungen).
6. **Benachrichtigung** — dezenter persönlicher Indikator (Punkt 6 + Ladenhüter-Variante C).
   Nur echte Aktionen für mich, niemals vollspammen.
7. **Question-Sektion** — schnelle Ja/Nein-Entscheidungen an die Gruppe, sortiert
   offen/beantwortet (Punkt 7; „klein anbieten").

**Regel:** Jeder Punkt fertig + selbst geprüft + committet, **bevor** der nächste beginnt —
pro Lauf so viele Punkte wie sauber machbar (Tempo siehe „## Aktuelle Richtung"). Screenshot-Beleg
im PROGRESS-LOG). Reihenfolge oben ist Vorschlag, nicht Zwang — Michael kann in „##
Aktuelle Richtung" jederzeit umsteuern. Bei echten Weggabelungen/Schema-Bedarf: „##
Rückmeldungen", stoppen.

---

## Aktuelle Richtung

### Tempo (Michael, 2026-07-19) — Vorrang vor jeder „ein Schritt pro Lauf"-Formulierung
**Die Regel „EIN fertiger Schritt pro Lauf" ist aufgehoben.** Nutze das Zeitfenster
(Deckel max 2 Std) **voll aus** — arbeite so viele Backlog-Punkte ab, wie **sauber**
machbar sind. **Qualitäts-Gate bleibt hart:** jeder Punkt fertig + selbst geprüft
(Screenshot) + committet, **bevor** der nächste beginnt. Keine halben Sachen — aber auch
**kein Aufhören nach dem ersten Commit**, solange die Zeit für einen weiteren runden Punkt
reicht. Niemals einen Punkt anfangen, den du nicht mehr sauber prüfen + committen kannst.
Takt: **12 Läufe/Tag, alle 2 Std** (Überlapp-Sperre/flock bleibt — läuft ein Lauf lang,
überspringt der nächste Slot sauber; das ist ok). Rate-/Kontingent-Limits → **selbst
drosseln** (Regel existiert) und unter „## Rate-Limit-Beobachtungen" notieren, dann
justiert Michael.

**Design-Angleichung an bestehende Seiten (Michael, 2026-07-19).** Lines-Übersicht und
Line-Detail sollen sich an bereits gebauten, guten Mustern orientieren — **Muster im Code
lesen und übernehmen, NICHT neu erfinden.** Referenzen sind unten mit Datei + Zeilen
zitiert (bereits recherchiert). DESIGN-LOCK gilt; Momentum-Logik bleibt. Reihenfolge:
**erst Schritt 1, dann Schritt 2** — beide (und weitere Backlog-Punkte) dürfen im selben
Lauf drankommen, sobald der vorige sauber fertig + geprüft + committet ist (siehe Tempo oben).

### Schritt 1 — LINES-ÜBERSICHT (`app/threads/page.tsx`) an die CHATS-Seite angleichen
Zeilen-Mechanik soll sich anfühlen wie `app/chats/page.tsx`. **Referenz + Muster übernehmen:**
- **State-Muster** (chats:45–53): `hoveredRow`, `selected:Set`, `menuOpenFor`, `renameFor` +
  `menuRef` mit Click-outside-Listener (chats:96–104). `isSelectMode = selected.size>0`.
- **Zeilen-Anatomie** (chats:252–346): flex-Row, `gap:0.75rem`, `padding:0.875rem 1rem`,
  `borderBottom:1px solid rgba(255,255,255,0.08)`. **Links festes 20px-Slot** für die
  Checkbox (`.chat-checkbox`, globals.css:1784–1816) — erscheint nur bei Hover ODER
  Select-Mode (kein Layout-Sprung). **Mitte** = klickbarer Zeileninhalt (Titel + Meta).
  **Rechts Kebab** (`MoreHorizontal`) — erscheint nur bei Hover & nicht im Select-Mode.
- **Hover** ist JS-State (nicht CSS): `onMouseEnter/Leave` → Row-Bg `rgba(255,255,255,0.05)`.
- **Kontextmenü-Dropdown** (chats:313–344): `position:absolute; top:100%; right:0`,
  `bg:rgba(30,30,35,0.98)`, `border:1px solid rgba(255,255,255,0.08)`, `radius:12px`,
  `minWidth:200px`, `boxShadow:0 4px 20px rgba(0,0,0,0.3)`, `backdropFilter:blur(8px)`.
  Items = volle Breite, JS-Hover-Bg. Divider vor der destruktiven Aktion (`#ef4444`).
  **Items sinngemäß für Lines:** Auswählen · **Abschließen** (statt Star/Markieren) ·
  Umbenennen (inline wie chats, kein Modal) · **Projekt zuordnen** · Löschen (destruktiv).
- **Select-Mode / Bulk-Bar** (chats:226–242): „alle auswählen" + Zähler + Bulk-Aktionen
  (für Lines sinnvoll: mehrere abschließen / Projekt zuordnen / löschen). `minHeight:32px`
  gegen Layout-Sprung.
- **BLEIBT:** Momentum-Farbcodierung (Links-Akzent per inset box-shadow) + Sortierung
  (liegengeblieben oben) — nur die Zeilen-Mechanik wird chats-like, die Momentum-Optik
  obendrauf. **DESIGN-LOCK-Korrektur:** chats nutzt als Auswahl-/Fallback-Blau hart
  `#3b82f6`/`rgba(59,130,246,.1)` — beim Übernehmen auf Brand-Accent **`#5F74D1`** ziehen.
- Icons (`MoreHorizontal, Check, Edit, Move, Trash`) sind in chats inline-SVG → passende
  in die Lines-Seite kopieren oder in `Icon.tsx` faktorisieren.

### Schritt 2 — LINE-DETAIL (`app/threads/[id]/page.tsx`) rechte Spalte wie Projekt-Detail
Heute einspaltig zentriert (`maxWidth:800px`). Ziel: **Timeline bleibt Hauptbühne links,
rechts eine Panel-Spalte** im Stil von `app/projects/[id]/page.tsx`. **Referenz + Muster:**
- **Layout** (projects:497–500): flex-Row, `gap:2.5rem`, links `flex:1, maxWidth:800px`,
  rechts feste Spalte. **Rechts-Wrapper** (projects:669): `width/minWidth:380px`,
  `border:1px solid rgba(255,255,255,0.08)`, `radius:18px`, `padding:1.4rem`,
  `overflowY:auto`, `height:calc(100vh-4rem)`.
- **Panels = gestapelte Divs** (kein Karten-Wrapper je Panel), je `marginBottom:1.75rem`,
  gleicher **Header** (projects:683): `h3 {margin:0; fontWeight:500; fontSize:0.95rem;
  color:rgba(255,255,255,0.7)}` + rechts ein transparenter Action-Button.
- **Panels für die Line (rechts):** (a) **Stand** = `summary`. (b) **Nächster Schritt** =
  `nextStep` (editierbar; die vorhandene nextStep-Box wandert in dieses Panel).
  (c) **Dateien/Medien** — Muster des Projekt-Files-Panels (projects:731–836): Upload-Button
  im Header + gestrichelte Dropzone + Zeilenliste mit Hover-Checkbox & Aktions-Cluster.
- **MOBIL (Michael-Vorgabe, MUSS neu gebaut werden):** Projekt-Detail hat **keine** Mobile-
  Logik (feste 380px, kein Breakpoint) → hier **selbst** ergänzen: unter einem Breakpoint
  (z.B. `window.innerWidth < 900`, State + resize-Listener) klappt die rechte Spalte
  **unter die Timeline** (Row → Column, volle Breite). Das ist der einzige Punkt, der
  bewusst NICHT kopiert, sondern ergänzt wird.
- **Datenlage prüfen:** Haben Lines/Threads überhaupt eine Datei-/Medien-Anbindung
  (Endpoint/Relation)? Falls nicht, ist das ein Backend-/Schema-Thema → **erst als
  Rückmeldung** notieren und stoppen, nicht im Alleingang bauen. Panels (a)/(b) gehen
  ohne Backend-Eingriff.

### Schritt 3 — NOTIERT FÜR SPÄTER (nicht jetzt bauen)
„Kydos" als kleine Apps/Widgets in der rechten Spalte der Line — Idee **geparkt**.
Wenn es soweit ist: `components/KydoCard.tsx` (`KydoCard`/`KydoGrid`/`KydoEmptyState`) ist
das fertige Reuse-Target.

_(Erledigt & abgelöst: „Eingang — beim Rausfischen den nächsten Schritt festhalten" (GTD,
Backlog #1) ist gebaut/geprüft/deployed, siehe PROGRESS-LOG 04:00-Lauf.)_

## Tabu (zusätzlich zu PROMPT.md / CLAUDE.md „NICHT MACHEN")

- Kein Umbau an Auth/Rollen, Prisma-Schema, Migrationsketten im Alleingang → hier als
  Vorschlag unter „## Rückmeldungen" ablegen und stoppen.
- Nur Branch `autopilot`. Nie main. Kein force-push.

**Nur noch Prod.** AiKydo ist aktuell reine Demo-Werkbank — keine echten Nutzer, nichts
schützenswert. Ab jetzt wird **direkt auf Prod** gebaut, geprüft (Screenshots) und deployed
(app.aikydo.de / api.aikydo.de). Die **Test-Umgebung wird nicht mehr mitbespielt** — sie wird
erst wieder relevant, wenn echte Nutzer live sind. **Einzige Ausnahme:** DB-Migrationen zuerst
kurz auf Test verifizieren (`migrate deploy` gegen kydo_test), dann auf Prod — danach das Feature
komplett auf Prod. Demo-/Sim-Daten weiter als solche markieren (Tenant `sim-team`, „sim:"-Prefix),
damit später EIN Befehl alles Demo+Sim restlos entfernt. Backup/Branch/Commit bleiben Pflicht;
STOP-Schalter und Cron-Deckel bleiben.

---

## Rate-Limit-Beobachtungen

_(Autopilot trägt hier ein, wenn er auf API-/Rate-Limits stößt — Datum, Dienst,
was passiert ist. Grundlage, um die Lauf-Frequenz zu justieren. Aktuell: 12 Läufe/Tag, alle 2h, je max 2h.)_

- _(noch keine)_

---

## Interview-Destillat — Volltext (verbatim, Michael 2026-07-19)

_Wörtliche Grundlage. Bei Zweifel gilt dieser Text vor jeder Paraphrase oben._

DER KERN: Fast jeder Task endet mit "Wer kümmert sich drum?" Das Portal ist ein
Verantwortungs-Verteiler mit Gedächtnis. Maßstab überall: schnellstes Erfassen,
einfachste Bedienung.

1. EINGANG: Quellen vielfältig (Telegram, WhatsApp, Mail, Voice, Text, Bild) —
   perspektivisch via Handy-Teilen-Funktion. Darstellung: Kacheln/Karten mit
   Merkmalen auf einen Blick: Herkunftsquelle + wer es eingeworfen hat (dezente
   Farbcodierung, Inspiration: Grid-UIs mit Eckpunkten/Rahmen — aber simuliere
   selbst, was am schnellsten erfassbar ist). Eher rechteckig, mit Kurzbeschreibung
   (gern Haiku-generiert). Klick → Detail: was, woher, von wem, Anhänge.
   Übernehmen = EIN Klick ("Das ist mein Fall"). Beim Übernehmen: beabsichtigten
   nächsten Schritt festhalten; unter 2 Min → "sofort erledigen"-Hinweis (GTD).
   Gemeinsamer Pool, JEDER kann sich ALLES rausfischen — das ist ausdrücklich
   die Stärke der Gruppe. Der Absender darf optional einen VORSCHLAG dranhängen,
   wer es übernehmen könnte (kleines Feld) — Vorschlag, nie Zwang. (Beim
   Handy-Teilen-Fluss evtl. schwierig → dann nur im Portal möglich, okay.)
   Später denkbar: KI schlägt vor, Kalender — nicht jetzt.

2. BEIM FISCHEN: Line (Storyline) entsteht. Projektordner NUR bei Bedarf/längeren
   Vorhaben, nicht automatisch für jeden Task (sonst 100 leere Ordner).

3. LINES-ÜBERSICHT (Ziel: 30-100 Lines beherrschen): pro Line sofort sichtbar:
   worum geht's, WER bearbeitet, Pflege-Status (kontinuierlich gepflegt vs.
   vernachlässigt — Rahmen/Fleck, best practice). Kategorien/Rankings: Top
   gepflegt, Top vernachlässigt, neueste, älteste — simuliere sinnvolle Ordnung
   als würdest du selbst die Firma führen.

4. LINE-DETAIL: HORIZONTALE Timeline. Links Initial-Ereignis (was, wann, wer
   übernommen), nach rechts die Schritte mit Datum, Hover → Details. Dynamische
   Skala: Tage/Wochen/Jahre je nach Alter. Wunsch: Bereich markieren (z.B. Woche
   16-18) → zoomt in diesen Ausschnitt. DARUNTER: Zusammenfassung (Haiku, gern
   Stichpunkte nach Datum — kein Blabla-Fließtext). DARUNTER: Dokumente/Files-Feld
   (wie WhatsApp-Medienansicht). Jeder kann jede Line einsehen; jeder kann in
   fremde Lines Material teilen/hochladen (Upload-Button in der Line; Teilen-Fluss
   von Handy/Desktop). Lines abschließbar über sauberes Menü.

5. MOMENTUM: zeigt aktiv vs. vernachlässigt; Top-10-Listen beide Richtungen;
   später evtl. nach Mitarbeitern. Ob alle Lines oder nur Extremes: simulieren.

6. BENACHRICHTIGUNG (klein!): dezenter Indikator à la Instagram-Herz-mit-Punkt:
   "für dich geteilt / schau drüber / Frage an dich". NIEMALS vollspammen —
   nur echte Aktionen für mich.

7. VORSCHLAG ZUM PRÜFEN (Michael: "vielleicht später", Fable hält es für kernnah):
   Question-Sektion — schnelle Ja/Nein-Entscheidungen an die Gruppe ("Messe?
   Alle abhaken"), sortiert nach offen/beantwortet. Simulieren, klein anbieten.

8. LADENHÜTER (WICHTIG, als Simulationsauftrag): Liegengebliebenes (Storylines
   UND unbeantwortete Ja/Nein-Fragen UND ungefischte Eingangs-Tasks) muss
   ERKANNT werden — aber Michael sieht selbst die Falle: Eine eigene
   "Akut"-Spalte könnte den natürlichen Workflow kannibalisieren (Leute arbeiten
   nur noch Akut ab, der Eingang verwaist). Der Soll-Workflow bleibt: fischen →
   vorantreiben → Ergebnis → archivieren. KEIN ständiges Aufpoppen.
   DEIN AUFTRAG: Simuliere mehrere Varianten mit deinen Personas (z.B. eigene
   vierte Sektion "Akut" / Ladenhüter nur prominent in Momentum / dezenter
   persönlicher Hinweis / Mischformen). Beachte Aufmerksamkeits-Ablenkung und
   Workflow-Kannibalisierung. Prüfe die Ergebnisse, empfiehl die beste Variante
   mit Begründung in ORCHESTRATOR.md. Name für so eine Sektion: NICHT "Probleme" —
   eher Richtung "Akut", offen für Besseres.

9. PLATTFORM-PRIORITÄT: Handy = Einwerfen (Teilen-Fluss) + schnelle Fortschritte
   in Storylines unterwegs. Desktop = die große Übersicht, das eigentliche
   Arbeiten. Beides muss gehen, aber optimiere die Übersichten (Lines, Momentum)
   für Desktop und das Einwerfen/Nachtragen für Mobil.

DESIGN (hart): Bestehende Designsprache beibehalten (Anthracit/Grau, matt,
minimalistisch, Claude-inspiriert). KEINE typische AI-Symbolsprache: keine
Roboter-Icons, keine grünen Häkchen/roten Kreuze, kein Emoji-Kitsch. DESIGN-LOCK gilt.

BEISPIEL-TASKS für die Simulation (NICHT limitierend, nur Geschmacksrichtung):
10.000 V2A-Schrauben nach PDF-Anforderung — wer kann liefern? / Transporter braucht
TÜV — wer kümmert sich? / Bohrmaschine kaufen — wer sucht Preis? / Marke eintragen /
Photovoltaik-Kostenaufstellung — alle drüberschauen / Messe ja/nein — alle abhaken.

Michael: "Du bist ausdrücklich erlaubt, in AiKydo zu spielen. Ich würde mich
freuen, wenn du Teil des Teams wirst."

---

## Rückmeldungen

_(Der Autopilot schreibt hier Vorschläge, Weggabelungen und Blocker rein —
Format: „Ich habe X gemacht, Vorschlag: … — oder Alternative …". Michael antwortet
unter „## Aktuelle Richtung".)_

- **2026-07-19 — Schritt 2 gebaut (Line-Detail zweispaltig) + Dateien-Panel braucht Schema-Entscheidung.**
  Gebaut: Line-Detail (`/threads/[id]`) ist jetzt zweispaltig im Projekt-Detail-Stil — Timeline
  bleibt Hauptbühne links, rechts eine 380px-Panel-Spalte (Stand + Nächster Schritt editierbar +
  Abschließen). Mobil (< 900px) klappt die Spalte unter die Timeline. Screenshots im PROGRESS-LOG.
  **Weggabelung Dateien/Medien-Panel (c) aus Schritt 2:** Der Thread/die Line hat KEINE Datei-/
  Medien-Anbindung — kein `Attachment`/`ThreadFile`-Modell, keine Relation Thread↔Document, kein
  Upload-Endpoint für Lines (es gibt zwar ein `Document`-Modell + `media`-Controller, aber unabhängig,
  nicht an Threads gehängt). Ein Dateien-Panel im WhatsApp-Medien-Stil (Interview Punkt 4: „jeder darf
  Material in jede Line teilen/hochladen") braucht also **ein neues Modell + Relation + Upload-Endpoint
  = Prisma-Schema-Wechsel** → laut Tabu kein Alleingang. **Deshalb bewusst NICHT gebaut** (Panels (a)
  Stand + (b) Nächster Schritt sind ohne Backend fertig). **Vorschlag zur Entscheidung:** ein Modell
  `ThreadAttachment` (id, tenantId, threadId, uploaderUserId, fileName, mimeType, url/storageKey,
  createdAt) + `POST /threads/:id/attachments` (Upload) + `GET` (Liste) + Storage-Ziel (S3/lokal?).
  Das ist die nächste größere Backend-Entscheidung. Bitte in „## Aktuelle Richtung" freigeben (dann baue
  ich Migration Test→Prod + Panel) oder zurückstellen (dann mache ich Backlog #3 Teilschritt 2 /
  Momentum-Ladenhüter #5 als schemafreie Schritte weiter).
- **2026-07-19 — Schritt 1 gebaut (Lines-Übersicht chats-artig) + „Projekt zuordnen" bewusst offen.**
  Gebaut: `/threads` fühlt sich jetzt an wie `/chats` (Hover-State, Checkbox-Slot ohne Layout-Sprung,
  klickbare Zeile, Kebab-Menü Auswählen/Abschliessen/Umbenennen/Löschen, Select-Mode + Bulk-Bar),
  Momentum-Optik bleibt obendrauf, Auswahl-Akzent auf Brand `#5F74D1`. Backend: `DELETE /threads/:id`
  (löst Zurufe ab, kein Datenverlust). Screenshots im PROGRESS-LOG.
  **Weggabelung „Projekt zuordnen" (im Muster für das Menü/die Bulk-Bar genannt):** Der Thread hat
  zwar schon `projectId String?`, aber ausdrücklich als „Fundament, noch ohne UI" — es gibt NIRGENDS
  eine Anzeige, welche Line zu welchem Projekt gehört. Eine „Projekt zuordnen"-Aktion würde also ein
  unsichtbares Ergebnis erzeugen (nicht per Screenshot abnehmbar) und die Zeile bloß mit einem toten
  Menü-Eintrag + Picker-Modal aufblähen. **Deshalb bewusst NICHT gebaut** (das Qualitäts-Gate verlangt
  ein prüfbares Ergebnis). **Vorschlag:** „Projekt zuordnen" erst bauen, wenn die Projekt↔Line-Zuordnung
  irgendwo sichtbar wird (z.B. Projekt-Badge in der Zeile / Filter nach Projekt / Projekt-Detail listet
  seine Lines). Dann ist es EIN runder Schritt: DTO+Service (`projectId`, kein Schema-Wechsel) + Picker
  (chats-Move-Modal-Muster) + Anzeige. Bitte in „## Aktuelle Richtung" bestätigen/umsteuern.
- **2026-07-19 — Backlog #3 gebaut (horizontale Timeline, Teilschritt 1) + #2 braucht Entscheidung.**
  Gebaut: die Line-Detail-Storyline ist jetzt eine echte HORIZONTALE Timeline (Interview Punkt 4) —
  Start links → Jetzt rechts, Datum/Medium/Kurztext je Knoten, Auswahl → Volltext-Panel darunter.
  Dynamische Zeitskala + Bereich-Zoom folgen als Teilschritte 2/3. Screenshots im PROGRESS-LOG.
  **Weggabelung Backlog #2 (Absender-Vorschlag „wer könnte übernehmen"):** `RawInput` hat schon
  `assignedToUserId`/`assignee`, aber das ist „wer ÜBERNIMMT" (Zuweisung nach dem Rausfischen), NICHT
  der optionale VORSCHLAG des Absenders beim Einwerfen. Außerdem entstehen Zurufe serverseitig
  (`profile.service`) — es gibt keine Einwerf-UI im Portal, an die ein Vorschlagsfeld andocken könnte.
  Sauber umsetzbar wären zwei Wege, beide brauchen Deine Entscheidung: **(A)** Vorschlag in das
  vorhandene `metadata`-JSON am RawInput schreiben (kein Schema-Wechsel) + im Eingang als dezenten
  Hinweis „Vorschlag: {Name}" anzeigen — Nachteil: der Einwerf-Kanal (Handy-Teilen) müsste den Namen
  liefern, den gibt's noch nicht. **(B)** Eigenes Feld `suggestedAssigneeId` am RawInput (Schema-Wechsel
  → Alleingang-Tabu). **Empfehlung:** #2 zurückstellen, bis der Handy-Einwerf-Fluss steht (der Vorschlag
  gehört an dessen Anfang); solange lieber Backlog #3 Teilschritt 2 (dynamische Zeitskala) oder #5
  (Momentum-Ladenhüter). Bitte in „## Aktuelle Richtung" bestätigen/umsteuern.
- **2026-07-19 — Interview-Destillat als Grundlage aufgenommen + Ladenhüter entschieden.**
  Michaels Interview-Destillat ist jetzt oben als „GRUNDLAGE ALLER UI-ARBEIT" verankert
  (Kern, Delta zum Stand, sequenzierter Backlog) und im Volltext verbatim hinterlegt.
  Punkt 8 (Ladenhüter-Simulationsauftrag) ist beantwortet: **Empfehlung B+C** — Ladenhüter
  in Momentum (Consult-Fläche, alle drei Typen) + dezenter persönlicher Hinweis; **keine
  eigene „Akut"-Spalte** (kannibalisiert den Eingang). Begründung siehe oben. Als nächster
  Bau-Schritt vorgeschlagen: Eingang — beim Rausfischen den nächsten Schritt festhalten
  (schema-frei). Bitte in „## Aktuelle Richtung" bestätigen oder umsortieren.
- **2026-07-18 — ✅ ENTSCHIEDEN + GEBAUT: „nextStep"-Feld je Line.** Michael hat das dedizierte
  Feld freigegeben. Umgesetzt: `nextStep String?` auf `Thread` (Migration `20260718_add_thread_nextstep`,
  Test→Prod verifiziert), im Line-Detail editierbar (Accent-Box, GTD-Nächster-Schritt), auf der
  Übersicht als „→ nächster Schritt" (Accent, eine Zeile). Damit ist Punkt 2 der aktuellen Richtung
  erfüllt. Ursprüngliche Rückmeldung zur Nachvollziehbarkeit unten belassen:
- **2026-07-18 — „nextStep"-Feld je Line (Entscheidung nötig).** Punkt 2 der aktuellen
  Richtung („nächster kleiner Schritt je Line auf der Übersicht sichtbar") lässt sich nicht
  sauber ableiten: Ein echtes `nextStep` existiert NIRGENDS (kein Prisma-Feld, kein
  Code); die Detail-Timeline ist nur eine chronologische Zuruf-Liste, keine
  GTD-Nächster-Schritt-Angabe. Ein truthful GTD-nextStep braucht ein eigenes,
  vom Nutzer/AI gepflegtes Feld → das ist ein Prisma-Schema-Wechsel und damit
  Alleingang-Tabu. **Vorschlag:** neues Feld `nextStep String?` auf `Thread`
  (nullable, kein Default), im Detail editierbar (wie `blockedBy`), auf der Übersicht
  als eine Zeile „→ nächster Schritt" anzeigbar. **Alternative (ohne Schema, bereits
  gebaut):** die vorhandene `summary`-Verlaufszusammenfassung je Line auf der Übersicht
  sichtbar gemacht — deckt „worum geht's / wo steht's auf einen Blick" ab, ist aber
  bewusst KEIN GTD-nextStep. Bitte entscheiden, ob das dedizierte Feld gebaut werden soll
  (dann Migration nötig: `prisma migrate deploy` Test→Prod) oder ob die Zusammenfassung
  als Ersatz reicht.
