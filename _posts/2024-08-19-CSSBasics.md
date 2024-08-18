---
title: "CSS 기초 웹페이지 디자인 시작하기"
description: ""
coverImage: "/assets/img/2024-08-19-CSSBasics_0.png"
date: 2024-08-19 02:52
ogImage: 
  url: /assets/img/2024-08-19-CSSBasics_0.png
tag: Tech
originalTitle: "CSS Basics"
link: "https://dev.to/thekarlesi/css-basics-1261"
isUpdated: false
---


이제 CSS를 사용하여 이 페이지의 모양을 개선하는 방법을 살펴보겠습니다.

head 요소를 살펴보세요:

```js
<head>
    <title>My First Web Page</title>
</head>
```

![이미지](/assets/img/2024-08-19-CSSBasics_0.png)

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

현재 head 요소 내에는 단일 요소가 있습니다. 제목 요소인 제목 요소입니다.

제목 다음에는 스타일이라는 다른 요소를 추가할 것입니다. 여기에 우리가 CSS 코드를 작성할 것입니다.

그래서 스타일 태그 사이에 CSS 규칙을 작성할 것입니다.

```js
<!DOCTYPE html>
<html>
    <head>
        <title>나의 첫 웹 페이지</title>
        <style>
            /* 여기에 CSS 규칙을 작성하세요 */
        </style>
    </head>
    <body>
        <img src="Images/altumcode.jpg">
        <p>@KarlgustaAnnoh</p>
        <p>HTML을 가르쳐드리는 것을 즐겨합니다!</p>
    </body>
</html>
```

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

아래는 마크다운 형식으로 변경한 내용입니다.


![CSSBasics_1](/assets/img/2024-08-19-CSSBasics_1.png)

First, we are going to work on this image. Our image is currently too big.

![CSSBasics_2](/assets/img/2024-08-19-CSSBasics_2.png)

So, let`s make it a bit smaller.


<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

VS Code로 돌아와서요. 스타일 태그 안에 다음을 입력할 거에요:

```js
<style>
    img {

    }
</style>
```

여기서 img는 이미지 요소를 참조합니다. 그리고 중괄호 '{}'를 쌍으로 쓰고 그 안에 하나 이상의 CSS 선언을 작성합니다. 각 선언에는 속성과 값이 포함되어 있습니다.

여기서 우리는 너비 속성을, 예를 들어, 100px로 설정할 수 있어요.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

```js
<style>
    img {
        width: 100px;
    }
</style>
```

따라서 속성을 입력한 후 세미콜론을 추가하고 값이 나옵니다. 그런 다음 여러 CSS 선언을 작성할 수 있도록 세미콜론으로 이 줄을 종료합니다.

```js
width: 100px;
```

이것이 VS Code에서 어떻게 보이는지 살펴보십시오:

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

```js
<!DOCTYPE html>
<html>
    <head>
        <title>나의 첫 웹 페이지</title>
        <style>
            img {
                width: 100px;
            }
        </style>
    </head>
    <body>
        <img src="Images/altumcode.jpg">
        <p>@KarlgustaAnnoh</p>
        <p>나는 HTML을 가르치는 것을 좋아해요!</p>
    </body>
</html>
```

![CSSBasics_3](/assets/img/2024-08-19-CSSBasics_3.png)

이제 변경 내용을 저장합시다.

브라우저로 돌아가보면 이미지가 더 작게 보입니다. 훨씬 좋아졌어요.


<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

<img src="/assets/img/2024-08-19-CSSBasics_4.png" />

하지만 가장자리를 보세요. 가장자리가 너무 날카로워요. 조금 둥글게 만들어 부드럽게 보이도록 하고 싶어요.

우리 규칙으로 돌아가서 - 여기서 우리는 border-radius를 10px로 설정할 거에요.

```css
<style>
    img {
        width: 100px;
        border-radius: 10px;
    }
