---
title: "Capacitor"
created: 2026-05-17
updated: 2026-05-17
sources: []
tags:
  - tool
  - mobile
  - web
  - cross-platform
---

# Capacitor

## 개요

Ionic에서 만든 오픈소스 도구로, 웹앱을 네이티브 앱 껍데기(WebView)에 넣어 Android/iOS 앱스토어에 배포할 수 있게 해준다. Apache Cordova의 후속으로, 네이티브 코드에 직접 접근할 수 있는 것이 특징.

## 기본 정보

- **유형**: 크로스플랫폼 네이티브 런타임
- **개발사**: Ionic
- **라이선스**: MIT
- **공식 사이트**: capacitorjs.com

## 동작 구조

```
┌─────────────────────────┐
│   App Store / Play Store │
├─────────────────────────┤
│   Capacitor (네이티브)    │
│   ┌───────────────────┐ │
│   │  WebView           │ │
│   │  ┌───────────────┐ │ │
│   │  │  웹앱 (게임)   │ │ │
│   │  │  HTML/JS/CSS  │ │ │
│   │  └───────────────┘ │ │
│   └───────────────────┘ │
│  + 네이티브 기능 브릿지   │
└─────────────────────────┘
```

## 개발 흐름

1. 웹으로 게임 개발/테스트 (브라우저)
2. `npx cap add android` / `npx cap add ios` — 네이티브 프로젝트 생성
3. `vite build` → `npx cap sync` — 빌드 결과를 네이티브 프로젝트에 복사
4. Android Studio / Xcode에서 실기기 테스트
5. 각 스토어에 제출

## 주요 플러그인

| 플러그인 | 기능 |
|---------|------|
| `@capacitor/push-notifications` | 푸시 알림 |
| `@capacitor/preferences` | 로컬 저장 |
| `@capacitor-community/admob` | AdMob 광고 |
| `@capacitor-community/in-app-purchases` | 인앱 결제 |
| `@capacitor/haptics` | 진동/햅틱 |

## 유사 도구 비교

| 도구 | 특징 |
|------|------|
| **Capacitor** | 최신, 네이티브 코드 직접 접근 가능 |
| Cordova | Capacitor의 전신, 레거시 |
| TWA | Android 전용, iOS 불가 |
| Electron | 데스크톱 전용 |

## 위키에서의 중요성

[픽셀 경영시뮬 웹 기술 스택 선정](../syntheses/pixel-game-web-stack.md)에서 웹 기반 게임을 앱스토어에 배포하기 위한 래핑 도구로 선정됨. [Phaser](phaser.md)로 만든 게임을 수정 없이 네이티브 앱으로 변환할 수 있다.

## 관련 페이지

- [픽셀 경영시뮬 웹 기술 스택 선정](../syntheses/pixel-game-web-stack.md)
- [Phaser](phaser.md)
