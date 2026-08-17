# 개인 지식 위키 스키마 (CLAUDE.md)

이 레포(`~/wiki`)는 LLM이 관리하는 **개인 지식 위키**다 (레포 루트 = 위키 루트).
Claude가 이 레포에서 작업할 때 이 스키마를 먼저 읽고 따른다.

## 이 레포가 무엇인가

LLM이 원본 자료를 읽고 구조화된 페이지를 유지하며, 가치 있는 답변을 다시 지식 자산으로 재편입하는 개인 지식 베이스다. 개인 프로젝트와 기술 리서치의 결정(WHY)·근거(분석·원본)를 기록하고, 구현(WHAT)은 각 프로젝트 레포에 링크만 한다.

## 이게 무엇인가 (그리고 아닌 것)

- 이 위키는 **WHY·HOW 레이어**다: *왜 이렇게 결정했나 / 무엇을 참고했나 / 어떻게 분석했나.*
- 실제 **구현(코드)의 레퍼런스가 아니다.** "무엇(WHAT)을 어떻게 짰나"는 각 프로젝트 레포의 코드/커밋에 있고, 위키는 그걸 **링크만** 한다. 절대 복사하지 않는다.
- 조직 원리(why-chain):
  > **결정(decision) → 근거가 된 분석(analysis) → 분석이 인용한 참고/불변 원본(sources) → 판단 기준인 원칙(principle)**

## 디렉토리 구조

```
wiki/
├── CLAUDE.md              # 이 스키마
├── index.md              # 카탈로그 (type별, 자동 생성 — 수기 편집 금지)
├── decisions-index.md    # 결정 인덱스 (status별, 자동 생성)
├── llms.txt              # 에이전트 진입점 (자동 생성)
├── log.md                # 시간순 작업 기록
├── inbox/                # 캡처 온램프: PR 링크·회의 노트·붙여넣기를 여기 던지면 Claude가 초안화
├── sources/              # 참고 / REFERENCES — Layer 1, 불변
│   ├── articles/         # 기사·블로그·Gist 캡처
│   ├── frameworks/       # 프레임워크/도구 문서 스냅샷
│   ├── papers/ media/    # 논문 · 영상/이미지
├── pages/                # Layer 2 — Claude가 관리
│   ├── decisions/        # 결정 / DECISIONS (ADR/RFC 로그)
│   ├── analysis/         # 분석 / ANALYSIS (teardown·매트릭스·tradeoff·spike)
│   ├── principles/       # 원칙 (3~5개, 안정)
│   ├── concepts/         # 개념
│   ├── notes/            # 작은 배움·TIL (결정 연결 의무 없음 — 가장 가벼운 타입)
│   ├── glossary.md       # 통제 어휘 (용어가 흔들리면 문서·코드·에이전트 답변이 어긋난다)
│   └── guides/           # how-we-decide, agents.md
├── templates/            # 페이지 양식 7종 (decision·analysis·principle·concept·guide·note·reference)
└── scripts/              # build-index.mjs, new.mjs, wiki-lint.mjs
```

## 2 레이어

1. **sources/ 는 불변이다** — 외부 원본을 캡처한 진실의 원천. 캡처 후 본문 수정하지 않는다(오직 back-link·status만).
2. **pages/ 는 Claude가 소유한다** — 생성·갱신·삭제. 단, `decision`은 아래 불변 규칙을 따른다.

## 페이지 타입

| type | 폴더 | 불변 여부 |
|------|------|-----------|
| `decision` | pages/decisions/ | **accepted 후 불변** (아래 규칙) |
| `analysis` | pages/analysis/ | 살아있는 문서 (갱신) |
| `principle` | pages/principles/ | 살아있는 문서 (갱신) |
| `concept` | pages/concepts/ | 살아있는 문서 |
| `guide` | pages/guides/ | 살아있는 문서 |
| `note` | pages/notes/ | 살아있는 문서 — **가장 가벼움** (TIL·실험 메모, 결정 연결 의무 없음) |
| `source` | sources/ | **항상 불변** |

`note` 는 결정 파이프라인 밖의 통로다: `informed_decisions`·`sources` 의무가 없고 필수 frontmatter 도 `type`/`title`/`created` 뿐. 결정까지는 아닌 작은 배움을 부담 없이 쌓고, 나중에 결정 재료가 되면 분석/결정으로 승격한다.

## Frontmatter 규격

