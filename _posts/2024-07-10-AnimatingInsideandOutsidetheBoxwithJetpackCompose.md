---
title: "Jetpack Compose로 박스 안팎을 애니메이션 하는 방법"
description: ""
coverImage: "/assets/img/2024-07-10-AnimatingInsideandOutsidetheBoxwithJetpackCompose_0.png"
date: 2024-07-10 01:37
ogImage: 
  url: /assets/img/2024-07-10-AnimatingInsideandOutsidetheBoxwithJetpackCompose_0.png
tag: Tech
originalTitle: "Animating Inside and Outside the Box with Jetpack Compose"
link: "https://medium.com/proandroiddev/animating-inside-and-outside-the-box-with-jetpack-compose-a56eba1b6af6"
isUpdated: true
---





## 안드로이드에서 Compose를 활용한 창의적인 애니메이션 구축

![Animation](/assets/img/2024-07-10-AnimatingInsideandOutsidetheBoxwithJetpackCompose_0.png)

# 소개

애니메이션은 사용자 인터페이스가 생동감 있고 매력적으로 느껴지도록 하는 힘을 지니고 있습니다. 안드로이드에서 Jetpack Compose를 활용하면 이 힘을 손쉽게 경험할 수 있으며, 고급 도구를 통해 참으로 동적인 UI를 만들어낼 수 있습니다. 이 글에서는 기본적인 내용을 넘어 Jetpack Compose 내에서 애니메이션의 심화된 부분을 탐구해 보겠습니다.

<div class="content-ad"></div>

우리는 유연한 물리학 기반 동작을 만들어 현실적인 감각을 더하는 기술부터, 서술적인 요소를 더하는 복잡한 안무 시퀀스까지 다룰 예정입니다. 당신이 기술을 세밀하게 다듬고 있는지 아니면 가능한 것에 대해 궁금해하는 중이던 중이든, 이 여정은 앱이 부드럽게 작동할 뿐만 아니라 모든 상호작용에서 사용자를 기쁘게 하는데 실용적인 통찰력을 제공할 것입니다.

자세히 알아보고, 이러한 애니메이션은 UI 디자인에 대한 접근 방식을 변화시키며 사용자를 위해 더 직관적이고 반응성이 있고 즐거운 경험을 만들어낼 수 있는지 확인해보겠습니다.

# 섹션 1 — Jetpack Compose에서 사용자 정의 애니메이션 핸들러

