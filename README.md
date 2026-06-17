# LLM Wiki Template — VS Code Foam

A template for building a personal wiki maintained by an LLM, using VS Code + [Foam](https://foambubble.github.io/foam/) as the IDE.

The LLM is the programmer. The wiki is the codebase. You curate sources and ask questions; the LLM does the bookkeeping.

Based on [Andrej Karpathy's LLM wiki idea](https://gist.github.com/karpathy/442a6bf555914893e9английsk94f).

---

## Quick start

1. Clone this repository.
2. Open it in VS Code. When prompted, install the recommended extension (Foam).
3. Open a new Claude Code session in this directory.
4. Drop a source document into `raw/` and tell Claude: **"ingest `raw/<filename>`"**.

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

See [llm-wiki.md](llm-wiki.md) for the full concept and [CLAUDE.md](CLAUDE.md) for the wiki schema.

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
