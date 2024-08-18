---
title: "iOS Android용 앱 아이콘과 스플래시 스크린 만들기 다크라이트 테마  Compose Multiplatform 가이드"
description: ""
coverImage: "/assets/img/2024-07-10-AppIconSplashScreenforiOSAndroidDarkLightComposeMultiplatform_0.png"
date: 2024-07-10 01:11
ogImage:
  url: /assets/img/2024-07-10-AppIconSplashScreenforiOSAndroidDarkLightComposeMultiplatform_0.png
tag: Tech
originalTitle: "App Icon , Splash Screen for iOS Android (Dark Light) — Compose Multiplatform"
link: "https://medium.com/@stevdza-san/app-icon-splash-screen-for-ios-android-dark-light-compose-multiplatform-382e9c647d24"
isUpdated: true
---

안녕하세요!

컴포즈 멀티플랫폼 프레임워크의 인기가 높아지면서 안드로이드 개발자 여러분들은 코드베이스를 대부분 공유하지만 iOS 환경에서 어떤 것들을 할 수 있어야 하는지 배워야 합니다.

# 그래픽 자산

여기에 제가 앱 아이콘 및 스플래시 화면에 사용할 그래픽 자산을 준비했습니다.

<!-- cozy-coder - 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

![App Icon](/assets/img/2024-07-10-AppIconSplashScreenforiOSAndroidDarkLightComposeMultiplatform_1.png)

- 제일 왼쪽에는 로고 자체와 배경색이 포함된 앱 아이콘이 있습니다.
- 오른쪽에는 배경 없이 로고만 있는 것이 있습니다. 안드로이드 환경에서는 두 개의 레이어로 구성된 앱 아이콘을 지정해야 합니다. 전경과 배경 레이어가 있습니다. 반면 iOS 부분에서는 두 레이어를 필요로 하지 않고 하나만 있으면 됩니다.
- 맨 아래 것은 안드로이드에서 특정하게 스플래쉬 화면에 사용됩니다.

이 모든 에셋은 픽셀 단위로 1024 x 1024의 크기를 가지고 있습니다. 가장 아래 것은 240 x 240의 크기를 가지고 있습니다. 첫 번째 것은 .PNG 형식으로 내보낼 수 있습니다(이것은 iOS에서 사용될 것입니다). 다른 것들은 .SVG로 내보낼 수 있습니다. 배경이 없는 앱 아이콘만 .PNG로 내보낼 수도 있습니다(이것은...