![Custom Animation Handlers in Jetpack Compose](https://miro.medium.com/v2/resize:fit:1400/1*C1_mzDHNHOfSZkIiULgRzw.gif)

<div class="content-ad"></div>

## 동적 상호작용을 포용하는 사용자 정의 애니메이션

이 섹션에서는 Jetpack Compose에서 고급 사용자 정의 애니메이션 핸들러의 사용을 탐구하여 동적이고 상호작용적인 UI 요소를 만드는 방법을 살펴보겠습니다. 사용자 상호작용이 애니메이션에 의미 있는 방식으로 영향을 미치는 실제 예제에 중점을 두었습니다.

## 예시 — 상호작용하는 게임 캐릭터 이동

이 개념을 설명하기 위해 게임 캐릭터(얼굴 아이콘으로 표시됨)가 사용자가 드래그할 수 있는 제어 점으로 결정된 경로를 따라가는 예제를 통해 설명하겠습니다.

<div class="content-ad"></div>


@Composable
fun GameCharacterMovement() {
    val startPosition = Offset(100f, 100f)
    val endPosition = Offset(250f, 400f)
    val controlPoint = remember { mutableStateOf(Offset(200f, 300f)) }
    val position = remember { Animatable(startPosition, Offset.VectorConverter) }

    LaunchedEffect(controlPoint.value) {
        position.animateTo(
            targetValue = endPosition,
            animationSpec = keyframes {
                durationMillis = 5000
                controlPoint.value at 2500 // midway point controlled by the draggable control point
            }
        )
    }

    val onControlPointChange: (offset: Offset) -> Unit = {
        controlPoint.value = it
    }

    Box(modifier = Modifier.fillMaxSize()) {

        Icon(
            Icons.Filled.Face, contentDescription = "Localized description", modifier = Modifier
                .size(50.dp)
                .offset(x = position.value.x.dp, y = position.value.y.dp)
        )

        DraggableControlPoint(controlPoint.value, onControlPointChange)
    }
}


## 설명

- GameCharacterMovement 함수는 게임 캐릭터를 나타내는 아이콘을 애니메이션화합니다. 애니메이션 경로는 사용자 상호작용에 의해 설정 및 업데이트되는 controlPoint에 의해 제어됩니다.
- Animatable은 아이콘의 위치를 시작 지점에서 endPosition으로 부드럽게 전환하기 위해 사용됩니다.
- LaunchedEffect는 controlPoint 값의 변경 사항을 감지하여 제어점이 이동될 때마다 애니메이션을 다시 트리거합니다.
- animationSpec은 애니메이션의 지속 시간, 지연 시간 및 이징을 정의하는 구성입니다. 애니메이션화된 값이 시간이 지남에 따라 어떻게 변하는지를 결정합니다.
- keyframes는 애니메이션 중 특정 시점에서 값을 지정할 수 있게 해주며, 애니메이션의 중간 지점을 제어하는 데 유용합니다. 복잡하고 조정된 애니메이션을 만드는 데 특히 유용합니다.
- keyframes 블록은 키프레임 시퀀스로 애니메이션을 정의합니다. 2500밀리초(중간 지점)에서 캐릭터는 제어점에 도달한 후 끝 위치로 이동합니다.


Composable
fun DraggableControlPoint(controlPoint: Offset, onControlPointChange: (Offset) -> Unit) {
    var localPosition by remember { mutableStateOf(controlPoint) }
    Box(
        modifier = Modifier
            .offset {
                IntOffset(
                    x = localPosition.x.roundToInt() - 15,
                    y = localPosition.y.roundToInt() - 15
                )
            }
            .size(30.dp)
            .background(Color.Red, shape = CircleShape)
            .pointerInput(Unit) {
                detectDragGestures(onDragEnd = {
                    onControlPointChange(localPosition)
                }) { _, dragAmount ->
                    // 화면 경계에 따라 조정
                    val newX = (localPosition.x + dragAmount.x).coerceIn(0f, 600f)
                    val newY = (localPosition.y + dragAmount.y).coerceIn(0f, 600f)
                    localPosition = Offset(newX, newY)
                }
            }
    )
}


<div class="content-ad"></div>

## 설명

- DraggableControlPoint는 사용자가 제어 지점의 위치를 상호 작용적으로 변경할 수 있게 하는 함수입니다.
- 제어 지점을 끌면 localPosition이 업데이트되며, 드래그 제스처(onDragEnd) 완료 시 GameCharacterMovement로 다시 반영됩니다. 이 상호 작용은 애니메이션된 아이콘의 경로를 변경합니다.

## 실제 사용 사례

- 대화형 교육 앱: 교육 앱에서 애니메이션을 사용하여 학습을 더 매력적으로 만들 수 있습니다. 예를 들어, 천문학 앱에서 행성을 궤도를 따라 끌어서 다양한 별자리를 볼 수 있습니다.
- 대화형 스토리텔링과 게임: 디지털 스토리텔링이나 게임 앱에서 사용자가 이야기나 게임 환경에 끌어 요소를 통해 영향을 미칠 수 있게하면 더 몰입적인 경험을 만들 수 있습니다.

<div class="content-ad"></div>

# 섹션 2 — 젯팩 컴포즈에서 복잡한 애니메이션 조정하기

## 조화로운 효과를 위해 여러 요소 동기화하기

이 섹션에서는 젯팩 컴포즈에서 복잡한 애니메이션을 조정하는 예술에 대해 탐구합니다. 우리는 여러 요소가 매끄럽게 상호작용하는 동기화된 애니메이션을 만드는 데 초점을 맞추어 전반적인 사용자 경험을 향상시킵니다.

## 가) 연쇄 반응 애니메이션 — 도미노 효과

<div class="content-ad"></div>

UI에서 도미노 효과를 만드는 방법은 하나의 애니메이션이 끝나면 다음 것이 시작될 수 있도록 일련의 애니메이션을 설정하는 것입니다.

```kotlin
@Composable
fun DominoEffect() {
    val animatedValues = List(6) { remember { Animatable(0f) } }

    LaunchedEffect(Unit) {
        animatedValues.forEachIndexed { index, animate ->
            animate.animateTo(
                targetValue = 1f,
                animationSpec = tween(durationMillis = 1000, delayMillis = index * 100)
            )
        }
    }

    Box(modifier = Modifier.fillMaxSize()) {
        animatedValues.forEachIndexed { index, value ->
            Box(
                modifier = Modifier
                    .size(50.dp)
                    .offset(x = ((index + 1) * 50).dp, y = ((index + 1) * 30).dp)
                    .background(getRandomColor(index).copy(alpha = value.value))
            )
        }
    }
}

fun getRandomColor(seed: Int): Color {
    val random = Random(seed = seed).nextInt(256)
    return Color(random, random, random)
}
```

## 설명

<div class="content-ad"></div>

**A) 애니메이션 그래픽 효과**

