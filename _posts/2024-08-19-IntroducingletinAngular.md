---
title: "Angular에서의 let 소개 어떤 동작을 하는 걸까요"
description: ""
coverImage: "/assets/img/2024-08-19-IntroducingletinAngular_0.png"
date: 2024-08-19 02:27
ogImage: 
  url: /assets/img/2024-08-19-IntroducingletinAngular_0.png
tag: Tech
originalTitle: "Introducing let in Angular"
link: "https://medium.com/angular-blog/introducing-let-in-angular-686f9f383f0f"
isUpdated: false
---


작성자: Mark Thompson, Kristiyan Kostadinov

![이미지](/assets/img/2024-08-19-IntroducingletinAngular_0.png)

Angular의 내장 템플릿 구문을 확장하여 개발자들에게 구성 요소 템플릿 내에서 변수를 더 잘 정의할 수 있는 방법을 제공할 예정입니다.

새로운 @let 구문을 사용하면 개발자들은 변수를 정의하고 템플릿 전체에서 해당 값 재사용이 가능합니다. 또한 이 새로운 구문은 커뮤니티에서 가장 많은 추천을 받은 문제 중 하나를 해결할 수 있습니다.

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

위의 내용을 번역해 드리겠습니다.

템플릿에서 @let을 사용하여 개발자 경험을 향상시킬 수 있는 방법을 알아보세요.

# 재사용 가능한 템플릿 변수 정의하는 새로운 방법

Angular의 템플릿 구문은 복잡한 JavaScript 표현식을 지원하지만 표현식의 결과를 저장하고 템플릿 전체에서 재사용할 수 있는 원활한 방법이 없었습니다. 개발자들은 지시문 및 기타 해결책을 사용하여이 목표를 달성하기 위해 비효율적인 해결책을 찾아야 했습니다.

새로운 구문을 살펴볼까요?

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


@let name = value; // 여기서 value는 유효한 Angular 표현식입니다


이제 템플릿 내에서 변수를 정의하고 Angular의 템플릿 구문 규칙과 규칙에 따라 사용할 수 있습니다. 예를 들어:


@let name = 'Frodo';
<h1>Dashboard for {name}</h1>
Hello, {name}


다음은 @let이 템플릿에서 사용되는 방법의 또 다른 예시입니다:

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
<!-- 요소를 참조하는 템플릿 변수와 함께 사용 -->
<input #name>

@let greeting = 'Hello ' + name.value;

<!-- Async 파이프와 함께 사용 -->
@let user = user$ | async;
```

새로운 @let 선언을 사용할 때 주의해야 할 점은 해당 선언이 현재 뷰 및 하위 뷰에 스코프가 지정되지만 부모 또는 형제 뷰에서 액세스할 수 없다는 것입니다.

```js
@let topLevel = value;

@if (condition) {
  @let nested = value;
}

<div *ngIf="condition">
  @let nestedNgIf = value;
</div>

<!-- 유효함 -->
{topLevel}
<!-- 에러, @if에서 호이스트되지 않음 -->

{nested}
<!-- 에러, *ngIf에서 호이스트되지 않음 -->

{nestedNgIf}
```

@let 선언은 읽기 전용이며 재할당할 수 없습니다. 그 값은 각 변경 감지마다 다시 계산될 것이며(예: async 파이프가 변경될 때), 직접 작성하려고 시도하면 타입 체크 오류가 발생합니다. 


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
@let value = 10;

<!-- 오류: 'value'는 할당할 수 없습니다 -->
<button (click)="value = value + 1">값 변경</button>
```

# 구문 정의

@let 구문의 세부 사항을 탐색해 봅시다. 새로운 구문의 공식적인 정의는 다음과 같습니다:

- @let 키워드.
- 새 줄을 제외한 하나 이상의 공백.
- 유효한 JavaScript 이름과 공백이 없는 한 개 이상.
- = 기호와 공백이 없는 한 개 이상.
- Angular 표현식으로 이어지며, 여러 줄일 수 있습니다.
- ; 기호로 종료됩니다.

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

# @즐거운 시간 시작하기

우리는 이 새로운 기능을 출시함으로써 정말 기쁩니다. 여러분의 개발자 경험을 향상시킬 수 있기를 바라며, 새로운 @let 구문에 대한 여러분의 의견을 듣고 싶어합니다. 이 블로그 게시물에 댓글을 달아주시거나 온라인에서 angular.dev, x.com/angular, youtube.com/@angular에서 저희를 찾아주시면 감사하겠습니다.