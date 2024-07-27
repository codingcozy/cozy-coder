---
title: "Capacitor로 SAMA 모바일 클라이언트 구축 하는 방법"
description: ""
coverImage: "/assets/img/2024-07-02-BuildingSAMAmobileclientwithCapacitor_0.png"
date: 2024-07-02 23:09
ogImage: 
  url: /assets/img/2024-07-02-BuildingSAMAmobileclientwithCapacitor_0.png
tag: Tech
originalTitle: "Building SAMA mobile client with Capacitor"
link: "https://medium.com/sama-communications/building-sama-mobile-client-with-capacitor-4b58c1de485e"
---


## 웹 앱을 모바일 앱으로 단 3번의 클릭으로 변경하는 방법

우리 모두가 원래, Web 💻 용 JS 코드를 작성하고 Android 및 iOS와 같은 원시 앱 📱에 사용할 수 있다면 상용화 할 수 있는 코드를 작성하고 싶어 했습니다. 이제 그 꿈이 이뤄졌음을 자랑스럽게 말할 수 있게 되었습니다.

#️⃣ 도구의 핵심

SAMA 클라이언트 내에서 활용한 Capacitor를 환영합니다. 웹 네이티브 앱을 만드는 데 사용되며 완전한 네이티브 SDK에 대한 완전한 액세스 포기 없이 웹을 먼저 사용합니다. 간단히 말해, Capacitor는 개발자가 크로스 플랫폼 모바일 애플리케이션을 작성하는 데 도움이 되는 Ionic에서 개발한 소프트웨어 도구입니다. 이 도구를 통해 HTML, CSS 및 JavaScript와 같은 웹 기술을 사용하여 iOS, Android 및 웹 브라우저에서 실행할 수 있는 애플리케이션을 구축할 수 있습니다.

<div class="content-ad"></div>

SAMA 클라이언트를 Capacitor 통합으로 확장했고 이제 네이티브 플랫폼용으로 빌드할 수 있습니다. 기존 앱에 Capacitor를 추가하는 것도 꽤 간단하며 몇 가지 작은 단계만 따르면 되어 몇 분 내로 완료할 수 있습니다. 더 흥미로운 점은 네이티브 플랫폼에서의 작동 방식이며, Capacitor 워크플로우의 세부 내용을 설명하면서 알려드리겠습니다.

## 🔹 빌드 프로세스

먼저, 모바일 기기에서 웹 앱을 테스트할 준비가 되면 Capacitor와 함께 사용하려면 배포를 위해 웹 앱을 빌드해야 합니다. 일반적으로 앱을 빌드하는 방법에 대해 걱정할 필요가 없습니다. 빌드하는 방법은 항상 사용하는 방법을 따르면 됩니다. SAMA 클라이언트 리포지토리를 사용할 경우 다음 명령을 실행하세요:

```js
npm run build
```

<div class="content-ad"></div>

## 🔹 코드 변환

두 번째로 가장 중요한 점은 빌드를 준비할 때, 웹 코드를 웹 네이티브 Capacitor 애플리케이션에 푸시해야 한다는 것입니다. 이를 위해 다음 명령어를 사용할 수 있습니다:

```js
npm run capacitor:sync
```

이 명령은 이미 빌드된 웹 번들을 Android 및 iOS 프로젝트로 복사하고 Capacitor가 사용하는 네이티브 종속성을 업데이트할 것입니다. 웹 번들을 네이티브 프로젝트로 동기화한 후에는 모바일 기기에서 애플리케이션을 테스트할 수 있게 될 것입니다.

<div class="content-ad"></div>

## 🔹Result preview

카파시터 앱의 디버그 빌드를 iOS 장치에서 테스트하려면 다음 명령을 실행하세요:


npm run ios


Android의 경우:

<div class="content-ad"></div>

```bash
npm run android
```

한 번 애플리케이션을 테스트하고 나면, 다른 모바일 기기에 배포할 때 사용할 네이티브 이진 파일을 컴파일해야 합니다. Capacitor에는 이를 도와 주는 명령이 없으므로 해당 플랫폼의 IDE를 열어야 합니다: iOS의 경우 Xcode, Android의 경우 Android Studio를 사용하여 최종 이진 파일을 직접 컴파일하고 각각 App Store 및 Google Play Store에 제출할 예정입니다.

# 📃 결론

동일한 코드베이스로 다양한 플랫폼에 애플리케이션을 제공하는 프로세스가 얼마나 쉬운지 알 수 있었습니다. SAMA 클라이언트는 이 기회를 제공하기위해 한 발짝 나아갔습니다. 이제 여러분은 SAMA 웹 애플리케이션으로만 제한되지 않고 Capacitor를 통해 강력한 모바일 앱을 구축할 수 있으며, 해당 스토어에 게시되기를 기다릴 수 있게 되었습니다.

<div class="content-ad"></div>

# 📌 SAMA를 시도해보세요

로컬에서 SAMA 서버를 실행하려면 — https://github.com/SAMA-Communications/sama-server를 참고하세요. 프론트엔드 앱은 https://github.com/SAMA-Communications/sama-client에서 확인할 수 있습니다.

발견하고 즐기고, 모든 피드백을 환영합니다. 모든 GitHub 스타, 이슈, 또는 댓글에 많은 감사를 드리겠습니다! 🌟