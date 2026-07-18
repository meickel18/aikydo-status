# ORCHESTRATOR — Steuerung des AiKydo-Autopilot

Diese Datei hat **Vorrang vor allem anderen**. Der Autopilot liest sie zu Beginn
jedes Laufs. Michael (und Fable) schreiben hier Richtungen rein; der Autopilot
schreibt Rückmeldungen unter „## Rückmeldungen".

Kurzregel: **Oben steht, was zu tun ist. Unten meldet der Autopilot zurück.**

---

## Aktuelle Richtung

**Fokus: Momentum-Übersicht schärfen.**
Die Momentum-Ansicht ist die Signatur des Portals: alle Lines auf einen Blick,
„liegengeblieben" vs. „läuft gut". Genau das muss visuell + kognitiv in <2 Sek.
erfassbar sein. Nächste kleine Schritte in dieser Richtung:

1. Liegengebliebene Lines müssen **herausstechen** — klare visuelle Hierarchie /
   Farbcodierung nach Momentum-Score (heute=+100 … 14T=-100), ohne DESIGN-LOCK zu brechen.
2. Der nächste kleine Schritt je Line (GTD-Geist) sollte auf der Übersicht sichtbar sein.
3. Storyline-Timeline (links Start → rechts nächster Schritt) auf schnelle Lesbarkeit prüfen.

Immer: **ein** fertiger, selbst geprüfter Schritt pro Lauf. Kein Klein-Klein, keine
halben Sachen. Screenshot-Beleg im PROGRESS-LOG.

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
was passiert ist. Grundlage, um die Lauf-Frequenz zu justieren. Aktuell: 6 Läufe/Tag, alle 4h, je max 2h.)_

- _(noch keine)_

---

## Rückmeldungen

_(Der Autopilot schreibt hier Vorschläge, Weggabelungen und Blocker rein —
Format: „Ich habe X gemacht, Vorschlag: … — oder Alternative …". Michael antwortet
unter „## Aktuelle Richtung".)_

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
