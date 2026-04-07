# wiki-hermes

Ein LLM-gepflegtes Branchen-Wiki nach dem [LLM Wiki Pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f). Das Wiki wird ausschließlich von LLM-Agenten geschrieben und gepflegt. Der Mensch kuratiert Quellen, stellt Fragen und gibt die Richtung vor.

---

## Architektur

```
raw/          → Unveränderliche Quelldokumente (Input)
wiki/         → LLM-generierte Wiki-Seiten (Verarbeitung)
content/      → Abgeleiteter Content für Veröffentlichung (Output)
```

### raw/ — Quellen

Immutable. Der Agent liest, aber verändert niemals Dateien in `raw/`.

| Unterordner | Inhalt |
|---|---|
| `raw/articles/` | Web-Artikel als Markdown |
| `raw/pdfs/` | PDF-Dokumente |
| `raw/notes/` | Eigene Notizen, Gesprächsnotizen, Ideen |

### wiki/ — Wissensbasis

Der Agent besitzt diesen Layer vollständig. Er erstellt Seiten, aktualisiert sie, pflegt Querverweise und hält alles konsistent.

| Unterordner | Inhalt |
|---|---|
| `wiki/topics/` | Fachthemen — Technologien, Methoden, Prozesse |
| `wiki/trends/` | Aufkommende Entwicklungen, schwache Signale, Prognosen |
| `wiki/regulation/` | Gesetze, Normen, Standards, Compliance |
| `wiki/market/` | Marktdaten, Segmente, Wachstumsbereiche, Preismodelle |
| `wiki/players/` | Firmen, Verbände, Personen, Forschungseinrichtungen, Partner |
| `wiki/sources/` | Eine Summary-Seite pro verarbeiteter Quelle |
| `wiki/synthesis/` | Eigene Analysen, Querverbindungen, Schlussfolgerungen |

Sonderdateien:

| Datei | Zweck |
|---|---|
| `wiki/index.md` | Katalog aller Wiki-Seiten mit Kategorie, Link und Kurzbeschreibung |
| `wiki/log.md` | Chronologisches Protokoll aller Operationen (append-only) |
| `wiki/overview.md` | High-Level-Branchenübersicht — das "Big Picture" |

### content/ — Output

Abgeleiteter Content für Veröffentlichung. Wird aus Wiki-Seiten generiert.

| Unterordner | Inhalt |
|---|---|
| `content/drafts/` | LLM-generierte Entwürfe (vor Review) |
| `content/published/` | Freigegebene, finale Inhalte |
| `content/templates/` | Vorlagen für wiederkehrende Formate |

---

## Konventionen

### Dateinamen

- Lowercase, Wörter mit Bindestrich: `kuenstliche-intelligenz.md`
- Keine Umlaute in Dateinamen: ä→ae, ö→oe, ü→ue, ß→ss
- Keine Sonderzeichen, keine Leerzeichen

### Frontmatter

Jede Wiki-Seite beginnt mit YAML-Frontmatter:

```yaml
---
title: "Künstliche Intelligenz im Maschinenbau"
category: topics          # topics|trends|regulation|market|players|sources|synthesis
created: 2026-04-06
updated: 2026-04-06
tags: [ki, maschinenbau, automatisierung]
sources: [quelle-1.md, quelle-2.md]
status: active            # active|draft|stale|archived
---
```

Für `sources/`-Seiten zusätzlich:

```yaml
source_type: article      # article|pdf|note|web
source_path: raw/articles/beispiel.md
ingested: 2026-04-06
```

Für `trends/`-Seiten zusätzlich:

```yaml
first_seen: 2026-04-06
momentum: rising          # rising|stable|declining|emerging
relevance: high           # high|medium|low
```

Für `players/`-Seiten zusätzlich:

```yaml
player_type: company      # company|person|association|research|partner
```

### Verlinkung

- Relative Markdown-Links: `[Künstliche Intelligenz](../topics/kuenstliche-intelligenz.md)`
- Jede Seite sollte mindestens einen eingehenden und einen ausgehenden Link haben
- Bei Widersprüchen zwischen Seiten: explizit markieren mit `> ⚠️ Widerspruch: ...`

### Sprache

- Wiki-Inhalte auf **Deutsch**
- Fachbegriffe dürfen englisch bleiben, wenn sie in der Branche üblich sind
- Frontmatter-Keys auf Englisch

---

## Workflows

### 1. Ingest — Neue Quelle verarbeiten

Auslöser: Neue Datei in `raw/` oder User gibt URL/Text zum Verarbeiten.

