---
title: "JavaScript에서 함수 사용하기 기초부터 심화까지"
description: ""
coverImage: "/assets/img/2024-08-19-FunctionsinJS_0.png"
date: 2024-08-19 02:16
ogImage: 
  url: /assets/img/2024-08-19-FunctionsinJS_0.png
tag: Tech
originalTitle: "Functions in JS"
link: "https://medium.com/@gigadexterity/functions-in-js-6b728fb4d72d"
isUpdated: false
---


![img](/assets/img/2024-08-19-FunctionsinJS_0.png)

이 함수 작성 방법에 대한 자습서를 마치면 JavaScript에서 함수를 처리하고 유지 관리(또는 생성)하는 참 전문가가 될 거예요.

함수는 JS의 핵심 부분이에요. 함수는 이름이 호출될 때 따라야 하는 일련의 지시사항이에요. 함수를 사용하는 이유는 프로그래밍을 조금 단순하게 만들어주기 때문이에요. 또한 코드의 본문을 복사하거나 붙여넣기하지 않고 사용하여 코드가 조금 더 깔끔해 보이게 만들어줄 수 있어요.

보통 함수의 구문에는 다음 요소들이 대부분 포함되어 있어요:

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

- 이름. (자명한 바와 같이) 함수를 호출할 때 사용됩니다.
- 매개변수는 함수에서 사용될 변수이며, 함수에는 매개변수의 수가 무한할 수 있습니다.
- 함수 본문은 특정 함수가 호출될 때 원하는 일을 작성하는 부분입니다.

예를 들어:

```js
function 함수이름(매개변수1, 매개변수2){
  // 함수 본문
}
```

함수는 그 이름을 호출함으로써 어디에서든 호출할 수 있습니다. 또는 이름 뒤에 괄호를 붙여 호출하고, 그 안에 입력 값을 제공하여 매개변수와 해당하는 값이 일치하게 할 수도 있습니다. 예를 들어:

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
functionName(input1,input2)
// 여기서의 입력은 매개변수에 해당합니다.
```

함수의 본문에서는 무언가/변수를 반환할 수 있습니다. 이는 return 뒤에 작성하는 내용이 표시되어 함수가 return을 수행하자마자 종료된다는 의미입니다.

예시:

```js
function multiply(a,b){
    return a * b
}
// 이것은 a와 b의 곱을 반환합니다.
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

알아차리셨을지 모르지만, 값들을 넣지 않아서 함수를 호출해도 아무 것도 표시되지 않거나 오류가 발생하지 않는 것을 눈치채셨을 겁니다. 이 예시에서 값이 들어가 있지 않은 이유는 매번 함수를 호출할 때 어떤 값을 어떻게 곱할지 결정할 수 있도록 하기 위해서입니다. 이것을 Function Expression이라고 합니다.

예를 들어:

```js
console.log(multiply(2,3))
//이것은 이제 출력값이 6이 될 것이며, 2x3은 6이기 때문입니다
```

함수가 내용이 거의 없을 경우 함수를 간결하게 줄일 수도 있습니다.

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

예를 들어:

```js
function multiply(a, b) {
  return a * b
}
//이것은 a와 b의 곱을 반환합니다.
```

이를 읽는 누구든지 함수의 기본 개념을 이해하고, 왜 우리가 그것을 사용하는지, 어떻게 사용하는지를 파악하고, 완벽한 애플리케이션을 개발할 수 있기를 바랍니다.