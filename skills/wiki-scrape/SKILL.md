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

5. **User informieren** — Kurze Bestätigung:
   - Titel und Dateiname
   - Geschätzte Länge (Wörter)
   - Fragen ob sofort ingestet werden soll

6. **Optional: Ingest anstoßen** — Falls der User zustimmt, direkt den wiki-ingest Skill ausführen mit der neuen Datei.

## Pitfalls

- Paywalled oder Login-geschützte Seiten können nicht gescrapt werden — User darauf hinweisen
- Sehr lange Artikel vollständig speichern, nicht kürzen — das Kürzen passiert beim Ingest
- PDFs nicht über diesen Skill verarbeiten — dafür direkt in `raw/pdfs/` ablegen
- Cookie-Consent-Banner, Werbung und Navigation sauber entfernen
- Bei Scraping-Fehlern den User informieren und alternative Vorgehensweisen vorschlagen (z.B. manuell kopieren nach `raw/notes/`)

## Verification

- Markdown-Datei existiert in `raw/articles/`
- Datei hat korrektes Frontmatter mit URL und Datum
- Inhalt ist sauberes Markdown ohne HTML-Artefakte
- Dateiname folgt der Konvention `YYYY-MM-DD-kurztitel.md`
