---
title: "2024 CSS 해킹 단 두 줄의 코드로 라이트 및 다크 모드 즉시 추가하는 방법"
description: ""
coverImage: "/assets/img/2024-08-17-2024CSSHackAddLightandDarkModeInstantlywithJust2LinesofCode_0.png"
date: 2024-08-17 00:00
ogImage: 
  url: /assets/img/2024-08-17-2024CSSHackAddLightandDarkModeInstantlywithJust2LinesofCode_0.png
tag: Tech
originalTitle: "2024 CSS Hack Add Light and Dark Mode Instantly with Just 2 Lines of Code"
link: "https://medium.com/@akashblog/2024-css-hack-add-light-and-dark-mode-instantly-with-just-2-lines-of-code-8f722b56ab45"
isUpdated: false
---


<img src="/assets/img/2024-08-17-2024CSSHackAddLightandDarkModeInstantlywithJust2LinesofCode_0.png" />

웹 개발에서는 다양한 장치와 테마 간에 원활한 사용자 경험을 제공하는 것이 매우 중요합니다. 현대 웹 애플리케이션에서 가장 요청이 많은 기능 중 하나는 밝은 모드와 어두운 모드 간에 전환할 수 있는 기능입니다. 기존에는 이 기능을 구현하는 데 복잡한 CSS 미디어 쿼리, 여러 스타일시트 및 많은 수동 조정이 필요했습니다. 그러나 CSS 2줄로도 이를 달성할 수 있다고 말씀드리면 어떨까요? 2024년 5월에 소개된 게임 체인징 기능인 light-dark() 함수를 소개합니다. 이 함수를 통해 프로세스를 혁신적으로 간단하게 만들 수 있습니다.

# 밝은 모드와 어두운 모드의 진화

# 예전 방식: 복잡하고 지루한 과정

<div class="content-ad"></div>

개발자들은 과거에 라이트 모드와 다크 모드를 구현하기 위해 여러 단계를 거쳐야 했습니다:

- 별도의 스타일시트: 라이트 모드와 다크 모드를 위한 다른 스타일시트를 관리하는 것이 코드베이스에 복잡성을 추가했습니다.
- 미디어 쿼리: 사용자의 시스템 테마를 감지하기 위해 미디어 쿼리를 작성해야 했으며, 종종 번거롭고 중복된 코드를 유발했습니다.
- 요소별 조정: 페이지의 각 요소가 올바르게 적응하도록 보장하는 것은 노동 집약적이었으며, 각 모드에 대해 색상, 배경 및 기타 UI 요소를 수동으로 조정해야 했습니다.

이러한 과정은 버그 발생 확률을 높이는 것뿐만 아니라 코드를 유지 보수하기 어렵게 만들었습니다. 더욱이, 웹 응용 프로그램이 복잡해지면서 여러 테마를 유지하는 작업이 번거로운 과제가 되었으며, 특히 다양한 장치 및 화면 크기로 확장할 때 더욱 어렵게되었습니다.

# 새로운 방법: 라이트다크() 함수로 간편하게 처리하기

<div class="content-ad"></div>

라이트-다크() 함수의 도입으로 그런 귀찮음은 먼 옛날 얘기가 되었습니다. 이 새로운 CSS 함수를 사용하면 한 줄에 라이트 및 다크 모드용 색상 값을 정의할 수 있습니다. 브라우저는 사용자의 시스템 설정에 따라 적절한 테마를 자동으로 적용합니다.

이 새로운 기능의 강점을 보여주는 빠른 예시가 있습니다.

# 라이트 모드와 다크 모드 구현: 실용적인 예시

# HTML

<div class="content-ad"></div>

먼저, 간단한 HTML 구조로 시작해봅시다:

```js
<p>라이트 모드에서 확인한 후 OS의 다크 모드로 전환하세요</p>
```

# CSS

이제, 이 놀라운 효과는 단 두 줄의 CSS로 구현됩니다:

<div class="content-ad"></div>