Schritte:

1. Quelle lesen und verstehen
2. `wiki/index.md` lesen — aktuellen Wissensstand erfassen
3. Summary-Seite erstellen in `wiki/sources/` mit Frontmatter
4. Kernaussagen extrahieren und mit User besprechen
5. Relevante bestehende Wiki-Seiten identifizieren und aktualisieren:
   - Neue Informationen einarbeiten
   - Widersprüche zu bestehenden Aussagen markieren
   - Neue Querverweise setzen
6. Bei Bedarf neue Seiten anlegen in `wiki/topics/`, `wiki/trends/`, `wiki/players/` etc.
7. `wiki/index.md` aktualisieren — neue Seiten eintragen
8. `wiki/overview.md` bei wesentlichen neuen Erkenntnissen aktualisieren
9. Eintrag in `wiki/log.md` schreiben

Hinweis: Eine einzelne Quelle kann 5–15 Wiki-Seiten betreffen.

### 2. Query — Fragen beantworten

Auslöser: User stellt eine Frage zum Branchenwissen.

Schritte:

1. `wiki/index.md` lesen — relevante Seiten identifizieren
2. Relevante Wiki-Seiten lesen
3. Antwort synthetisieren mit Verweisen auf Wiki-Seiten
4. Bei hochwertigen Antworten: als neue Seite in `wiki/synthesis/` speichern
5. `wiki/index.md` und `wiki/log.md` aktualisieren

### 3. Lint — Gesundheitscheck

Auslöser: User fordert Lint an, oder periodisch via Cron.

Prüfungen:

- **Widersprüche**: Behauptungen die sich zwischen Seiten widersprechen
- **Stale Content**: Seiten deren Aussagen durch neuere Quellen überholt sind
- **Orphan Pages**: Seiten ohne eingehende Links
- **Missing Pages**: Konzepte die erwähnt aber nicht als eigene Seite existieren
- **Missing Cross-References**: Seiten die thematisch zusammenhängen aber nicht verlinkt sind
- **Datenlücken**: Wichtige Themen mit wenig Quellenmaterial

Ergebnis als Bericht in `wiki/log.md` dokumentieren.

### 4. Draft — Content-Entwurf erstellen

Auslöser: User will Content für Veröffentlichung.

Schritte:

1. Thema und Format klären (Blog, Newsletter, Briefing, LinkedIn, Slides)
2. Relevante Wiki-Seiten lesen
3. Template aus `content/templates/` laden (falls vorhanden)
4. Entwurf erstellen in `content/drafts/` mit Frontmatter:
   ```yaml
   ---
   title: "..."
   format: blog            # blog|newsletter|briefing|linkedin|slides
   based_on: [wiki/trends/ki-agenten.md, wiki/topics/automatisierung.md]
   created: 2026-04-06
   status: draft            # draft|review|approved|published
   ---
   ```
5. Eintrag in `wiki/log.md`

### 5. Newsletter — Wochenrückblick

Auslöser: User fordert Newsletter an, oder wöchentlich via Cron.

Schritte:

1. `wiki/log.md` scannen — Aktivitäten der letzten 7 Tage
2. Neue/aktualisierte Trend-Seiten prüfen
3. Newsletter-Entwurf in `content/drafts/` erstellen
4. Format: 3–5 Highlights mit Einordnung, Links zu Wiki-Seiten als Referenz

---

## Index-Format

Die `wiki/index.md` ist der zentrale Katalog. Format:

```markdown
## Topics
- [Künstliche Intelligenz](topics/kuenstliche-intelligenz.md) — Überblick KI-Anwendungen in der Branche (3 Quellen)

## Trends
- [KI-Agenten](trends/ki-agenten.md) — Autonome Agenten für Geschäftsprozesse, rising (2 Quellen)

## Regulation
...
```

Jeder Eintrag: `[Titel](pfad) — Kurzbeschreibung (Anzahl Quellen)`

## Log-Format

Die `wiki/log.md` ist append-only. Format:

```markdown
## [2026-04-06] ingest | Artikeltitel
- Quelle: raw/articles/beispiel.md
- Erstellt: wiki/sources/beispiel.md, wiki/topics/neues-thema.md
- Aktualisiert: wiki/trends/trend-x.md, wiki/players/firma-y.md
- Zusammenfassung: Kernaussage der Quelle in einem Satz.

## [2026-04-06] query | Wie entwickelt sich Trend X?
- Antwort gespeichert: wiki/synthesis/trend-x-analyse.md
```
