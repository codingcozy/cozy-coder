---
title: "Jetpack Compose에서 BottomSheet 상태 변경을 신뢰성 있게 감지하는 방법"
description: ""
coverImage: "/assets/img/2024-07-25-HowtoReliablyDetectBottomSheetStateChangesinJetpackCompose_0.png"
date: 2024-07-25 11:51
ogImage: 
  url: /assets/img/2024-07-25-HowtoReliablyDetectBottomSheetStateChangesinJetpackCompose_0.png
tag: Tech
originalTitle: "How to Reliably Detect BottomSheet State Changes in Jetpack Compose"
link: "https://medium.com/@efebu/how-to-reliably-detect-bottomsheet-state-changes-in-jetpack-compose-76a91524e9a2"
---


<img src="/assets/img/2024-07-25-HowtoReliablyDetectBottomSheetStateChangesinJetpackCompose_0.png" />

간단한 시나리오입니다. Jetpack Compose에서 BottomSheetScaffold를 사용하고 있고, 하단 시트가 숨겨질 때 작업을 트리거해야 합니다. 직관적으로 보이죠? 그렇다고 하지요.

문제는 BottomSheetState에는 currentValue 속성이 있지만, 사용자 상호작용이 완료되었을 때 직접 알려주지 않는다는 것입니다. 우리는 그 순간을 위한 콜백을 원합니다.

아래에서 Hidden 상태에 대한 해결책을 공유할 텐데, Expanded 및 PartiallyExpanded 상태에 대해서도 적용할 수 있습니다. 제스처 및 하단 시트 상태 추적을 실험했지만 어느 접근 방법도 완벽히 작동하지 않았습니다. (쉽게 할 수 있는 방법이 있으면 댓글에서 공유해 주세요!)

<div class="content-ad"></div>

# 우리가 갖고 있는 것

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

우리는 `bottomSheetState`와 Kotlin 코루틴의 힘만 있으면 됩니다.

# 해결책

<div class="content-ad"></div>

```js
private const val DELAY_DRAGGING = 200L // 이 값은 조정 가능합니다

var jobBottomSheetCollapsed: Job? = null

LaunchedEffect(bottomSheetScaffoldState.bottomSheetState.currentValue) {
  if (bottomSheetScaffoldState.bottomSheetState.targetValue == SheetValue.Hidden) {
    jobBottomSheetCollapsed?.cancel()
    delay(DELAY_DRAGGING)

    jobBottomSheetCollapsed = launch {
      if (bottomSheetScaffoldState.bottomSheetState.currentValue == SheetValue.Hidden) {
          // 여기가 바텀 시트가 접힌 곳입니다
      }
    }
  }
}
```

해결 방법 코드를 자세히 살펴보겠습니다:

- var jobBottomSheetCollapsed: Job? = null: 이 변수는 코루틴 작업에 대한 참조를 저장합니다. 사용자가 숨겨진 상태에 도달하기 전에 아래쪽 시트와 상호 작용을 계속할 경우 이 작업을 취소합니다.
- LaunchedEffect(bottomSheetScaffoldState.bottomSheetState.currentValue): 이 LaunchedEffect 블록은 아래쪽 시트 상태의 currentValue가 변경될 때마다 트리거됩니다. 이곳에서 currentValue는 상호 작용 중에 자주 변경됩니다.
- if (bottomSheetScaffoldState.bottomSheetState.targetValue == SheetValue.Hidden): targetValue가 성공의 열쇠입니다. 기대대로 targetValue는 아래쪽 시트가 목표로 하는 상태를 나타냅니다(사용자가 작업을 변경하지 않는 한).
- delay(DELAY_DRAGGING): 이 짧은 지연은 아래쪽 시트 애니메이션이나 진행 중인 사용자 상호 작용이 완료될 수 있도록 중요합니다.
- if (bottomSheetScaffoldState.bottomSheetState.currentValue == SheetValue.Hidden): 마지막으로 currentValue를 다시 확인합니다. 만약 아래쪽 시트가 실제로 숨겨진 상태에 도달했다면 여기에서 원하는 작업을 안전하게 트리거할 수 있습니다.

<img src="https://miro.medium.com/v2/resize:fit:1200/1*1pyNukvljjPczGLT9DXyZA.gif" />


<div class="content-ad"></div>

이전에 언급했던 대로, 이 솔루션을 다른 원하는 최종 상태에 쉽게 맞출 수 있습니다. 코드에서 SheetValue.Hidden을 SheetValue.Expanded 또는 SheetValue.PartiallyExpanded로 바꿔서 해당 상태를 감지하도록 하면 됩니다.

이 솔루션이 작동했는지 알려주세요. 도움이 되었다면 👏을 눌러주세요!