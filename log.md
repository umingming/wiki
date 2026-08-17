# 작업 로그

## 2026-08-17

- MIGRATE: 결정·근거 기반 지식 위키로 구조 전환 (project-decision-wiki 스킬)
  - 목적: 개인 지식 베이스의 '왜/근거/분석' 레이어. 결정은 ID(DEC-####)로 인용하라.
  - 스키마: `schema.md` → `CLAUDE.md` 통합 (2-레이어, 결정 불변 규칙, domain enum: knowledge-ai·game-dev·web-dev·tooling·meta)
  - 구 카테고리(summaries/concepts/entities/syntheses) → 새 타입 체계(decision·analysis·principle·concept·guide·note·source)로 재편:
    - syntheses/pixel-game-web-stack → note로 이전 ([[pages/notes/2026-05-17-pixel-game-web-stack]])
    - summaries/karpathy-2026-llm-wiki → 소스 캡처 [[sources/articles/karpathy-2026-llm-wiki]] 에 병합
    - entities/{andrej-karpathy, phaser, capacitor, aseprite} → pages/concepts/ 로 이동
  - 양식 7종(templates/)·스크립트 3종(scripts/build-index·new·wiki-lint) 설치, index.md·decisions-index.md·llms.txt 자동 생성 체계로 전환
  - 가이드: [[pages/guides/how-we-decide]], [[pages/guides/agents]] · 스텁: [[pages/glossary]], [[pages/decisions/open-decisions]](백로그)
  - TODO(나중): 용어집 채우기, 진행 중 프로젝트(suika-game 등)의 결정 기록 시작

## 이전 로그 (구 구조, 2026-04-15 ~ 2026-05-17)

[2026-04-15 00:00] **INIT** - 위키 초기 구조 생성
[2026-04-15 00:01] **INGEST** - LLM Wiki 개념 페이지 추가 (`pages/concepts/llm-wiki.md`), Andrej Karpathy 엔티티 페이지 추가 (`pages/entities/andrej-karpathy.md`)
[2026-04-15 00:02] **INGEST** - Karpathy 원본 Gist 소스 추가 (`sources/karpathy-2026-llm-wiki.md`), 요약 페이지 생성 (`pages/summaries/karpathy-2026-llm-wiki.md`), 기존 concept/entity 페이지에 소스 참조 연결
[2026-04-15 23:24] **QUERY** - LLM Wiki 관점에서 구조 검토 후 운영 규칙 강화 (`CLAUDE.md`, `schema.md`) 및 Codex용 에이전트 가이드 추가 (`AGENTS.md`)
[2026-04-15 23:26] **QUERY** - `index.md` 메타데이터 형식 강화 및 synthesis 템플릿 추가 (`pages/syntheses/_template.md`)
[2026-05-17 00:00] **QUERY** - 픽셀 경영시뮬 모바일 게임 기술 스택 질의 결과 재편입: synthesis 생성 (`pages/syntheses/pixel-game-web-stack.md`), entity 3건 생성 (`pages/entities/phaser.md`, `pages/entities/capacitor.md`, `pages/entities/aseprite.md`)
