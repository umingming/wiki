# Wiki Schema

## 목적

이 위키는 LLM이 원본 자료를 읽고, 구조화된 마크다운 페이지를 유지하며, 유용한 답변을 다시 지식 자산으로 편입하는 개인 지식 시스템이다. 스키마의 목적은 파일 배치뿐 아니라 축적 방식과 유지보수 원칙을 고정하는 데 있다.

## 구조

```
wiki/
├── CLAUDE.md          # Claude 계열 에이전트 운영 규칙
├── AGENTS.md          # Codex 계열 에이전트 운영 규칙
├── schema.md          # 이 파일 - 위키 규칙과 컨벤션
├── index.md           # 전체 페이지 카탈로그
├── log.md             # 활동 로그
├── sources/           # 원본 자료 보관 (불변)
├── pages/
│   ├── summaries/     # 소스별 요약
│   ├── concepts/      # 개념 정리
│   ├── entities/      # 인물, 조직, 도구
│   └── syntheses/     # 질의 결과, 비교, 통찰의 재편입
└── .wiki-config.json  # 메타데이터
```

## 저장소 규칙

### Raw Sources
- `sources/`는 원본 자료의 보관소다.
- 저장한 원본은 수정하지 않는다. 정정이 필요하면 새 파일을 추가하고 차이를 기록한다.
- 소스 파일명은 가능한 한 `[author]-[year]-[short-title].[ext]` 형식을 따른다.

### Wiki Pages
- `pages/` 아래 문서는 LLM이 생성하고 유지한다.
- 가능한 한 기존 페이지를 갱신하며, 중복되는 주제를 새 파일로 분산하지 않는다.
- 중요한 답변은 반드시 적절한 페이지에 재편입한다.

## 페이지 작성 컨벤션

### 파일명
- 소문자, 하이픈 구분: `transformer-architecture.md`
- 날짜 접두사 불필요 (`log.md`에서 추적)

### 프론트매터
모든 페이지는 YAML 프론트매터를 포함한다:

```yaml
---
title: "페이지 제목"
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources:
  - source-filename.pdf
tags:
  - tag1
  - tag2
---
```

### 출처 추적
- 모든 페이지는 `sources` 배열에 직접 참조한 원본 소스를 기록한다.
- 여러 페이지를 종합한 경우에도 가능하면 근거가 된 원본 소스를 함께 적는다.
- 출처 없는 핵심 주장, 비교, 결론은 금지한다.

### 내부 링크
상대 경로로 다른 페이지를 참조한다:

```markdown
[Transformer](../concepts/transformer-architecture.md)
```

### 링크 밀도
- Summary 페이지는 관련 concept/entity/synthesis로 연결한다.
- Concept와 Entity 페이지는 관련 summary 및 다른 핵심 페이지를 연결한다.
- Synthesis 페이지는 근거가 된 summary, concept, entity를 반드시 링크한다.

### Index 메타데이터
- `index.md`는 각 페이지의 제목, 한 줄 요약, `updated`, `sources` 개수, 핵심 태그를 기록한다.
- 에이전트는 ingest나 주요 query 이후 index 항목을 함께 갱신한다.
- index는 질의 시 첫 탐색 진입점으로 사용하므로, 설명은 검색 가능한 명사와 주제를 포함해야 한다.

## 워크플로우

### 1. Ingest (소스 추가)
1. 원본 자료를 `sources/`에 저장
2. `pages/summaries/`에 요약 페이지 생성
3. 관련 concept/entity/synthesis 페이지 생성 또는 업데이트
4. 새 정보가 기존 설명과 충돌하면 차이를 문서에 남김
5. `index.md` 업데이트
6. `log.md`에 기록

### 2. Query (질의)
1. `index.md`를 먼저 읽고 관련 페이지를 찾음
2. 관련 페이지를 검색하여 답변 구성
3. 재사용 가치가 있는 답변은 새로운 synthesis 페이지로 저장하거나 기존 synthesis에 편입
4. 필요한 경우 concept/entity 페이지도 함께 갱신
5. `log.md`에 기록

### 3. Lint (정합성 검사)
1. 모든 페이지의 프론트매터 유효성 확인
2. 깨진 내부 링크 탐지
3. `index.md`와 실제 페이지 동기화 확인
4. 고아 페이지(index에 없는 페이지) 탐지
5. 누락된 핵심 엔티티/개념 페이지 탐지
6. 출처 없는 핵심 주장 탐지
7. 상충하는 서술, 오래된 주장, 약한 크로스레퍼런스 탐지
8. 가치 있는 query 결과가 위키에 반영되었는지 점검
9. `log.md`에 결과 기록

## 페이지 유형별 가이드

### Summaries
- 하나의 소스에 대한 요약
- 핵심 주장, 방법론, 결론, 열린 질문 포함
- 원본 소스 파일 참조 필수
- 관련 concept/entity/synthesis로 링크

### Concepts
- 하나의 개념을 깊이 다룸
- 정의, 배경, 관련 개념, 관련 엔티티, 주요 논점 포함
- 여러 소스에서 정보를 종합 가능
- 가능하면 서로 다른 소스 간 공통점과 차이도 반영

### Entities
- 인물, 조직, 도구, 프로젝트 등
- 기본 정보, 역할, 관련 주장/작업, 관련 페이지 링크
- 이 엔티티가 왜 위키에서 중요한지 드러나야 함

### Syntheses
- 여러 소스/페이지를 종합한 분석
- 비교, 트렌드 분석, 의사결정 기록, 통찰 도출, 질의 결과의 재편입에 사용
- 참조한 모든 핵심 페이지/소스 명시
- 일회성 채팅 답변이 아니라 반복 참조 가능한 형태로 작성
- 기본 구조는 `pages/syntheses/_template.md`를 따른다
