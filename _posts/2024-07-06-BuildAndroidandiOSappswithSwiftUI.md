---
title: "SwiftUI로 Android와 iOS 앱 만들기 방법"
description: ""
coverImage: "/assets/img/2024-07-06-BuildAndroidandiOSappswithSwiftUI_0.png"
date: 2024-07-06 03:16
ogImage: 
  url: /assets/img/2024-07-06-BuildAndroidandiOSappswithSwiftUI_0.png
tag: Tech
originalTitle: "Build Android and iOS apps with SwiftUI"
link: "https://medium.com/@rohitsainier/build-android-and-ios-apps-with-swiftui-7c89a92c184f"
---


오늘날, iOS와 안드로이드 모두를 대상으로하는 앱을 보유하는 것이 성공적인 비즈니스 전략의 중요한 측면이 됩니다. 그러나 각 플랫폼별로 별개의 앱을 만드는 것은 시간이 많이 걸리고 비용이 많이 드는 일일 수 있습니다. 이러한 도전에 대처하기 위해 많은 기업들이 플러터, 리액트 네이티브 및 코틀린 멀티플랫폼(KMP)과 같은 인기있는 프레임워크를 사용한 하이브리드 또는 크로스 플랫폼 앱 개발을 선택합니다.

개발자들에게는 이러한 프레임워크로 전환하는 것이 비교적 간단할 수 있습니다. 네이티브 Android 개발자들은 쉽게 KMP를 채택할 수 있으며, 리액트 개발자들은 기존의 React 기술을 활용하여 React Native를 사용할 수 있습니다. 그러나 iOS 개발자들은 종종 두 플랫폼을 위한 앱을 구축하기 위해 새로운 지식을 습득해야 하기 때문에 더 큰 학습 곡선에 직면할 수 있습니다.

이때 Skip이 구원의 손길을 내밀어줍니다. Skip은 iOS 개발자들이 익숙한 Xcode 환경 내에서 Swift와 SwiftUI에 대한 기존 지식을 활용하여 안드로이드 앱을 만들 수 있도록 합니다. 이는 개발 프로세스를 최적화할 뿐만 아니라 iOS 개발자들이 사랑하는 도구와 언어로 계속 작업할 수 있게 함으로써 궁극적으로 iOS와 안드로이드 앱 개발 사이의 간극을 줄여줍니다.

<div class="content-ad"></div>

# Skip을 사용하여 iPhone과 Android용 원래 앱 만들기:

Skip은 Swift 앱 개발을 Android로 가져옵니다. 이는 개발자들이 동시에 iOS 및 Android용으로 진정한 원래 앱을 만들 수 있는 단일한 현대 프로그래밍 언어(Swift)와 일류 개발 환경(Xcode)을 사용할 수 있게 하는 도구입니다.

# Skip이 어떻게 작동하나요?

Xcode에서 Swift 및 SwiftUI 앱을 개발하는 동안, Skip Xcode 플러그인은 지속적으로 해당 코틀린과 Jetpack Compose로 변경하여 Android용으로 변환합니다. 네이티브 성능과 네이티브 사용자 인터페이스를 양 플랫폼에서 모두 제공하는 듀얼 플랫폼 라이브러리 또는 전체 앱을 개발하세요.

<div class="content-ad"></div>

/assets/img/2024-07-06-BuildAndroidandiOSappswithSwiftUI_1.png

# 스킵으로 시작하기: 설치 가이드

## 시스템 요구 사항

설치하기 전에, 개발 환경이 다음 요구 사항을 충족하는지 확인하세요:

<div class="content-ad"></div>

**사용 환경 요구사항:**

- macOS 13 이상: 당신의 기기는 macOS 13 이상이어야 합니다.
- Xcode 15: 최신 버전의 Xcode가 설치되어 있는지 확인해주세요.
- Android Studio 2023: 안드로이드 엠뮬레이터 및 안드로이드 개발 작업을 관리하는 데 사용됩니다.
- Homebrew: macOS용 패키지 관리자입니다.

# Skip 설치하기

Skip를 통해 Swift와 SwiftUI를 사용하여 크로스 플랫폼 애플리케이션을 빌드하는 프로세스를 간단하게 만들어보세요. 아래 단계를 따라 Skip를 설치해보세요:

# 단계 1: Homebrew를 통해 Skip 설치하기

<div class="content-ad"></div>

터미널을 열고 다음 명령을 실행해 주세요:

```bash
brew install skiptools/skip/skip
```

이 명령은 Skip 도구와 Kotlin/Android 앱의 빌드 및 테스트를 위한 Grale 및 JDK와 같은 필수 종속성을 다운로드하고 설치합니다.

# 단계 2: 설치 확인

<div class="content-ad"></div>

인스톨이 완료되면, 스킵과 그 의존성들이 올바르게 설치되었는지 확인하기 위해 다음을 실행해주세요:

```js
skip checkup
```

![이미지](/assets/img/2024-07-06-BuildAndroidandiOSappswithSwiftUI_2.png)

# 문제 해결

<div class="content-ad"></div>

🔮"만약 점검에서 실패한다면, 더 많은 세부 정보를 얻기 위해 명령을 자세하게 실행하는 것이 권장됩니다.

```js
skip checkup --verbose
```

Skip 문서의 FAQ 섹션을 확인하여 점검 과정 중 발생할 수 있는 문제에 대한 일반적인 해결책을 확인해보세요.

# 스텝 3: 개발 준비 완료" 🔮

<div class="content-ad"></div>

멋지세요! 건강 진단을 통과했다면 Skip으로 개발을 시작할 준비가 되었습니다!

```js
/*터미널에서 아래 명령어를 실행하여 프로젝트를 만드세요*/

skip init --appid=bundle.id project-name AppName
```

![Click here](/assets/img/2024-07-06-BuildAndroidandiOSappswithSwiftUI_3.png)

![Click here](/assets/img/2024-07-06-BuildAndroidandiOSappswithSwiftUI_4.png)

<div class="content-ad"></div>

그게 다야! 가장 흥미로운 점은 두 가지 명령어로 내 프로젝트를 실행할 수 있다는 것이다. 다른 것은 할 필요가 없어요 — Skip을 설치하고 확인한 후 Android와 iOS에서 실행할 프로젝트를 만들기만 하면 돼요.

# 결론

Skip은 Swift와 SwiftUI 개발자들이 크로스 플랫폼 애플리케이션을 만들기 위해 혁신적인 해결책을 제공해요. 설정 프로세스를 간소화하고 익숙한 도구들을 활용함으로써.

Skip은 매우 새롭고 현재 iOS 16 이상만 지원하며 여러 문제가 발생할 수 있지만, 사랑받는 SwiftUI를 사용하여 Android와 iOS 앱을 개발할 수 있다는 것은 흥미롭답니다. 이것은 크로스 플랫폼 개발 프로세스를 간소화하려는 개발자들에게 유망한 발전입니다. 즐거운 코딩하세요!