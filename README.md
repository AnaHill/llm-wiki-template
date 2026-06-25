# LLM Wiki Template

A template for building a personal wiki maintained by an AI agent. The pattern works with any LLM that supports an instruction file; the reference implementation uses [Claude Code](https://claude.ai/code) and its `CLAUDE.md` convention.

Drop a source document into `raw/`. The LLM reads it, writes a summary page, updates relevant concept and entity pages, and logs the operation. Ask questions against the wiki anytime — it synthesizes answers from everything it has read so far.

Based on [Andrej Karpathy's LLM wiki idea](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

---

## Quick start

### Create your wiki (recommended)

Use the GitHub template to get a clean repo with no connection to this template:

```bash
gh repo create my-wiki-name --public --template AnaHill/llm-wiki-template --clone
cd my-wiki-name
```

Or click **"Use this template"** on [github.com/AnaHill/llm-wiki-template](https://github.com/AnaHill/llm-wiki-template) and clone the result.

### Then

1. Open the project folder and start an AI agent session in this directory.
2. Drop a source document into `raw/` and say: **"ingest `raw/<filename>`"**.

That's it. The wiki grows from there.

---

## How it works

| Layer | Path | Who owns it |
|-------|------|-------------|
| Raw sources | `raw/` | You — drop files here, never edited |
| Wiki pages | `wiki/` | LLM — creates and maintains all pages |
| Schema | `CLAUDE.md` | You + LLM — co-evolves over time |

**Three operations:**
- **Ingest** — drop a source in `raw/`, ask the LLM to process it
- **Query** — ask a question; LLM reads the wiki and synthesizes an answer
- **Lint** — ask for a health check; LLM flags stale claims, orphans, gaps

```
ingest                                         query
  │                                              │
  ▼                                              ▼
raw/ ──► wiki pages ──► wiki/index.md ──► answer with citations
```

See [llm-wiki.md](llm-wiki.md) for the full concept and [CLAUDE.md](CLAUDE.md) for the wiki schema.

---

## Why not just RAG?

|                   | Classic RAG         | This wiki                      |
|-------------------|---------------------|--------------------------------|
| What's retrieved  | raw chunks          | curated wiki pages             |
| Quality over time | flat                | compounds with each ingest     |
| Storage           | vector DB           | markdown in git                |
| Contradictions    | silently coexist    | surfaced by `lint`             |
| Ownership         | vendor-specific DB  | a git repo you own             |
| Portability       | migrate DB, reindex | `git clone` / local copy       |

---

## Repository layout

```
raw/          source documents (immutable)
wiki/
  index.md    content catalog — LLM reads this first on every query
  log.md      append-only operation log
skills/
  karpathy-guidelines/  behavioral guidelines for the LLM
CLAUDE.md     wiki schema and LLM instructions
llm-wiki.md   the pattern explained
EXAMPLES.md   examples of good vs. bad LLM behavior
```

---

## Optional: VS Code + Foam

If you use VS Code as your editor, [Foam](https://foambubble.github.io/foam/) is an extension that adds graph visualization, wikilink navigation, and backlink panels on top of the wiki's markdown files. It's not required — the wiki works fine without it — but it's a nice way to explore the structure as it grows.

**To set it up:**

1. Install the Foam extension in VS Code: open the Extensions panel (`Ctrl+Shift+X`) and search for `foam.foam-vscode`.
2. Optionally add Foam-specific workspace settings to `.vscode/settings.json`:
   ```json
   "foam.openDailyNote.onVSCodeStartup": false,
   "foam.files.exclude": [
     "**/.git/**",
     "**/node_modules/**",
     "**/out/**",
     "raw/**"
   ],
   "foam.graph.style": {
     "background": "#202020",
     "node": {
       "regular": { "color": "#277da1" },
       "placeholder": { "color": "#aa5042" }
     }
   }
   ```
3. Open the graph with `Foam: Show Graph` from the Command Palette.

**Key features once installed:**

- **Graph view** shows the shape of your wiki — hubs, orphans, clusters.
- **Wikilinks** (`[[page-name]]`) let the LLM cross-reference pages; Foam resolves them into clickable links.
- **Backlinks panel** shows every page that links to the current one.
