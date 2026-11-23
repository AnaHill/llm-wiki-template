# AGENTS.md

This file provides guidance to the coding agent (e.g. Claude Code, Codex, or similar) when working in this repository. `CLAUDE.md` only imports this file.

## What this repository is

This is an **LLM-maintained personal wiki**, not a software project. There is no build system, no tests, no deployment. The LLM (you) is the wiki maintainer. The human curates sources and asks questions; the LLM does the bookkeeping: summarizing, cross-referencing, filing, and keeping pages consistent as the wiki grows.

The full pattern is described in [llm-wiki.md](llm-wiki.md). Read it when starting a new session.

## Directory layout

| Path | Purpose |
|------|---------|
| `raw/` | Immutable source documents. Never modify or delete these. `.claude/settings.json` asks the human before any write to `raw/`. |
| `wiki/` | LLM-maintained markdown pages. You own this layer entirely. |
| `wiki/index.md` | Content catalog — read this first when answering any query. |
| `wiki/log.md` | Append-only chronological log of all operations. |
| `skills/karpathy-guidelines/` | Invoke with `/karpathy-guidelines` — behavioral guidelines for disciplined LLM work. |

## Three operations

### Ingest
When the human drops a source into `raw/` and asks you to process it:
1. Read the source.
2. Discuss key takeaways if the human wants to stay involved.
3. Write or update a summary page in `wiki/`.
4. Update any relevant entity or concept pages across the wiki.
5. Update `wiki/index.md`.
6. Append an entry to `wiki/log.md`.

A single source may touch 10–15 wiki pages. That's normal.

### Query
When the human asks a question:
1. Read `wiki/index.md` first to find relevant pages.
2. Drill into those pages and synthesize an answer.
3. If the answer is substantial (comparison, analysis, new synthesis), offer to file it as a new wiki page so it compounds into the knowledge base rather than disappearing into chat history.

### Lint
When the human asks for a health check:
- Look for contradictions between pages, stale claims superseded by newer sources, orphan pages (no inbound links), concepts mentioned but lacking their own page, and missing cross-references.
- Suggest new questions or sources that would fill visible gaps.

## Conventions

**Log format:** Every `wiki/log.md` entry must start with a consistent prefix so it's grep-parseable:
```
## [YYYY-MM-DD] init | Wiki initialized
## [YYYY-MM-DD] ingest | Source Title
## [YYYY-MM-DD] query | Question summary
## [YYYY-MM-DD] lint | pass
```

**Decisions go in the log.** A decision and its reasons (what to leave out, which option was declined, why a page was merged or split) get a `wiki/log.md` entry, even when nothing else changes. A decision recorded only in a chat or a to-do list is lost; the log is what a later session reads.

**No stray tags.** If the wiki is read in Obsidian, a `#word` in prose becomes a tag. Tags belong only in the `tags:` front matter. Whenever a hash-prefixed identifier appears in text and is not meant as a tag (for example a catalogue number such as `\#AB01`), write the hash with a backslash escape, as in `\#AB01`, so it displays as "#AB01" without becoming a tag.

**Index format:** Each entry in `wiki/index.md` should include a link, a one-line summary, and optionally metadata (date added, source count). Organize by category (entities, concepts, sources, analyses).

**Raw sources are immutable.** Never edit files in `raw/`. The LLM reads from them; that's all.

## Schema evolution

This AGENTS.md *is* the schema for this wiki instance. As the wiki grows, update it to reflect new conventions, page formats, or workflow decisions discovered in collaboration with the human. The schema and the wiki co-evolve. When a rule is absolute and mechanical (for example that `raw/` is never edited), enforce it in `.claude/settings.json` as well, not only in this text.

## Behavioral guidelines

**Think before acting.** State assumptions explicitly. If a source is ambiguous, ask rather than guessing. If you're about to make a structural change to the wiki (renaming pages, reorganizing categories), say so first.

**Surgical changes.** When updating a wiki page, change only what the new source warrants. Don't rewrite sections that aren't affected. Don't "improve" prose that isn't broken.

**Minimum necessary.** Don't create new wiki pages speculatively. If a concept appears once, note it inline. Create a dedicated page when the concept recurs or becomes substantial enough to warrant its own page.
