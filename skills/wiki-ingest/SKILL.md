# wiki-ingest

Verarbeite eine neue Quelle und integriere das Wissen ins Wiki.

## Wann nutzen

- Neue Datei wurde in `raw/` abgelegt
- User gibt eine URL oder Text zum Verarbeiten
- User sagt "ingest", "verarbeite", "lies das" o.ä.

## Eingabe

- Pfad zu einer Datei in `raw/` ODER
- URL zum Scrapen ODER
- Text direkt vom User

## Schritte

1. **Quelle lesen** — Datei lesen, URL scrapen oder Text übernehmen. Bei PDFs: Text extrahieren.

2. **Aktuellen Wissensstand erfassen** — `wiki/index.md` lesen, um zu verstehen was bereits im Wiki existiert.

3. **Summary erstellen** — Neue Seite in `wiki/sources/` anlegen:
   ```yaml
   ---
   title: "Titel der Quelle"
   category: sources
   created: YYYY-MM-DD
   updated: YYYY-MM-DD
   tags: [relevante, tags]
   source_type: article|pdf|note|web
   source_path: raw/articles/dateiname.md
   ingested: YYYY-MM-DD
   status: active
   ---
   ```
   Danach: Zusammenfassung der Kernaussagen, wichtigste Fakten, Zitate.

4. **Kernaussagen mit User besprechen** — Kurz die 3–5 wichtigsten Takeaways auflisten. Fragen ob der User Schwerpunkte setzen will.

5. **Bestehende Wiki-Seiten aktualisieren** — Für jede relevante bestehende Seite:
   - Neue Informationen einarbeiten
   - Widersprüche mit `> ⚠️ Widerspruch: ...` markieren
   - Neue Querverweise setzen (relative Markdown-Links)

6. **Neue Seiten anlegen** — Falls die Quelle Themen, Trends, Player oder Konzepte enthält die noch keine eigene Seite haben, neue Seiten erstellen in den passenden Unterordnern. Frontmatter-Konventionen aus `AGENTS.md` beachten.

7. **Index aktualisieren** — Alle neuen Seiten in `wiki/index.md` eintragen. Format: `- [Titel](pfad) — Kurzbeschreibung (X Quellen)`

8. **Overview prüfen** — Falls die Quelle wesentliche neue Erkenntnisse enthält, `wiki/overview.md` aktualisieren.

9. **Log schreiben** — Eintrag in `wiki/log.md` (append):
   ```markdown
   ## [YYYY-MM-DD] ingest | Titel der Quelle
   - Quelle: raw/articles/dateiname.md
   - Erstellt: wiki/sources/..., wiki/topics/...
   - Aktualisiert: wiki/trends/..., wiki/players/...
   - Zusammenfassung: Kernaussage in einem Satz.
   ```

## Regeln

- Dateien in `raw/` NIEMALS verändern
- Dateinamen: lowercase, Bindestriche, keine Umlaute (ä→ae, ö→oe, ü→ue, ß→ss)
- Wiki-Inhalte auf Deutsch, Frontmatter-Keys auf Englisch
- Eine Quelle kann 5–15 Wiki-Seiten betreffen — lieber zu viele Seiten aktualisieren als zu wenige
