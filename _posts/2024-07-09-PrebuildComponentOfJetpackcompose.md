---
title: "2024년에 알아두어야 할 Jetpack Compose 미리 빌드된 컴포넌트 목록"
description: ""
coverImage: "/milky-road.github.io/assets/no-image.jpg"
date: 2024-07-09 10:42
ogImage: 
  url: /milky-road.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Prebuild Component Of Jetpack compose"
link: "https://medium.com/@adityatheprogrammer/prebuild-component-of-jetpack-compose-4705edfc0aa7"
isUpdated: true
---





🌟 안녕하세요! 이쪽은 아디티야입니다.

이전 포스트에서는 젯팩 컴포즈에 대해 어떻게 동작하는지 등에 대해 논의했었습니다. 이를 확인하지 않으셨다면 "젯팩 컴포즈 소개"를 확인해보세요.

이제는 화면의 UI를 구축하는 데 도움이 되는 사전 빌드된 구성 요소에 대해 이야기해보겠습니다. 젯팩 컴포즈에서 제공하는 Text, Row, Column, Card, Box 등과 같은 많은 기본 구성 요소가 있습니다.

기본적으로 UI를 두 부분으로 나눌 수 있습니다:

<div class="content-ad"></div>

- 구성 요소
- 레이아웃

자, 그럼 구성 요소부터 시작해볼게요. 구성 요소란 기본적으로 다양한 UI를 화면에 표시할 수 있도록 도와주는 미리 빌드된 조합 가능한 함수입니다. 예를 들어 Text, Image, Card, Scaffold, Slider, Switch 등이 있죠. 아래에서 몇 가지를 살펴볼게요.

젯팩 컴포즈에서 텍스트를 표시하려면 Text 구성 요소를 사용할 수 있습니다. 아래에 예시가 있습니다.

```kotlin
@Composable
fun SimpleText() {
    Text("Hello World")
}
```

<div class="content-ad"></div>

제트팩 코킷에서 png, jpg, 벡터 리소스 등과 같이 로컬에서 사용 가능한 모든 이미지를 보여주고 싶다면 Image Composable을 사용하면 됩니다. 아래는 이에 대한 예시입니다.

```js
Image(
    painter = painterResource(id = R.drawable.dog),
    contentDescription = stringResource(id = R.string.dog_content_description)
)
```

하지만 서버에 저장된 원격 이미지를 보여주고 싶다면 AsyncImage Composable이나 Coil, Glide와 같은 써드파티 라이브러리를 사용해야 합니다.

AsyncImage의 간단한 예시를 살펴보죠.

<div class="content-ad"></div>


AsyncImage(
    model = "https://example.com/image.jpg",
    contentDescription = "이미지가 포함하고 있는 내용에 대한 번역된 설명"
)


젯팩 컴포즈에서는 Button 컴포저블이 미리 빌드되어 있기 때문에 처음부터 만들 필요가 없습니다. Button을 사용하려면 Button Composable을 사용해야 합니다. Jetpack Compose는 Filled, Filled Tonal, Elevated, Outlined, Text 등 5가지 다른 유형의 버튼을 제공합니다.

Button의 작은 예시를 살펴보겠습니다.


@Composable
fun FilledButtonExample(onClick: () -> Unit) {
    Button(onClick = { onClick() }) {
        Text("Filled")
    }
}


<div class="content-ad"></div>

젯팩 코믹스(Compose)에서는 카드 뷰가 마테리얼 디자인 컨테이너처럼 작용하며, 이를 예시로는 쇼핑 카트의 제품이나 뉴스 애플리케이션의 뉴스 스토리 등을 볼 수 있습니다.

간단한 Card의 예시를 살펴봅시다.

```kotlin
@Composable
fun CardMinimalExample() {
    Card {
        Text(text = "안녕, 세상아!")
    }
}
```

젯팩 코믹스에서 체크박스를 구현할 때는 CheckBox를 사용할 수 있습니다. 체크박스를 사용하면 사용자가 목록에서 하나 이상의 항목을 선택할 수 있습니다. 온라인에서 약관에 동의하는 동안 체크박스를 탭해야 하는 예시를 본 적이 있을 것입니다.

