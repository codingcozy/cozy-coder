---
title: "동적 성능을 위한 새로운 스마트 아이템 구성  ContextualFlowRow Column 활용 방법"
description: ""
coverImage: "/assets/img/2024-07-23-NewSmartItemCompositionforBetterPerformanceContextualFlowRowColumn_0.png"
date: 2024-07-23 11:25
ogImage: 
  url: /assets/img/2024-07-23-NewSmartItemCompositionforBetterPerformanceContextualFlowRowColumn_0.png
tag: Tech
originalTitle: "New Smart Item Composition for Better Performance  ContextualFlowRow Column"
link: "https://medium.com/@stevdza-san/new-smart-item-composition-for-better-performance-contextualflowrow-column-390db2eb6bd5"
---



![이미지](/assets/img/2024-07-23-NewSmartItemCompositionforBetterPerformanceContextualFlowRowColumn_0.png)

이전에 FlowRow 또는 FlowColumn을 사용해본 적이 있다면, 이 새로운 컴포저블이 더욱 행복하게 만들어줄 것입니다! 이 두 가지와의 주요 차이점은, 이 새로운 솔루션이 처음에 고정된 수의 UI 구성 요소만 렌더링하고, 필요에 따라 수동으로 더 많은 항목을 나타낼 수 있도록 한다는 것입니다. 추가적인 사용자 정의 옵션을 제공하여, 처음에 확장된 항목 중 일부 또는 대부분을 숨길 수 있습니다.

# 미리보기