- animatedValues는 각 상자의 알파(불투명도)를 제어하는 Animatable 값 목록입니다.
- LaunchedEffect는 이러한 값들에 대한 애니메이션 순서를 트리거하며, 각 상자가 이전 상자 뒤를 따라서 서서히 나타나는 연출을 만듭니다. 마치 도미노가 넘어지는 것처럼요.
- getRandomColor 함수는 각 상자에 대해 랜덤한 회색 색조를 생성하여 시퀀스 내 각 구성 요소에 독특한 시각적 요소를 추가합니다.
- 상자들은 화면을 대각선으로 따라 배치되어, 도미노 효과를 강조합니다.

## B) 인터랙티브 스크롤 가능한 타임라인

![Timeline Gif](https://miro.medium.com/v2/resize:fit:1400/1*Kk-V0g5pEqy83NajRy_6lA.gif)

이 타임라인에서 각 요소는 사용자가 타임라인을 스크롤할 때 서서히 나타나고 위치로 이동합니다. 스크롤 가능한 목록에는 LazyColumn을 사용하고 애니메이션에는 Animatable을 사용할 것입니다.

<div class="content-ad"></div>

```kotlin
@Composable
fun InteractiveTimeline(timelineItems: List<String>) {
    val scrollState = rememberLazyListState()

    LazyColumn(state = scrollState) {
        itemsIndexed(timelineItems) { index, item ->
            val animatableAlpha = remember { Animatable(0f) }
            val isVisible = remember {
                derivedStateOf {
                    scrollState.firstVisibleItemIndex <= index
                }
            }

            LaunchedEffect(isVisible.value) {
                if (isVisible.value) {
                    animatableAlpha.animateTo(
                        1f, animationSpec = tween(durationMillis = 1000)
                    )

                }
            }

            TimelineItem(
                text = item,
                alpha = animatableAlpha.value,
            )
        }
    }
}

@Composable
fun TimelineItem(text: String, alpha: Float) {
    Row(
        modifier = Modifier
            .fillMaxWidth()
            .background(Color.DarkGray.copy(alpha = alpha))
            .padding(16.dp),
        verticalAlignment = Alignment.CenterVertically
    ) {
        Text(
            text = text,
            color = Color.White,
            modifier = Modifier.fillMaxWidth(),
            textAlign = TextAlign.Center,
            fontSize = 18.sp,
            fontWeight = FontWeight.SemiBold
        )
    }
}
```

### 설명

- animatableAlpha은 각 타임라인 항목의 투명도를 제어하며, 처음에는 0으로 설정됩니다 (완전 투명).
- isVisible 상태는 현재 스크롤 위치에서 파생되어, 항목이 보이는지 여부를 결정합니다.
- 사용자가 스크롤할 때, LaunchedEffect가 뷰포트에 진입하는 항목에 대한 페이드 인 애니메이션을 트리거합니다.

### 사용 사례


<div class="content-ad"></div>

이 상호작용적인 타임라인은 여러 이벤트나 단계를 시각적으로 매력적인 방식으로 제시하고 싶은 애플리케이션에 이상적입니다. 애니메이션은 항목들이 나타날 때 주목을 끌어 사용자 참여도를 높입니다.

# 섹션 3 — Jetpack Compose에서 현실적 물리 기반 애니메이션

![gif](https://miro.medium.com/v2/resize:fit:1400/1*lZ_rpGorcFzewpUJN6WPAQ.gif)

## UI 역학 향상을 위한 물리 활용

<div class="content-ad"></div>

이 섹션에서는 물리학 원리를 Jetpack Compose 애니메이션에 통합하는 방법을 탐색하며, UI에 현실성과 상호작용을 추가합니다. 우리는 탄성 드래그 상호작용 예제에 초점을 맞출 것입니다.

## 드래그 시 탄성 효과

이 예제는 아이콘에 대한 탄성 드래그 상호작용을 보여줍니다. 수직으로 드래그될 때 아이콘이 늘어나며 탄성 효과로 튕겨져서, 스프링이나 고무줄의 행동을 시뮬레이션합니다.

```kotlin
@Composable
fun ElasticDraggableBox() {
    var animatableOffset by remember { mutableStateOf(Animatable(0f)) }

    Box(modifier = Modifier.fillMaxSize().background(Color(0xFFFFA732)), contentAlignment = Alignment.Center) {
        Box(
            modifier = Modifier
                .offset(y = animatableOffset.value.dp)
                .draggable(
                    orientation = Orientation.Vertical,
                    state = rememberDraggableState { delta ->
                        animatableOffset = Animatable(animatableOffset.value + delta)
                    },
                    onDragStopped = {
                        animatableOffset.animateTo(0f, animationSpec = spring())
                    }
                )
                .size(350.dp),
            contentAlignment = Alignment.Center
        ) {
            Icon(
                Icons.Filled.Favorite,
                contentDescription = "heart",
                modifier = Modifier.size(animatableOffset.value.dp + 150.dp),
                tint = Color.Red
            )
        }
    }
}
```

<div class="content-ad"></div>

## 설명

- 아이콘을 포함하는 상자(composable)를 만들고, draggable modifier를 사용하여 드래그할 수 있게 합니다.
- animatableOffset은 드래깅으로 인해 아이콘의 수직 오프셋을 추적합니다.
- 드래그 중에는 아이콘의 크기가 드래그 양에 따라 변경되어 스트레칭 효과가 만들어집니다.
- 드래그가 멈출 때 (onDragStopped), animatableOffset이 스프링 애니메이션을 사용하여 0f로 다시 애니메이팅되어 아이콘이 원래 크기와 위치로 되돌아갑니다.

# 섹션 4 — Jetpack Compose에서 제스처 기반 애니메이션

## 응답형 제스처로 사용자 경험 향상하기

<div class="content-ad"></div>

이 섹션에서는 사용자 제스처로 제어되는 애니메이션을 만드는 데 Jetpack Compose를 사용하는 방법을 살펴보겠습니다. 우리는 두 가지 예제에 초점을 맞출 것입니다 - 멀티 터치 가능한 이미지와 제스처 제어 오디오 파형.

## A) 멀티 터치 가능한 이미지

이 예에서는 사용자가 핀치, 줌, 회전과 같은 멀티 터치 제스처를 사용하여 상호 작용할 수 있는 이미지 뷰를 만들 것입니다.

![멀티 터치 이미지](https://miro.medium.com/v2/resize:fit:1400/1*34WxcBivTWhiCY6KVSVelQ.gif)

<div class="content-ad"></div>

```kotlin
@Composable
fun TransformableImage(imageId: Int = R.drawable.android) {
    var scale by remember { mutableStateOf(1f) }
    var rotation by remember { mutableStateOf(0f) }
    var offset by remember { mutableStateOf(Offset.Zero) }

    Box(modifier = Modifier.fillMaxSize().background(Color.DarkGray), contentAlignment = Alignment.Center) {
        Image(
            painter = painterResource(id = imageId),
            contentDescription = "Transformable image",
            contentScale = ContentScale.Crop,
            modifier = Modifier
                .size(300.dp)
                .graphicsLayer(
                    scaleX = scale,
                    scaleY = scale,
                    rotationZ = rotation,
                    translationX = offset.x,
                    translationY = offset.y
                )
                .pointerInput(Unit) {
                    detectTransformGestures { _, pan, zoom, rotate ->
                        scale *= zoom
                        rotation += rotate
                        offset += pan
                    }
                }
        )
    }
}
```

## 설명

- Image 컴포저블은 graphicsLayer를 사용하여 스케일, 회전 및 이동과 같은 변환을 적용합니다.
- detectTransformGestures를 사용하여 멀티 터치 제스처를 처리하고, 스케일, 회전 및 오프셋을 업데이트합니다.

## B) 제스처로 제어되는 Waveform


<div class="content-ad"></div>

어떨까요? 위에 보여드린 웨이브폼 시각화는 사용자 제스처에 따라 모양이 변하죠. 그렇게 하면 진폭과 주파수 같은 측면을 제어할 수 있답니다.

![Waveform Visualization](https://miro.medium.com/v2/resize:fit:1400/1*qKzb1XpUrSGKdCL-OxhtLw.gif)

```kotlin
@Composable
fun GestureControlledWaveform() {
    var amplitude by remember { mutableStateOf(100f) }
    var frequency by remember { mutableStateOf(1f) }

    Canvas(modifier = Modifier
        .fillMaxSize()
        .pointerInput(Unit) {
            detectDragGestures { _, dragAmount ->
                amplitude += dragAmount.y
                frequency += dragAmount.x / 500f 
                // 드래그에 따라 주파수 조절
            }
        }
        .background(
            Brush.verticalGradient(
                colors = listOf(Color(0xFF003366), Color.White, Color(0xFF66B2FF))
            )
        )) {
        val width = size.width
        val height = size.height
        val path = Path()

        val halfHeight = height / 2
        val waveLength = width / frequency

        path.moveTo(0f, halfHeight)

        for (x in 0 until width.toInt()) {
            val theta = (2.0 * Math.PI * x / waveLength).toFloat()
            val y = halfHeight + amplitude * sin(theta.toDouble()).toFloat()
            path.lineTo(x.toFloat(), y)
        }

        val gradient = Brush.horizontalGradient(
            colors = listOf(Color.Blue, Color.Cyan, Color.Magenta)
        )

        drawPath(
            path = path,
            brush = gradient
        )
    }
}
```

## 설명

여기까지요! 이제 사용자 제스처로 진폭과 주파수를 제어할 수 있는 멋진 웨이브폼 시각화를 만들어 보세요. 만든 다음 저에게 댓글을 남겨 주세요! 🌟

<div class="content-ad"></div>

- 진폭과 주파수는 각각 파형의 진폭과 주파수를 제어하는 상태 변수입니다.
- Canvas composable은 파형을 그리는 데 사용됩니다. Canvas 내부의 그리기 로직은 사인 함수를 기반으로 각 X 위치에 대한 Y 위치를 계산하여 파동 효과를 만듭니다.
- detectDragGestures 수정자는 사용자 드래그 제스처를 기반으로 진폭과 주파수를 업데이트하는 데 사용됩니다. 수평 드래그는 주파수를 조절하고, 수직 드래그는 진폭을 조절합니다.
- 사용자가 화면을 가로지르는 동안 파형의 모양이 그에 따라 변경되어 상호 작용적인 경험을 만듭니다.

## 참고

- 이것은 기본적인 구현 예시입니다. 더 현실적인 오디오 파형을 위해서는 실제 오디오 데이터를 통합해야 합니다.
- 파형의 제스처에 대한 반응성은 드래그 중에 진폭과 주파수가 어떻게 수정되는지 조절하여 미세 조정할 수 있습니다.

이 예시는 Compose에서 기본적인 상호작용 파형을 만드는 방법을 보여줍니다. 이를 확장하거나 변경하여 더 복잡한 사용 사례에 적용하거나 보다 정교한 제스처를 처리할 수 있습니다.

<div class="content-ad"></div>

# 섹션 5 — 제트팩 콤포즈(State-driven Animation Patterns in Jetpack Compose)

![State-driven Animation Patterns in Jetpack Compose](https://miro.medium.com/v2/resize:fit:1400/1*nkfhmC6JjQnshL3y_izhKg.gif)

## 데이터 및 상태 변경에 따라 UI 애니메이션 적용

이 섹션에서는 데이터 또는 UI 상태 변경에 따라 움직이는 애니메이션을 생성하는 데 초점을 맞춥니다. 앱의 상호 작용성 및 응답성을 향상시킵니다. 데이터 그래프 애니메이션 및 다중 상태 UI에서 상태 전환 구현을 탐구해 보겠습니다.

<div class="content-ad"></div>

## A) 데이터 주도형 그래프 애니메이션

이 예시는 데이터 세트의 변경에 따라 그래프의 경로가 애니메이션되는 선 그래프를 보여줍니다.

```js
@Composable
fun AnimatedGraphExample() {
    var dataPoints by remember { mutableStateOf(generateRandomDataPoints(5)) }

    Column(
        modifier = Modifier
            .fillMaxSize()
            .background(Color.DarkGray)
    ) {
        AnimatedLineGraph(dataPoints = dataPoints)

        Spacer(modifier = Modifier.height(16.dp))

        Button(
            onClick = {
                dataPoints = generateRandomDataPoints(5)
            },
            modifier = Modifier.align(Alignment.CenterHorizontally),
            colors = ButtonDefaults.buttonColors(containerColor = Color.Green)
        ) {
            Text(
                "데이터 업데이트",
                fontWeight = FontWeight.Bold,
                color = Color.DarkGray,
                fontSize = 18.sp
            )
        }
    }
}

@Composable
fun AnimatedLineGraph(dataPoints: List<Float>) {
    val animatableDataPoints = remember { dataPoints.map { Animatable(it) } }
    val path = remember { Path() }

    LaunchedEffect(dataPoints) {
        animatableDataPoints.forEachIndexed { index, animatable ->
            animatable.animateTo(dataPoints[index], animationSpec = TweenSpec(durationMillis = 500))
        }
    }

    Canvas(
        modifier = Modifier
            .fillMaxWidth()
            .height(400.dp)
    ) {
        path.reset()
        animatableDataPoints.forEachIndexed { index, animatable ->
            val x = (size.width / (dataPoints.size - 1)) * index
            val y = size.height - (animatable.value * size.height)
            if (index == 0) path.moveTo(x, y) else path.lineTo(x, y)
        }
        drawPath(path, Color.Green, style = Stroke(5f))
    }
}

fun generateRandomDataPoints(size: Int): List<Float> {
    return List(size) { Random.nextFloat() }
}
```

## 설명

<div class="content-ad"></div>

- AnimatedGraphExample 코브 중에선 선 그래프의 데이터 포인트를 업데이트할 수 있는 환경을 만들어요.
- 그래프는 Canvas에서 그려지는데, drawPath 메서드는 animatableDataPoints의 애니메이션 값들을 사용해요.
- 그래프의 각 데이터 포인트에 대해 캔버스 상의 해당 x(가로)와 y(세로) 위치를 계산해야 해요.
- x 계산 — x 위치는 데이터 포인트의 인덱스와 캔버스의 전체 너비에 기반하여 계산돼요. 데이터 포인트들을 캔버스 너비에 고르게 분배해요.

```js
val x = (size.width / (dataPoints.size - 1)) * index
```

- y 계산 — y 위치는 데이터 포인트의 값(animatable.value)과 캔버스의 높이에 기반하여 계산돼요.

```js
val y = size.height - (animatable.value * size.height)
```

<div class="content-ad"></div>

**A) 멀티스테이트 UI에서 상태 전이**

멀티스테이트 UI에서 상태 전이를 구현하는 방법은 각기 다른 UI 상태 간을 애니메이션화하기 위해 Animatable을 사용하는 것입니다.

```kotlin
enum class UIState { StateA, StateB, StateC }

@Composable
fun StateTransitionUI() {
    var currentState by remember { mutableStateOf(UIState.StateA) }

    Box(
        modifier = Modifier
            .fillMaxSize()
            .background(getBackgroundColorForState(currentState)),
        contentAlignment = Alignment.Center
    ) {
        AnimatedContent(currentState = currentState)

        Button(
            onClick = { currentState = getNextState(currentState) },
            modifier = Modifier.align(Alignment.BottomCenter)
        ) {
            Text("다음 상태")
        }
    }
}

@Composable
fun AnimatedContent(currentState: UIState) {
    AnimatedVisibility(
        visible = currentState == UIState.StateA,
        enter = fadeIn(animationSpec = tween(durationMillis = 2000)) + expandVertically(),
        exit = fadeOut(animationSpec = tween(durationMillis = 2000)) + shrinkVertically()
    ) {
        Text("이것은 ${currentState.name}입니다", fontSize = 32.sp)
    }

    // B 및 C에 대한 유사한 블록들
}

fun getBackgroundColorForState(state: UIState): Color {
    return when (state) {
        UIState.StateA -> Color.Red
        UIState.StateB -> Color.Green
        UIState.StateC -> Color.Blue
    }
}

fun getNextState(currentState: UIState): UIState {
    return when (currentState) {
        UIState.StateA -> UIState.StateB
        UIState.StateB -> UIState.StateC
        UIState.StateC -> UIState.StateA
    }
}
```

<div class="content-ad"></div>

## 설명

- 이 예시에서 AnimatedVisibility는 각 상태의 내용의 나타나고 사라지는 애니메이션을 다루기 위해 사용됩니다. 이는 상태가 변경될 때 부드러운 전환 효과를 추가합니다.
- 각 상태(StateA, StateB, StateC)마다 해당 내용의 가시성을 페이드 및 확대/축소 애니메이션으로 제어하는 AnimatedVisibility 블록이 있습니다.
- AnimatedVisibility의 enter 및 exit 매개변수는 내용이 나타나거나 숨겨질 때의 애니메이션을 정의합니다.

# Section 6 — Compose에서 모프형 모양 변환

{% raw %}
![Example GIF](https://miro.medium.com/v2/resize:fit:1400/1*q82EIocVzR8XBMuG_14mdg.gif)
{% endraw %}

<div class="content-ad"></div>

도형 간 변환 애니메이션을 만드는 것은 이러한 도형들의 속성을 보간하는 것을 포함합니다.

```kotlin
@Composable
fun ShapeMorphingAnimation() {
    val animationProgress = remember { Animatable(0f) }

    LaunchedEffect(Unit) {
        animationProgress.animateTo(
            targetValue = 1f,
            animationSpec = infiniteRepeatable(
                animation = tween(2000, easing = LinearOutSlowInEasing),
                repeatMode = RepeatMode.Reverse
            )
        )
    }

    Canvas(modifier = Modifier.padding(40.dp).fillMaxSize()) {
        val sizeValue = size.width.coerceAtMost(size.height) / 2
        val squareRect = Rect(center = center, sizeValue)

        val morphedPath = interpolateShapes(progress = animationProgress.value, squareRect = squareRect)
        drawPath(morphedPath, color = Color.Blue, style = Fill)
    }
}

fun interpolateShapes(progress: Float, squareRect: Rect): Path {
    val path = Path()

    val cornerRadius = CornerRadius(
        x = lerp(start = squareRect.width / 2, stop = 0f, fraction = progress),
        y = lerp(start = squareRect.height / 2, stop = 0f, fraction = progress)
    )

    path.addRoundRect(
        roundRect = RoundRect(rect = squareRect, cornerRadius = cornerRadius)
    )

    return path
}

fun lerp(start: Float, stop: Float, fraction: Float): Float {
    return (1 - fraction) * start + fraction * stop
}
```

## 설명

- ShapeMorphingAnimation은 0과 1 사이의 animationProgress 값을 토글하는 무한 애니메이션을 설정합니다.
- Canvas 컴포저블을 사용하여 도형을 그립니다. 여기서 캔버스 크기를 기반으로 정사각형(squareRect)의 차원을 정의합니다.
- interpolateShapes는 현재 애니메이션 진행 상황과 정사각형의 사각형을 가져와 원과 정사각형 사이를 보간합니다. 선형 보간(lerp)을 사용하여 반올림된 직사각형(cornerRadius)의 모서리 반경을 서서히 조정하여 우리의 형태를 변형합니다.
- progress가 0일 때 cornerRadius는 직사각형 크기의 반으로 설정되어 원 모양을 만듭니다. progress가 1일 때 cornerRadius는 0으로 설정되어 정사각형을 만듭니다.

<div class="content-ad"></div>

## 현실 세계에서의 사용 사례

- 로딩 및 진행 표시기 - 형태 변화를 이용하여 로딩 또는 진행 표시기를 만들어 더 매혹적인 로딩 또는 진행 상태를 표시할 수 있습니다. 시각적으로 흥미로운 방식으로 진행이나 로딩 상태를 표시할 수 있습니다.
- UI에서의 아이콘 전환 - 형태 변화를 이용하여 아이콘들이 사용자 작업에 대한 시각적 피드백을 제공하는 데 사용될 수 있습니다. 예를 들어, 재생 버튼이 클릭되었을 때 일시 중지 버튼으로 변하는 것이나 햄버거 메뉴 아이콘이 뒤로 화살표로 변하는 것과 같은 것이 있습니다.
- 데이터 시각화 - 복잡한 데이터 시각화에서 형태 변화는 사용자가 시간이 지남에 따른 변경이나 범주 간의 변화를 따르고 이해하는 데 도움이 되는 다양한 데이터 뷰나 상태 간의 전환을 돕습니다.

# 눈 내리는 효과를 원하십니까?

간단한 입자 시스템을 사용하여 눈이 내리는 효과를 생성하는 방법을 시연하겠습니다.

<div class="content-ad"></div>

안녕하세요! 오늘은 눈송이 애니메이션에 관한 코드를 살펴보겠습니다.

```kotlin
class Snowflake(
    var x: Float,
    var y: Float,
    var radius: Float,
    var speed: Float
)

@Composable
fun SnowfallEffect() {
    val snowflakes = remember { List(100) { generateRandomSnowflake() } }
    val infiniteTransition = rememberInfiniteTransition(label = "")

    val offsetY by infiniteTransition.animateFloat(
        initialValue = 0f,
        targetValue = 1000f,
        animationSpec = infiniteRepeatable(
            animation = tween(durationMillis = 5000, easing = LinearEasing),
            repeatMode = RepeatMode.Restart
        ), label = ""
    )

    Canvas(modifier = Modifier.fillMaxSize().background(Color.Black)) {
        snowflakes.forEach { snowflake ->
            drawSnowflake(snowflake, offsetY % size.height)
        }
    }
}

fun generateRandomSnowflake(): Snowflake {
    return Snowflake(
        x = Random.nextFloat(),
        y = Random.nextFloat() * 1000f,
        radius = Random.nextFloat() * 2f + 2f, // 눈송이 크기
        speed = Random.nextFloat() * 1.2f + 1f  // 낙하 속도
    )
}

fun DrawScope.drawSnowflake(snowflake: Snowflake, offsetY: Float) {
    val newY = (snowflake.y + offsetY * snowflake.speed) % size.height
    drawCircle(Color.White, radius = snowflake.radius, center = Offset(snowflake.x * size.width, newY))
}
```

## 설명

- SnowfallEffect 함수는 여러 눈송이(눈송이 객체)로 구성된 입자 시스템을 설정합니다.
- 각 눈송이는 위치(x, y), 반지름(크기), 속도와 같은 속성을 가지고 있습니다.
- rememberInfiniteTransition 및 animateFloat 함수를 사용하여 연속적인 수직 움직임 효과를 만들어 눈 내리는 것을 시뮬레이션합니다.
- Canvas 콤포저블을 사용하여 각 눈송이를 그립니다. drawSnowflake 함수는 각 눈송이의 새 위치를 계산하며, 이동 속도와 애니메이션된 offsetY를 기반으로 합니다.
- 눈송이는 하단에 사라질 시 상단으로 재등장하여 반복되는 눈내리는 효과를 만듭니다. 여기까지가 오늘의 코드 설명이었습니다! 더 궁금한 점이 있거나 질문이 있다면 언제든지 물어주세요. 함께 즐겁게 코딩해봐요! 😊✨

<div class="content-ad"></div>

# 결론

Jetpack Compose에서 애니메이션을 탐색하면서 애니메이션이 시각적 장식 이상의 중요한 도구임을 알 수 있었습니다. 사용자 경험을 흥미롭고 직관적이며 즐거운 것으로 만드는 필수 도구입니다.

## 상호작용 허용하기

다이나믹한 게임 캐릭터 이동부터 상호작용하는 타임라인까지, 애니메이션을 통해 사용자 상호작용을 더욱 흥미롭고 정보 전달력있게 만들 수 있다는 것을 확인했습니다.

<div class="content-ad"></div>

## 현실적인 경험 구현하기

눈 내리는 효과와 변화하는 모양들이 이 툴킷이 디지털 영역에 현실감과 유동성을 가져다 주는 능력을 보여줍니다. 이러한 애니메이션들은 사용자와 공명하는 몰입형 경험을 만들어 냅니다.

## 복잡함을 단순화하다

여러 요소를 통합하거나 상태 전환을 애니메이션화하는 것, 모두 간단하게 수행할 수 있는 점이 두드러집니다.

<div class="content-ad"></div>

# 마무리 말씀

만약 읽은 내용이 마음에 드셨다면, 소중한 피드백이나 감사의 뜻을 자유롭게 남겨주세요. 언제나 개발자 분들과 함께 배우고 협업하며 성장하고 싶어 합니다.

질문이 있으시면 언제든지 메시지 보내주세요!

더 많은 글을 보고 싶으시다면 제 Medium 프로필을 팔로우해주세요.

<div class="content-ad"></div>

마법 예술을 통해 새로운 창조성을 발현하는 것은 정말 흥미로운 일이죠! 저와 같은 영감을 주는 예술가와 소통하고 싶다면, LinkedIn과 Twitter에서 저와 연락하세요! 함께 창조적인 작업을 해보는 것도 좋을 것 같네요.

행복한 마법 예술의 시간 보내세요! ✨🔮✨