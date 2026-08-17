---
title: "LLM Wiki"
type: source
category: articles
url: "https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f"
author: "Andrej Karpathy"
created: 2026-04-15
tags: [llm, knowledge-management]
domains: [knowledge-ai]
status: active
---

## 왜 중요한가

이 위키 구조의 원형이 된 제안서다. LLM이 영구적·복리적으로 성장하는 개인 위키를 구축·유지하는 패턴(Raw Sources / Wiki / Schema 3계층, Ingest·Query·Lint 3연산)을 정의한다.

## 발췌 / 스냅샷

A pattern for building personal knowledge bases using LLMs.

This is an idea file, designed to be copied to your own LLM Agent (e.g. OpenAI Codex, Claude Code, OpenCode / Pi, etc.). Its goal communicates the high-level idea, with your agent building specifics in collaboration with you.

### The Core Idea

Most people experience with LLMs and documents resembles RAG: upload files, LLM retrieves relevant chunks at query time, generates answers. This works but lacks accumulation—knowledge rediscovered from scratch each query.

The alternative: LLMs **incrementally build and maintain a persistent wiki**—a structured, interlinked markdown collection sitting between you and raw sources. When adding new sources, the LLM doesn't just index for later retrieval. It reads, extracts key information, integrates into existing wiki, updates entity pages, revises summaries, flags contradictions, strengthens synthesis. Knowledge compiles once, stays current.

**Key difference: the wiki is persistent, compounding.** Cross-references already exist. Contradictions flagged. Synthesis reflects everything read. Wiki grows richer with each source and question.

You source and explore; LLM does bookkeeping—summarizing, cross-referencing, filing, maintenance. Practically: LLM on one side, Obsidian on the other. LLM makes edits; you browse results in real time.

### Applications

- **Personal**: tracking goals, health, psychology, self-improvement
- **Research**: deep dives over weeks/months, building comprehensive wiki with evolving thesis
- **Reading**: file chapters, build pages for characters, themes, plot threads, connections
- **Business/team**: internal wiki maintained by LLMs, fed by Slack, transcripts, documents
- **Other**: competitive analysis, due diligence, trip planning, hobby deep-dives

### Architecture

Three layers:

**Raw sources**—curated collection of documents (articles, papers, images, data). Immutable; LLM reads but never modifies. Source of truth.

**The wiki**—directory of LLM-generated markdown files. Summaries, entity pages, concepts, comparisons, overviews, synthesis. LLM owns entirely: creates, updates, maintains cross-references, ensures consistency.

**The schema**—document (CLAUDE.md for Claude Code) telling LLM how wiki structures, conventions, workflows for ingesting, answering, maintaining. Key configuration file making LLM disciplined maintainer rather than generic chatbot. Co-evolve with LLM over time.

### Operations

**Ingest.** Drop new source, tell LLM to process. LLM reads, discusses takeaways, writes summary, updates index, updates relevant pages, appends log entry. Single source touches 10-15 pages. Ingest one at time with involvement or batch with less supervision.

**Query.** Ask questions against wiki. LLM searches relevant pages, synthesizes answers with citations. Answers take various forms—markdown page, comparison table, slide deck, chart, canvas. **Good answers file back into wiki as new pages.** Comparisons, analyses, discovered connections valuable, shouldn't disappear into chat history.

**Lint.** Periodically health-check wiki. Look for: contradictions, stale claims, orphan pages, important concepts lacking pages, missing cross-references, data gaps. LLM suggests new questions, sources to investigate. Keeps wiki healthy as it grows.

### Indexing and Logging

Two special files help navigate growing wiki:

**index.md**—content-oriented catalog. Each page listed with link, one-line summary, optional metadata (date, source count). Organized by category. LLM updates on every ingest. When answering queries, LLM reads index first to find relevant pages, then drills in. Works surprisingly well at moderate scale (~100 sources, ~hundreds pages), avoids embedding-based RAG infrastructure.