- **언어 규칙:** 본문·섹션 헤더는 **한국어**(고유명사·기술 용어 원어 병기 가능). frontmatter 값은 **영문 enum / ID / ISO 날짜**로 기계가 읽을 수 있게.

### 도메인(domain) enum

결정·분석의 `domain`은 아래 중 하나:

| domain | 범위 |
|--------|------|
| `knowledge-ai` | LLM·AI·지식 관리 (LLM Wiki 패턴 등) |
| `game-dev` | 게임 개발 — 엔진·에셋·모바일 배포 스택 |
| `web-dev` | 웹 개발 일반 — 프레임워크·아키텍처 |
| `tooling` | 개발 도구·환경·워크플로우 |
| `meta` | 위키 자체 |

### 결정(decision) frontmatter

| 필드 | 필수 | 설명 |
|------|:----:|------|
| `id` | ✅ | `DEC-####`. **생성 시 즉시 부여**, 단조증가, 재사용 금지. 슬러그·title이 바뀌어도 변치 않는 앵커. |
| `type` | ✅ | `decision` |
| `title` | ✅ | 문제→해결 어투 |
| `status` | ✅ | `proposed \| accepted \| superseded \| deprecated \| rejected` |
| `domain` | ✅ | 위 domain enum |
| `owner` | ✅ | 담당자 **1인** (팀 아님 — 분산 소유는 썩음 1순위) |
| `created` | ✅ | ISO 날짜 |
| `updated` |  | ISO 날짜 |
| `y_statement` |  | 한 줄 요약 (에이전트 triage용) |
| `last_verified` |  | (선택) 마지막으로 유효성 확인한 날 — accepted 후에도 갱신 허용 |
| `sources` |  | 근거 `[[sources/...]]` 배열 |
| `supersedes` |  | 이 결정이 대체한 옛 결정 `[[...]]` |
| `superseded_by` |  | 나를 대체한 새 결정 (대체될 때 추가) |
| `impl_pr` |  | 구현 PR/커밋 (결정→코드 다리, 고아 방지) |

### 상태 enum (두 종류, 절대 섞지 않는다)

- **결정:** `proposed → accepted → superseded | deprecated | rejected`
- **지식(analysis/principle/concept/guide):** `draft | active | stale | archived`

---

## ★ 결정 불변 규칙 (이 위키에서 가장 중요)

1. **`status: accepted` 되는 순간 본문 동결.** (`proposed` 동안은 자유롭게 수정 — 아직 확정 전)
2. accepted 이후 **허용되는 변경은 이것뿐:**
   - `status` (accepted → superseded / deprecated)
   - `superseded_by` (대체될 때 추가)
   - `updated` / `last_verified` 날짜
   - **오타·깨진 링크 수정** (내용 의미 변경 X)
   - **`## 회고` 섹션 append** (배포 후 결과 기록 — 추가만, 기존 문장 수정 X)
3. 그 외 본문 수정 **금지.** 내용이 바뀌면 → **대체(supersede)**, 수정 아님.
4. **대체 절차:**
   1. 새 결정 문서 생성 + `supersedes: [[옛 결정]]`
   2. 옛 문서: `status: superseded` + `superseded_by: [[새 결정]]` (양방향)
   3. 옛 문서 본문은 그대로 둔다. 대체된 결정도 "한때 맞았던 역사"로 보존.
5. `deprecated` = 대체 없이 폐기 / `rejected` = 채택 안 함. 둘 다 **삭제하지 말고 남긴다.**
6. **적용 범위:** `decision`(accepted 후) + `sources/` 는 불변. `analysis`/`principle`/`concept`/`guide` 는 살아있는 문서로 갱신한다.

## "결정 문서로 남길 가치가 있는가?"

- 기준: **미래의 나(또는 에이전트)가 "왜 이렇게 했지?"를 물을 만한 갈림길**이면 남긴다. 갈림길이 아니었으면 결정이 아니다.
- 결정까지는 아닌 작은 배움·실험 메모는 `note`(pages/notes/)로 가볍게 남긴다.
- 작은 결정 → 4섹션 경량(맥락/결정/결과/링크). 큰 결정 → `검토한 선택지 / 장단점 / 검증 방법` 추가.

## ID · 파일명 · Wikilink

