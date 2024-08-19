---
title: "React Hooks에서 반드시 알아야 할 5가지 내용"
description: ""
coverImage: "/assets/img/2024-08-19-LearningReact5ImportantPrinciplesAboutHooksYouHaveToKnow_0.png"
date: 2024-08-19 02:11
ogImage: 
  url: /assets/img/2024-08-19-LearningReact5ImportantPrinciplesAboutHooksYouHaveToKnow_0.png
tag: Tech
originalTitle: "Learning React 5 Important Principles About Hooks You Have To Know"
link: "https://medium.com/code-like-a-girl/learning-react-5-important-principles-about-hooks-you-have-to-know-4967cd9d4eb4"
isUpdated: true
updatedAt: 1724033385254
---


## 예를 들어, 때로는 Hooks가 필요 없을 수도 있어요!

![Learning React](/assets/img/2024-08-19-LearningReact5ImportantPrinciplesAboutHooksYouHaveToKnow_0.png)

2025년이 다가오고 있는데, React는 여전히 프론트엔드 애플리케이션을 만드는 데 가장 인기 있는 선택지 중 하나일 것입니다.

React가 인기를 얻게된 이유는 단점이 없기 때문은 아니지만, 년 년이 지나며 React가 얻은 엄청난 커뮤니티와 광범위한 인기 때문입니다. 그리고 React 19도 큰 기대를 모으고 있기 때문에 React가 그 위치를 잃지 않을 것이라고 생각합니다.

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

리액트 없이 훅을 상상하기 어려운데, 불행하게도 종종 개발자들이 훅을 과도하게 사용하는 것을 보곤 합니다. 결과적으로 의존성 배열로 문제를 해결하거나 쓸모없는 다시 렌더링을 해야하며, 종료적으로 그들이 만든 혼란뿐만 아니라 모든 것이 리액트에서 훅만을 이용해야 한다는 심오한 신념 때문에 만들어진 문제들을 처리해야 합니다.

이 기사에서는 새로운 리액트 프로그래머가 알아야 할 다섯 가지 원칙에 대해 논의하겠습니다. 코드를 개선하고 단순화하는 데 도움이 될 것입니다.

# 1. 모든 함수가 훅이 될 필요는 없습니다

기본부터 시작해 정의를 살펴보죠.

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

훅은 함수처럼 보일 수 있지만 몇 가지 차이점이 있습니다:

- 훅은 기능 컴포넌트 또는 사용자 지정 훅에서만 사용할 수 있습니다.
- 훅의 이름은 항상 "use"로 시작하고 대문자로 시작합니다.
- 사용자 정의 훅에 내장된 다른 훅 호출이 없는 경우 함수가 아닌 훅입니다. 이는 내부에 상태 논리 또는 효과가 있는지 확인하는 데 도움이 됩니다.

이러한 요구 사항을 충족시키기 위해 useBoolean이라는 간단한 사용자 정의 훅을 만들어 보겠습니다. 이를 사용하여 패널을 열거나 닫거나 대화 상자를 표시하거나 숨기는 등의 작업을 수행할 수 있습니다.

공식 문서를 살펴보면 훅이 반환하는 모든 함수를 useCallback으로 래핑하는 것이 권장된다는 것을 알 수 있습니다. 그에 따라 우리도 그렇게 할 것입니다.

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
인터페이스 ICallbacks {
  setFalse: () => void;
  setTrue: () => void;
  toggle: () => void;
}

