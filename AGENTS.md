# Agent Guide

이 레포는 LLM이 관리하는 **개인 지식 위키**다 (결정·근거·분석의 WHY·HOW 레이어).

**스키마는 [CLAUDE.md](CLAUDE.md) 가 단일 정본이다** — 여기 규칙을 중복 기술하지 않는다. 작업 전 반드시 읽고 따를 것. 에이전트 진입점(권위 순서·인용법·경계)은 [pages/guides/agents.md](pages/guides/agents.md), 결정 절차는 [pages/guides/how-we-decide.md](pages/guides/how-we-decide.md).

핵심만 요약하면:

1. `sources/` 는 불변 원본, `pages/` 는 LLM이 관리한다. 단 `decision` 은 accepted 후 본문 동결 — 내용이 바뀌면 대체(supersede)한다.
2. `index.md` · `decisions-index.md` · `llms.txt` 는 자동 생성물이다. 직접 편집하지 말고 `node scripts/build-index.mjs` 로 재생성한다.
3. 커밋 전 게이트: `node scripts/wiki-lint.mjs --report && node scripts/build-index.mjs --check` (에러 0).
4. 새 페이지는 `node scripts/new.mjs <type> <slug> [title]` 로 만든다 (ID·날짜·owner 자동 stamp).
5. 질의는 `llms.txt`/`index.md` 에서 시작하고, 재사용 가치 있는 답변은 위키로 재편입한 뒤 `log.md` 에 기록한다.
