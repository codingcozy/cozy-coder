---
title: "Jetpack Compose로 DNA 이중나선 애니메이션 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-06-HowtoCreateaDNAHelixAnimationinJetpackCompose_0.png"
date: 2024-07-06 11:13
ogImage: 
  url: /assets/img/2024-07-06-HowtoCreateaDNAHelixAnimationinJetpackCompose_0.png
tag: Tech
originalTitle: "How to Create a DNA Helix Animation in Jetpack Compose"
link: "https://medium.com/@kappdev/how-to-create-a-dna-helix-animation-in-jetpack-compose-b43244cf734f"
---


환영합니다! 

이번 글에서는 Jetpack Compose를 사용하여 5분 만에 동적 DNA 나선 애니메이션을 만들어 보겠습니다 ⌚️

계속 읽어보시고, 함께 알아보죠! 🚀

![2024-07-06-HowtoCreateaDNAHelixAnimationinJetpackCompose_0](/assets/img/2024-07-06-HowtoCreateaDNAHelixAnimationinJetpackCompose_0.png)

<div class="content-ad"></div>

# 기능

우리는 DNAHelix 조합 가능한 함수를 정의하고 사용자 정의를 위한 매개변수를 탐색하는 것으로 시작해봅시다:

```kotlin
@Composable
fun DNAHelix(
    modifier: Modifier,
    firstColor: Color,
    secondColor: Color,
    pointSize: Dp = 5.dp,
    lineWidth: Dp = 1.5.dp,
    spacing: Dp = 10.dp,
    shifting: Dp = 0.dp,
    curvature: Float = 16f,
    cycleDuration: Int = 3000,
    lineBrush: (firstPoint: Offset, secondPoint: Offset) -> Brush = { fp, sp ->
        Brush.linearGradient(
            colors = listOf(firstColor, secondColor),
            start = fp,
            end = sp
        )
    }
)
```

## 매개변수:

<div class="content-ad"></div>

🧬 modifier 👉 캔버스에 적용할 수정자.

🧬 firstColor 👉 첫 번째 세트의 색상.

🧬 secondColor 👉 두 번째 세트의 색상.

🧬 pointSize 👉 점의 지름.

<div class="content-ad"></div>

🧬 **lineWidth** 👉 점들을 연결하는 선의 너비입니다.

🧬 **spacing** 👉 연이은 점들 사이의 거리입니다.

🧬 **shifting** 👉 점들의 가로 이동량입니다. 나선의 왜곡을 가능하게 합니다.

🧬 **curvature** 👉 나선의 곡률입니다. 곡선이 얼마나 자주 반복되는지에 영향을 줍니다.

<div class="content-ad"></div>

🧬 cycleDuration 👉 한 애니메이션 주기의 지속 시간(밀리초).

🧬 lineBrush 👉 선에 대한 브러시를 정의하는 함수. 기본값은 firstColor에서 secondColor로 선형 그라데이션입니다.

# 구현

## 좌표 유틸리티

<div class="content-ad"></div>

로직을 구현하기 전에, 현재 각도와 점의 위치를 기반으로 좌표를 계산하는 유틸리티 함수를 정의해 봅시다.

```kotlin
private fun calculateCoordinates(angle: Float, radius: Float, centerX: Float, centerY: Float): Offset {
    val y = centerY + radius * sin(Math.toRadians(angle.toDouble())).toFloat()
    return Offset(centerX, y)
}
```

## 애니메이션 처리

무한 애니메이션을 생성하려면, `rememberInfiniteTransition()`과 `animateFloat()`를 함께 사용하여 각도를 0부터 360도까지 애니메이션화할 수 있습니다:

<div class="content-ad"></div>

```kotlin
val helixTransition = rememberInfiniteTransition()
val animatedAngle by helixTransition.animateFloat(
    initialValue = 0f,
    targetValue = 360f,
    animationSpec = infiniteRepeatable(
        animation = tween(durationMillis = cycleDuration, easing = LinearEasing),
        repeatMode = RepeatMode.Restart
    )
)
```

## 드로잉 로직

마지막으로, 캔버스를 사용하여 나선을 그려봅시다:

