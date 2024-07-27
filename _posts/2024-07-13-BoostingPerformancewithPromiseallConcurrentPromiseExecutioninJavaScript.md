---
title: "자바스크립트에서 Promiseall로 성능 향상하기 동시 Promise 실행 방법"
description: ""
coverImage: "/assets/img/2024-07-13-BoostingPerformancewithPromiseallConcurrentPromiseExecutioninJavaScript_0.png"
date: 2024-07-13 00:18
ogImage: 
  url: /assets/img/2024-07-13-BoostingPerformancewithPromiseallConcurrentPromiseExecutioninJavaScript_0.png
tag: Tech
originalTitle: "Boosting Performance with Promise.all: Concurrent Promise Execution in JavaScript"
link: "https://medium.com/@louistrinh/boosting-performance-with-promise-all-concurrent-promise-execution-in-javascript-b4bb6a95999e"
---


<img src="/assets/img/2024-07-13-BoostingPerformancewithPromiseallConcurrentPromiseExecutioninJavaScript_0.png" />

## Promise.all이란?

Promise.all은 자바스크립트에서 여러 개의 Promise를 동시에 실행하고, 모든 서브 프로미스가 해결될 때 단일 Promise를 반환하는 메소드입니다.

사용 방법:

<div class="content-ad"></div>

```js
Promise.all([promise1, promise2, ..., promiseN])
.then((results) => {
  // 모든 Promise의 결과를 처리합니다.
  // results는 각 하위 Promise의 결과를 담고 있는 배열입니다.
})
.catch((error) => {
  // 에러가 발생했을 경우 처리합니다.
});
```

```js
const promise1 = fetch('https://api1.com');
const promise2 = fetch('https://api2.com');
const promise3 = fetch('https://api3.com');
Promise.all([promise1, promise2, promise3])
.then((results) => {
  // results는 API에서 가져온 데이터를 포함한 배열입니다.
  const data1 = results[0];
  const data2 = results[1];
  const data3 = results[2];
  // 데이터를 처리합니다.
})
.catch((error) => {
  // 에러가 발생했을 경우 처리합니다.
});
```

Promise.all과 map()을 사용한 예시:

```js
const urls = ['https://api1.com', 'https://api2.com', 'https://api3.com'];
const promises = urls.map(url => fetch(url));
Promise.all(promises)
.then((results) => {
  // results는 API에서 가져온 데이터를 포함한 배열입니다.
  // 데이터를 처리합니다.
})
.catch((error) => {
  // 에러가 발생했을 경우 처리합니다.
});
```

<div class="content-ad"></div>

## Promise.all은 어떻게 성능을 향상시키나요?

Promise.all은 순차적으로 실행하는 대신 동시에 Promises를 실행하여 성능을 향상시키는 데 도움을 줍니다.

예시:

3개의 API를 호출해야 하고 각 API가 데이터를 반환하는 데 1초가 걸린다고 가정해보세요. 만약 API를 순차적으로 호출한다면 총 대기 시간은 3초가 될 것입니다.

<div class="content-ad"></div>

```js
const data1 = await fetch('https://api1.com');
const data2 = await fetch('https://api2.com');
const data3 = await fetch('https://api3.com');
// 총 대기 시간: 3초
```

하지만 Promise.all을 사용하면 3개의 API를 동시에 호출할 수 있고 총 대기 시간은 1초만 소요됩니다.

```js
const promises = [
  fetch('https://api1.com'),
  fetch('https://api2.com'),
  fetch('https://api3.com'),
];
const results = await Promise.all(promises);
const data1 = results[0];
const data2 = results[1];
const data3 = results[2];
// 총 대기 시간: 1초
```

## Promise.all을 사용하는 시점

<div class="content-ad"></div>

- 데이터를 가져오기 위해 여러 개의 API를 동시에 호출해야 할 때.
- 일련의 비동기 작업을 수행하고 모든 작업이 완료될 때까지 기다려야 할 때.

## Promise.all을 사용하여 API 데이터 가져오기:

```js
const urls = ['https://api1.com', 'https://api2.com', 'https://api3.com'];
const promises = urls.map(url => fetch(url));
Promise.all(promises)
.then((responses) => {
  // 응답 처리
  const data1 = await responses[0].json();
  const data2 = await responses[1].json();
  const data3 = await responses[2].json();
  // 데이터 처리
})
.catch((error) => {
  // 발생한 오류 처리
});
```