---
name: wiki-lint
description: Führe einen Gesundheitscheck des Wikis durch.
version: 1.0.0
metadata:
  hermes:
    tags: [wiki, knowledge, maintenance]
    category: wiki
    requires_toolsets: [terminal]
---

# wiki-lint

Führe einen Gesundheitscheck des Wikis durch.

## When to Use

- User fordert Lint, Health-Check oder Qualitätsprüfung an
- Periodisch via Cron (z.B. wöchentlich)
- Nach einer größeren Ingest-Serie

## Procedure

1. **Alle Wiki-Seiten lesen** — `wiki/index.md` und dann alle referenzierten Seiten durchgehen.

2. **Prüfungen durchführen:**

   **Widersprüche** — Behauptungen die sich zwischen Seiten widersprechen. Prüfe besonders Zahlen, Daten und Einschätzungen.

   **Stale Content** — Seiten deren `updated`-Datum weit zurückliegt oder deren Aussagen durch neuere Quellen überholt sein könnten. Setze `status: stale` im Frontmatter.

   **Orphan Pages** — Seiten auf die keine andere Seite verlinkt. Jede Seite sollte mindestens einen eingehenden Link haben.

   **Missing Pages** — Konzepte, Firmen oder Trends die auf mehreren Seiten erwähnt werden, aber keine eigene Seite haben.

   **Missing Cross-References** — Seiten die thematisch zusammenhängen, aber nicht aufeinander verlinken.

   **Datenlücken** — Wichtige Themen die nur auf einer oder keiner Quelle basieren.

   **Index-Konsistenz** — Seiten die existieren aber nicht im Index stehen, oder Index-Einträge die auf nicht-existente Seiten zeigen.

3. **Bericht erstellen** — Ergebnisse dem User präsentieren, gruppiert nach Kategorie.

4. **Log schreiben** — Eintrag in `wiki/log.md`:
   ```markdown
   ## [YYYY-MM-DD] lint | Gesundheitscheck
   - Geprüft: X Seiten
   - Widersprüche: X gefunden
   - Orphan Pages: X
   - Missing Pages: X
   - Empfehlungen: ...
   ```

5. **Fixes vorschlagen** — Dem User konkrete Vorschläge machen:
   - Welche Querverweise fehlen
   - Welche neuen Seiten angelegt werden sollten
   - Welche Quellen gesucht werden sollten um Lücken zu schließen

## Pitfalls

- Keine Änderungen ohne Rücksprache — nur Bericht und Vorschläge
- Bei Widersprüchen: beide Positionen darstellen, nicht eigenmächtig eine wählen

## Verification

- Alle Wiki-Seiten wurden geprüft
- Bericht wurde dem User präsentiert
- Log-Eintrag wurde geschrieben
