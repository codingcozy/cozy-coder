---
title: "고급 JavaScript 인터뷰 질문 Promise 관련 문제 해결 해보세요"
description: ""
coverImage: "/assets/img/2024-08-17-CanYouAnswerThisSeniorLevelJavaScriptPromiseInterviewQuestion_0.png"
date: 2024-08-17 00:44
ogImage: 
  url: /assets/img/2024-08-17-CanYouAnswerThisSeniorLevelJavaScriptPromiseInterviewQuestion_0.png
tag: Tech
originalTitle: "Can You Answer This Senior Level JavaScript Promise Interview Question"
link: "https://medium.com/frontend-canteen/can-you-answer-this-senior-level-javascript-promise-interview-question-69f7b6ffc2e7"
isUpdated: false
---


아래 코드의 결과는 무엇인가요?

```js
const promise = new Promise((resolve, reject) => {
  console.log(1);
  setTimeout(() => {
    console.log("timerStart");
    resolve("success");
    console.log("timerEnd");
  }, 0);
  console.log(2);
});

promise.then((res) => {
  console.log(res);
});

console.log(4);
```

# 분석

Junior JS 코더에게 친근하게 접근하려면 이 질문은 적절하지 않습니다.

<div class="content-ad"></div>

JavaScript의 핵심 기능 중 하나는 비동기 프로그래밍입니다. 인터프리터는 항상 동기 코드를 먼저 실행한 후 비동기 코드를 실행합니다.

예를들어, 다음 코드에서:

```js
console.log('start');

const promise1 = new Promise((resolve, reject) => {
  console.log(1)
  resolve(2)
})
promise1.then(res => {
  console.log(res)
})
console.log('end');
```

promise1.then()은 비동기 함수 호출입니다.

<div class="content-ad"></div>


![Image](/assets/img/2024-08-17-CanYouAnswerThisSeniorLevelJavaScriptPromiseInterviewQuestion_0.png)

So the output is start, 1, end, and 2.

But in JavaScript, asynchronous tasks are also divided into microtasks and macrotasks.

We generally divide vehicles into two categories:


<div class="content-ad"></div>

- 일반 차량
- 화재 차량 및 응급 구조 차량 등 응급 임무를 위한 차량.

혼잡한 교차로를 통과할 때, 우리는 화재 차량과 응급 구조 차량이 먼저 통과하도록 허용할 것입니다. 응급 차량은 다른 차량보다 우선권이 높습니다. 키워드: 우선순위.

![이미지](/assets/img/2024-08-17-CanYouAnswerThisSeniorLevelJavaScriptPromiseInterviewQuestion_1.png)

JavaScript EventLoop에도 우선순위 개념이 있습니다.

<div class="content-ad"></div>

- 더 높은 우선 순위를 가지는 작업은 마이크로태스크라고 합니다. 이에 포함된 것은 Promise, ObjectObserver, MutationObserver, process.nextTick, async/await 등입니다.
- 더 낮은 우선 순위를 가지는 작업은 마크로태스크라고 합니다. 이에 포함된 것은 setTimeout, setInterval 및 XHR 등입니다.

<img src="/assets/img/2024-08-17-CanYouAnswerThisSeniorLevelJavaScriptPromiseInterviewQuestion_2.png" />

다음 코드 스니펫에서:

```js
console.log('start')

setTimeout(() => {
  console.log('setTimeout')
})
Promise.resolve().then(() => {
  console.log('resolve')
})
console.log('end')
```

<div class="content-ad"></div>

setTimeout과 Promise.resolve()가 동시에 완료되었더라도, setTimeout의 코드가 아직 앞서 있더라도 우선순위가 낮기 때문에, 그에 속한 콜백 함수는 나중에 실행됩니다.

![이미지](/assets/img/2024-08-17-CanYouAnswerThisSeniorLevelJavaScriptPromiseInterviewQuestion_3.png)

자, 이제 기본 사항을 설명했으니, 원래 문제로 돌아가 봅시다.

이 문제를 해결하기 위해 우리는 단 세 가지 단계만 수행하면 됩니다:

<div class="content-ad"></div>

- 동기화 코드를 찾아보세요.
- 마이크로태스크 코드를 찾아보세요.
- 매크로태스크 코드를 찾아보세요.

첫 번째로 동기화 코드를 실행합니다:

첫 번째로 동기화 코드를 실행합니다:

![이미지](/assets/img/2024-08-17-CanYouAnswerThisSeniorLevelJavaScriptPromiseInterviewQuestion_4.png)

<div class="content-ad"></div>

Output 1, 2, and 4.

그런 다음 다음 미니 작업을 실행하십시오:


![image](/assets/img/2024-08-17-CanYouAnswerThisSeniorLevelJavaScriptPromiseInterviewQuestion_5.png)


하지만 함정이 있습니다: 현재 프로미스가 아직 보류 중인 상태이기 때문에 현재 코드는 현재 실행되지 않습니다.

<div class="content-ad"></div>

그런 다음 macrotask를 실행하십시오:

![image](/assets/img/2024-08-17-CanYouAnswerThisSeniorLevelJavaScriptPromiseInterviewQuestion_6.png)

그리고 promise의 상태가 fulfilled로 전환됩니다.

그런 다음 Event Loop를 통해 microtask를 다시 실행하십시오:

<div class="content-ad"></div>


![image](/assets/img/2024-08-17-CanYouAnswerThisSeniorLevelJavaScriptPromiseInterviewQuestion_7.png)

And the state of the promise becoming fulfilled.

Then, with Event Loop, execute the microtask again:

![image](/assets/img/2024-08-17-CanYouAnswerThisSeniorLevelJavaScriptPromiseInterviewQuestion_8.png)


<div class="content-ad"></div>

## 결과: