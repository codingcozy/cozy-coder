---
title: "Jetpack Compose로 더 접근성 좋은 그래프 만들기 파트 4 화면 제어 버튼 사용 방법"
description: ""
coverImage: "/assets/img/2024-07-10-MoreAccessibleGraphswithJetpackComposePart4On-ScreenControlButtons_0.png"
date: 2024-07-10 01:45
ogImage: 
  url: /assets/img/2024-07-10-MoreAccessibleGraphswithJetpackComposePart4On-ScreenControlButtons_0.png
tag: Tech
originalTitle: "More Accessible Graphs with Jetpack Compose Part 4: On-Screen Control Buttons"
link: "https://medium.com/proandroiddev/more-accessible-graphs-with-jetpack-compose-part-4-on-screen-control-buttons-6187e6991ddc"
---


![example](/assets/img/2024-07-10-MoreAccessibleGraphswithJetpackComposePart4On-ScreenControlButtons_0.png)

안녕하세요! 이 블로그 포스트는 제타컴포즈를 활용한 보다 접근성 있는 그래프 시리즈의 네 번째 이야기입니다. 이전 글들은 아래 링크에서 확인하실 수 있어요:

- 제타컴포즈로 더 많은 접근성 있는 그래프 만들기 1편: 콘텐츠 설명 추가하기
- 제타컴포즈로 더 많은 접근성 있는 그래프 만들기 2편: 키보드 상호작용 추가하기
- 제타컴포즈로 더 많은 접근성 있는 그래프 만들기 3편: 색상 없이 구분하기

드래그 제스처와 같은 지속적인 (또는 경로 기반의) 포인터 입력은 일부 사용자에게 문제가 될 수 있습니다. 예를 들어, 사용자의 손에 떨림이 있을 경우, 제스처가 항상 지속적이지 않을 수 있으며, 앱이 이러한 종류의 제스처에 의존하여 작동하는 경우, 해당 사용자에게는 사용이 어려울 수 있습니다.

<div class="content-ad"></div>

이 블로그 포스트 시리즈를 따라오셨다면 궁금해하실 수도 있어요 — 우리는 키보드 상호작용을 추가했죠; 그게 충분한 걸까요? 아니에요 — 실제 키보드가 필요할 거예요. 많은 사용자들이 실제 키보드를 사용하지 않는데도 그것으로 혜택을 받을 수 있어요.

그래서, 이 블로그 포스트에서는 그래프에 화면 상에 제어 버튼을 추가할 거에요. 가장 좋은 부분은 키보드 상호작용에 대한 블로그 포스트 맨 끝에서 언급한 스위치 장치에 대한 이슈도 해결된다는 거에요! 스크린 액세스에 대한 단락으로 이동해 더 자세한 설명을 확인해 보세요.

# 버튼 추가하기

그래서, path 기반 동작의 이 문제를 해결하는 한 가지 방법은 그래프 내에서 탐색하는 대안 방법으로 가시적인 버튼을 추가하는 거에요. 이 그래프 경우, 앞으로 가기와 뒤로 가기를 위한 두 개의 버튼을 추가하고 싶어요. 제어를 추가한 후 UI가 어떻게 보이게 될 지를 보여드릴게요:

<div class="content-ad"></div>

지금 새로운 구성 요소 `ControlButtons`를 추가했어요. 이 구성 요소는 다음과 같은 버튼들을 감싸는 용도입니다:

```js
@Composable
fun ControlButtons(
    highlightedX: Float?,
    lastIndex: Int,
    setFocus: (Int) -> Unit
) {
    ...
}
```

세 가지 매개변수를 사용합니다: highlightedX는 현재 x축에 하이라이트 표시된 연도입니다, lastIndex는 마지막 연도의 인덱스이며, setFocus라는 함수는 다음으로 포커스를 이동해야 할 인덱스를 받고 Unit을 반환합니다. 부모 구성 요소에서 호출할 때는 다음과 같이 보입니다:

<div class="content-ad"></div>

제가 사용해오던 highlightedX를 형성하는 데 사용했던 것과 픽셀 포인트 목록의 마지막 색인을 지금 넘겨주죠. 그런 다음, 람다 블록 안에서 새로운 선택한 색인을 가져와서 그 색인에 해당하는 픽셀 포인트 목록의 x-값으로 highlightedX를 설정합니다.

이제 ControlButtons 컴포저블로 돌아가봅시다. 컴포저블의 시작 부분에서 두 가지를 정의할 거에요:

```js
// ControlButtons

var selectedIndex by remember {
    mutableStateOf(-1)
}

fun setSelectedIndexAndFocus(newIndex: Int) {
    selectedIndex = newIndex
    setFocus(newIndex)
}
```

<div class="content-ad"></div>

먼저, 현재 선택된 인덱스를 selectedIndex 변수에 저장하여 기억하고, 초기 값은 -1로 설정합니다. 초기에는 선택된 인덱스가 없기 때문입니다. 또한, 컴포넌트 내에서 상태를 업데이트하고 부모 컴포넌트에서 새로 강조된 연도(또는 x)를 설정하기 위해 선택된 인덱스와 포커스를 설정하는 함수가 필요합니다. 우리는 이 함수를 setSelectedIndexAndFocus로 부를 것입니다.

