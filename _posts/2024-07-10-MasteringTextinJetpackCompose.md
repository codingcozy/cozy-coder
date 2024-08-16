---
title: "Jetpack Compose로 텍스트 마스터하기 2024 최신 가이드"
description: ""
coverImage: "/assets/img/2024-07-10-MasteringTextinJetpackCompose_0.png"
date: 2024-07-10 01:13
ogImage: 
  url: /assets/img/2024-07-10-MasteringTextinJetpackCompose_0.png
tag: Tech
originalTitle: "Mastering Text in Jetpack Compose"
link: "https://medium.com/proandroiddev/mastering-text-in-jetpack-compose-e99dbf031aed"
isUpdated: true
---





## 간단한 문자열에서 풍부한 HTML로

![Jetpack Compose](/assets/img/2024-07-10-MasteringTextinJetpackCompose_0.png)

젯팩 콤포즈는 아름다운 사용자 인터페이스를 구축하기 위한 강력하고 표현력있는 도구상으로 자리 잡았습니다. 조립성과 상태 관리에 초점을 맞춘 이 도구상은 개발 프로세스를 간소화하였고, 우리에게 화소 완벽한 경험을 만들 수 있는 기회를 줬습니다. 그러나, 개발자들이 갈망한 유연성이 부족했던 한 가지 분야는 텍스트 조작이었습니다. 기본 텍스트를 처리하는 것은 항상 가능했지만, HTML 문자열을 렌더링할 수 없는 제한으로 인해 실제로 동적이고 시각적으로 매력적인 콘텐츠의 잠재력이 제한되었습니다.

오늘부로 이 모든 것이 바뀝니다! 마침내 콤포즈에서 직접 HTML을 렌더링할 수 있습니다. 이 보다는 간단한 추가가 우리를 지루한 수동 포맷팅이나 복잡한 텍스트 조작 라이브러리와 씨름하는 것에서 해방시켜 줍니다. HTML을 사용하면 사용자를 위한 풍부하고 매력적인 경험을 만들기 위해 잘 확립된 표준의 파워와 익숙함을 활용할 수 있습니다.

<div class="content-ad"></div>

이 기사는 HTML 문자열 지원이 Jetpack Compose에서 개발자를 어떻게 지원하는지 탐구합니다. 이 새로운 기능의 기술적 세부 사항을 파헤치고, 실제 예제를 통해 그 잠재력을 소개하며, Jetpack Compose에서 UI 개발에 대한 더 넓은 함의를 논의하겠습니다.

## 현재 상황 분석

Jetpack Compose는 이미 문자열 관리를 위한 견고한 기반을 제공했습니다. Text composable은 AnnotatedString을 함께 사용하여 텍스트 외관을 상당한 수준으로 제어할 수 있었습니다. 우리는 전체 문자열이나 특정 부분에 스타일을 정의할 수 있었죠. 예를 들어, 우리는 문장 내에서 중요한 용어를 강조하기 위해 문자열을 세 부분으로 나누어 구분할 수 있었습니다: 시작 부분, 강조된 단어 (굵게 스타일링된) 및 나머지 텍스트입니다.

```js
  Column(Modifier.padding(12.dp)) {
        val annotatedString = buildAnnotatedString {
            withStyle(style = SpanStyle(color = Color.Black)) {
                append("This is a ")
            }
            withStyle(style = SpanStyle(color = Color.Red, fontWeight = FontWeight.Bold)) {
                append("styled")
            }
            append(" text.")
        }

        Text(text = annotatedString)
    }
```

<div class="content-ad"></div>

![image](/assets/img/2024-07-10-MasteringTextinJetpackCompose_1.png)

We can easily extract strings like this:

```js
<string name="part1">"This is a "</string>
<string name="part2_styled">styled</string>
<string name="part3">" text."</string>
```

Although the AnnotatedString method worked well for simple text formatting, it had limitations as project complexity grew.

