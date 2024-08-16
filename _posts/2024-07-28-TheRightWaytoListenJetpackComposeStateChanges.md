---
title: "Jetpack Compose 상태 변경을 올바르게 감지하는 방법"
description: ""
coverImage: "/assets/img/2024-07-28-TheRightWaytoListenJetpackComposeStateChanges_0.png"
date: 2024-07-28 13:47
ogImage: 
  url: /assets/img/2024-07-28-TheRightWaytoListenJetpackComposeStateChanges_0.png
tag: Tech
originalTitle: "The Right Way to Listen Jetpack Compose State Changes"
link: "https://medium.com/@efebu/the-right-way-to-listen-jetpack-compose-state-changes-281d8d47c2ec"
---


<img src="/assets/img/2024-07-28-TheRightWaytoListenJetpackComposeStateChanges_0.png" />

저는 3일 전에 "Jetpack Compose에서 BottomSheet 상태 변경을 신뢰할 수 있는 방법"에 대한 게시물을 게시했고 커뮤니티의 도움으로 Jetpack Compose 상태를 더 잘 듣는 방법을 배웠습니다.

더 나은 비교를 위해 동일한 시나리오와 동일한 예제 코드를 사용할 것입니다.

## 시나리오

<div class="content-ad"></div>

Jetpack Compose에서 BottomSheetScaffold을 사용하고 있으며, 하단 시트가 숨겨질 때 작업을 실행해야 합니다.

## 우리가 가진 것

```js
val bottomSheetScaffoldState = rememberBottomSheetScaffoldState(
  bottomSheetState = rememberStandardBottomSheetState(
    initialValue = SheetValue.Expanded,
  ),
)

BottomSheetScaffold(
  modifier = modifier,
  scaffoldState = bottomSheetScaffoldState,
  // ... ,
) {
  // ...
}
```

## 해결 방법

<div class="content-ad"></div>

snapshotFlow는 해결책입니다.

```js
LaunchedEffect(bottomSheetScaffoldState.bottomSheetState) {
    snapshotFlow { bottomSheetScaffoldState.bottomSheetState.currentValue }
        .collect {
            if (it == SheetValue.Hidden) {
                isBottomSheetHidden.value = true
            }
        }
}
```

snapshotFlow의 좋은 점은 jetpack compose 상태에 모두 사용할 수 있습니다. 어떤 구성 요소에서든 변경 사항을 청취하는 실버 불릿입니다.

여기 스크롤 상태에 대한 추가적인 샘플이 있습니다.

<div class="content-ad"></div>

```kotlin
val scrollState = rememberScrollState()
LaunchedEffect(scrollState) {
    snapshotFlow { scrollState.maxValue }
        .collect { scrollState.animateScrollTo(it) }
}
```

![image](https://miro.medium.com/v2/resize:fit:1200/1*u8EqOumO67zn1wP69S_Fag.gif)