---
title: "자바스크립트에서 에러 처리 문제 해결하는 방법"
description: ""
coverImage: "/assets/img/2024-07-27-IFixedErrorHandlinginJavaScript_0.png"
date: 2024-07-27 14:04
ogImage: 
  url: /assets/img/2024-07-27-IFixedErrorHandlinginJavaScript_0.png
tag: Tech
originalTitle: "I Fixed Error Handling in JavaScript"
link: "https://medium.com/gitconnected/i-fixed-error-handling-in-javascript-4e3c1a28a292"
---



![](/assets/img/2024-07-27-IFixedErrorHandlinginJavaScript_0.png)

JavaScript의 오류 처리에 대한 유연성은 양날의 칼입니다. 오류 처리에 대해 전혀 생각하지 않고도 꽤 멀리 갈 수 있습니다. 임의로 큰 블록의 코드를 가져와 try/catch 블록으로 감싸 모든 잠재적인 오류를 한꺼번에 처리할 수 있습니다. 끝났어요!

하지만 물론 이는 대가가 따릅니다. 코드를 빠르게 작성할 수 있지만 보다 견고하지는 않으며 (로직이 매우 간단하지 않는 한) 디버깅 또한 어려울 수 있습니다. 그리고 코드베이스의 크기가 커질수록 이러한 효과가 더 커집니다.

저는 다른 언어에서 (주로 Rust와 Go) 오류 처리에 대해 더 많이 배우면서 그들이 더 나은 방법을 가지고 있다고 확신하게 되었습니다. 이러한 언어들의 접근법은 더 많은 양의 코드를 작성해야 할 수 있지만, 디버깅하기 쉬우며 예기치 않은 방식으로 실패하기가 덜할 뿐만 아니라 성능도 더 우수합니다.


<div class="content-ad"></div>

이 글에서는 TypeScript와 ESLint를 사용하여 이러한 언어들의 명시적인 오류 처리를 JavaScript로 가져오는 한 가지 방법을 공유하겠습니다. 하지만 먼저, 왜 JavaScript의 오류 처리를 개선해야 한다고 생각하는지 알아볼까요?

# 문제

JavaScript에서 오류 처리가 잘못될 수 있는 몇 가지 예를 살펴봅시다. 시작하기 전에, 우리는...