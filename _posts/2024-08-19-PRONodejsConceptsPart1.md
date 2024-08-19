---
title: "2024년 최신 Nodejs 핵심 개념 정리"
description: ""
coverImage: "/assets/no-image.jpg"
date: 2024-08-19 02:34
ogImage: 
  url: /assets/no-image.jpg
tag: Tech
originalTitle: "PRO Nodejs Concepts Part 1"
link: "https://medium.com/@devdiwan/pro-node-js-concepts-part-1-76e0e0af3e6e"
isUpdated: true
updatedAt: 1724034727498
---


## 메모리 관리와 가비지 콜렉션 마스터하기

알렉스와 조던은 노드.js 프로젝트에 깊게 몰두하던 중 뭔가 불안한 점을 발견했습니다 — 어느 정도의 시간 동안 애플리케이션을 실행한 후에는 점점 더 많은 메모리를 소비하다가 결국 무너지기 시작했습니다. 조던은 당황한 모습을 보였지만, 경험이 풍부한 알렉스는 즉시 메모리 누수를 의심했습니다. "효율적인 코드를 작성하는 것 이상의 문제입니다," 알렉스가 설명했습니다. "이는 Node.js가 메모리를 처리하는 방식을 이해하고 가비지 콜렉터를 만족시키는 방법을 알아야 합니다."

## 1. V8의 메모리 관리 이해하기

Node.js는 자체의 메모리 관리 및 가비지 콜렉션 메커니즘을 갖춘 V8 JavaScript 엔진을 사용합니다. 그러나 Node.js의 단일 스레드 특성으로 인해 메모리 관리에 특별한 주의가 필요합니다. V8 엔진은 메모리를 주로 힙(heap) 및 스택(stack) 두 가지 주요 영역에 할당합니다. 스택은 정적 메모리 할당을 다루고, 힙은 동적 메모리 할당에 사용됩니다.

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

경험이 있는 개발자들은 힙(heap)이 가장 많은 메모리 문제가 발생하는 곳임을 알고 있습니다. Node.js에서 힙은 제한된 크기를 가지고 있으며, 일반적으로 64비트 시스템에서는 약 1.5GB에서 4GB 사이입니다. 이 한도를 초과하면 메모리 부족 오류가 발생하여 응용 프로그램이 다운될 수 있습니다.

## 2. 메모리 누수 식별과 방지

Node.js에서 메모리 누수는 할당된 메모리가 더 이상 필요하지 않지만 해제되지 않아 시간이 지남에 따라 메모리 사용량이 계속 증가하는 경우에 발생합니다. 여기에는 일반적인 원인 몇 가지와 이를 방지하는 방법이 있습니다:

- 전역 변수: 변수가 의도하지 않게 전역에 저장될 때 메모리 누수가 발생하기 쉽습니다. 변수를 항상 "let", "const", 또는 "var"로 선언하여 전역 범위로 누출되지 않도록 주의하십시오.
- 클로저: 외부 함수에서 변수에 대한 참조를 보유하는 클로저는 해당 참조가 정리되지 않을 경우 메모리 누수를 초래할 수 있습니다. 오랜 기간 사용되는 클로저를 주의 깊게 관리하세요.
- 이벤트 리스너: 제거되지 않은 이벤트 리스너는 쌓이며, 가비지 수집되어야 할 객체에 대한 참조를 보유합니다. "removeListener" 또는 "off"로 항상 리스너를 정리하세요.

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

아래는 Markdown 형식의 표입니다.


| Syntax      | Description |
| ----------- | ----------- |
| Header      | Title       |
| Paragraph   | Text        |


위의 예시에서 첫 번째 예시는 메모리 누수를 발생시키는 클로저로 인한 타이머를 시작합니다. 두 번째 예시는 작업이 완료된 후 정확한 정리를 보여줍니다.

## 3. 가비지 컬렉션: 효율적으로 유지하기

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

Node.js에서의 가비지 수집은 자동적이지만 그 특이한 면도 있어요. V8 엔진은 세대별 가비지 수집기를 사용해서 메모리를 새로운 공간(젊은 세대)과 오래된 공간(늙은 세대) 두 부분으로 나누어요. 여러 차례의 가비지 수집을 버티는 객체들은 오래된 공간으로 승격돼요.

이러한 접근 방식은 효율적이지만 제대로 관리하지 않으면 성능 문제로 이어질 수 있어요. 예를 들면, 객체들이 많이 생성되면 빈번한 가비지 수집을 유발해 실행 코드보다 가비지 수집에 더 많은 시간을 쓰게 되는 GC thrashing 현상이 발생할 수 있어요.

코드 예시:

```js
const data = [];
function addData() {
  for (let i = 0; i < 1e6; i++) {
    data.push({ item: i }); // 쉽게 많은 객체를 생성해냄
  }
}
addData();
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

이 예제에서는 수백만 개의 객체가 빠르게 생성되어 GC 스래싱으로 이어질 수 있습니다. 더 나은 접근 방식은 더 작은 일괄 처리로 객체를 생성하고 폐기하거나 대규모 데이터셋을 관리하는 더 효율적인 데이터 구조를 사용하는 것입니다.

## 결론

메모리 관리와 가비지 수집은 초보 개발자에 의해 종종 간과되지만, 이러한 기술은 확장 가능하고 효율적인 Node.js 애플리케이션을 개발하려는 사람들에게 필수적입니다. V8가 메모리를 할당하는 방법을 이해하고 메모리 누수의 일반적인 원인을 인식하며 가비지 수집을 최적화하는 것은 애플리케이션이 우아하게 확장되는지 또는 부하 하에서 충돌하는지의 차이를 만들어 줄 수 있습니다.

다음 시리즈에서는 Node.js에서 비동기 작업을 최적화하는 고급 기술들을 탐색할 것입니다. 백프레셔를 관리하고 워커 스레드를 효과적으로 사용하는 것을 포함합니다. 기대해 주세요!