const useBoolean = (initialValue: boolean): [boolean, ICallbacks] => {

  const [value, setValue] = useState(initialValue);
  
  const setFalse = useCallback(() => {
    setValue(false);
  }, []);
  
  const setTrue = useCallback(() => {
    setValue(true);
  }, []);
  
  const toggle = useCallback(() => {
    setValue(curValue => !curValue);
  }, []);

return [value, {setFalse, setTrue, toggle}];
}
```

기본을 기억할 때 깊게 파고들어 섬세한 점을 더 잘 이해할 수 있습니다.

# 2. 다시 렌더링에 대해 배우기

아마 궁금해할 수 있는 것입니다.

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

React가 작동하는 방식 및 setter 함수를 통해 컴포넌트 상태를 변경할 때 무슨 일이 일어나는지 이해하는 것이 중요합니다. 처음 보면 상태가 변경되었고 결과가 즉시 나타나야 할 것처럼 보이지만, 그렇지 않을 수도 있습니다.

상태가 변경될 때 무슨 일이 일어나는지 알고 있다면, useEffect나 의존성 배열이 있는 다른 훅이 언제 왜 트리거되는지 이해하기 훨씬 쉽습니다.

간단한 예시를 살펴봅시다. 처음 버튼을 누르고 onChangeText를 호출하여 값 "newValue"를 전달한다고 상상해봅시다.

```js
const [text, setText] = useState("defaultValue");

const onChangeText = (value: string) => {
  // value는 "newValue"와 같다고 가정
  setText(value);
  
  console.log(text); //여기에 값은 무엇일까요?
}
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

"콘솔에서 'newValue'를 확인해야 할 것 같지만 실제로는 'defaultValue'가 표시될 것입니다. 왜냐하면 새 값은 다시 렌더링 후에만 사용 가능하기 때문입니다.

다음에서 일어나는 단계를 살펴보는 것이 중요합니다:

- React가 액션을 취할 것을 알리기 위해 세터 함수를 통해 상태를 변경합니다.
- 렌더링. React는 새 JSX를 계산하기 위해 컴포넌트를 호출하고 반환될 것입니다.
- 커밋. 변경 사항을 계산한 후, React는 DOM을 수정하고 최소한의 조치가 취해질 것입니다.
- 이전 단계를 거친 후에 화면상의 시각적 변화가 나타날 것입니다 ('브라우저 렌더링').

상태의 값을 변경하고 싶을 때마다, 이러한 단계들이 매번 수행된다는 것을 기억해야 합니다."

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

# 3. useState은 항상 정답이 아닙니다

React에서 컴포넌트 상태를 관리하는 두 가지 방법이 있습니다 - useState와 useReducer가 있습니다. 두 번째 방법은 상태에 있는 더 복잡한 객체를 위한 것이기 때문에, 처음 보는 프로그래머에게는 너무 어려워 보일 수 있지만 사실은 그렇지 않습니다.

그러나 useState는 매우 간단하고 이해하기 쉬우므로 새로운 프로그래머들이 필요 이상으로 자주 사용하는 경우가 많습니다.

사용자 상호작용에 따라 컴포넌트를 다시 그리는 상태를 관리하도록 의도되어 있습니다. 렌더링하지 않고 무언가를 기억하고 싶다면, 상태에 넣지 말아야 합니다. useRef가 더 나은 선택일 것입니다.

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

## useState가 필요하지 않은 경우:

- 사용자에게 표시하지 않고 다시 렌더링하는 동안 일부 값을 기억하고 싶을 때.
- 이미 상태에 데이터가 있는 경우, 또는 props를 통해 데이터가 제공되지만 변환해야 하는 경우; 새로운 useState 객체에 그 새로운 값을 유지할 필요가 없을 때, 새 변수를 만들어서 작업하고 불필요한 다시 렌더링을 발생시키지 않도록 하세요.

## 상태에 값을 유지해야 하는 경우:

- 값이 변경될 때 구성요소를 다시 그릴 때; 가장 흔한 예는 패널을 표시/숨기기, 회전기, 오류 메시지, 및 배열 수정 등이 있습니다.

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
/**
React의 기본 동작을 활용하면 이름이나 설명이 변경될 때
더 많은 re-render를 유발하지 않고 올바른 값을 얻을 수 있습니다.
**/
const [name, setName] = useState();
const { description, index } = props;
const nameWithDescription = `${name} - ${description}`;
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

# 4. useEffect에 대해 주의해야 합니다

