# wiki-draft

Erstelle einen Content-Entwurf auf Basis des Wiki-Wissens.

## Wann nutzen

- User will einen Blogartikel, ein Briefing oder LinkedIn-Post schreiben
- User sagt "schreib mir einen Artikel über...", "erstelle ein Briefing zu..." o.ä.

## Eingabe

- Thema
- Format: blog, newsletter, briefing, linkedin, slides
- Optional: Zielgruppe, Tonalität, Länge

## Schritte

1. **Thema und Format klären** — Falls nicht eindeutig, kurz nachfragen.

2. **Wiki durchsuchen** — `wiki/index.md` lesen, relevante Seiten identifizieren und lesen. Besonders `wiki/synthesis/` und `wiki/trends/` prüfen.

3. **Template laden** — Passendes Template aus `content/templates/` laden:
   - `content/templates/blog.md` für Blogartikel
   - `content/templates/newsletter.md` für Newsletter
   - `content/templates/briefing.md` für Briefings
   - Für LinkedIn/Slides: freie Form

4. **Entwurf schreiben** — In `content/drafts/` mit Frontmatter:
   ```yaml
   ---
   title: "Titel des Contents"
   format: blog
   based_on: [wiki/trends/thema.md, wiki/topics/thema.md]
   created: YYYY-MM-DD
   status: draft
   ---
   ```
   Dateiname: `YYYY-MM-DD-kurztitel.md`

5. **User informieren** — Entwurf dem User zeigen, auf Stellen hinweisen die Review brauchen.

6. **Log schreiben** — Eintrag in `wiki/log.md`:
   ```markdown
   ## [YYYY-MM-DD] draft | Titel des Contents
   - Format: blog
   - Basiert auf: wiki/trends/..., wiki/topics/...
   - Gespeichert: content/drafts/YYYY-MM-DD-kurztitel.md
   ```

## Regeln

- Entwürfe immer in `content/drafts/`, nie direkt in `content/published/`
- Immer das Wiki als Basis nutzen, nicht frei erfinden
- Quellenverweise einbauen wo möglich
- Tonalität an die Zielgruppe anpassen
