---
title: "안드로이드에서 스플래시 스크린 구현하는 방법"
description: ""
coverImage: "/assets/img/2024-07-10-SplashScreensonAndroid_0.png"
date: 2024-07-10 01:36
ogImage:
  url: /assets/img/2024-07-10-SplashScreensonAndroid_0.png
tag: Tech
originalTitle: "Splash Screens on Android"
link: "https://medium.com/@elifduzcan6/splash-screens-on-android-8f8186dadfaf"
isUpdated: true
---

![Splash Screen](/assets/img/2024-07-10-SplashScreensonAndroid_0.png)

Hey there fellow Tarot enthusiasts!

Today, let's dive into the mystical world of Splash Screens and understand their significance in mobile applications.

So, what exactly is a Splash Screen?

A Splash Screen is like the welcoming embrace of the digital realm when you first open a mobile app. It's that initial screen you encounter, often flaunting a fancy animated logo that represents the soul of the application.

And why do we even bother with Splash Screens, you might wonder?

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

앱을 처음으로 열 때는 사용하기 전에 로드해야 할 데이터가 있습니다. 이 데이터를 로드할 때 사용자에게 로딩 화면을 보여 주기보다는 앱의 로고가 표시된 애니메이션 화면을 보여 주고 로딩 프로세스를 백그라운드에서 처리하는 것이 개발자들에 의해 선호됩니다. 이렇게 하면 사용자들에게 더 아름다운 경험을 제공할 수 있습니다.

# 안드로이드 앱에 스플래시 화면 추가하는 방법은?

설명을 시작하기 전에, 안드로이드 12 버전 이후에는 Splash Screen API만 사용할 수 있지만 낮은 버전에서는 이 API를 사용하기 위해 몇 가지 변경이 필요하다는 점을 기억해야 합니다.

스플래시 화면을 만드는 두 가지 방법이 있습니다. SplashScreen 플랫폼 API를 사용하거나 SplashScreen API를 감싸는 스플래시 화면 호환성 프레임워크를 사용하는 것입니다. 이 섹션에서는 프레임워크를 사용하여 스플래시 화면을 만드는 방법에 대해 이야기하겠습니다.

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

먼저, 프로젝트의 build.gradle 파일에 사용할 프레임워크를 추가해야 합니다.

```js
dependencies {
    implementation("androidx.core:core-splashscreen:1.0.0")
}
```

다음으로, 화면의 모습을 변경할 테마를 만들 것입니다. themes.xml 파일로 이동한 다음, 다음과 같이 스플래시 화면을 위한 새로운 스타일을 만드세요.

```js
<style name="Theme.App.Starting" parent="Theme.SplashScreen">
  <item name="windowSplashScreenBackground">@color/white</item>
  <item name="postSplashScreenTheme">@style/Theme.PetDiary</item>
  <item name="windowSplashScreenAnimatedIcon">@drawable/animated_logo</item>
</style>
```

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

화면의 배경 색상은 windowSplashScreenBackground로 설정합니다. core-splashscreen 프레임워크를 사용하고 있으므로, 어플리케이션 파일에서 시작 테마를 바꾸는 대신에 스플래시 화면으로 manifest의 테마를 변경해야 합니다. postSplashScreenTheme을 사용하여 어플리케이션의 주 테마를 지정하여 스플래시 화면이 나타난 후 어플리케이션이 주 테마로 전환되도록 합니다.

windowSplashScreenAnimatedIcon을 사용하여 화면에 나타날 어플리케이션의 애니메이션 아이콘을 추가할 수 있습니다. 이러한 애니메이션에는 투명도가 없는 단일 배경 색상을 설정하거나 선택한 아이콘이 AnimatedVectorDrawable(AVD) XML이어야 하는 등의 요구 사항이 있습니다. 이러한 요구 사항은 developer.android.com 웹사이트에서 확인하고 스플래시 화면 개념을 더 자세히 살펴볼 수 있습니다.

# 애니메이션 없는 스플래시 화면

저희는 애니메이션 스플래시 화면을 언급했지만, 모든 기기가 애니메이션 로고를 지원하는 것은 아닙니다. windowSplashScreenAnimatedIcon은 API 레벨 31 이상의 기기에서 애니메이션 되지만, 이 레벨 미만의 기기에서는 애니메이션이 표시되지 않습니다.

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

안녕하세요!

비 애니메이션 아이콘은 res/drawable-v31에서 비 애니메이션 아이콘을 재정의하여 애니메이션 벡터 드로어블로 만들 수 있어요.

## 스플래시 화면 지속 시간

스플래시 화면은 일반적으로 1000 밀리초에서 3000 밀리초 사이에 사용되지만, 실제로 이 기간을 무제한으로 증가하거나 변경할 수 있어요.

## 안드로이드 11 및 이전 버전에서 스플래시 화면을 만들 때 고려해야 할 사항은 무엇인가요?

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

안드로이드 12 이후 버전에서 사용되는 스플래시 화면 API에 대해 언급했었습니다. 이전 안드로이드 버전에 대한 호환성 API가 있어서, 이 API를 사용하면 이전 버전에서도 스플래시 화면을 사용할 수 있습니다.

이를 위해 이전과는 달리 다음 스플래시 화면 종속성을 build.gradle 파일에 추가해야 합니다. 이것을 추가하는 것은 안드로이드 11 및 이하 버전을 지원하기 위해 필요합니다.

```js
implementation 'androidx.core:core-splashscreen:1.0.0-alpha02'
```

사용할 아이콘이 선택되고 필요한 조정이 이루어진 후, 스플래시 화면 테마가 결정됩니다. 그런 다음 AndroidManifest.xml 파일에서 애플리케이션 테마를 스플래시 화면 테마로 변경합니다. 이러한 방식으로 스플래시 화면을 완성할 수 있습니다.

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

이 글에서는 일반적으로 스플래시 화면의 개념에 대해 이야기했고, 다양한 안드로이드 버전에서 어떻게 사용될 수 있는지 예를 들었습니다. 유용한 글이었기를 바랍니다.
