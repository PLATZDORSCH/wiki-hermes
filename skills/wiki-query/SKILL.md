# wiki-query

Beantworte eine Frage auf Basis des kompilierten Wiki-Wissens.

## Wann nutzen

- User stellt eine inhaltliche Frage zur Branche
- User will eine Einschätzung, Analyse oder Einordnung
- User sagt "was wissen wir über...", "wie steht es um..." o.ä.

## Schritte

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

5. **Index und Log aktualisieren** — Falls eine neue Seite erstellt wurde:
   - `wiki/index.md` → neuen Eintrag unter Synthesis
   - `wiki/log.md` → Eintrag mit `query | Fragetitel`

## Regeln

- Immer zuerst den Index lesen, nie direkt raten
- Wenn das Wiki keine ausreichende Antwort hergibt: das offen sagen und ggf. Quellenlücken benennen
- Triviale Fragen müssen nicht als Synthesis-Seite gespeichert werden
