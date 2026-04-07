---
name: wiki-ingest
description: Verarbeite eine neue Quelle und integriere das Wissen ins Wiki.
version: 1.0.0
metadata:
  hermes:
    tags: [wiki, knowledge, ingest]
    category: wiki
    requires_toolsets: [terminal]
---

# wiki-ingest

Verarbeite eine neue Quelle und integriere das Wissen ins Wiki.

## When to Use

- Neue Datei wurde in `raw/` abgelegt
- User gibt eine URL oder Text zum Verarbeiten
- User sagt "ingest", "verarbeite", "lies das" o.ä.

## Procedure

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

7. **Verwandte Seiten verlinken** — Jede neue/aktualisierte Seite braucht einen "Verwandte Seiten"-Abschnitt am Ende mit mindestens 2 Links. Verlinkungen sind bidirektional — wenn A auf B verlinkt, MUSS B auch auf A verlinken. In bestehenden Seiten Rückverweise ergänzen.

8. ⛔ **Index aktualisieren** — `wiki/index.md` LESEN und PRÜFEN. Alle neuen Seiten eintragen. Format: `- [Titel](pfad) — Kurzbeschreibung (X Quellen)`

9. ⛔ **Overview aktualisieren** — `wiki/overview.md` LESEN und AKTUALISIEREN. Neue Themen, Trends, Akteure, Marktdaten einarbeiten. Quellenzähler hochsetzen. NICHT OPTIONAL.

10. ⛔ **Log schreiben** — Eintrag in `wiki/log.md` ANFÜGEN (append-only):
    ```markdown
    ## [YYYY-MM-DD] ingest | Titel der Quelle
    - Quelle: raw/articles/dateiname.md
    - Erstellt: wiki/sources/..., wiki/topics/...
    - Aktualisiert: wiki/trends/..., wiki/players/...
    - Zusammenfassung: Kernaussage der Quelle in einem Satz.
    ```

## Pitfalls

- Dateien in `raw/` NIEMALS verändern
- Dateinamen: lowercase, Bindestriche, keine Umlaute (ä→ae, ö→oe, ü→ue, ß→ss)
- Wiki-Inhalte auf Deutsch, Frontmatter-Keys auf Englisch
- Eine Quelle kann 5–15 Wiki-Seiten betreffen — lieber zu viele Seiten aktualisieren als zu wenige
- Schritte 8–10 (Index, Overview, Log) NIEMALS überspringen

## Verification

⛔ BEVOR du den Workflow als abgeschlossen meldest, prüfe JEDEN dieser Punkte:

1. Lies `wiki/index.md` — sind ALLE neuen Seiten eingetragen? Falls nein: JETZT nachtragen.
2. Lies `wiki/overview.md` — spiegelt sie die neuen Erkenntnisse wider? Falls nein: JETZT aktualisieren.
3. Lies `wiki/log.md` — gibt es einen Eintrag für diesen Ingest? Falls nein: JETZT schreiben.
4. Haben alle neuen/aktualisierten Seiten einen "Verwandte Seiten"-Abschnitt mit mindestens 2 bidirektionalen Links?
5. Haben alle neuen Seiten korrektes YAML-Frontmatter?
6. Erst dann dem User Abschluss melden.
