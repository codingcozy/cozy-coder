---
title: "Jetpack Compose 입문 시작하는 방법 및 기본 개념"
description: ""
coverImage: "/milky-road.github.io/assets/no-image.jpg"
date: 2024-07-06 03:15
ogImage: 
  url: /milky-road.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Introduction To Jetpack Compose"
link: "https://medium.com/@adityatheprogrammer/introduction-to-jetpack-compose-87fdb0154fdb"
---


🌟 안녕하세요! 이쪽은 아디티야입니다. 요즘 네이티브 안드로이드 개발에서 Jetpack Compose라는 용어가 많은 주목을 받고 있어요. 하지만 궁금증이 생깁니다. 바로, Jetpack Compose가 무엇인가요?

Jetpack Compose는 Google에서 개발한 안드로이드를 위한 선언형 UI 프레임워크에요. 기본적으로 앱을 위한 사용자 인터페이스(UI)를 구축하는 데 사용됩니다. 선언형의 의미가 여기서 무엇인지 의문이 드실지도 모르겠죠. ✨

<div class="content-ad"></div>

"선언적"이라는 용어란 무엇을 의미할까요?

선언적이란 UI가 상태에 기반하여 렌더링되며, 상태가 변경되면 UI도 변경된다는 것을 의미합니다. 기본적으로 Jetpack Compose에서는 상태 변경에 따라 UI를 렌더링합니다. 간단한 예시를 보여드릴게요.

```kotlin
@Composable
fun DisplayMessage(isSuccess: Boolean) {
    Box(
        modifier = Modifier.fillMaxSize(),
        contentAlignment = Alignment.Center
    ) {
        Text(
            text = if (isSuccess) "Success" else "Failed",
            style = MaterialTheme.typography.bodyLarge
        )
    }
}

DisplayMessage(isSuccess = true)
```

위 예시에서는 isSuccess를 true로 전달하면 "Success"가 표시되고, false로 전달하면 "Failed"가 표시됩니다.

<div class="content-ad"></div>

그것은 선언형 UI 프레임워크의 작은 예제예요.

🤔 어째서 Jetpack Compose를 사용해야 할까요?

이 질문을 하신다면 올바른 생각을 하고 있어요. 많은 회사들이 아직 XML을 사용하고 있고 XML을 기반으로 구축된 레거시 소프트웨어도 존재해요. 그러나 Jetpack Compose는 XML보다 여러 이점이 있답니다. 제가 보여 드릴게요.

- 선언형 UI 프레임워크: 이전에 이야기했듯이 Jetpack Compose는 상태에 따라 UI를 렌더링하는 선언형 UI 프레임워크에요. XML에서는 상태에 기반한 UI를 렌더링 할 수 없고, UI를 먼저 XML에 모두 생성한 다음에 activity나 fragment에서 UI를 수동으로 변경해야 해요.
- 재사용 가능: Compose에서 UI는 구성 가능한 함수(위 예제에서 본 것처럼)로 빌드돼요. 이는 재사용성을 높이고 더 복잡한 UI를 작은 구성 가능한 함수에서 구축할 수 있게 도와 줘요.
- Kotlin 호환성: Compose는 코루틴, 확장 함수 및 타입 안전성과 같은 Kotlin의 언어 기능을 활용하여 구축됐어요.
- Android Studio 지원: Compose는 Android Studio와 탁월한 통합을 제공하며, 실시간 미리보기, 보다 인터랙티브한 UI 편집기 및 강력한 디버깅 경험 등과 같은 기능을 포함하고 있어요.
- 성능: Compose는 성능을 염두에 두고 디자인됐으며, 단일 기본 렌더링 엔진을 사용하여 효율적으로 UI 구성 요소를 관리하고 업데이트하며 UI 요소를 언제 어떻게 다시 구성해야 하는지에 대한 세밀한 제어를 제공하여 최적화된 성능을 가능하게 해줘요.
- 애니메이션 및 전환: Compose는 애니메이션 및 전환을 구현하기 위한 간단한 API를 제공해 원활하고 동적인 UI를 쉽게 만들 수 있어요.
- 테마 및 스타일링: Compose는 유연한 테마 및 스타일링 시스템을 제공하여 앱 전체에 일관된 스타일을 쉽게 적용할 수 있어요.

<div class="content-ad"></div>

마지막으로 Jetpack Compose은 성장 중인 기술이며 Google이 완전히 지원하고 있으며 최신 안드로이드 플랫폼 기능과 모범 사례에 대해 최신 정보를 제공하고 있습니다. 또한 커뮤니티도 성장 중이며 많은 사람들이 매우 열심히 노력하고 있어 자원 및 라이브러리의 부족이 없습니다.

결론

Jetpack Compose은 새로운 라이브러리와 효율적인 성능을 제공하며 네이티브 안드로이드 앱의 UI 개발에서 큰 변화를 보여주고 있습니다. 또한 Kotlin과 호환되기 때문에 개발자 친화적입니다.

XML에서 Jetpack Compose로 넘어가보세요! 💡

<div class="content-ad"></div>

제가 지금 Medium 기사 작성을 시작했고, 안드로이드 개발에 대한 학습과 경험을 모두 공유할 예정인 '젯팩 컴포즈 시리즈'를 시작했습니다.

감사합니다.