특정 경우에는 Class 구성 요소에서 특정 라이프사이클 메서드를 사용했습니다. React 16.8 버전부터 모든 것에 대해 하나의 useEffect 훅을 가지고 있는 것처럼 보일 수 있습니다.

모든 곳에 대해 아니지만, 2019년에 훅 주변의 놀라운 열기와 문서 부족으로 인해 모든 곳에서 사용되려는 경향이 있었습니다.

공식 문서를 살펴보겠습니다:

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

하지만 실제로는, useEffect를 필요 이상으로 많이 사용합니다. 컴포넌트가 마운트될 때 데이터를 가져오는 데 탁월하지만, 불행히도 새로운 프로그래머들은 상태를 변경하는 데 이 훅을 사용하려고 합니다. 이것이 최선의 해결책은 아닙니다.

한 컴포넌트 내에 연이어 useEffect를 작성하고 있는 자신을 발견하면 코드를 멈추고 검토하세요. 보통 그런 것들이 필요하지 않으며, 쉽게 제거할 수 있습니다.

## useEffect가 필요하지 않은 경우:

- 사용자 이벤트(클릭)를 처리해야 하는 경우; 어떤 액션이 어떤 로직을 트리거하는지 알 때, 해당 로직을 호출하기 위해 useEffect를 사용하지 마세요.
- 렌더링을 위해 데이터를 변환해야 하는 경우, 예를 들어, 상태나 프롭스에서 문자열을 연결해야 하는 경우. 반응형 값이나 로직을 종속성 배열에 넣으면 useEffect가 너무 자주 호출되어 무한 루프에 빠지는 경우가 있습니다.

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

## useEffect를 선택해야 하는 경우:

- 컴포넌트가 마운팅될 때 데이터를 가져와야 하거나 일정 간격으로 작업을 수행하거나 상태를 다른 시스템과 동기화해야 하는 경우

# 5. useRef를 두려워하지 마세요

안타깝게도 useRef는 소홀히 다루는 경우가 많습니다. 가장 인기 있는 훅 중에는 포함되어 있지 않지만 실제로 매우 유용합니다. 어떻게 사용해야 하며 어디에 사용해야 하는지 알면 좋은 결과를 얻을 수 있습니다.

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

자, 기본부터 시작해 봅시다.

React는 useRef를 통해 생성한 값을 기억합니다. DOM의 노드를 참조하는 JavaScript 객체로 만들거나 단순한 값으로 만들더라도, 이 값은 재 랜더링 중에 소실되지 않습니다.

이것이 우리에게 제공하는 것은 무엇인가요?

- 우리는 DOM의 요소들에 쉽게 접근할 수 있습니다. 예를 들어, 입력 필드의 값을 가져오거나 특정 요소에 초점을 맞추거나, 그 높이와 너비를 가져오거나, 화면의 특정 부분으로 스크롤하는 등의 작업을 수행할 수 있습니다.
- 컴포넌트를 재 렌더링하지 않고 필요한 모든 데이터를 기억할 수 있습니다. 예를 들어, 카운터나 타이머가 필요하다면, useState가 아닌 useRef를 선택하세요.

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

예시:

```js
// 숫자에 대한 참조
const refCount= useRef(0);
// 입력 요소에 대한 참조
const refInputField = useRef(null);

/**
값에 접근하려면 current 속성을 사용해야 합니다.
useRef는 re-render를 트리거하지 않으므로 의존성 배열에 대해 걱정하지 않고 useEffect 내에서 사용할 수 있습니다.
*/

const onClick = () => {
refCount.current = refCount.current + 1;
refInputField.current.focus();
}

  return (
    <>
      <button onClick={onClick}>
        Click me!
      </button>
      <input ref={refInputField}></input>
    </>
  );
```

Hooks는 훌륭하며, 사용해야 합니다. React의 작동 방식을 이해하고 hook을 올바르게 적용하면 많은 것을 달성할 수 있습니다.

이것이 새로운 React 개발자를 위한 팁입니다. 이 팁이 도움이 되기를 바랍니다.