- ID: 결정 `DEC-####`, 분석 `ANL-####`. 생성 시 즉시 부여, 단조증가, 재사용 금지. (형식·중복은 lint 가 검사)
- 파일명: kebab-case 영문. 예: `dec-0001-<slug>.md`.
- 링크: 페이지 간은 `[[wikilink]]`. 새 페이지를 만들 때 관련 기존 페이지에 역링크를 추가한다.
- 새 페이지는 `node scripts/new.mjs <decision|analysis|principle|concept|guide|note|source> <slug> [title]` 로 만들면 ID·날짜·owner가 자동 stamp 된다. (`source` 는 slug 를 `<category>/<slug>` 로, `note` 는 파일명에 날짜 prefix 가 붙는다)

## 크로스링크 규칙

- 모든 **결정은 근거를 남긴다**: 오래 참조할 원본은 `sources` 캡처, 가벼우면 본문 링크 섹션의 생 URL. accepted 결정에 `sources` 가 비면 lint 가 지적한다(심각도는 스캐폴드 설정 — 팀 error / 1인 warn). 중요하면 관련 분석 + `impl_pr` 링크도.
- 모든 **분석은 결정(또는 백로그)으로 귀결**: `informed_decisions`로 자신이 이끈 결정(`DEC-####`)을 가리킨다. **아직 결정 전이면 `[[pages/decisions/open-decisions]]`(백로그)를 가리켜도 되며, DEC 승격 시 실제 DEC로 교체한다.** (안 그러면 shelfware)
- 대체는 frontmatter + `[[wikilink]]` 양방향으로 미러링.

## AX(에이전트) 장치 — 전부 frontmatter에서 생성

- `index.md` (type별), `decisions-index.md` (status별), `llms.txt` — **생성물, 수기 편집 금지.** `node scripts/build-index.mjs` 로 재생성.
- `pages/guides/agents.md` = 에이전트 진입점 (권위 순서·인용법·경계).

## Lint (강제)

`scripts/wiki-lint.mjs`로 점검: 깨진 `[[wikilink]]` / ID 형식·placeholder·중복 / domain enum 위반 / source category↔폴더 불일치 / 고아 결정(accepted 인데 sources 없음) / 분석의 `informed_decisions` 누락·잘못된 대상(결정/open-decisions 아님) / **accepted 결정의 본문·frontmatter 동결 위반(git HEAD 기준, 순수 append 와 status/superseded_by/updated/last_verified 는 허용)** / index·llms.txt drift / stale.
→ 실행: 레포 루트에서 `node scripts/wiki-lint.mjs --report && node scripts/build-index.mjs --check`. **커밋 전 통과가 게이트다.** 오타·회고 수정을 승인할 땐 `--allow-edit=<slug>` 로 해당 문서만 예외 처리.

## 하지 말 것

- **accepted 결정 본문 수정** (→ 대체하라)
- sources/ 원본 수정
- 구현 코드/API를 위키에 복사 (→ 각 프로젝트 레포의 코드/커밋 링크)
- frontmatter 없이 pages/ 파일 생성
- index.md / decisions-index.md / llms.txt 수기 편집 (→ build-index.mjs)
- 근거 없는 결정 작성 (sources 캡처, 최소한 생 URL 링크)

## 운영 워크플로우 (개인 위키 관습)

### Ingest ("위키에 추가해줘")
1. 원본은 `node scripts/new.mjs source <category>/<slug> "제목"` 으로 `sources/` 에 캡처 (발췌·핵심 주장 포함)
2. 관련 `pages/` 생성 또는 갱신 — 작은 배움은 `note`, 개념은 `concept`, 비교·조사는 `analysis`, 갈림길은 `decision`
3. `node scripts/build-index.mjs` 로 인덱스 3종 재생성
4. `log.md` 기록 → lint 게이트 통과 → Git 커밋

### Query (질의)
1. `llms.txt` 또는 `index.md` 를 먼저 읽고 관련 페이지를 탐색해 답한다
2. 재사용 가치가 있는 답변은 `note`/`analysis` 로 재편입하고, 결정할 갈림길이 드러나면 백로그 [[pages/decisions/open-decisions]] 에 올린다
3. 인덱스 재생성 → `log.md` 기록 → 커밋

## 커밋

이 위키는 독립 Git 레포다. **한 작업 = 한 커밋**, 커밋 전 `node scripts/wiki-lint.mjs --report && node scripts/build-index.mjs --check` 통과가 게이트다. 1인 레포이므로 직접 커밋한다. 커밋 메시지: `wiki(<type>): <한 줄>` (`decision`/`analysis`/`note`/`add`/`update`/`supersede`/`lint`/`meta`).