![이미지](https://miro.medium.com/v2/resize:fit:1400/1*oLPu0Z8XcJ_NpgIRqyBlxA.gif)


<div class="content-ad"></div>

이 작동 방식을 보여드릴게요. 이 데모 화면에서 여러 문자열 항목 목록이 Row에 표시될 것입니다. 그 아래에는 동적으로 업데이트되는 정수 상태가 있습니다. 우리가 이 상태가 필요한 이유는, 사용자가 버튼을 클릭할 때 ContextualFlowRow 내에서 더 많은 행을 보이거나 숨기기 위함입니다.

# 시작해볼까요! 🚀

```js
const val DEFAULT_MAX_LINES = 2

@OptIn(ExperimentalLayoutApi::class)
@Composable
fun MainScreen() {
    val languages = listOf(
        "Kotlin",
        "Java",
        "C++",
        "Python",
        "GO",
        "Javascript",
        "Dart",
        "Swift",
        "Ruby",
        "PHP",
        "Rust",
        "Perl",
        "Scala",
        "Typescript"
    )

    var maxLines by remember { mutableIntStateOf(DEFAULT_MAX_LINES) }

    ...
}
```

그 아래에서 ContextualFlowRow composable을 호출해봅시다. 여기에 표시하려는 항목 수를 전달하세요. 그리고 maxLines에는 기본 줄 수에 대한 상수를 선언할 겁니다. 예를 들어, 이 경우에는 숫자 2로 남겨둘 수 있습니다. 각 행에 표시할 항목 수를 사용자 정의할 수도 있고, 이 매개변수를 제외할 수도 있습니다. 이렇게 하면 각 행에 가능한 한 많은 항목을 맞출 수 있습니다. 선택은 당신의 것이에요.

<div class="content-ad"></div>

그 다음은 overflow 매개변수가 있는데, 이를 통해 이 행 내의 항목을 처리하는 방법을 지정할 수 있습니다. 여기에는 두 가지 옵션이 있습니다. 첫 번째 옵션인 expandIndicator()는 더로드할 수 있는 항목이 더 있을 때 확장 버튼만 표시하며, 두 번째 옵션인 expandOrCollapseIndicator()는 확장 및 숨기기 버튼/구성요소 모두를 표시합니다. 모든 항목을 표시할 때까지 숨기기 버튼은 보이지 않습니다.

```js
const val DEFAULT_MAX_LINES = 2

@OptIn(ExperimentalLayoutApi::class)
@Composable
fun MainScreen() {
    val languages = listOf(
        "Kotlin",
        "Java",
        "C++",
        "Python",
        "GO",
        "Javascript",
        "Dart",
        "Swift",
        "Ruby",
        "PHP",
        "Rust",
        "Perl",
        "Scala",
        "Typescript"
    )

    var maxLines by remember { mutableIntStateOf(DEFAULT_MAX_LINES) }

    ContextualFlowRow(
        modifier = Modifier
            .systemBarsPadding()
            .animateContentSize()
            .padding(all = 8.dp),
        itemCount = languages.size,
        maxLines = maxLines,
        overflow = ContextualFlowRowOverflow.expandOrCollapseIndicator(
            expandIndicator = {},
            collapseIndicator = {}
        ),
        horizontalArrangement = Arrangement.spacedBy(8.dp)
    ) { index ->
       ...
    }
}
```

확장 및 축소 지시자에 사용자 정의 구성요소를 제공할 것입니다. 두 요소 모두 TextButton 구성으로 표시될 것입니다. 확장 지시자를 클릭하면 maxLines 상태를 1 증가시킴으로써 전체 구성요소에 바로 추가 항목 행이 표시되도록 할 수 있습니다.

totalItemCount와 shownItemCount와 같은 기존 속성을 사용하여 더 많은 항목을 로드할 수 있는지를 계산할 수 있습니다.

<div class="content-ad"></div>

한편, 접힘 표시기에 대해서 사용자가 해당 구성요소를 클릭했을 때 maxLines 상태를 기본값으로 재설정할 수 있습니다. 정말 깔끔하죠.

```kotlin
ContextualFlowRow(
        modifier = Modifier
            .systemBarsPadding()
            .animateContentSize()
            .padding(all = 8.dp),
        itemCount = languages.size,
        maxLines = maxLines,
        overflow = ContextualFlowRowOverflow.expandOrCollapseIndicator(
            expandIndicator = {
                TextButton(
                    onClick = { maxLines += 1 },
                    colors = ButtonDefaults.textButtonColors(
                        contentColor = MaterialTheme.colorScheme.surfaceVariant,
                        containerColor = MaterialTheme.colorScheme.onSurface
                    )
                ) {
                    Text(text = "${this@expandOrCollapseIndicator.totalItemCount - this@expandOrCollapseIndicator.shownItemCount}+ 더 보기")
                }
            },
            collapseIndicator = {
                TextButton(
                    onClick = { maxLines = DEFAULT_MAX_LINES },
                    colors = ButtonDefaults.textButtonColors(
                        contentColor = MaterialTheme.colorScheme.errorContainer,
                        containerColor = MaterialTheme.colorScheme.error
                    )
                ) {
                    Icon(
                        modifier = Modifier.size(10.dp),
                        imageVector = Icons.Default.Close,
                        contentDescription = "닫기 아이콘"
                    )
                    Text(text = "숨기기")
                }
            }
        ),
        horizontalArrangement = Arrangement.spacedBy(8.dp)
    ) { index ->
         ...
    }
```

마지막으로, ContextualFlowRow 조합 가능의 후위 람다로 위 목록의 각 항목에 대한 버튼을 표시할 수 있습니다. 완벽합니다!

```kotlin
ContextualFlowRow(
        ...
    ) { index ->
        Button(onClick = { }) {
            Text(text = languages[index])
        }
    }
```

<div class="content-ad"></div>


![image](/assets/img/2024-07-23-NewSmartItemCompositionforBetterPerformanceContextualFlowRowColumn_1.png)

이 UI 구성 요소를 이미 사용해 보셨나요? 아래에 댓글을 달아 제게 알려주세요!

또한, 이 기사를 유익하다고 생각하시면 박수를 꼭 눌러주세요.
감사합니다! ☺️💚
