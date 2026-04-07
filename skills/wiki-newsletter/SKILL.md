---
name: wiki-newsletter
description: Erstelle einen Wochenrückblick aus den letzten Wiki-Aktivitäten.
version: 1.0.0
metadata:
  hermes:
    tags: [wiki, content, newsletter]
    category: wiki
    requires_toolsets: [terminal]
---

# wiki-newsletter

Erstelle einen Wochenrückblick aus den letzten Wiki-Aktivitäten.

## When to Use

- User fordert Newsletter oder Wochenrückblick an
- Wöchentlich via Cron
- User sagt "was ist diese Woche passiert", "Wochenupdate" o.ä.

## Procedure

1. **Log scannen** — `wiki/log.md` lesen und alle Einträge der letzten 7 Tage identifizieren.

2. **Neue und aktualisierte Seiten prüfen** — Alle Seiten deren `updated`-Datum in den letzten 7 Tagen liegt, lesen. Besonders `wiki/trends/` auf Veränderungen im Momentum prüfen.

3. **Template laden** — `content/templates/newsletter.md` laden.

4. **Newsletter schreiben** — In `content/drafts/` als `YYYY-MM-DD-newsletter-kwXX.md`:
   - 3–5 Top-Themen der Woche mit Einordnung
   - Trend-Update-Tabelle (was ist gestiegen, was gefallen)
   - 1–2 Themen "auf dem Radar" (schwache Signale)
   - Frontmatter:
     ```yaml
     ---
     title: "Wochenrückblick KW XX"
     format: newsletter
     based_on: [verarbeitete-quellen-der-woche]
     created: YYYY-MM-DD
     status: draft
     ---
     ```

5. **Log schreiben** — Eintrag in `wiki/log.md`:
   ```markdown
   ## [YYYY-MM-DD] newsletter | Wochenrückblick KW XX
   - Zeitraum: YYYY-MM-DD bis YYYY-MM-DD
   - Neue Quellen: X
   - Aktualisierte Seiten: X
   - Gespeichert: content/drafts/YYYY-MM-DD-newsletter-kwXX.md
   ```

## Pitfalls

- Nur Fakten aus dem Wiki verwenden, nichts erfinden
- Falls keine Aktivitäten in der Woche: kurze Meldung statt leeren Newsletter
- Immer Einordnung liefern, nicht nur auflisten

## Verification

- Newsletter-Entwurf existiert in `content/drafts/`
- Alle referenzierten Daten stammen aus tatsächlichen Wiki-Seiten
- Log-Eintrag wurde geschrieben
