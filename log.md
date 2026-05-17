# Activity Log

> 모든 ingest, query, lint 활동을 시간순으로 기록합니다.

---

[2026-04-15 00:00] **INIT** - 위키 초기 구조 생성
[2026-04-15 00:01] **INGEST** - LLM Wiki 개념 페이지 추가 (`pages/concepts/llm-wiki.md`), Andrej Karpathy 엔티티 페이지 추가 (`pages/entities/andrej-karpathy.md`)
[2026-04-15 00:02] **INGEST** - Karpathy 원본 Gist 소스 추가 (`sources/karpathy-2026-llm-wiki.md`), 요약 페이지 생성 (`pages/summaries/karpathy-2026-llm-wiki.md`), 기존 concept/entity 페이지에 소스 참조 연결
[2026-04-15 23:24] **QUERY** - LLM Wiki 관점에서 구조 검토 후 운영 규칙 강화 (`CLAUDE.md`, `schema.md`) 및 Codex용 에이전트 가이드 추가 (`AGENTS.md`)
[2026-04-15 23:26] **QUERY** - `index.md` 메타데이터 형식 강화 및 synthesis 템플릿 추가 (`pages/syntheses/_template.md`)
[2026-05-17 00:00] **QUERY** - 픽셀 경영시뮬 모바일 게임 기술 스택 질의 결과 재편입: synthesis 생성 (`pages/syntheses/pixel-game-web-stack.md`), entity 3건 생성 (`pages/entities/phaser.md`, `pages/entities/capacitor.md`, `pages/entities/aseprite.md`)
