---
title: "Angular v18  let으로 템플릿에서 변수를 선언하고 로직 처리하는 새로운 방법 "
description: ""
coverImage: "/assets/img/2024-07-10-Angularv18IntroducingletANewWaytoDeclareVariablesanddoLogicinTemplates_0.png"
date: 2024-07-10 00:37
ogImage: 
  url: /assets/img/2024-07-10-Angularv18IntroducingletANewWaytoDeclareVariablesanddoLogicinTemplates_0.png
tag: Tech
originalTitle: "Angular v18 — Introducing @let: A New Way to Declare Variables and do Logic in Templates 🔥🚀"
link: "https://medium.com/@giorgio.galassi/angular-v18-introducing-let-a-new-way-to-declare-variables-and-do-logic-in-templates-8b3f4d196b23"
isUpdated: true
---





<img src="/assets/img/2024-07-10-Angularv18IntroducingletANewWaytoDeclareVariablesanddoLogicinTemplates_0.png" />

또 한 번 Angular v18의 새로운 기능을 깊이 알아보는 다른 기사입니다.

이 기사에서는 새로운 @let 템플릿 구문을 탐구하고, 이해하고, 그 장단점을 이해하며 어떻게 사용하는지 배울 것입니다. 엉클 벤이 말하듯, 이 강력한 기능은 큰 책임과 함께 제공됩니다.

# @let이란?

<div class="content-ad"></div>

TypeScript 개발자로서, var, const, 그리고 let을 사용하여 변수를 만드는 것에 익숙합니다. 이 상황에서는 @let이 유사하게 작동하지만 한 가지 중요한 차이가 있습니다: 이제 템플릿 레벨에서 직접 변수를 생성할 수 있습니다.

# 사용 방법은?

무언가를 이해하기 위해서는 코드를 보는 것이 최고입니다.

기본 예제를 살펴보겠습니다:

<div class="content-ad"></div>

```js
@let name = 'Giorgio';

<label>{ name }</label>
```  

당신이 기대한 대로, 변수의 내용이 정확하게 렌더링됩니다:

```js
<label>Giorgio</label>
```

이렇게 하면 코드를 처리하는 새로운 방법이 가능해지며, 템플릿 자체 내에서 일부 로직을 직접 처리할 수 있게 됩니다. @let을 사용하면 변수를 선언하고 템플릿 전체에서 사용할 수 있습니다.

<div class="content-ad"></div>

# 고급 사용법

물론, @let의 힘은 여기서 끝나지 않습니다. 조건부 논리도 수행할 수 있습니다:

```js
@let isAuth = authService.isAuth();
@let username = authService.getUsername();

@let message = isAuth ? '환영합니다, ' + username + ' 님!' : '환영합니다 익명!'

<label>{ message }</label>
```

이 간단한 예제에서도 코드가 얼마나 깔끔해질 수 있는지 이미 보실 수 있습니다. 한걸음 더 나아가 봅시다.

<div class="content-ad"></div>

# 배열 작업

가끔은 배열을 맵핑하거나 필터링하고 싶을 때가 있습니다. 일반적으로는 해당 작업을 수행하는 함수를 작성하여 TypeScript 파일에 출력물을 저장하고, 템플릿에서 사용합니다. 이제 다음과 같이 할 수 있습니다:

```js
@let onlineUsers = users.filter(({ status }) => status === 'online');

@for(user of onlineUsers; track user.id) {
  <label>{ user.name } { user.surname }</label>
}
```

# 비동기 파이프 작업

<div class="content-ad"></div>

만약 Angular와 함께 작업을 했다면, async 파이프를 사용했을 가능성이 높습니다. 여기에 @let을 함께 사용하는 방법이 있습니다:

```js
@let user = user$ | async;

<per>{ user | json }</pre>

@let userPosts = user.posts || [];

@for(post of userPosts; track post.id) {
  <label>{ post.title }</label>
  <p>{ post.content }</p>
} @empty {
  <label>No post to show!</label>
}
```

이 예시에서 @let은 비동기 데이터를 처리하고 사용자 게시물을 반복하는 데 사용되었습니다. 이 코드는 보다 깔끔하며 템플릿 내에서 로직을 유지하여 추가적인 컴포넌트 코드를 줄여줍니다.

다만, @let을 async 파이프와 함께 사용할 때 성능 문제가 발생할 수 있으니 주의해야 합니다.

<div class="content-ad"></div>

위에서와 같이 예제에서 보여진 것처럼, 구독을 처리할 변수를 선언하고 필요한 곳에서 이 변수를 사용하는 방식으로 문제를 해결할 수 있습니다.

이 방법은 단일 구독을 보장하고 여러 번의 재평가를 피할 수 있어 성능을 향상시킬 수 있습니다.

# 결론

Angular v18의 @let 구문은 템플릿 내에서 변수를 선언하고 로직을 직접 처리하는 강력한 방법을 제공합니다. 이는 코드를 간소화하고 가독성을 향상시켜 동적 및 조건부 콘텐츠를 관리하기 쉽게 만들어 줍니다. 그러나 큰 권한이 따르면 큰 책임도 함께 오기 때문에 템플릿을 깨끗하고 유지보수가 용이하도록 유의해서 사용해야 합니다.

<div class="content-ad"></div>

함께 해주셔서 감사합니다. 모든 것이 명확히 이해되길 바라요.
더 많은 기사를 즐겨보고 탐험하고 싶다면 기쁠거에요.

다음 글에서 뵙겠습니다,
G.