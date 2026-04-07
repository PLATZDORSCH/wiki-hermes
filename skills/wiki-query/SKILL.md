---
name: wiki-query
description: Beantworte eine Frage auf Basis des kompilierten Wiki-Wissens.
version: 1.0.0
metadata:
  hermes:
    tags: [wiki, knowledge, query]
    category: wiki
    requires_toolsets: [terminal]
---

# wiki-query

Beantworte eine Frage auf Basis des kompilierten Wiki-Wissens.

## When to Use

- User stellt eine inhaltliche Frage zur Branche
- User will eine Einschätzung, Analyse oder Einordnung
- User sagt "was wissen wir über...", "wie steht es um..." o.ä.

## Procedure

1. **Index lesen** — `wiki/index.md` lesen, um relevante Seiten zu identifizieren.

2. **Relevante Seiten lesen** — Alle thematisch passenden Wiki-Seiten lesen. Bei Unsicherheit lieber zu viele als zu wenige.

3. **Antwort synthetisieren** — Antwort formulieren mit:
   - Verweisen auf Wiki-Seiten als Belege
   - Kennzeichnung wenn etwas auf wenigen Quellen basiert
   - Markierung von Widersprüchen oder Unsicherheiten

4. **Speichern prüfen** — Falls die Antwort substanziell ist (Analyse, Vergleich, neue Erkenntnis): als neue Seite in `wiki/synthesis/` speichern:
   ```yaml
   ---
   title: "Analyse: Thema"
   category: synthesis
   created: YYYY-MM-DD
   updated: YYYY-MM-DD
   tags: [relevante, tags]
   sources: [referenzierte-seiten.md]
   status: active
   ---
   ```
   Die Seite braucht einen "Verwandte Seiten"-Abschnitt mit mindestens 2 bidirektionalen Links.

5. ⛔ **Index, Overview und Log aktualisieren** — Falls eine neue Seite erstellt wurde:
   - `wiki/index.md` → neuen Eintrag unter Synthesis
   - `wiki/overview.md` → falls die Analyse neue Erkenntnisse enthält
   - `wiki/log.md` → Eintrag mit `query | Fragetitel`

## Pitfalls

- Immer zuerst den Index lesen, nie direkt raten
- Wenn das Wiki keine ausreichende Antwort hergibt: offen sagen und Quellenlücken benennen
- Triviale Fragen müssen nicht als Synthesis-Seite gespeichert werden
- Falls gespeichert: Index, Overview und Log NIEMALS vergessen

## Verification

⛔ BEVOR du den Workflow als abgeschlossen meldest:

1. Antwort basiert auf tatsächlichen Wiki-Seiten, nicht auf Allgemeinwissen
2. Falls gespeichert: neue Seite hat korrektes Frontmatter UND "Verwandte Seiten"-Abschnitt
3. Falls gespeichert: `wiki/index.md` enthält die neue Seite
4. Falls gespeichert: `wiki/log.md` hat einen Eintrag
5. Erst dann dem User Abschluss melden.
