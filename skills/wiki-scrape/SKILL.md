---
name: wiki-scrape
description: Scrape eine URL, konvertiere den Inhalt zu Markdown und speichere ihn als Quelle in raw/.
version: 1.0.0
metadata:
  hermes:
    tags: [wiki, knowledge, scraping, web]
    category: wiki
    requires_toolsets: [terminal]
---

# wiki-scrape

Scrape eine URL, konvertiere den Inhalt zu Markdown und speichere ihn als Quelle in `raw/articles/`.

## When to Use

- User gibt eine URL und will den Inhalt ins Wiki aufnehmen
- User sagt "lies diesen Artikel", "scrape das", "nimm das auf" o.ä.
- User teilt einen Link per Telegram, Discord oder Chat

## Procedure

1. **URL entgegennehmen** — URL vom User übernehmen. Falls mehrere URLs: einzeln nacheinander verarbeiten.

2. **Inhalt abrufen** — URL fetchen und Inhalt zu sauberem Markdown konvertieren:
   - HTML-Boilerplate entfernen (Navigation, Footer, Ads, Cookie-Banner)
   - Hauptinhalt extrahieren (Artikel-Body)
   - Überschriften, Listen, Links, Bilder und Code-Blöcke beibehalten
   - Bilder-URLs als Markdown-Links belassen (nicht herunterladen)

3. **Metadaten extrahieren** — Aus der Seite extrahieren:
   - Titel
   - Autor (falls vorhanden)
   - Veröffentlichungsdatum (falls vorhanden)
   - URL als Quellenangabe

4. **Als Markdown speichern** — Datei in `raw/articles/` ablegen:
   - Dateiname: `YYYY-MM-DD-kurztitel.md` (lowercase, Bindestriche, keine Umlaute)
   - Frontmatter am Anfang der Datei:
     ```yaml
     ---
     title: "Originaltitel des Artikels"
     url: "https://example.com/artikel"
     author: "Name"
     published: YYYY-MM-DD
     scraped: YYYY-MM-DD
     ---
     ```
   - Danach der konvertierte Markdown-Inhalt

5. **User kurz informieren** — Titel, Dateiname und geschätzte Länge (Wörter) bestätigen.

6. ⛔ **Ingest ausführen** — Direkt den vollständigen wiki-ingest Workflow ausführen (Schritte 1–10 aus wiki-ingest). Der Ingest ist der DEFAULT — nur überspringen wenn der User explizit sagt, dass er nicht ingestieren will. Scrape ohne Ingest ist ein halber Job.

## Pitfalls

- Paywalled oder Login-geschützte Seiten können nicht gescrapt werden — User darauf hinweisen
- Sehr lange Artikel vollständig speichern, nicht kürzen — das Kürzen passiert beim Ingest
- PDFs nicht über diesen Skill verarbeiten — dafür direkt in `raw/pdfs/` ablegen
- Cookie-Consent-Banner, Werbung und Navigation sauber entfernen
- Bei Scraping-Fehlern den User informieren und alternative Vorgehensweisen vorschlagen (z.B. manuell kopieren nach `raw/notes/`)
- ⛔ NIEMALS den Ingest-Schritt vergessen — Scrape ohne Ingest lässt die Quelle unverarbeitet in raw/ liegen

## Verification

⛔ BEVOR du den Workflow als abgeschlossen meldest, prüfe JEDEN dieser Punkte:

1. Markdown-Datei existiert in `raw/articles/` mit korrektem Frontmatter
2. Inhalt ist sauberes Markdown ohne HTML-Artefakte
3. Dateiname folgt der Konvention `YYYY-MM-DD-kurztitel.md`
4. Falls Ingest ausgeführt: ALLE Ingest-Verification-Punkte prüfen (index.md, overview.md, log.md, Verlinkungen)
5. Erst dann dem User Abschluss melden.
