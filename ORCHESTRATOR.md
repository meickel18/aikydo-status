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
- Nur Branch `autopilot`, nur Test-Umgebung. Nie Prod. Nie main.

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

- _(noch keine)_