</style>
```

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

이 속성들을 모두 외우지 않으셔도 괜찮아요. 튜토리얼 동안 여러 번 다뤄볼 거예요. 이 섹션에서는 CSS를 사용하는 느낌을 익히는 게 목적이에요.

```js
<!DOCTYPE html>
<html>
    <head>
        <title>나의 첫 번째 웹 페이지</title>
        <style>
            img {
                width: 100px;
                border-radius: 10px;
            }
        </style>
    </head>
    <body>
        <img src="Images/altumcode.jpg">
        <p>@KarlgustaAnnoh</p>
        <p>HTML을 가르치는 것을 즐겨요!</p>
    </body>
</html>
```

<img src="/assets/img/2024-08-19-CSSBasics_5.png" />

이제 변경 사항을 저장하세요.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

보세요 - 가장자리가 둥글고 부드러워졌어요. 이제, 한 가지 요령을 보여드릴게요.

```js
<!DOCTYPE html>
<html>
    <head>
        <title>나의 첫 웹 페이지</title>
        <style>
            img {
                width: 100px;
                border-radius: 50px;
            }
        </style>
    </head>
    <body>
        <img src="Images/altumcode.jpg">
        <p>@KarlgustaAnnoh</p>
        <p>HTML을 가르쳐드릴 거예요!</p>
    </body>
</html>
```

![CSSBasics_6](/assets/img/2024-08-19-CSSBasics_6.png)

border-radius를 사용하면, 너비의 절반 값으로 라운드된 이미지를 얻을 수 있어요. 그래서, 우리의 너비의 절반인 50px로 border-radius를 설정할 거에요.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

```json
{
    "표": "마크다운 양식으로 테이블를 변경하십시오."
}
```

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

그리고 여기가 결과입니다:

<img src="/assets/img/2024-08-19-CSSBasics_8.png" />

이게 훨씬 나아졌어요.

이제 우리 요소들은 수직으로 쌓여 있어요. 하지만 이미지를 왼쪽으로 밀어 넣고 싶어요.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

그래서, float 속성을 왼쪽으로 설정해 봅시다.

```js
<style>
    img {
        width: 100px;
        border-radius: 50px;
        float: left;
    }
</style>
```

이렇게 하면 이미지 요소가 텍스트 요소의 왼쪽으로 이동합니다. 한 번 확인해 보세요:

<img src="/assets/img/2024-08-19-CSSBasics_9.png" />

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

이미지와 텍스트 사이에 공간을 추가하고 싶다고 하셨네요. 그래서 margin-right 속성을 사용하여 이미지 뒤에 10픽셀의 공간을 설정할 건데요.

```js
<style>
    img {
        width: 100px;
        border-radius: 50px;
        float: left;
        margin-right: 10px;
    }
</style>
```

VS Code에서 이렇게 보입니다:

```js
<!DOCTYPE html>
<html>
    <head>
        <title>My First Web Page</title>
        <style>
            img {
                width: 100px;
                border-radius: 50px;
                float: left;
                margin-right: 10px;
            }
        </style>
    </head>
    <body>
        <img src="Images/altumcode.jpg">
        <p>@KarlgustaAnnoh</p>
        <p>I love to teach you HTML!</p>
    </body>
</html>
```

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

<table>
  <tr>
    <td><img src="/assets/img/2024-08-19-CSSBasics_10.png" /></td>
  </tr>
</table>

변경 사항을 저장하세요.

<table>
  <tr>
    <td><img src="/assets/img/2024-08-19-CSSBasics_11.png" /></td>
  </tr>
</table>

훨씬 나아졌네요. 이제 사용자 이름을 굵게 표시해 봅시다. 이 과정을 한 번 더 반복하겠습니다.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

이번에는 단락 요소에 규칙을 적용할 거에요. 폰트 두껍기 속성을 설정해주세요.

```js
<style>
    img {
        width: 100px;
        border-radius: 50px;
        float: left;
        margin-right: 10px;
    }

    p {
        font-weight: bold;
    }
