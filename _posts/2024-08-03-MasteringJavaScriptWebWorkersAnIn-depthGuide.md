---
title: " 자바스크립트 웹 워커 완벽 마스터 심층 가이드"
description: ""
coverImage: "/assets/img/2024-08-03-MasteringJavaScriptWebWorkersAnIn-depthGuide_0.png"
date: 2024-08-03 18:34
ogImage: 
  url: /assets/img/2024-08-03-MasteringJavaScriptWebWorkersAnIn-depthGuide_0.png
tag: Tech
originalTitle: " Mastering JavaScript Web Workers An In-depth Guide"
link: "https://medium.com/stackademic/javascript-web-workers-lightning-fast-performance-unleashed-2163f05dcf34"
---


![이미지](/assets/img/2024-08-03-MasteringJavaScriptWebWorkersAnIn-depthGuide_0.png)

안녕하세요 개발자 여러분! 🚀 만일 자바스크립트 코드를 빠르고 효율적으로 만들기 위해 노력 중이라면, 웹 워커(Web Workers)가 여러분의 새로운 친구가 될 수 있을 거예요! 오늘은 웹 앱을 위한 슈퍼히어로 동료같은 멋진 개념에 대해 알아볼 거예요 — 웹 워커(Web Workers). 메인 작업에 집중할 수 있는 동안 모든 번거로운 일을 처리해 주는 도우미가 있다고 상상해 보세요. 바로 웹 워커(Web Workers)가 할 수 있는 일이에요!

# 웹 워커(Web Workers)란 정확히 무엇인가요?

웹 워커(Web Workers)를 여러분의 비밀 에이전트로 생각해 보세요. 이들은 복잡한 계산이나 데이터 처리와 같은 작업을 메인 스레드를 느리게 만들지 않고 백그라운드에서 처리해 주는 요원들입니다. 이는 여러분의 웹 앱이 무거운 작업을 다루더라도 부드럽고 빠른 상태를 유지할 수 있다는 것을 의미합니다.

<div class="content-ad"></div>


![Web Workers](/assets/img/2024-08-03-MasteringJavaScriptWebWorkersAnIn-depthGuide_1.png)

## 웹 워커를 사용해야 하는 이유는?

자바스크립트는 싱글 스레드로 작동하기 때문에 한 번에 한 가지 일만 할 수 있습니다. 메인 스레드에서 긴 작업을 실행할 때 웹 앱이 멈추거나 로딩에 오랜 시간이 걸릴 수 있어 사용자를 기다리게 하고 짜즯거릴 수도 있습니다. 웹 워커는 이러한 작업을 별도의 스레드에서 처리하여 앱이 반응하는 상태를 유지하도록 돕습니다. 그 결과 사용자는 빠른 애플리케이션으로 행복해집니다!

## 약간 실용적으로 생각해 봅시다!


<div class="content-ad"></div>

몇 가지 간단한 단계로 나눠보겠습니다. 그러면 어떻게 작동하는지 이해할 수 있을 거에요!

## 단계 1: 워커 스크립트 생성하기

첫 번째로, 워커를 위한 별도의 JavaScript 파일이 필요합니다. worker.js 또는 당신이 선택한 다른 이름으로 지어봅시다!

worker.js:

<div class="content-ad"></div>

```js
self.onmessage = function (e) {
 const number = e.data;
 const result = fibonacci(number);
 postMessage(result);
 };

function fibonacci(n) {
 if (n <= 1) return n;
 return fibonacci(n - 1) + fibonacci(n - 2);
}
```

이 파일에서는 self.onmessage를 사용하여 메시지를 수신하고, 피보나치 수 계산과 같은 무거운 작업을 수행한 후 postMessage를 사용하여 결과를 다시 보내는 워커를 설정했습니다.

self가 무엇인지 궁금할 수도 있고, postMessage와 onmessage가 무엇인지 궁금할 수도 있습니다.

웹 워커는 익숙한 창(window)이 아닌 다른 글로벌 컨텍스트에서 작동합니다. 그 특별한 글로벌 컨텍스트를 self라고 부릅니다!

<div class="content-ad"></div>

웹 워커는 이벤트를 사용하여 주 스레드와 통신합니다. 이벤트를 사용하면 메시지나 데이터를 보내고 받을 수 있습니다. 코드에서는 일반적으로 onmessage가 주 스레드로부터 메시지나 데이터를 받아오며, postMessage는 웹 워커에서 처리된 데이터를 주 스레드로 다시 보냅니다.

#### 단계 2: 워커 생성 및 통신하기

이제, 워커를 생성하고 주 스크립트에서 메시지/데이터를 보내는 방법을 살펴봅시다.

main.js:

<div class="content-ad"></div>