<div class="content-ad"></div>

이 접근 방식에는 세 가지 주요 단점이 있습니다:

- 수동 문자 조작과 오류 발생 가능성: 다양한 스타일이 필요한 구절로 이루어진 문단을 상상해보세요. 문자열을 정교하게 여러 부분으로 나누고, 각각에 스타일을 적용한 후 다시 조합해야 할 것입니다. 이 과정은 귀찮을 뿐만 아니라 오류 발생 가능성도 높습니다. 잘못된 스타일 정의 하나만으로 전체 서식이 엉망이 될 수 있습니다. 복잡한 문자열 조작을 통한 대규모 코드 블록의 유지는 쉽게 유지관리 악몽으로 변할 수 있습니다.
- 가독성 문제: 스타일이 지정된 섹션이 늘어날수록 코드가 읽기 어려워질 수 있습니다. 여러 곳에 흩어진 스타일이 지정된 단어로 가득 찬 긴 문장을 상상해보세요. 포맷팅 뒤에 숨겨진 논리는 문자열 분할과 스타일 적용의 바다 속에서 사라집니다. 이는 코드의 명료성을 떨어뜨리고 다른 개발자와의 협업을 어렵게 만듭니다.
- 번역 문제: 이 방법은 국제화에 큰 장애물을 제공합니다. 여러 부분으로 나뉜 문자열을 번역하면 로캘라이제이션 팀에게 부담이 될 수 있습니다. 단어의 순서와 구조가 다른 언어에서 급격하게 변경될 수 있습니다. 원래 형식화된 문자열 조각을 나타내는 여러 개의 문자열 자원을 관리하는 것은 매우 비효율적입니다.

## HTML 문자열로 작업하기

이러한 제한 사항을 해결하기 위한 가능한 해결책은 문자열 자원 내에서 HTML 문자열을 사용하는 것입니다. 이 방법은 문자열 리소스 내에 HTML 코드를 포함하는 방식입니다.

<div class="content-ad"></div>


    <string name="completed_text">This is a **styled** text.</string>


그러나 이 방법은 Jetpack Compose와 직접 통합이 부족했습니다. UI 내에서 이러한 HTML 문자열을 구문 분석하려면 외부 라이브러리나 사용자 정의 논리가 필요하여 개발 프로세스에 복잡성이 추가되었습니다.

## fromHtml 소개

다행히도, Jetpack Compose foundation API의 새 버전은 HTML 렌더링에 대한 네이티브 솔루션을 제공합니다. 이 섹션에서는 구현 세부 정보를 탐색하고이 기능을 Compose 애플리케이션에 통합하는 방법을 보여줄 것입니다.


<div class="content-ad"></div>

이 글을 쓰는 시점에서 foundation API의 최신 베타 버전은 1.7.0-beta04입니다. 항상 최신 정보는 공식 문서를 참고해주세요.

```js
[versions]
...
foundation = "1.7.0-beta04"

[libraries]
...
androidx-foundation = { group = "androidx.compose.foundation", name = "foundation", version.ref = "foundation" }
```

최신 foundation 라이브러리 종속성이 추가되었으므로 이제 내장 HTML 구문 분석 기능을 활용할 수 있습니다. 이를 통해 사용자 정의 논리나 외부 라이브러리가 필요 없어졌습니다. 이 동작을 실습해보겠습니다:

```js
Text(
      text = AnnotatedString.fromHtml(
        htmlString =
          stringResource(id = R.string.completed_text)
        )
     )
```

<div class="content-ad"></div>

이 코드 스니펫은 Compose 구성 요소 내에서 HTML 렌더링을 통합하는 방법을 보여줍니다. 우리는 AnnotatedString.fromHtml 확장 기능을 활용하여 원하는 HTML 문자열을 전달합니다. 이 함수는 HTML 콘텐츠를 구문 분석하고 해당하는 스타일이 적용된 AnnotatedString을 자동으로 생성합니다.