:root {
  color-scheme: light dark;
}

body {
  background-color: light-dark(#fff, #444);
  color: light-dark(#444, #fff);
}


# 코드 분석

이 코드를 분석하여 작동 방식을 이해해 봅시다:

- color-scheme 속성: 이 속성은 :root 요소(즉 HTML 문서 자체)에 설정됩니다. 브라우저에 light 및 dark 테마를 인식하도록 지시합니다. light dark를 지정함으로써 사용자의 시스템 설정에 기반하여 이 테마들 간을 전환할 수 있게 됩니다.
- light-dark() 함수: 이 함수는 두 개의 인수를 사용합니다. 첫 번째는 라이트 모드를, 두 번째는 다크 모드를 나타냅니다. 예시에서는 다음과 같습니다:

<div class="content-ad"></div>

이 최소한의 설정은 사용자의 OS 설정에 반응하여 웹 사이트가 자동으로 테마를 조정하도록 보장합니다. 이에 추가적인 노력이 필요하지 않습니다.

# 기능 확장

light-dark() 함수의 놀라운 점은 그 유연성에 있습니다. body 요소에만 국한되지 않고 페이지의 모든 요소에 적용할 수 있어 웹 사이트 전체에 일관된 테마를 제공할 수 있습니다.

다음은 이 기능을 확장하는 예시입니다:

<div class="content-ad"></div>

```js
헤더와 푸터는 밝은 모드에서는 더 밝은 배경과 더 어두운 텍스트가 적용됩니다. 어두운 모드에서는 그 반대가 적용됩니다.

버튼 요소도 비슷하게 조정되어 일관된 사용자 경험을 제공합니다.

# light-dark() 함수 사용의 장점
```

<div class="content-ad"></div>

# 1. 효율성과 간결함

light-dark() 함수를 사용하는 가장 중요한 장점 중 하나는 그 간결성입니다. 이제 더 이상 라이트 모드와 다크 모드를 위한 별도 스타일 시트를 작성하고 유지할 필요가 없어졌습니다. 이로 인해 작성해야 하는 코드 양이 줄어들고 CSS가 더 깔끔하고 유지보수가 쉬워졌습니다.

# 2. 향상된 사용자 경험

이 기능을 사용하면 웹 사이트가 사용자의 선호 테마와 자동으로 일치하여 연속적인 경험을 제공할 수 있습니다. 예를 들어 다크 모드를 선호하는 사용자는 밤에 사이트를 방문할 때 눈부시게 흰 화면을 바라볼 필요가 없습니다.

<div class="content-ad"></div>

# 3. 크로스 브라우저 호환성

color-scheme 속성과 light-dark() 함수는 모든 최신 브라우저에서 지원되므로 모든 사용자에게 일관된 경험을 제공합니다. 더 많은 사용자가 테마 전환 기능을 요구할수록 크로스 브라우저 호환성을 보장하는 것이 중요합니다.

# 4. 웹사이트 미래를 준비하는 방법

웹 표준이 발전함에 따라 light-dark() 함수와 같은 기능이 더욱 중요해질 것입니다. 이러한 기능을 지금 채택함으로써 현재 사용자 경험을 향상시키는 것뿐만 아니라 웹사이트를 미래를 대비하는 것입니다.

<div class="content-ad"></div>

# 전체 예시: 모두 결합해보기

전체 그림을 이해하기 위해 여기에 자체적으로 시도해볼 수 있는 전체 예제가 있습니다:

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>라이트 모드 및 다크 모드 예제</title>
    <style>
        :root {
            /* 라이트 모드와 다크 모드 전환 활성화 */
            color-scheme: light dark;

            /* 라이트 모드 색상 */
            --light-bg: #e0f7fa; /* 청록색 라이트 */
            --light-color: #004d40; /* 틸 다크 */
            --light-code: #d32f2f; /* 빨강 다크 */

            /* 다크 모드 색상 */
            --dark-bg: #263238; /* 청회색 다크 */
            --dark-color: #e0f7fa; /* 청록색 라이트 */
            --dark-code: #ffab00; /* 호박색 */
        }

        /* 테마에 따라 배경색과 텍스트 색상 적용 */
        * {
            background-color: light-dark(var(--light-bg), var(--dark-bg));
            color: light-dark(var(--light-color), var(--dark-color));
        }

        /* 코드 요소에 대한 스타일 */
        code {
            color: light-dark(var(--light-code), var(--dark-code));
        }

        /* 강제 라이트 테마 */
        .light {
            color-scheme: light;
        }

        /* 강제 다크 테마 */
        .dark {
            color-scheme: dark;
        }

        /* 섹션에 대한 일반적인 스타일링 */
        section {
            padding: 1rem;
            border: 2px solid light-dark(#00796b, #ff8f00);
            margin-bottom: 1rem;
            border-radius: 10px;
            box-shadow: 0 4px 8px light-dark(#b2dfdb, #000000);
        }

        h1 {
            text-align: center;
            margin-bottom: 2rem;
            font-family: 'Arial', sans-serif;
            text-transform: uppercase;
        }

        h2 {
            margin-bottom: 0.5rem;
        }

        p {
            font-size: 1.1rem;
            line-height: 1.5;
        }
    </style>
</head>
<body>
    <h1><code>light-dark()</code> CSS 함수 데모</h1>

    <section>
        <h2>자동 테마</h2>
        <p>이 섹션은 사용자의 시스템 테마에 자동으로 적응합니다.</p>
    </section>

    <section class="light">
        <h2>강제 라이트 테마</h2>
        <p>이 섹션은 <code>color-scheme: light;</code>를 사용하여 라이트 모드로 강제 표시됩니다.</p>
    </section>

    <section class="dark">
        <h2>강제 다크 테마</h2>
        <p>이 섹션은 <code>color-scheme: dark;</code>를 사용하여 다크 모드로 강제 표시됩니다.</p>
    </section>
</body>
</html>
```

# 개선 사항 설명:

<div class="content-ad"></div>

- 생동감 있는 색상: 라이트 모드는 밝은 사이언 라이트와 틸 다크를 사용하여 신선하고 공기가 새로운 느낌을 줍니다. 반면 다크 모드는 깊은 블루 그레이 다크 배경과 대조적인 앰버를 사용하여 화려한 효과를 연출합니다.
- 테두리와 그림자: 두껍고 더 다채로운 테두리 및 섬세한 상자 그림자가 추가되어 섹션 정의를 높이고 내용을 빛나게 합니다.
- 타이포그래피: 대문자 헤더와 개선된 텍스트 스타일링으로 내용이 시각적으로 더 매력적으로 표현됩니다.

![이미지](/assets/img/2024-08-17-2024CSSHackAddLightandDarkModeInstantlywithJust2LinesofCode_1.png)

더 많은 정보 및 원본 참조를 보려면 MDN 문서에서 light-dark() CSS 함수를 확인해주세요.

# 결론: 현대 웹 개발에 반드시 필요한 도구

<div class="content-ad"></div>

light-dark() 함수는 CSS에 혁명적인 기능으로, 웹 사이트의 라이트 모드와 다크 모드를 간편하게 구현할 수 있습니다. 이 함수는 코드의 복잡성을 줄이고 사용자 경험을 향상시키며, 웹 사이트를 미래에 대비할 수 있도록 도와줍니다. 작은 개인 프로젝트나 대규모 웹 애플리케이션을 개발하더라도, 이 기능은 개발 툴킷에서 가치 있는 도구입니다.

더 많은 팁과 트릭을 위해 팔로우 해주세요. 웹 개발 스킬을 업그레이드하고 코드를 깔끔하게 유지하는 데 도움이 될 것입니다. 즐거운 코딩하세요! 다양한 색상 조합으로 시각적으로 멋진 사용자 친화적인 웹 사이트를 만들어보는 걸 잊지 마세요!