이후에, 해당 컴포넌트에서 실제 버튼을 생성하는 데 필요한 모든 것이 갖춰질 것입니다. 버튼들이 굉장히 유사하기 때문에, type(나중에 이에 대해 이야기할 것입니다), 현재 선택된 인덱스, 마지막 인덱스, 첫 포커스 인덱스(즉, 아무 것도 선택되지 않은 경우 이 버튼을 누를 때 포커스가 먼저 이동해야하는 위치), 강조된 X 값 및 포커스를 설정하는 함수를 가져오는 ControlButton이라는 새 컴포넌트를 만들 것입니다:

```kotlin
@Composable
fun ControlButton(
    type: GraphControlType,
    currentIndex: Int,
    lastIndex: Int,
    firstFocusIndex: Int,
    highlightedX: Float?,
    setFocus: (Int) -> Unit,
) {
...
}
```

다음 및 이전 연도 모두 작동하도록 함수를 좀 더 추상화하기 위해 GraphControlType이라는 enum을 만들었습니다:

<div class="content-ad"></div>

```kotlin
enum class GraphControlType {
    Next {
        override val icon = Icons.Filled.ArrowForward
        override val textResId = R.string.button_next_year
    },
    Previous {
        override val icon = Icons.Filled.ArrowBack
        override val textResId = R.string.button_previous_year
    };

    abstract val icon: ImageVector
    abstract val textResId: Int
}
```

이 enum은 사용할 아이콘과 버튼 텍스트의 리소스 ID를 포함하고 있어요.

이 버튼의 아이디어는 눌렀을 때 다음 해로 초점을 옮기는 것이에요. "다음 해" 버튼이라면 작년부터 시작해서 첫 해로 전진할 거예요. "이전 해" 버튼인 경우에는 반대로 동작할 거에요. 첫 해에 있을 때에는 지난 해로 이동할 거예요.

우리는 이를 사용하여 다음으로 초점을 맞출 인덱스(또는 연도)를 가져오는 도우미 함수가 있어요 (모든 변화를 담고 있는 커밋에서 확인할 수 있어요). 우리는 전달하는 유형에 따라 어떤 함수를 사용할지 정의하고 싶어요:

<div class="content-ad"></div>

```kotlin
// ControlButton

val nextIndexFunc = when (type) {
    GraphControlType.Next -> ::getNextIndex
    GraphControlType.Previous -> ::getPreviousIndex
}
```

이제 우리가 모든 것을 정의했으니 버튼을 추가할 거에요:

```kotlin
// ControlButton

OutlinedButton(onClick = {
    val newSelectedIndex = when (highlightedX) {
        null -> firstFocusIndex
        else -> nextIndexFunc(currentIndex, lastIndex)
    }

    setFocus(newSelectedIndex)
}) {
    if (type == GraphControlType.Previous) 
        ControlButtonIcon(icon = type.icon)
    Text(stringResource(id = type.textResId))
    if (type == GraphControlType.Next) 
        ControlButtonIcon(icon = type.icon)
}
```

그래서 버튼의 onClick 핸들러에서는 하이라이트된 x 값이 null인지 확인하고, 그렇다면 초점을 첫 번째 초점 인덱스로 이동시키고, 이는 "다음 해" 버튼에 대한 첫 번째 인덱스이거나 "이전 해" 버튼에 대한 마지막 인덱스입니다. 그렇지 않으면 정의된 nextIndexFunc를 사용하여 초점이 이동해야 할 다음 인덱스를 가져올 거에요. 인덱스를 얻으면 해당 인덱스로 setFocus 함수를 호출합니다.


<div class="content-ad"></div>

버튼의 내용에는 아이콘의 위치를 정의하는 데 type을 사용합니다. "전년도" 버튼의 경우 텍스트 앞에 있고, "다음 년도" 버튼의 경우 텍스트 뒤에 있습니다.

마지막 단계는 ControlButtons-composable에 컨트롤을 추가하는 것입니다. 이들을 같은 행에 배치하고 싶으므로 Row-component로 감쌉니다. 명확하게 표현하기 위해 일부 수정자를 생략했습니다:

```js
Row(...) {
    ControlButton(
        type = GraphControlType.Previous,
        currentIndex = selectedIndex,
        lastIndex = lastIndex,
        firstFocusIndex = lastIndex,
        highlightedX = highlightedX,
        setFocus = {
            setSelectedIndexAndFocus(it)
        },
    )
    ControlButton(
        type = GraphControlType.Next,
        currentIndex = selectedIndex,
        lastIndex = lastIndex,
        firstFocusIndex = 0,
        highlightedX = highlightedX,
        setFocus = {
            setSelectedIndexAndFocus(it)
        },
    )
}
```

이제 컨트롤을 추가했습니다. 여기서 작동 방법을 보여주는 비디오가 있습니다:

<div class="content-ad"></div>

