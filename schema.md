# Wiki Schema

## 구조

```
wiki/
├── schema.md          # 이 파일 - 위키 규칙과 컨벤션
├── index.md           # 전체 페이지 카탈로그
├── log.md             # 활동 로그
├── sources/           # 원본 자료 보관
├── pages/
│   ├── summaries/     # 소스별 요약
│   ├── concepts/      # 개념 정리
│   ├── entities/      # 인물, 조직, 도구
│   └── syntheses/     # 종합 분석
└── .wiki-config.json  # 메타데이터
```

## 페이지 작성 컨벤션

### 파일명
- 소문자, 하이픈 구분: `transformer-architecture.md`
- 날짜 접두사 불필요 (log.md에서 추적)

### 프론트매터
모든 페이지는 YAML 프론트매터를 포함합니다:

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

### 내부 링크
상대 경로로 다른 페이지를 참조합니다:
```markdown
[Transformer](../concepts/transformer-architecture.md)
```

## 워크플로우

### 1. Ingest (소스 추가)
1. 원본 자료를 `sources/`에 저장
2. `pages/summaries/`에 요약 페이지 생성
3. 관련 concept/entity 페이지 생성 또는 업데이트
4. `index.md` 업데이트
5. `log.md`에 기록

### 2. Query (질의)
1. 사용자 질문을 받음
2. 관련 페이지를 검색하여 답변 구성
3. 필요시 새로운 synthesis 페이지 생성
4. `log.md`에 기록

### 3. Lint (정합성 검사)
1. 모든 페이지의 프론트매터 유효성 확인
2. 깨진 내부 링크 탐지
3. `index.md`와 실제 페이지 동기화 확인
4. 고아 페이지(index에 없는 페이지) 탐지
5. `log.md`에 결과 기록

## 페이지 유형별 가이드

### Summaries
- 하나의 소스에 대한 요약
- 핵심 주장, 방법론, 결론 포함
- 원본 소스 파일 참조 필수

### Concepts
- 하나의 개념을 깊이 다룸
- 정의, 배경, 관련 개념 링크 포함
- 여러 소스에서 정보를 종합 가능

### Entities
- 인물, 조직, 도구, 프로젝트 등
- 기본 정보와 관련 페이지 링크

### Syntheses
- 여러 소스/페이지를 종합한 분석
- 비교, 트렌드 분석, 통찰 도출
- 참조한 모든 페이지/소스 명시
