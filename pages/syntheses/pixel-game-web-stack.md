---
title: "픽셀 경영시뮬 모바일 게임 — 웹 기술 스택 선정"
created: 2026-05-17
updated: 2026-05-17
sources: []
tags:
  - game-dev
  - pixel-art
  - mobile
  - web
  - synthesis
---

# 픽셀 경영시뮬 모바일 게임 — 웹 기술 스택 선정

## 질문

- 카이로소프트/월간아이돌 스타일의 아기자기한 도트 경영시뮬 모바일 게임을 만들 때, 어떤 기술 스택이 적합한가?
- 웹 기술로 만들어도 되는가, 네이티브가 필요한가?
- 앱스토어 배포는 어떻게 하는가?

## 짧은 결론

- 이 장르는 고성능 렌더링이 불필요하므로 **웹 기술로 충분**하다.
- 추천 조합: **[Phaser](../entities/phaser.md) 3 + TypeScript + Vite + [Capacitor](../entities/capacitor.md)**.
- 도트 에셋 제작에는 **[Aseprite](../entities/aseprite.md)** + **Tiled**(타일맵 에디터)가 적합하다.
- 평소 개발은 브라우저에서 하고, 스토어 배포 시 Capacitor로 네이티브 래핑한다.
- 네이티브 전환은 3D, 실시간 PvP, AR 등 고성능 요구가 있을 때만 필요하다.

## 핵심 근거

- 카이로소프트/월간아이돌 장르 분석 (UI + 데이터 로직 중심, 터치 입력 단순)
- 웹 기반 게임 성공 사례 (CrossCode — Phaser 기반, Vampire Survivors — 초기 웹 기반)
- 기존 웹 개발 경험 활용 가능성

## 분석

### 게임 엔진/프레임워크 비교

| 엔진 | 언어 | 웹 배포 | 모바일 배포 | 특징 |
|------|------|---------|------------|------|
| **Phaser 3** | JS/TS | 네이티브 지원 | Capacitor 래핑 | 웹 2D 게임 최고 성숙도, 픽셀 퍼펙트 렌더링 |
| Kaboom.js / Kaplay | JS | 네이티브 지원 | Capacitor 래핑 | API 간결, 프로토타이핑 빠름, 대규모 UI 관리 약함 |
| Godot | GDScript/C# | 가능 | 네이티브 빌드 | 무료 오픈소스, 2D 강력, 웹 개발자에겐 학습 필요 |
| Unity | C# | WebGL | 네이티브 빌드 | 생태계 성숙, 이 장르에는 오버스펙 |
| PICO-8 | Lua | 가능 | 제한적 | 128x128 판타지 콘솔, 레트로 극대화, 상용 배포 어려움 |
| Flutter + Flame | Dart | 가능 | 네이티브 빌드 | UI 비중 큰 게임에 적합, 게임 생태계 작음 |

### 웹앱 vs 네이티브 판단 기준

**웹으로 충분한 이유:**

- 도트 그래픽은 렌더링 부하 극히 낮음
- 경영 시뮬은 UI + 데이터 로직 중심 (60fps도 과분)
- 터치 입력이 단순 탭/드래그 수준
- 저장은 LocalStorage / IndexedDB로 처리 가능

**네이티브가 필요한 경우:**

- 3D 렌더링, 고성능 물리 연산
- 복잡한 실시간 멀티플레이어 (PvP)
- 네이티브 하드웨어 깊은 접근 (AR, 블루투스)
- 고사양 파티클/셰이더 효과

### 모바일 스토어 배포 방식

| 방식 | 설명 |
|------|------|
| **Capacitor** | 웹앱을 네이티브 WebView에 래핑, Android/iOS 모두 지원 |
| PWA | 홈 화면 추가로 앱처럼 동작, 스토어 심사 불필요 |
| TWA | Android 전용, Play Store 배포 가능, iOS 불가 |

### Capacitor 개발 흐름

```
브라우저에서 Phaser 게임 개발/테스트
  → vite build (dist/ 생성)
  → npx cap sync (네이티브 프로젝트에 복사)
  → Android Studio / Xcode에서 실기기 테스트
  → 각 스토어 제출
```

Capacitor 플러그인으로 네이티브 기능도 사용 가능:

- 푸시 알림: `@capacitor/push-notifications`
- AdMob 광고: `@capacitor-community/admob`
- 인앱 결제: `@capacitor-community/in-app-purchases`
- 진동/햅틱: `@capacitor/haptics`

### 도트 에셋 제작 도구

| 도구 | 용도 |
|------|------|
| **Aseprite** | 캐릭터 스프라이트, 걷기 애니메이션 |
| **Tiled** | 타일맵 에디터 — 건물 배치, 맵 구성 (Phaser 플러그인 있음) |
| **TexturePacker** | 스프라이트 시트 패킹 |
| **Piskel** | 웹 기반 무료 픽셀 에디터 |
| **Lospec** | 팔레트, 튜토리얼 모음 |

### 무료 에셋 사이트

- **itch.io** — 무료/유료 픽셀 에셋 다수 (`pixel-art` 태그)
- **OpenGameArt.org** — 오픈 라이선스 게임 아트
- **Kenney.nl** — 고품질 무료 에셋 팩

### 카이로소프트 스타일 핵심 구현 요소

```
├── 타일맵 기반 월드 (건물, 시설 배치)
├── NPC 자동 이동 (간단한 AI/패스파인딩)
├── 시간 시스템 (턴 or 실시간 타이머)
├── 스탯/경영 데이터 관리
├── 이벤트 시스템 (랜덤 이벤트, 선택지)
└── UI 레이어 (메뉴, 팝업, 그래프)
```

## 모순 또는 불확실성

- Phaser 4가 개발 중이나 안정 릴리스 시점이 불확실. 현 시점에서는 Phaser 3이 안전한 선택.
- Capacitor WebView 성능은 기기에 따라 차이가 있을 수 있으나, 이 장르에서는 문제 되지 않을 수준.
- 인앱 결제/광고 SDK의 Capacitor 플러그인 유지보수 상태는 사용 전 확인 필요.

## 위키에 반영할 변경

- [x] Entity 페이지 생성: [Phaser](../entities/phaser.md), [Capacitor](../entities/capacitor.md), [Aseprite](../entities/aseprite.md)

## 관련 페이지

- [Phaser](../entities/phaser.md)
- [Capacitor](../entities/capacitor.md)
- [Aseprite](../entities/aseprite.md)

## 후속 질문

- Phaser 3 + Capacitor 프로젝트 초기 세팅 구체적 절차는?
- 카이로소프트 스타일 타일맵 + NPC 이동 구현 패턴은?
- 수익화 모델(광고 vs 유료 vs 인앱결제) 비교는?