<div class="content-ad"></div>

작은 체크박스 예제를 살펴보겠습니다.

```kotlin
@Composable
fun CheckboxMinimalExample() {
    var checked by remember { mutableStateOf(true) }

    Row(
        verticalAlignment = Alignment.CenterVertically,
    ) {
        Text(
            "최소한의 체크박스"
        )
        Checkbox(
            checked = checked,
            onCheckedChange = { checked = it }
        )
    }

    Text(
        if (checked) "체크박스가 선택됨" else "체크박스가 해제됨"
    )
}
```

젯팩 컴포즈에서 스위치를 사용하려면 스위치 콤포저블을 사용해야 합니다. 일반적으로 스위치는 활성화 또는 비활성화와 같은 것들을 다룰 때 사용됩니다. 예를 들어, 기기에 다크 테마나 라이트 테마와 같은 테마 변경 기능이 있다면, 스위치를 사용하여 다크 테마인 경우 true로 설정한 후 화이트 테마인 경우 false로 설정할 수 있습니다.

작은 스위치 예제를 살펴보겠습니다.

<div class="content-ad"></div>

```kotlin
@Composable
fun SwitchMinimalExample() {
    var checked by remember { mutableStateOf(true) }

    Switch(
        checked = checked,
        onCheckedChnage = {
            checked = it
        }
    )
}
```

많은 구성 요소가 있습니다. 모두 여기에 넣을 수는 없지만, 만약 그것들을 모두 알고 싶다면 구글에서 제공하는 공식 젯팩 코포즈 문서를 방문해 보세요.

그럼 젯팩 코포즈의 레이아웃부터 시작해 보죠. 레이아웃은 화면 디자인의 구조를 만드는 데 도움이 되며, 기본적으로 화면에 구성 요소(이전에 논의한 것)를 배열하는 데 사용하는 레이아웃을 사용해야 합니다. 젯팩 코포즈에서 사용할 수 있는 레이아웃이 있습니다.

3가지 표준 레이아웃이 있습니다.


<div class="content-ad"></div>

Jetpack Compose을 사용하면 항목을 수평 방향으로 설정하려면 Row Composable을 사용할 수 있습니다. 사용하기 쉽고 verticalAlignment, horizontalArrangement 속성이 내장되어 있어 항목의 위치를 더 유연하게 조정할 수 있습니다.

아래 예시를 살펴보겠습니다:

```js
@Composable
fun ArtistCardArrangement(artist: Artist) {
    Row(
        verticalAlignment = Alignment.CenterVertically,
        horizontalArrangement = Arrangement.End
    ) {
        ![Artist image](artist.image "Artist image")
        Column { /*...*/ }
    }
}
```

Jetpack Compose을 사용하면 항목을 수직 방향으로 설정하려면 Column Composable을 사용할 수 있습니다. 사용하기 쉽고 verticalArrangement, horizontalAlignment 속성이 내장되어 있어 항목의 위치를 더 유연하게 조정할 수 있습니다.

<div class="content-ad"></div>

아래 예시를 살펴보세요:

```kotlin
@Composable
fun ArtistCardColumn() {
    Column {
        Text("알프레드 시슬리")
        Text("3분 전")
    }
}
```

젯팩 컴포즈에서는 다른 항목 위에 다른 항목을 배치하거나 특정 위치에 특정 항목을 제공하려는 경우에 Box Composable을 사용할 수 있습니다. 기본적으로 Box에는 Column과 Row에 있듯이 특정 속성이 없습니다.

아래 예시를 살펴보겠습니다:

<div class="content-ad"></div>

**Tarot Expert's Reading:**

안녕하세요! 🌟

저희의 주요 레이아웃 빌더인 Jetpack Compose의 표준 레이아웃 빌더가 모두 존재합니다. 이제는 고급 레이아웃 빌더 몇 가지를 살펴볼 차례입니다. 그러므로 계속 주목해 주세요! 만약 지금 살펴보고 싶다면, 공식 Android Jetpack Compose 문서를 아래에 첨부해드리겠습니다. ✨

---

**참고 문서:**
[Android Jetpack Compose 문서](https://developer.android.com/jetpack/compose)
