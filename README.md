# LLM Wiki Template — VS Code Foam

A template for building a personal wiki maintained by an LLM, using VS Code + [Foam](https://foambubble.github.io/foam/) as the IDE.

The LLM is the programmer. The wiki is the codebase. You curate sources and ask questions; the LLM does the bookkeeping.

Based on [Andrej Karpathy's LLM wiki idea](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

---

## Quick start

### Create your wiki (recommended)

Use the GitHub template to get a clean repo with no connection to this template:

```bash
gh repo create my-wiki-name --public --template AnaHill/LLM-wiki-template-with-VSCodeFoam --clone
cd my-wiki-name
```

Or click **"Use this template"** on [github.com/AnaHill/LLM-wiki-template-with-VSCodeFoam](https://github.com/AnaHill/LLM-wiki-template-with-VSCodeFoam) and clone the result.

### Then

1. Open the folder in VS Code — accept the Foam extension recommendation.
2. Open a Claude Code session in this directory.
3. Drop a source document into `raw/` and say: **"ingest `raw/<filename>`"**.

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

## VS Code Foam tips

- **Graph view** (`Foam: Show Graph`) shows the shape of your wiki — hubs, orphans, clusters.
- **Wikilinks** (`[[page-name]]`) let the LLM cross-reference pages; Foam resolves them.
- **Backlinks panel** shows every page that links to the current one.

Install Foam from the Extensions panel or accept the workspace recommendation prompt.
