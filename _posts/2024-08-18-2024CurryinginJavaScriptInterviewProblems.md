---
title: "자바스크립트 커링 Currying 관련 면접 질문"
description: ""
coverImage: "/assets/img/2024-08-18-2024CurryinginJavaScriptInterviewProblems_0.png"
date: 2024-08-18 11:12
ogImage: 
  url: /assets/img/2024-08-18-2024CurryinginJavaScriptInterviewProblems_0.png
tag: Tech
originalTitle: "2024Currying in JavaScript  Interview Problems"
link: "https://medium.com/javascript-in-plain-english/2024-currying-in-javascript-interview-problems-78c7d14adb7b"
isUpdated: true
updatedAt: 1723950971565
---

<img src="/assets/img/2024-08-18-2024CurryinginJavaScriptInterviewProblems_0.png" />

안녕하세요 개발자 여러분,

잘 지내고 계시나요? 오늘은 JavaScript에서의 커링과 관련된 인터뷰 문제에 대해 이야기해보도록 하죠.

커링(Currying)이란 무엇인가요?

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

한 예시로 이해해봅시다:

```js
function curriedFunction(a) {
  // 이 함수는 단일 인수 'a'를 받아서 다른 함수를 반환합니다.
  return function (b) {
    // 반환된 함수는 단일 인수 'b'를 받아 다시 다른 함수를 반환합니다.
    return function (c) {
      // 가장 안쪽 함수는 단일 인수 'c'를 받아 'a', 'b', 'c'의 합을 반환합니다.
      return a + b + c;
    };
  };
}

console.log(curriedFunction(1)(2)(3)); //출력: 6
/* 
  여기서 함수는 세 개의 인수를 하나씩 호출합니다.
  먼저, curriedFunction(1)은 'b'를 기대하는 함수를 반환합니다.
  그런 다음 반환된 함수가 (2)로 호출되어 
  'c'를 기대하는 또 다른 함수를 반환합니다.
  마지막으로 마지막 함수가 (3)으로 호출되어 
  합인 1 + 2 + 3 = 6을 계산하고 반환합니다.
*/
```

면접 문제 1:

n개의 인수에 대한 커링 함수를 구현하고, 비어있는 인수로 호출했을 때 값을 반환하세요.

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

구현

```js
const sumOfNumbers = (...args) => {
  // 초기 인수는 'storage' 배열에 저장됩니다.
  const storage = [...args];

  // 인수가 제공되지 않은 경우, 합계로 0을 반환합니다.
  if (storage.length === 0) {
    return 0;
  } else {
    // 연이은 호출과 인수를 처리하는 'temp' 함수를 정의합니다.
    const temp = function (...args2) {
      // 새로운 인수(args2)를 'storage' 배열에 추가합니다.
      storage.push(...args2);

      // 현재 호출에서 인수가 전달되지 않은 경우 'storage' 배열을 합산하고 결과를 반환합니다.
      if (args2.length === 0) {
        return storage.reduce((a, b) => a + b, 0);
      } else {
        // 그렇지 않은 경우, 추가적인 연결을 위해 'temp' 함수 자체를 반환합니다.
        return temp;
      }
    };

    // 호출을 추가적으로 연결할 수 있도록 'temp' 함수를 반환합니다.
    return temp;
  }
};

console.log(sumOfNumbers(1)(2, 3)(2)(3)(4)(10)());
/* 연결 프로세스 설명:
  1. sumOfNumbers(1)은 'storage'가 [1]로 초기화된 'temp' 함수를 반환합니다.
  
  2. (2,3) => 'temp'가 이를 'storage'에 추가 => [1, 2, 3] 및 'temp' 반환.
  
  3. (2) => 'temp'가 이를 'storage'에 추가 => [1, 2, 3, 2] 및 'temp' 반환.
  
  4. (3) => 'temp'가 이를 'storage'에 추가 => [1, 2, 3, 2, 3] 및 'temp' 반환.
  
  5. (4) => 'temp'가 이를 'storage'에 추가 => [1, 2, 3, 2, 3, 4] 및 'temp' 반환.
  
  6. (10) => 'temp'가 이를 'storage'에 추가 => [1, 2, 3, 2, 3, 4, 10] 및 'temp' 반환.
  
  7. () => 'temp'가 인수를 감지하지 못해 'storage'의 합계를 반환: 1 + 2 + 3 + 2 + 3 + 4 + 10 = 25.
*/
```

인터뷰 문제 2:→

이전 전달된 값 기억하여 이전 값과 현재 값의 합계를 반환하는 함수입니다.

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

구현

```js
const sumOfPreviousAndCurrentValue = () => {
  // 'sum'을 0으로 초기화합니다.
  let sum = 0;

  // 반환된 함수는 'num'이라는 숫자를 받아 'sum'에 추가합니다. 기본값은 0입니다.
  return function (num = 0) {
    sum += num;
    return sum; // 업데이트된 'sum'을 반환합니다.
  };
};

let sum = sumOfPreviousAndCurrentValue();
// 'sum'은 이제 누적 합을 계속 더할 수 있는 함수입니다.

console.log(sum(1)); // 첫 호출: 0 + 1 = 1, 따라서 1이 출력됩니다.
console.log(sum(4)); // 두 번째 호출: 1 + 4 = 5, 따라서 5가 출력됩니다.
```

위의 문제는 커링 기술에 대한 이해를 돕습니다.

질문이 있으면 댓글로 알려주세요. 이 스레드가 유익했다면 박수를 부탁드립니다.

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

저에게 커피 사줘:→ 링크

자바스크립트에 대해 더 배우고 싶다면 이 사이트를 방문해보세요.

링크드인에서 나를 팔로우하세요.

# 쉽게 설명한 것들 🚀

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

In Plain English 커뮤니티에 참여해 주셔서 감사합니다! 떠나시기 전에:

- 반드시 작가를 클립하고 팔로우하세요 👏
- 팔로우하기: X | LinkedIn | YouTube | Discord | Newsletter
- 다른 플랫폼 방문하기: CoFeed | Differ
- 더 많은 컨텐츠: PlainEnglish.io
