# LLM Wiki Agent Guide

This repository is an LLM-maintained wiki. Codex should treat it as a persistent knowledge base, not as a loose collection of notes.

## Mission

- Read raw sources from `sources/`
- Maintain structured markdown pages in `pages/`
- Keep `index.md` and `log.md` current
- Turn reusable answers into wiki assets instead of leaving them only in chat

## Core Rules

1. `sources/` is immutable raw input. Do not rewrite or summarize in place.
2. Prefer updating existing pages over creating duplicate pages.
3. Every important claim, comparison, or synthesis must be traceable to source files.
4. When a query reveals reusable insight, create or update a page under `pages/syntheses/`.
5. Maintain cross-links across summaries, concepts, entities, and syntheses.
6. When new information conflicts with existing pages, preserve the conflict explicitly instead of silently overwriting it.

## Operating Workflow

### Ingest
1. Confirm the raw source exists under `sources/`
2. Create or update a summary in `pages/summaries/`
3. Update relevant pages in `pages/concepts/`, `pages/entities/`, and `pages/syntheses/`
4. Update `index.md`
5. Append an `INGEST` entry to `log.md`

### Query
1. Read `index.md` first to find relevant pages
2. Use the wiki pages as the main working set for answers
3. If the answer produces reusable comparison, framing, insight, or decision support, save it into `pages/syntheses/` or expand an existing page
4. If the answer changes the understanding of a concept or entity, update those pages too
5. Append a `QUERY` entry to `log.md`

### Lint
1. Check frontmatter validity
2. Check broken internal links
3. Check `index.md` coverage
4. Check orphan pages
5. Check missing important concept/entity pages
6. Check unsupported claims and weak cross-references
7. Check whether important query outputs were failed to be reintegrated
8. Append a `LINT` entry to `log.md`

## Page Conventions

- Required frontmatter on every page: `title`, `created`, `updated`, `sources`, `tags`
- Use lowercase kebab-case filenames
- Use relative markdown links
- Keep pages compact, link-rich, and easy to extend
- Start new synthesis pages from `pages/syntheses/_template.md`

## Page Intent

- `pages/summaries/`: one source, faithfully compressed
- `pages/concepts/`: stable ideas synthesized across sources
- `pages/entities/`: people, organizations, tools, projects
- `pages/syntheses/`: reusable answers, comparisons, trends, decisions, cross-source insights

## Logging Format

Use:

`[YYYY-MM-DD HH:MM] **INGEST** - ...`

`[YYYY-MM-DD HH:MM] **QUERY** - ...`

`[YYYY-MM-DD HH:MM] **LINT** - ...`

## Priority

When there is tension between chat convenience and wiki quality, prefer the action that makes the wiki more reusable later.