```kotlin
Canvas(modifier) {
    val spacingPx = spacing.toPx()
    val pointsCount = (size.width / spacingPx).toInt()
    val helixRadius = size.height / 2

    val pointRadiusPx = pointSize.toPx() / 2
    val lineWidthPx = lineWidth.toPx()
    val shiftingPx = shifting.toPx()

    // 각 점을 순회하며 나선을 그립니다
    for (i in 1 until pointsCount) {
        // 나선 애니메이션에 대한 현재 각도 계산
        val currentAngle = (animatedAngle + i * curvature) % 360
        // 현재 점의 x 오프셋 계산
        val xOffset = i * spacingPx

        // 첫 번째 점의 좌표 계산
        val firstPoint = calculateCoordinates(currentAngle, helixRadius, xOffset - shiftingPx, helixRadius)
        // 두 번째 점의 좌표 계산 (180도 차이)
        val secondPoint = calculateCoordinates(currentAngle + 180, helixRadius, xOffset + shiftingPx, helixRadius)

        // 두 점을 연결하는 선 그리기
        drawLine(
            brush = lineBrush(firstPoint, secondPoint),
            strokeWidth = lineWidthPx,
            start = firstPoint,
            end = secondPoint
        )

        // 첫 번째 점 그리기
        drawCircle(
            color = firstColor,
            radius = pointRadiusPx,
            center = firstPoint
        )

        // 두 번째 점 그리기
        drawCircle(
            color = secondColor,
            radius = pointRadiusPx,
            center = secondPoint
        )
    }
}
```

<div class="content-ad"></div>

축하해요 🥳! 성공적으로 만들어냈어요 👏. 전체 코드 구현은 GitHub Gist에서 확인할 수 있어요 🧑‍💻. 이제 어떻게 활용할 수 있는지 살펴봐요.

어학 학습 중이고 새로운 어휘에 어려움을 겪고 계신가요?

그렇다면, 여행을 쉽고 편리하게 만들어줄 이 어플리케이션을 꼭 확인해보시길 강력히 추천해요!

<div class="content-ad"></div>

# 사용 방법 💁‍♂️

DNAHelix 함수를 사용하는 다양한 방법을 살펴봅시다:

## 간단한 나선

기본 설정으로 간단한 나선을 생성하세요: 

<div class="content-ad"></div>

```kotlin
DNAHelix(
    firstColor = Color.Red,
    secondColor = Color.Blue,
    modifier = Modifier
        .fillMaxWidth()
        .height(50.dp)
)
```

![DNA Helix](https://miro.medium.com/v2/resize:fit:1400/1*Q-1BOGvSugts-yUcCqU0rQ.gif)

## White Lines

백색 대신 선을 그라디언트로 사용자 정의하세요:

<div class="content-ad"></div>

```kotlin
DNAHelix(
    firstColor = Color.Red,
    secondColor = Color.Blue,
    lineBrush = { _, _ ->
        SolidColor(Color.White)
    },
    modifier = Modifier
        .fillMaxWidth()
        .height(50.dp)
)
```

![DNA Helix](https://miro.medium.com/v2/resize:fit:1400/1*8fPHvtWuA_leBFH8Ou3WQw.gif)

## 왜곡

수평 이동을 적용하고 왜곡된 나선 구조를 위해 곡률을 조정하세요:

<div class="content-ad"></div>


DNAHelix(
    firstColor = Color.Red,
    secondColor = Color.Blue,
    shifting = 10.dp,
    curvature = 12f,
    modifier = Modifier
        .fillMaxWidth()
        .height(50.dp)
        .rotate(-15f)
)


![Animation of a DNA helix](https://miro.medium.com/v2/resize:fit:1400/1*JA8o47XdCa246_C-mXtBpQ.gif)

아래 내용도 마음에 드실지도요 👇

이 글을 읽어주셔서 감사합니다! ❤️ 즐거우시고 가치 있게 느꼈으면 👏 손뼉을 치거나 Kappdev를 팔로우하여 더 매력적인 글들을 읽어보세요 😊


<div class="content-ad"></div>

제 최신 컨텐츠를 받아보려면 이메일 알림 🔔을 구독해보세요.

코딩 즐기세요!

![이미지](/assets/img/2024-07-06-HowtoCreateaDNAHelixAnimationinJetpackCompose_2.png)