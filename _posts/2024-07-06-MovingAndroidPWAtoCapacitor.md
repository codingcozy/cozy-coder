---
title: "Capacitor로 Android PWA 이전하는 방법"
description: ""
coverImage: "/assets/img/2024-07-06-MovingAndroidPWAtoCapacitor_0.png"
date: 2024-07-06 03:14
ogImage: 
  url: /assets/img/2024-07-06-MovingAndroidPWAtoCapacitor_0.png
tag: Tech
originalTitle: "Moving Android PWA to Capacitor"
link: "https://medium.com/adamant-im/moving-android-pwa-to-capacitor-e64b923284c0"
isUpdated: true
---





이전에는 몇 가지 제한 사항이 있는 Android 앱을 빌드하기 위해 pwabuilder.com을 사용했습니다:

- 타겟 API 레벨 구성 지원되지 않음
- 별도의 코드 제어 불가
- 자동화 불가

PWA v4.7 업데이트로 ADM의 Android 앱은 Capacitor.js로 이동되었습니다.

<div class="content-ad"></div>

***Full control of native code, Cordova API for running native code such as Push notifications and Camera, optimized code, custom plugins, and CI/CD automation.***  

You can now download the latest version of the Android ADAMANT app from Google Play.  

![ADAMANT app](/assets/img/2024-07-06-MovingAndroidPWAtoCapacitor_1.png)

### Why choose Capacitor.js?  

<div class="content-ad"></div>

ADAMANT Messenger는 앞서 나가는 분산형 메시징 플랫폼입니다. 우리의 주요 목표는 앱이 뛰어난 성능을 발휘하고 안전하며 유지보수가 쉽도록 하는 것입니다. 우리가 Capacitor.js를 선택한 이유는 다음과 같습니다:

- 통합의 용이성: Capacitor는 Vue.js와 같은 현대적인 웹 프레임워크와 매끄럽게 통합됩니다.
- 크로스 플랫폼: 한 번 빌드하고 iOS, Android, Web과 같은 다양한 플랫폼에 배포할 수 있습니다.
- 네이티브 기능: 웹 경험을 희생하지 않으면서 네이티브 API에 액세스할 수 있습니다.
- 커뮤니티와 지원: 활발한 개발 및 견고한 문서화

![이미지](/assets/img/2024-07-06-MovingAndroidPWAtoCapacitor_2.png)

# 비교: 네이티브 안드로이드, PWABuilder 및 Capacitor.js

<div class="content-ad"></div>

우리의 선택을 더 잘 이해하기 위해 이 비교를 살펴보세요.

## 네이티브 안드로이드 개발

장점:

- 모든 안드로이드 기능과 API에 완전히 액세스할 수 있음
- 안드로이드 장치에 최적화된 고성능
- UI 및 기능에 대한 세세한 제어

<div class="content-ad"></div>

단점:
- Java/Kotlin 지식 필요
- 각 플랫폼(iOS, Android) 별 별도의 코드베이스 필요
- 시간이 많이 소요되며 개발 비용이 높을 수 있음

# PWABuilder

장점:

<div class="content-ad"></div>

- PWA를 네이티브 앱으로 쉽게 변환할 수 있어요.
- 최소한의 설정으로 빠르게 배포할 수 있어요.
- 네이티브 기능이 제한된 간단한 앱에 적합해요.

단점:
- 네이티브 기기 기능에 접근이 제한되어요.
- 성능이 완전한 네이티브 앱만큼 우수하지 않을 수 있어요.
- 변환에 써드파티 서비스에 의존해야 해요.

# Capacitor.js

<div class="content-ad"></div>

### 장점:

- 단일 코드베이스로 다중 플랫폼 지원
- 네이티브 API 및 플러그인 접근 가능
- 현대적인 웹 개발 도구 및 프레임워크 지원
- 활발한 커뮤니티 및 지속적인 업데이트

### 단점:

- 웹네이티브 브릿지에 익숙하지 않은 경우 가파른 학습 곡선
- 일부 네이티브 기능은 여전히 사용자 정의 플러그인이 필요할 수 있음

<div class="content-ad"></div>

# 기술적 구현

우리는 Capacitor.js를 사용하여 Android 앱을 네이티브로 빌드하고 Github Actions를 활용하였습니다:

- GitHub Actions 워크플로우 추가
- Capacitor 구성 추가
- Manifest 파일 추가
- 스플래시 스크린 이미지 및 앱 아이콘 포함
- 빌드 스크립트 작성

전체 변경 내용은 풀 리퀘스트에서 확인하실 수 있어요. 🌟

<div class="content-ad"></div>

![Moving Android PWA to Capacitor](/assets/img/2024-07-06-MovingAndroidPWAtoCapacitor_3.png)