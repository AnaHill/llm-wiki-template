# LLM Wiki Template

A template for building a personal wiki maintained by an AI agent. The pattern works with any LLM agent that reads an instruction file: the instructions are in `AGENTS.md` (read by Codex, Cursor and others), and `CLAUDE.md` imports it for [Claude Code](https://claude.ai/code).

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
| Schema | `AGENTS.md` | You + LLM — co-evolves over time |

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

See [llm-wiki.md](llm-wiki.md) for the full concept and [AGENTS.md](AGENTS.md) for the wiki schema.

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
AGENTS.md     wiki schema and LLM instructions (CLAUDE.md just imports it)
.claude/settings.json  asks before any write to raw/ (Claude Code)
llm-wiki.md   the pattern explained
EXAMPLES.md   examples of good vs. bad LLM behavior
```

---

## Optional: a viewer with a graph view (Obsidian or VS Code + Foam)

The wiki is plain markdown, so any editor works. A viewer with a graph view and clickable `[[wikilinks]]` makes it much easier to see the shape of the wiki as it grows — hubs, orphans, clusters — and to follow what the agent changed. Two options:

### Obsidian

[Obsidian](https://obsidian.md/) is a free markdown app with a graph view, backlinks and search built in. Open the repository folder as a vault (**Open folder as vault**); nothing needs to be installed in the repository. Keep the agent in a terminal or in VS Code next to it.

- **Graph view:** in the graph settings, filter with `path:wiki -file:index -file:log`, so that the index and the log, which link to almost everything, do not turn the graph into one star.
- **Colour groups:** add groups in the graph settings by folder or by tag, for example `tag:#concept`, or `path:wiki/sources` if you keep source pages in a subfolder, and last `path:wiki` for the rest (a node takes the colour of the first group it matches, so the more specific groups go first). Search syntax: <https://obsidian.md/help/plugins/search>.
- **Tags:** a `#word` in prose becomes a tag in Obsidian. Keep tags in the `tags:` front matter, and write a hash that is not meant as a tag with a backslash, `\#AB01`.
- Obsidian writes its settings to `.obsidian/` in the folder; add it to `.gitignore` if you do not want them in the repository.

### VS Code + Foam

If you use VS Code as your editor, [Foam](https://foambubble.github.io/foam/) is an extension that adds graph visualization, wikilink navigation, and backlink panels on top of the wiki's markdown files.

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
