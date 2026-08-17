---
type: concept
title: "LLM Wiki"
status: active
created: 2026-04-15
updated: 2026-08-17
sources:
  - "[[sources/articles/karpathy-2026-llm-wiki]]"
related:
  - "[[pages/concepts/andrej-karpathy]]"
---

# LLM Wiki

## 정의

LLM Wiki는 Andrej Karpathy가 제안한 개인 지식 관리 패턴이다. LLM이 원본 자료(논문, 기사, 영상 등)를 읽고, 구조화된 마크다운 위키 페이지를 생성·관리하는 "compounding artifact" 방식이다. 이 위키 자체가 이 패턴의 구현이다(스키마: `CLAUDE.md`).

## 핵심 아이디어

- **LLM as Curator**: 사람이 직접 정리하는 대신, LLM이 자료를 읽고 위키 페이지를 작성한다.
- **Compounding Artifact**: 자료가 쌓일수록 위키의 가치가 복리처럼 증가한다. 새 자료가 기존 페이지와 연결되며 지식 그래프가 풍부해진다.
- **Plain Text**: 마크다운 파일 기반으로 어떤 LLM이든 접근 가능하며, Git으로 버전 관리된다.

## 워크플로우

### Ingest
원본 자료를 위키에 추가하는 과정:
1. 소스를 `sources/` 폴더에 캡처 (발췌·핵심 주장 포함)
2. 관련 개념(concept), 분석(analysis), 결정(decision), 노트(note) 페이지 생성 또는 업데이트
3. 인덱스 재생성 및 로그 업데이트

### Query
위키를 기반으로 질의에 답변하는 과정:
1. 사용자 질문을 받음
2. 관련 위키 페이지를 참조하여 답변 구성
3. 재사용 가치가 있으면 분석/노트로 재편입

### Lint
위키의 정합성을 검사하는 과정:
1. 프론트매터 유효성, 깨진 링크, 인덱스 동기화 확인
2. 품질 유지를 위한 자동 점검 (`scripts/wiki-lint.mjs`)

## 장점

- **지식 축적**: 한번 정리한 내용은 영구적으로 재활용 가능
- **크로스 레퍼런스**: 여러 소스의 정보가 자연스럽게 연결
- **LLM 독립적**: 특정 LLM에 종속되지 않음 (plain text)
- **버전 관리**: Git으로 변경 이력 추적

## 관련 개념

- [Zettelkasten](https://en.wikipedia.org/wiki/Zettelkasten) - 유사한 메모 기반 지식 관리 방법론
- Second Brain / PARA Method - 개인 지식 관리 프레임워크
- RAG (Retrieval-Augmented Generation) - 위키를 검색 소스로 활용 가능

## 관련 페이지

- [[sources/articles/karpathy-2026-llm-wiki]] — 원본 제안서 캡처 (발췌·핵심 주장)
- [[pages/concepts/andrej-karpathy]] — 패턴 제안자