예스, 영원히 사랑을 꿈꾸는 일은 해도 즐겨볼 만한 가치가 충분하지만 가끔은 현실성이 중요할 때가 있죠. 무덤이라는 것은 현실을 직시하고 받아들이는 과정이자 영원한 사랑에 이르는 길 중 하나랍니다. 무덤 카드는 변화와 새로운 시작을 상징하며, 과거의 것을 떠나 새로운 시대로 나아가는 것을 의미합니다. 이 카드를 받았을 때는 과거에 대한 무거운 짊어진 짐을 내려놓고, 새로운 시작에 기회를 주는 준비가 되었다는 신호일 수 있어요. 계속 앞으로 나아가고 변화를 환영해 보세요!

<div class="content-ad"></div>

그래서 어떻게 생각하는지 궁금할 거예요. 스위치 장치 사용자의 경우 앱이 어떻게 보이는지 알려드릴게요. 짧은 비디오를 녹화했어요:

비디오를 보면 이 버튼을 추가하면 스위치 장치 사용자가 강조된 연도를 변경하고 각 연도의 백분율을 볼 수 있어요.

# 고려 사항

이 구현은 접근성 관점에서 몇 가지 고려 사항이 있어요. 만약 처음부터 이러한 컨트롤로 시작했다면, 화면 판독기 및 키보드 사용자의 관점에서 전체 그래프를 조금 다르게 구축했을 것 같아요. 하지만 이번에는 이전 블로그 게시물 위에 구축했기 때문에 그런 제약이 있었어요. 이 구현에 대한 몇 가지 고민과 내 결정에 대한 이유를 공유할게요.

<div class="content-ad"></div>

# 논설명 버튼

먼저, UI의 버튼에는 "다음 해"와 "이전 해"라는 텍스트 내용이 있습니다. 스크린 리더 사용자가 이 버튼을 누르면 아무 일도 일어나지 않습니다. 특히 시각 장애가 있거나 화면을 끈 사용자의 경우 더욱 그렇습니다. 현재 선택된 연도를 이동시켜도 접근성 포커스는 이동되지 않습니다. 실제로 포커스는 전혀 이동하지 않습니다.

만약 처음에 버튼 컨트롤을 추가했다면, 선택된 연도 값을 포함하는 레이블 구성 요소를 라이브 영역으로 만들었을 것입니다. 이렇게 하면 연도가 변경될 때마다 내용이 알려질 것입니다.

이번 버전에도 그것을 추가할 수도 있었지만, 이 블로그 글을 가능한 한 간단하게 유지하고 싶었습니다. 또한 사용자가 선형적으로 탐색하는 경우 버튼을 클릭하기 전에 그래프와 데이터에 먼저 만나게 될 것입니다. 이렇게 하면 버튼에 대한 문맥과 연도를 탐색할 수 있는 방법이 생깁니다. 선형적으로 탐색하지 않는 경우, 사용자가 사용하는 모드에 따라 상황이 달라질 수 있습니다.

<div class="content-ad"></div>

# 추가 탭 정지 지점

또 하나의 고려해야 할 사항은 이 구현이 키보드 사용자에게 추가적인 탭 정지 지점을 만든다는 것입니다. 단지 두 개 뿐이지만, 각 키를 누를 때마다 고통스러운 경우라면 그 두 개도 많을 수 있습니다.

이 경우 스크린 리더 사용자를 위해 설명한 비슱한 구현이 키보드 사용자에게도 더 나을 수 있습니다. 그렇게 하면 그래프에서 많은 탭 정지 지점을 제거하고 이 두 개 버튼만 남게 할 수 있습니다. 물론, 버튼은 키 입력으로 활성화되어야 하므로 그 부분도 고려해야 합니다.

# 마무리

<div class="content-ad"></div>

이번 블로그 포스트에서는 그래프 탐색을 위한 가시적 컨트롤을 추가하는 방법에 대해 살펴보았어요. 이 접근 방식은 패스 기반 제스처에 문제가 있는 사람들이나 스위치 장치를 사용하는 사람들에게 문제를 해결해 주었습니다. 또한 이 구현이 스크린 리더 및 키보드 탐색과 관련된 몇 가지 고려 사항을 살펴보았어요.

이 블로그 포스트 시리즈에서는 이제까지 포인터 입력 기반의 그래프 탐색 문제를 해결하기 위한 두 가지 접근 방식을 제공했어요: 그래프의 연도 위에 오버레이 및 포커싱 가능한 영역을 추가하고 제어 버튼을 추가하는 것입니다. 모든 앱과 사용 사례가 다르기 때문에 그래프를 더 접근 가능하게 만들기 위해 둘 중 하나 또는 둘 다 필요할 수 있습니다. 때로는 다른 유형의 솔루션이 답이 될 수도 있어요.

# 블로그 포스트에 있는 링크들

- Jetpack Compose를 사용하여 더 접근 가능한 그래프 만들기 Part 1: 콘텐츠 설명 추가하기
- Jetpack Compose를 사용하여 더 접근 가능한 그래프 만들기 Part 2: 키보드 상호작용 추가하기
- Jetpack Compose를 사용하여 더 접근 가능한 그래프 만들기 Part 3: 색상으로 구분하기
- 이 커밋에서 최종 코드를 찾을 수 있어요.