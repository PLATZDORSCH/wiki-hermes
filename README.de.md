# wiki-hermes

Ein LLM-gepflegtes Branchen-Wiki nach dem [LLM Wiki Pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) von Andrej Karpathy.

Statt bei jeder Frage Rohdaten per RAG zusammenzusuchen, baut ein LLM-Agent **inkrementell ein persistentes Wiki auf** — strukturiert, verlinkt, wachsend. Wissen wird einmal kompiliert und dann gepflegt, nicht bei jeder Anfrage neu abgeleitet.

Der Mensch kuratiert Quellen und stellt Fragen. Der Agent erledigt den Rest.

[English version](README.md)

## Wie es funktioniert

```
URL / Datei   Link teilen oder Datei ablegen
                 ↓ Scrape
raw/          Saubere Markdown-Quellen
                 ↓ Ingest
wiki/         Agent baut strukturiertes Wiki auf
                 ↓ Draft
content/      Content für Veröffentlichung ableiten
```

**Drei Schichten:**

| Schicht | Wer schreibt | Zweck |
|---|---|---|
| `raw/` | Du | Unveränderliche Quelldokumente |
| `wiki/` | Der Agent | Strukturierte, verlinkte Wissensbasis |
| `content/` | Der Agent (du reviewst) | Blogartikel, Newsletter, Briefings |

## Workflows

| Workflow | Was passiert |
|---|---|
| **Scrape** | URL abrufen → zu sauberem Markdown konvertieren, in `raw/articles/` mit Metadaten speichern |
| **Ingest** | Neue Quelle verarbeiten → Summary erstellen, Wiki-Seiten aktualisieren, Querverweise setzen |
| **Query** | Frage ans Wiki stellen → Antwort aus kompiliertem Wissen, nicht aus Rohdaten |
| **Lint** | Gesundheitscheck → Widersprüche, verwaiste Seiten, fehlende Verknüpfungen finden |
| **Draft** | Content erstellen → Blog, Newsletter oder Briefing aus Wiki-Wissen ableiten |
| **Newsletter** | Wochenrückblick → Automatische Zusammenfassung der letzten Aktivitäten |

## Wiki-Struktur

```
wiki/
├── index.md          Katalog aller Seiten (Einstiegspunkt für den Agent)
├── log.md            Chronologisches Protokoll aller Operationen
├── overview.md       Branchenübersicht — das Big Picture
├── topics/           Fachthemen, Technologien, Methoden
├── trends/           Aufkommende Entwicklungen und Signale
├── regulation/       Gesetze, Normen, Standards
├── market/           Marktdaten, Segmente, Dynamiken
├── players/          Firmen, Verbände, Personen, Institutionen
├── sources/          Eine Summary pro verarbeiteter Quelle
└── synthesis/        Analysen, Querverbindungen, Schlussfolgerungen
```

## Quickstart

1. **Quelle hinzufügen** — Lege eine Datei in `raw/` ab, oder teile dem Agent eine URL
2. **Agent starten** — Öffne das Projekt mit einem LLM-Agenten ([Hermes](https://github.com/NousResearch/hermes-agent), Cursor, Claude Code, etc.)
3. **Scrape & Ingest** — Sag dem Agent: *"Scrape diese URL und ingest sie"* oder *"Verarbeite die neue Quelle in raw/articles/..."*
4. **Wiki wächst** — Der Agent liest `AGENTS.md`, folgt den Workflows und baut das Wiki auf

Kein Code nötig. Das `AGENTS.md` enthält alle Regeln und Workflows — der Agent ist die Runtime.

## Agent-Kompatibilität

Das Wiki funktioniert mit jedem LLM-Agenten, der Dateien lesen und schreiben kann:

| Agent | Konfiguration |
|---|---|
| [Hermes](https://github.com/NousResearch/hermes-agent) | Liest `AGENTS.md` automatisch als Projektkontext |
| [Cursor](https://cursor.com) | Liest `AGENTS.md` als Regel |
| [Claude Code](https://docs.anthropic.com/en/docs/agents) | Liest `CLAUDE.md` (Kopie von `AGENTS.md`) |
| [Codex](https://openai.com/codex) | Liest `AGENTS.md` automatisch |
| Andere | `AGENTS.md` als System-Prompt oder Kontext übergeben |

## Content-Produktion

Das Wiki ist nicht nur eine Wissensdatenbank — es ist eine **Content-Maschine**. Aus dem kompilierten Branchenwissen lassen sich direkt Inhalte ableiten:

- **Blogartikel** — Trend-Seite + Quellen → fundierter Artikel
- **Newsletter** — Letzte Wiki-Aktivitäten → Wochenrückblick
- **Briefings** — Topic-Seiten → Management-Summary
- **LinkedIn-Posts** — Synthese-Seiten → pointierte Einordnung

Vorlagen liegen in `content/templates/`.

## Inspiration

Basiert auf dem [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) Konzept von Andrej Karpathy:

> *"The wiki is a persistent, compounding artifact. The cross-references are already there. The contradictions have already been flagged. The synthesis already reflects everything you've read."*

## Lizenz

[MIT](LICENSE)
