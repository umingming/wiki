# LLM Wiki 규칙

이 디렉토리는 LLM이 관리하는 개인 지식 위키입니다.

## 워크플로우

### Ingest (소스 추가)
1. `sources/`에 원본 자료 저장
2. `pages/summaries/`에 요약 페이지 생성
3. 관련 `pages/concepts/`, `pages/entities/` 페이지 생성 또는 업데이트
4. `index.md` 업데이트 (카테고리별 링크 + 한 줄 요약)
5. `log.md`에 `[YYYY-MM-DD HH:MM] **INGEST**` 형식으로 기록
6. Git 커밋

### Query (질의)
1. 관련 위키 페이지를 참조하여 답변
2. 필요시 `pages/syntheses/`에 종합 분석 페이지 생성
3. `log.md`에 `[YYYY-MM-DD HH:MM] **QUERY**` 형식으로 기록

### Lint (정합성 검사)
1. 프론트매터 유효성, 깨진 링크, index 동기화 확인
2. `log.md`에 `[YYYY-MM-DD HH:MM] **LINT**` 형식으로 기록

## 페이지 컨벤션
- 모든 페이지에 YAML 프론트매터 필수 (title, created, updated, sources, tags)
- 파일명: 소문자, 하이픈 구분 (`transformer-architecture.md`)
- 내부 링크: 상대 경로 사용
- 상세 규칙은 `schema.md` 참조