</style>
```

여기는 VS Code에서 코드입니다:

```js
<!DOCTYPE html>
<html>
    <head>
        <title>나의 첫 웹 페이지</title>
        <style>
            img {
                width: 100px;
                border-radius: 50px;
                float: left;
                margin-right: 10px;
            }

            p {
                font-weight: bold;
            }
        </style>
    </head>
    <body>
        <img src="Images/altumcode.jpg">
        <p>@KarlgustaAnnoh</p>
        <p>나는 HTML을 가르치는 것을 좋아해요!</p>
    </body>
<html>
```

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

이미지를 보세요.

이미지를 확인하셨나요? 두 텍스트 요소 모두 굵게 표시되어 있습니다. 하지만, 만약 이 스타일을 사용자 이름에만 적용하고 싶다면 어떻게 하면 좋을까요?

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

그래서, 이 두 개의 단락 요소를 분리해야 합니다.

```js
<body>
    <p>@KarlgustaAnnoh</p>
    <p>I love to teach you HTML!</p>
</body>
```

그래서, 첫 번째 요소에 class라는 속성을 추가할 거에요.

```js
<body>
    <p class="">@KarlgustaAnnoh</p>
    <p>I love to teach you HTML!</p>
</body>
```

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

Class는 분류(Classification)의 줄임말이에요. 이 속성을 사용하여 이 요소를 다른 클래스나 카테고리 안에 넣을 수 있어요. 마치 슈퍼마켓에 있는 제품들처럼 말이죠.

스타는 각자 카테고리에 속해 있어요, 맞죠?

그래서, 이 문단 요소를 'username'이라는 클래스나 카테고리 안에 넣을 거에요:

```js
<body>
    <p class="username">@KarlgustaAnnoh</p>
    <p>HTML을 가르치는 것을 좋아해요!</p>
</body>
```

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

그럼 제가 이 규칙을 변경할 거에요:

```js
<style>
    p.username {
        font-weight: bold;
    }
</style>
```

현재 이 규칙은 모든 단락 요소에 적용되지만, 이제는 username 클래스를 가진 단락 요소에만 적용되도록 바꿀 거에요.

따라서, p 뒤에 p.username을 써주세요.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>


    p.username {
        font-weight: bold;
    }


이제, 우리는 p를 제거하고 이 규칙이닉명이라는 클래스를 가진 모든 요소에 적용되도록 할 수 있습니다. 이 요소들이 단락 요소이든 다른 유형의 요소이든 관계없이 적용됩니다.


    .username {
        font-weight: bold;
    }


이제 변경 사항을 저장하세요. 


<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

다음은 VS Code에서의 모습입니다.

```js
<!DOCTYPE html>
<html>
    <head>
        <title>나의 첫 번째 웹 페이지</title>
        <style>
            img{
                width: 100px;
                border-radius: 50px;
                float: left;
                margin-right: 10px;
            }

            .username{
                font-weight: bold;
            }
        </style>
    </head>
    <body>
        <img src="Images/altumcode.jpg">
        <p class="username">@KarlgustaAnnoh</p>
        <p>HTML을 가르치는 것을 좋아해요!</p>
    </body>
</html>
```

<img src="/assets/img/2024-08-19-CSSBasics_14.png" />

이제 한 번 살펴보세요.

<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>


![이미지](/assets/img/2024-08-19-CSSBasics_15.png)

그 훨씬 더 낫네요.

CSS가 작동 중입니다. 보시다시피, CSS는 HTML과 다른 구문을 가지고 있어요.

이 튜토리얼의 나머지 부분에서는 이 두 언어를 자세히 배우게 될 거에요.


<!-- cozy-coder - 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1107185301"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

다음에는 Prettier를 사용하여 코드를 어떻게 포맷하는지 보여드릴게요.

다음 시간에 뵙겠습니다!

추신: 🚀 웹 개발에서 경력을 시작하고 싶나요? HTML, CSS, JavaScript, React 등을 숙달할 수 있는 강의에 참여하세요! 실전 프로젝트를 만들어보고, 업계 전문가로부터 배우며, 지지력 있는 커뮤니티와 소통하세요. 지금 등록하세요!