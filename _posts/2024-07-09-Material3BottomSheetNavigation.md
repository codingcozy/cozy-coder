---
title: "Material3 BottomSheet Navigation 사용하는 방법 2024년 최신 가이드"
description: ""
coverImage: "/assets/img/2024-07-09-Material3BottomSheetNavigation_0.png"
date: 2024-07-09 10:47
ogImage:
  url: /assets/img/2024-07-09-Material3BottomSheetNavigation_0.png
tag: Tech
originalTitle: "Material3 BottomSheet Navigation"
link: "https://medium.com/@stefanoq21/material3-bottomsheet-navigation-13f726c13d6b"
isUpdated: true
---

## 새로운 라이브러리로 간편하게 Material3에서 Bottomsheet Navigation을 간소화하세요

![이미지](/assets/img/2024-07-09-Material3BottomSheetNavigation_0.png)

Material3 이주 과정 중에 해결해야 하는 특정 과제가 있었던 많은 사용자들 중 한 명으로서, 바로 바텀시트를 활용한 내비게이션입니다.

Material 디자인 시대에는 Accompanist Navigation Material과 공식 androidx.compose.material.navigation과 같은 라이브러리들 덕분에 바텀시트 내비게이션을 구현할 수 있었습니다. 이 라이브러리들은 프로젝트에 매끄럽게 통합되어 바텀시트를 일반 화면과 함께 내비게이션 경로로 정의할 수 있게 해 주었습니다. 이는 다양한 기능에 바텀시트를 광범위하게 활용하는 앱들에 매우 유용했습니다.

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

However, the migration to Materail3 presented a significant challenge. It turned out that the navigation libraries for bottom sheets were closely tied to the Material library. This posed a dilemma:

- Abandoning Bottomsheet Routes: Disregarding the use of bottom sheets as routes meant resorting to standard ModalBottomSheet instances, sacrificing the seamless navigation experience.
- Dealing with Dual Material Dependencies: The introduction of the Material library alongside Material3 caused issues in managing dependencies. It became essential to carefully select components to prevent inconsistencies. This could lead to ongoing frustration and potential bugs.

Confronted with these less-than-ideal options, I set out on a personal journey to develop bottom sheet navigation with Material3 from scratch. This experiment was successful, resulting in a solution that not only optimized my app's navigation but also provided a consistent Material3 experience. Encouraged by this progress, I decided to create a reusable library and share it as a GitHub package.

In the next phase, I further improved the library by incorporating a new feature introduced in androidx.navigation:navigation-compose 2.8.0. This enhancement allows for defining bottom sheet routes using data objects/classes instead of plain text, offering more flexibility and robust routing capabilities.

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

이 라이브러리를 사용하는 데 궁금하신가요? 다음 섹션에서 간단한 통합 단계를 안내해 드릴게요. 코드를 먼저 사용하시는 분들을 위해 전체 구현은 여기에서 확인할 수 있어요.

## 라이브러리 통합

라이브러리를 사용하기 전에, Compose 프로젝트에 정확히 통합되었는지 확인해 보아요.

먼저 settings.gradle.kts부터 시작해 볼까요?

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

dependencyResolutionManagement {
repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
repositories {
google()
mavenCentral()
maven {
url = uri("https://maven.pkg.github.com/stefanoq21/BottomSheetNavigator3")
credentials {
val properties = Properties()
properties.load(file("local.properties").reader())
username = properties.getProperty("githubUserName") as String
password = properties.getProperty("githubToken") as String
}
}
}
}

여러분은 local.properties 파일에 GitHub 자격 증명을 필요로 할 것입니다 (자격 증명 생성 방법에 대한 안내서는 여기 있습니다). (이 단계를 피하려면, 라이브러리는 Maven Central에 발표 예정입니다. 향후 업데이트와 개선 사항을 위해 저장소를 팔로우하세요!)

githubToken=blablabla
githubUserName=your-github-username

이미 프로젝트에서 버전 카다로그를 활용 중이라면, 이 라이브러리를 위해 libs.versions.toml에 필요한 종속 항목을 추가하는 방법은 다음과 같습니다:

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

```js
[버전]
...
navigationCompose = "2.8.0-beta04"
material3Navigation = "0.0.3"
material3 = "1.3.0-beta04"

[라이브러리]
...
androidx-material3 = { group = "androidx.compose.material3", name = "material3", version.ref = "material3" }
androidx-navigation-compose = { module = "androidx.navigation:navigation-compose", version.ref = "navigationCompose" }
material3-navigation = { group = "io.github.stefanoq21", name = "material3-navigation", version.ref = "material3Navigation" }
```

마지막으로, 당신의 build.gradle.kts에 다음 코드를 추가해주세요.

```js
...
dependencies {
...
 implementation(libs.androidx.material3)
 implementation(libs.androidx.navigation.compose)
 implementation(libs.material3.navigation)
```

## 사용법

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

이제 라이브러리가 통합되었으니, Material3 bottomsheet 네비게이션을 원할하게 사용해보겠습니다. 이 과정의 핵심은 BottomSheetNavigator 클래스인데, 이 라이브러리에서 제공하는 클래스입니다. 이 네비게이터는 네비게이션 그래프 내에서 bottomsheet 목적지를 관리합니다.

