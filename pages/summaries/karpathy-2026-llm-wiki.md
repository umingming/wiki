---
title: "Summary: LLM Wiki by Andrej Karpathy"
created: 2026-04-15
updated: 2026-04-15
sources:
  - karpathy-2026-llm-wiki.md
tags:
  - summary
  - llm
  - knowledge-management
---

# Summary: LLM Wiki by Andrej Karpathy

> 원본: [GitHub Gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)

## 핵심 주장

기존 RAG 방식은 매 질문마다 지식을 처음부터 재발견한다. 대안으로 LLM이 **영구적이고 복리로 성장하는 위키**를 점진적으로 구축·유지하는 패턴을 제안한다.

## 아키텍처 (3계층)

| 계층 | 역할 | 소유자 |
|------|------|--------|
| **Raw Sources** | 원본 자료 (논문, 기사, 데이터). 불변. | 사람 |
| **Wiki** | LLM이 생성한 마크다운 페이지들. 요약, 엔티티, 개념, 종합 분석. | LLM |
| **Schema** | 위키 구조, 컨벤션, 워크플로우 정의. | 사람 + LLM 공동 진화 |

## 3가지 연산

1. **Ingest**: 소스 추가 → 요약 작성 → 관련 페이지 업데이트 → 인덱스/로그 기록. 하나의 소스가 10~15개 페이지에 영향.
2. **Query**: 위키 기반 질의 응답. 좋은 답변은 새 페이지로 위키에 재편입.
3. **Lint**: 정합성 검사 - 모순, 고아 페이지, 누락된 크로스레퍼런스 탐지.

## 핵심 파일

- **index.md**: 카테고리별 페이지 카탈로그. LLM이 질의 시 먼저 읽고 관련 페이지를 찾음. ~100개 소스 규모에서 임베딩 기반 RAG 없이도 충분.
- **log.md**: 시간순 활동 기록. 파싱 가능한 접두사 형식 권장.

## 활용 분야

- 개인 자기계발, 연구 심층 분석, 독서 정리, 팀/비즈니스 내부 위키, 경쟁 분석 등

## 왜 작동하는가

위키 유지의 고통은 읽기/사고가 아니라 **북키핑**(크로스레퍼런스, 요약 갱신, 일관성 유지)이다. 인간은 유지 부담이 가치보다 빨리 커져 위키를 포기하지만, LLM은 이 작업에 지치지 않는다. Vannevar Bush의 Memex(1945) 비전의 현대적 실현.

## 관련 페이지

- [LLM Wiki (개념)](../concepts/llm-wiki.md)
- [Andrej Karpathy](../entities/andrej-karpathy.md)