```js
// Worker()를 사용하여 새로운 웹 워커를 만듭니다.
const worker = new Worker('worker.js');

// 워커에 데이터를 보냅니다.
worker.postMessage(40); // 40번째 피보나치 수를 계산합니다.

// 워커로부터 데이터를 받습니다.
worker.onmessage = function(e) {
  console.log('결과는:', e.data);
};

// 워커 오류 처리
worker.onerror = function(error) {
  console.error('워커 오류:', error);
};
```

여기서는 Worker()를 사용하여 새로운 워커를 만들고, worker.postMessage로 메시지를 보내고 worker.onmessage로 결과를 받습니다. 또한 worker.onerror로 잠재적인 오류를 처리합니다.

postMessage: 워커로 데이터를 보냅니다.

onmessage: 워커로부터 처리된 데이터를 받습니다.

<div class="content-ad"></div>

onerror: 에러를 처리합니다

# 웹 워커를 활용하는 멋진 방법들

웹 워커는 다양한 시나리오에서 굉장히 유용할 수 있습니다. 몇 가지 실용적인 예제를 살펴보겠습니다.

## 예제 1: 대규모 배열 정렬

<div class="content-ad"></div>

거대한 배열을 정렬하는 데는 많은 시간이 소요될 수 있어요. 웹 워커를 이용해서 그 작업을 처리해보는 건 어때요?

worker.js:

```js
self.onmessage = function (e) {
 const array = e.data;
 array.sort((a, b) => a — b);
 postMessage(array);
 };
```

main.js:

<div class="content-ad"></div>

```js
const worker = new Worker('worker.js');

const largeArray = Array.from({ length: 1000000 }, () => Math.floor(Math.random() * 1000000)); // 데모 시나리오이므로 배열이 동적으로 생성되었습니다.

worker.postMessage(largeArray);

worker.onmessage = function (e) {
  console.log('정렬된 배열:', e.data);
};
```

이 예시에서는 무작위 숫자로 이루어진 거대한 배열을 만들고, 워커에게 정렬을 요청하여 정렬된 배열을 받아옵니다.

## 예시 2: API에서 데이터 가져오기

웹 워커는 주 스레드를 차단하지 않고 API 요청을 처리할 수도 있습니다.

<div class="content-ad"></div>

worker.js:

```js
self.onmessage = async function (e) {
 const url = e.data;
 try {
 const response = await fetch(url);
 const data = await response.json();
 postMessage(data);
 } catch (error) {
 postMessage({ error: '데이터를 가져오지 못했습니다' });
 }
 };
```

main.js:

```js
const worker = new Worker('worker.js');
 
worker.postMessage('https://api.example.com/data');
 
worker.onmessage = function (e) {
if (e.data.error) {
console.error(e.data.error);
} else {
console.log('데이터를 가져왔습니다:', e.data);
}
};
```

<div class="content-ad"></div>

여기에 워커에게 URL을 보내고, 워커가 데이터를 가져와 다시 보내줍니다. 무언가 문제가 발생하면, 워커가 오류 메시지를 보냅니다.

# 웹 워커를 위한 최상의 사례

- 가벼운 상태 유지: 웹 워커는 DOM에 직접 액세스할 수 없으므로 작업을 집중시켜야 합니다.
- 오류 처리: 항상 오류 처리를 포함하여 문제를 감지해야 합니다.

# 웹 워커 시각화

<div class="content-ad"></div>

웹 워커를 공장에서 전문가 팀으로 상상해보세요. 메인 스레드는 매니저이고, 각 워커는 다른 작업을 처리합니다.

![Workers](/assets/img/2024-08-03-MasteringJavaScriptWebWorkersAnIn-depthGuide_2.png)

각 워커는 특정 작업에 집중하여 메인 매니저(스레드)가 전체 그림을 감독하는 데 집중할 수 있도록 합니다.

# 자원

<div class="content-ad"></div>

더 자세한 내용을 알고 싶다면 Web Workers에 관한 다음 리소스를 확인해보세요:

- MDN의 Web Workers
- web.dev의 Web Workers
- W3Schools의 Web Workers

# 마무리

Web Workers는 자바스크립트에서 슈퍼파워를 가지는 것과 같습니다. 무거운 작업을 백그라운드 스레드로 옮겨 웹 앱을 빠르고 반응적으로 유지할 수 있습니다. 다음에 앱이 느려지는 것 같다면, 당신의 슈퍼히어로 곁에 있는 Web Workers가 왔다는 것을 기억하세요!

<div class="content-ad"></div>

행복한 코딩하세요! 🌟

# Stackademic 🎓

끝까지 읽어주셔서 감사합니다. 떠나시기 전에:

- 작성자를 클랩하고 팔로우해주시면 감사하겠습니다! 👏
- 다른 플랫폼 방문하기: In Plain English | CoFeed | Differ
- Stackademic.com에서 더 많은 콘텐츠 확인하기