BottomSheetNavigator를 구성하는 중요한 측면은 bottomsheet 경로가 부분적으로 확장된 상태를 건너뛸지 여부를 지정하는 것입니다. 여기서 부분적으로 확장된 상태는 bottomsheet가 화면에 부분적으로 표시되는 중간 단계를 의미합니다.

```kotlin
val bottomSheetNavigator =
   rememberBottomSheetNavigator(skipPartiallyExpanded = true/false)
val navController = rememberNavController(bottomSheetNavigator)
```

BottomSheetNavigator를 정의했다면, 이제 bottomsheet 내용을 표시하는 방법을 살펴봅시다. 여기서 ModalBottomSheetLayout 구성 요소가 라이브러리에서 제공됩니다.

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

ModalBottomSheetLayout은 당신의 바텀 시트 콘텐츠를 담는 컨테이너 역할을 합니다. 네비게이션 그래프와 원활하게 통합되며, 생성 시 BottomSheetNavigator를 매개변수로 받습니다. 아래는 어떻게 적용하는지 살펴보겠습니다:

```kotlin
ModalBottomSheetLayout(
                        modifier = Modifier
                            .fillMaxSize(),
                        bottomSheetNavigator = bottomSheetNavigator
                    ) {
                        NavHost(
                            navController = navController,
                            startDestination = Screen.Home
                        ) {
```

이제 ModalBottomSheetLayout을 설정했으니, 바텀시트 목적지에 대한 경로를 어떻게 정의하는지 살펴봅시다. 이 경로를 정의하는 방법은 사용 중인 Compose Navigation 버전에 따라 다릅니다:

- 문자열 사용 (Compose Navigation 버전 ` 2.8.0):

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

만약 이전 버전의 Compose Navigation을 사용 중이라면, 각 BottomSheet 목적지를 나타내는 고유한 식별자로 간단한 문자열로 라우트를 정의할 수 있습니다.

```js
bottomSheet("BottomSheetFullScreen") {
   BSFullScreenLayout()
}
```

- 데이터 클래스 사용 (Compose Navigation 버전 `= 2.8.0`):

Compose Navigation의 최신 버전에서는 데이터 클래스/객체의 강력함을 활용하여 라우트를 정의할 수 있습니다. 이를 통해 목적지에 대한 추가 정보를 라우트 자체에 캡슐화할 수 있습니다.

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

마법의 세계로 여행을 떠나 보세요! 이제 라우트가 정의되었으니 Compose Navigation에서 제공하는 표준 navigate 함수를 사용하여 바텀시트 목적지로 이동할 준비가 되었습니다.

[BottomSheetFullScreen](#)으로 이동하기

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

## 사용자 정의

이 라이브러리를 사용하면 앱의 디자인과 기능과 완벽하게 일치하는 바텀시트를 만들 수 있습니다. 현재 이 라이브러리는 표준 material3.ModalBottomSheet의 동일한 사용자 정의 옵션을 지원합니다. 내비게이션 그래프에서 사용되는 모든 바텀시트의 외관을 ModalBottomSheetLayout에 매개변수를 전달하여 사용자 정의할 수 있습니다.

여기 아이디어를 얻기 위해 컴포넌트의 정의가 있습니다:

```js
@Composable
fun ModalBottomSheetLayout(
    bottomSheetNavigator: BottomSheetNavigator,
    modifier: Modifier = Modifier,
    sheetMaxWidth: Dp = BottomSheetDefaults.SheetMaxWidth,
    shape: Shape = BottomSheetDefaults.ExpandedShape,
    containerColor: Color = BottomSheetDefaults.ContainerColor,
    contentColor: Color = contentColorFor(containerColor),
    tonalElevation: Dp = BottomSheetDefaults.Elevation,
    scrimColor: Color = BottomSheetDefaults.ScrimColor,
    dragHandle: @Composable (() -> Unit)? = { BottomSheetDefaults.DragHandle() },
    content: @Composable () -> Unit,
)
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

# 결론

이 가이드를 통해 젯팩 콤포즈에서 Material3 바텀시트 네비게이션을 간편하게 처리할 수 있는 새로운 라이브러리의 기능을 탐험해 보셨습니다.

이 라이브러리는 지속적으로 발전하고 있으며, 여러분의 피드백은 귀중합니다. 라이브러리를 탐험하고 실험해 보시고, GitHub에서 질문이나 문제가 발생할 경우 언제든지 댓글을 남기거나 이슈를 열어주십시오. 어떤 어려움에 직면하더라도 도와드리고 문제를 해결하는 데 즐거움을 느끼겠습니다.

만약 이 라이브러리가 유용하다고 느끼신다면 GitHub 커뮤니티에 알려주세요! 라이브러리를 탐험하고 기여하며, 별표까지 찍어주세요: [https://github.com/stefanoq21/BottomSheetNavigator3](https://github.com/stefanoq21/BottomSheetNavigator3)

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

좋은 하루 되세요!