![이미지](/assets/img/2024-07-10-MasteringTextinJetpackCompose_2.png)

이 접근 방식은 우리가 목표로 했던 스타일이 적용된 텍스트를 얻으면서 더 깔끔한 구현을 달성합니다.

## HTML 문자열 내의 하이퍼링크 지원

<div class="content-ad"></div>

AnnotatedString.fromHtml의 아름다움은 기본적인 스타일링을 넘어 확장됩니다. 하이퍼링크에 대한 내장 지원도 제공합니다. HTML 문자열 내에서 링크를 정의하면 Compose에서 자동으로 처리해줍니다.

Markdown 형식 태그로 img 태그 변경:

```js
<string name="completed_text"><![CDATA[This is a [link](https://medium.com/@stefanoq21).]]></string>
```

```js
Text(
    text = AnnotatedString.fromHtml(
      htmlString =
      stringResource(id = R.string.completed_text),
      linkStyles = TextLinkStyles(
        style = SpanStyle(
          textDecoration = TextDecoration.Underline,
          fontStyle = FontStyle.Italic
        )
      )
    )
)
```

이 HTML 문자열을 fromHtml을 사용하여 렌더링할 때 "link"라는 단어는 자동으로 클릭 가능한 요소로 처리됩니다. 사용자는 웹 브라우저에서 표준 하이퍼링크와 동일하게 상호작용할 수 있습니다.

<div class="content-ad"></div>


![image](https://miro.medium.com/v2/resize:fit:1400/1*JhhZcgi6X7jTmBtZa9TUYw.gif)

fromHtml에서는 링크에 대한 기본 동작을 제공하지만, linkInteractionListener 매개변수를 사용하여이 기능을 더 맞춤화할 수 있습니다. 이를 사용하면 Compose UI 내에서 링크를 클릭했을 때 사용자 정의 작업을 정의할 수 있습니다.
예를 들어, 링크를 사용자 정의 탭에서 열 수 있습니다.

```js
Text(
  text = AnnotatedString.fromHtml(
    htmlString =
    stringResource(id = R.string.completed_text),
    linkStyles = TextLinkStyles(
      style = SpanStyle(
        textDecoration = TextDecoration.Underline,
        fontStyle = FontStyle.Italic
      )
    ),
    linkInteractionListener = {
      val urlString = (it as LinkAnnotation.Url).url
      val builder: CustomTabsIntent.Builder = CustomTabsIntent.Builder()
      val customTabsIntent: CustomTabsIntent = builder.build()
      customTabsIntent.launchUrl(context, Uri.parse(urlString))
    }
  )
)
```

![image](https://miro.medium.com/v2/resize:fit:1400/1*aA3yAB82ONpXjJTtE_Lm0A.gif)


<div class="content-ad"></div>

# 결론

젯팩 콤포즈에서의 HTML 렌더링은 그저 멋진 새로운 기능만이 아닙니다. 이 기능은 수동 문자 조작이 필요없어져 더 깔끔하고 읽기 쉬운 코드를 작성할 수 있게 됩니다.

또한, 이 기능은 여러 언어로 된 앱 버전을 손쉽게 만들 수 있습니다. 조그마한 텍스트 조각들을 다루는 것을 잊으세요! HTML을 이용하면 모든 것을 한 곳에서 처리할 수 있어서 앱을 진정으로 글로벌하게 만들고 다양한 언어로 유지보수하기 쉽게 만들어줍니다.

요컨대, 젯팩 콤포즈에서의 HTML 렌더링은 아름답고 사용자 친화적인 UI를 빠르고 쉽게 구축할 수 있도록 해줍니다.

<div class="content-ad"></div>

자유롭게 의견을 나누어 주세요. 혹은 원하신다면 LinkedIn에서 저에게 연락해주세요.

좋은 하루 되세요!