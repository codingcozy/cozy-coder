---
title: "프론트엔드 개발 면접 단골 질문 trycatch 문"
description: ""
coverImage: "/assets/img/2024-07-29-BecauseofaQuestionAbouttrycatchIFailedMyInterview_0.png"
date: 2024-07-29 14:05
ogImage: 
  url: /assets/img/2024-07-29-BecauseofaQuestionAbouttrycatchIFailedMyInterview_0.png
tag: Tech
originalTitle: "Because of a Question About trycatch, I Failed My Interview"
link: "https://medium.com/stackademic/because-of-a-question-about-try-catch-i-failed-my-interview-2cea0225820c"
isUpdated: true
---





![image](/assets/img/2024-07-29-BecauseofaQuestionAbouttrycatchIFailedMyInterview_0.png)

try...catch에 관해 이야기할 때는 매우 친근한 느낌을 받습니다. 코드 블록에서 에러를 잡는 데 흔히 사용되며, 자주 사용합니다.

그러나 지식이 부족하여 면접 중에 간단한 개념에 걸렸습니다: try...catch는 동기 코드 블록에서만 에러를 잡을 수 있다는 것이었습니다.

질문은 다음 코드에 문제가 있습니까? 그렇다면 어떻게 수정해야 합니까?

<div class="content-ad"></div>

```js
try {
  setTimeout(() => {
    throw new Error('err')
  }, 200);
} catch (err) {
  console.log(err);
}

try {
  Promise.resolve().then(() => {
    throw new Error('err')
  })
} catch (err) {
  console.log(err);
}
```

요컨데, 전에는 알지 못했던 부분이라서 무슨 일이 일어나고 있는지 알 수 없었어요: 비동기 코드에서 try...catch는 비동기 블록이기 때문에 에러를 잡을 수 없어요. 그래서 이 질문을 보고 당황했어요.

평소에 우리가 코드를 작성할 때, try...catch로 에러를 잡는 것이 정상적이지 않나요? 그래서 제가 모른다고 대답하고 에러가 없다고 생각했어요... 면접관께서 저를 무력하게 쳐다보시며 나중에 찾아보라고 제안하신 후 끝이 났어요.