**log.md**—chronological append-only record. What happened and when—ingests, queries, lint passes. Tip: if entries start with consistent prefix (e.g. `## [2026-04-02] ingest | Article Title`), log becomes parseable with unix tools—`grep "^## \[" log.md | tail -5` gives last 5 entries. Timeline of wiki's evolution helps LLM understand recent work.

### Optional: CLI Tools

At scale, small tools help LLM operate more efficiently. Search engine over wiki pages most obvious—at small scale index file sufficient; as wiki grows, want proper search. qmd is good option: local search engine for markdown with hybrid BM25/vector search and LLM re-ranking, all on-device. Both CLI (LLM shells out) and MCP server (native tool). Could build simpler yourself—LLM helps vibe-code as need arises.

### Tips and Tricks

- **Obsidian Web Clipper** converts web articles to markdown quickly for raw collection
- **Download images locally.** Obsidian Settings → Files and links, set attachment folder. Settings → Hotkeys, bind "Download attachments" (e.g. Ctrl+Shift+D). After clipping, hit hotkey; images download locally. LLM views, references images directly instead of relying on URLs.
- **Obsidian's graph view** shows wiki shape—connections, hubs, orphans
- **Marp**—markdown-based slide format. Obsidian plugin available. Generates presentations from wiki content
- **Dataview**—Obsidian plugin running queries over page frontmatter. If LLM adds YAML frontmatter (tags, dates, source counts), generates dynamic tables, lists
- **Wiki is git repo** of markdown files. Version history, branching, collaboration free

### Why This Works

Knowledge base maintenance tedium isn't reading/thinking—it's bookkeeping. Updating cross-references, keeping summaries current, noting contradictions, maintaining consistency across pages. Humans abandon wikis because maintenance burden grows faster than value. LLMs don't bore, don't forget updates, touch 15 files in one pass. Wiki stays maintained because maintenance costs near zero.

Human curates sources, directs analysis, asks good questions, thinks about meaning. LLM does everything else.

Related in spirit to Vannevar Bush's Memex (1945)—personal, curated knowledge store with associative trails between documents. Closer to original vision than what web became: private, actively curated, connections between documents as valuable as documents themselves. Bush couldn't solve maintenance; LLM handles that.

### Note

This document intentionally abstract—describes idea, not specific implementation. Directory structure, schema conventions, page formats, tooling depend on domain, preferences, LLM choice. Everything mentioned optional, modular—pick useful, ignore rest.

Right way to use: share with LLM agent, work together instantiating version fitting needs. Document communicates pattern; LLM figures rest.

## 핵심 주장

- 기존 RAG는 매 질문마다 지식을 처음부터 재발견한다. 대안은 LLM이 **영구적이고 복리로 성장하는 위키**를 점진적으로 구축·유지하는 것.
- 아키텍처는 3계층: **Raw Sources**(원본, 불변, 사람 소유) / **Wiki**(LLM 생성 마크다운, LLM 소유) / **Schema**(구조·컨벤션·워크플로우 정의, 사람+LLM 공동 진화).
- 연산은 3가지: **Ingest**(소스 추가 → 요약 → 관련 페이지·인덱스·로그 갱신, 소스 하나가 10~15개 페이지에 영향) / **Query**(위키 기반 응답, 좋은 답변은 위키로 재편입) / **Lint**(모순·고아 페이지·누락 크로스레퍼런스 탐지).
- **index.md**는 카테고리별 카탈로그로, ~100개 소스 규모에선 임베딩 RAG 없이 충분. **log.md**는 파싱 가능한 접두사 형식의 시간순 기록.
- 위키 유지의 고통은 읽기/사고가 아니라 **북키핑**이다. 인간은 유지 부담이 가치보다 빨리 커져 포기하지만, LLM은 지치지 않는다. Vannevar Bush의 Memex(1945) 비전의 현대적 실현.

## 인용한 곳 (Cited by)

- [[pages/concepts/llm-wiki]] — 패턴 개념 정리
- [[pages/concepts/andrej-karpathy]] — 제안자 프로필
