---
type: concept
title: "Phaser"
status: active
created: 2026-05-17
updated: 2026-08-17
related:
  - "[[pages/notes/2026-05-17-pixel-game-web-stack]]"
  - "[[pages/concepts/capacitor]]"
  - "[[pages/concepts/aseprite]]"
---

# Phaser

## 개요

웹 브라우저용 2D 게임 프레임워크. HTML5 Canvas와 WebGL 기반으로 동작하며, JavaScript/TypeScript로 게임을 개발한다. 2013년 첫 출시 이후 웹 2D 게임 분야에서 가장 널리 사용되는 프레임워크.

## 기본 정보

- **유형**: 오픈소스 2D 게임 프레임워크
- **언어**: JavaScript / TypeScript
- **라이선스**: MIT
- **현재 버전**: Phaser 3 (Phaser 4 개발 중)
- **공식 사이트**: phaser.io

## 주요 기능

- 픽셀 퍼펙트 렌더링 (`pixelArt: true` 설정)
- 스프라이트, 애니메이션, 타일맵 내장 지원
- 씬(Scene) 기반 게임 구조 관리
- 물리 엔진 (Arcade, Matter.js)
- Tiled 맵 에디터 연동 플러그인
- 입력 처리 (키보드, 마우스, 터치)

## 위키에서의 쓰임

[[pages/notes/2026-05-17-pixel-game-web-stack]]에서 카이로소프트 스타일 도트 게임의 메인 엔진으로 선정됨. 웹 개발 경험을 그대로 활용할 수 있고, [[pages/concepts/capacitor]]와 조합하여 모바일 앱스토어 배포가 가능하다.

## 관련 페이지

- [[pages/notes/2026-05-17-pixel-game-web-stack]] — 웹 기술 스택 정리
- [[pages/concepts/capacitor]]
- [[pages/concepts/aseprite]]
