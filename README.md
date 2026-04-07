# wiki-hermes

An LLM-maintained industry knowledge wiki based on the [LLM Wiki Pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) by Andrej Karpathy.

Instead of re-retrieving raw data via RAG on every question, an LLM agent **incrementally builds a persistent wiki** — structured, interlinked, compounding. Knowledge is compiled once and kept current, not re-derived on every query.

You curate sources and ask questions. The agent does the rest.

[Deutsche Version](README.de.md)

## How it works

```
URL / file    Share a link or drop a file
                 ↓ Scrape
raw/          Clean markdown sources
                 ↓ Ingest
wiki/         Agent builds structured wiki
                 ↓ Draft
content/      Derive publishable content
```

**Three layers:**

| Layer | Who writes | Purpose |
|---|---|---|
| `raw/` | You | Immutable source documents |
| `wiki/` | The agent | Structured, interlinked knowledge base |
| `content/` | The agent (you review) | Blog posts, newsletters, briefings |

## Workflows

| Workflow | What happens |
|---|---|
| **Scrape** | Grab a URL → convert to clean markdown, save to `raw/articles/` with metadata |
| **Ingest** | Process new source → create summary, update wiki pages, set cross-references |
| **Query** | Ask the wiki → answer from compiled knowledge, not raw data |
| **Lint** | Health check → find contradictions, orphan pages, missing links |
| **Draft** | Create content → derive blog posts, newsletters or briefings from wiki knowledge |
| **Newsletter** | Weekly review → automatic summary of recent activity |

## Wiki structure

```
wiki/
├── index.md          Catalog of all pages (agent entry point)
├── log.md            Chronological log of all operations
├── overview.md       Industry overview — the big picture
├── topics/           Domain topics, technologies, methods
├── trends/           Emerging developments and signals
├── regulation/       Laws, standards, compliance
├── market/           Market data, segments, dynamics
├── players/          Companies, associations, people, institutions
├── sources/          One summary per processed source
└── synthesis/        Analyses, cross-connections, conclusions
```

## Quickstart

1. **Add a source** — Drop a file into `raw/`, or share a URL with the agent
2. **Start an agent** — Open the project with an LLM agent ([Hermes](https://github.com/NousResearch/hermes-agent), Cursor, Claude Code, etc.)
3. **Scrape & ingest** — Tell the agent: *"Scrape this URL and ingest it"* or *"Process the new source in raw/articles/..."*
4. **Wiki grows** — The agent reads `AGENTS.md`, follows the workflows and builds the wiki

No code needed. `AGENTS.md` contains all rules and workflows — the agent is the runtime.

## Agent compatibility

Works with any LLM agent that can read and write files:

| Agent | Configuration |
|---|---|
| [Hermes](https://github.com/NousResearch/hermes-agent) | Reads `AGENTS.md` automatically as project context |
| [Cursor](https://cursor.com) | Reads `AGENTS.md` as a rule |
| [Claude Code](https://docs.anthropic.com/en/docs/agents) | Reads `CLAUDE.md` (copy of `AGENTS.md`) |
| [Codex](https://openai.com/codex) | Reads `AGENTS.md` automatically |
| Others | Pass `AGENTS.md` as system prompt or context |

## Content production

The wiki isn't just a knowledge base — it's a **content engine**. Compiled industry knowledge translates directly into publishable content:

- **Blog posts** — Trend page + sources → substantive article
- **Newsletters** — Recent wiki activity → weekly review
- **Briefings** — Topic pages → management summary
- **LinkedIn posts** — Synthesis pages → sharp industry takes

Templates are in `content/templates/`.

## Inspiration

Based on the [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) concept by Andrej Karpathy:

> *"The wiki is a persistent, compounding artifact. The cross-references are already there. The contradictions have already been flagged. The synthesis already reflects everything you've read."*

## License

[MIT](LICENSE)
