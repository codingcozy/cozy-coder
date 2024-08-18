---
title: "Swift의 힘을 풀어라 고차 함수 완벽 정복 방법"
description: ""
coverImage: "/assets/img/2024-07-02-UnleashingthePowerofSwiftMasteringHigher-OrderFunctions_0.png"
date: 2024-07-02 23:10
ogImage:
  url: /assets/img/2024-07-02-UnleashingthePowerofSwiftMasteringHigher-OrderFunctions_0.png
tag: Tech
originalTitle: "Unleashing the Power of Swift: Mastering Higher-Order Functions"
link: "https://medium.com/@ragul.m_19701/unleashing-the-power-of-swift-mastering-higher-order-functions-fb8bb10dcf3e"
isUpdated: true
---

## 높은차 함수란 무엇인가?

높은차 함수는 하나 이상의 함수를 매개변수로 받거나 함수를 반환하거나 둘 다를 수행하는 함수입니다. 다른 함수들을 다룰 수 있는 이 능력 덕분에, 컬렉션을 조작하거나 데이터의 흐름을 제어하거나 복잡한 알고리즘을 더 읽기 쉬운 방식으로 구현하는 등 다양한 프로그래밍 작업에 매우 유연하게 사용할 수 있습니다.

# Swift에서의 주요 높은차 함수들

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

안녕하세요! 오늘은 스위프트에서 제공하는 몇 가지 내장 고차 함수에 대해 이야기해 볼게요. 배열이나 딕셔너리와 같은 컬렉션에서 작동하는 이 함수들은 다음과 같아요:

- map(\_:)
- filter(\_:)
- reduce(_:_:\_)
- compactMap(\_:)
- flatMap(\_:)
- sort(\_:)
- forEach(\_:)

각 함수를 예제와 함께 살펴보겠습니다.

# 고차 함수 예제

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

# 1. map(\_:)

The map function is a powerful tool that allows you to transform each element in a collection by applying a specific operation.

```swift
let numbers = [1, 2, 3, 4, 5]
let squaredNumbers = numbers.map { $0 * $0 }
print(squaredNumbers) // Output: [1, 4, 9, 16, 25]
```

In this example, we square each number in the `numbers` array using the map function, resulting in a new array `squaredNumbers` with the squared values.

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

## 2. filter(\_:)

Filter 함수는 지정된 조건을 충족하는 요소만 포함하는 새 배열을 반환합니다.

```js
let numbers = [1, 2, 3, 4, 5]
let evenNumbers = numbers.filter { $0 % 2 == 0 }
print(evenNumbers) // 결과: [2, 4]
```

위 예제에서는 filter가 원래 배열에서 짝수만 포함하는 배열을 만드는 데 사용되었습니다.

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

### 3. reduce(_:_:\_)

The `reduce` function is a powerful tool that allows you to merge all elements of a collection into one using a closure to define how the elements will be combined.

```swift
let numbers = [1, 2, 3, 4, 5]
let sum = numbers.reduce(0) { $0 + $1 }
print(sum) // Result: 15
```

In this example, the `reduce` function adds up all the numbers in the array.

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

**4. compactMap(\_:)**

The `compactMap` function is super handy when you need to transform elements of an array and filter out any nil values.

```swift
let strings = ["1", "two", "3", "four"]
let numbers = strings.compactMap { Int($0) }
print(numbers) // Output: [1, 3]
```

In the code snippet above, `compactMap` swiftly converts strings that can be transformed into integers and removes any elements that fail the transformation.

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

# 5. flatMap(\_:)

**flatMap** 함수는 컬렉션의 컬렉션을 하나의 배열로 평평하게 만든 다음 각 요소에 변환을 적용합니다.

```swift
let numbers = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
let flattenedNumbers = numbers.flatMap { $0 }
print(flattenedNumbers) // 결과: [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

위 예제에서 **flatMap**은 중첩된 배열을 단일 배열로 병합합니다.

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

# 6. sort(\_:)

sort 함수는 클로저에 의해 지정된 순서에 따라 컬렉션의 요소를 정렬합니다. 이 클로저는 첫 번째 요소가 두 번째 요소보다 먼저 정렬되어야 하는지 여부를 나타내는 부울 값(Boolean value)을 반환합니다.

```swift
let names = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
let sortedNames = names.sorted(by: { $0 < $1 })
print(sortedNames) // 결과: ["Alex", "Barry", "Chris", "Daniella", "Ewa"]
```

위의 예제는 sort 메소드를 사용하여 이름을 알파벳 순서로 정렬하는 방법을 보여줍니다.

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

# 7. forEach(\_:)

forEach 함수는 컬렉션의 각 요소를 반복하고 각 요소에 대해 클로저를 실행합니다.

```js
let names = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
names.forEach { print($0) }
// 출력:
// Chris
// Alex
// Ewa
// Barry
// Daniella
```

위 예시에서 forEach를 사용하면 컬렉션의 각 요소에 대해 작업을 수행할 수 있습니다.

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

# Higher-Order Functions를 사용하는 장점

간결함:

- Higher-Order Functions를 사용하면 더 간결한 코드를 작성할 수 있습니다. 루프와 수동 확인을 작성하는 대신 Higher-Order Functions를 사용하여 한 줄의 코드로 동일한 결과를 얻을 수 있습니다.

가독성:

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

- 하이어오더 함수를 사용하는 코드는 더 가독성이 좋고 표현력이 풍부할 수 있어요. 이를 통해 어떻게 하는지가 아닌 무엇을 하는지를 더 명확하게 설명할 수 있어요.

함수 합성:

- 하이어오더 함수는 함수를 조합하고 모듈식으로 재사용할 수 있도록 돕습니다. 여러 하이어오더 함수를 연결하여 복잡한 변환을 간결하게 달성할 수 있어요.

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

- 이러한 기능들은 종종 변경이 아닌 사본을 사용하여 컬렉션을 다루며 적용함으로써 더 안전하고 예측 가능한 코드를 작성하도록 장려합니다.

병렬 처리와 성능:

- 맵(map) 및 필터(filter)와 같은 고차 함수들은 성능을 최적화할 수 있습니다. 이는 원소들을 독립적으로 처리하므로 보다 쉽게 병렬화할 수 있습니다.

# 결론

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

Higher-order functions는 Swift에서 강력한 기능으로, 코드를 크게 단순화하고 향상시킬 수 있습니다. map, filter, reduce 등의 함수를 이해하고 활용하면 더 우아하고 가독성이 좋고 유지보수하기 쉬운 코드를 작성할 수 있습니다. 이러한 함수들을 포용함으로써 Swift 코드를 표현력 있게 만들 뿐만 아니라 현대적인 함수형 프로그래밍 관행과 일치시킬 수 있습니다.

간단한 앱이나 복잡한 시스템을 작업하고 있더라도, Higher-order functions는 데이터 변환과 조작을 쉽고 명확하게 처리할 수 있습니다.

Higher-order functions를 활용하면 Swift 프로그래밍 기술을 한 단계 더 나아가게 됩니다. 프로젝트에서 이러한 함수들을 실험해보고 코드를 간단하게 하고 향상시킬 수 있는 방법을 살펴보세요! 의문이나 명확화가 필요한 사항이 있으면 언제든지 연락해주세요.

# 🧑🏻‍🏫🧑🏻‍💻🎓Happy Coding!🎓🧑🏻‍💻🧑🏻‍🏫
