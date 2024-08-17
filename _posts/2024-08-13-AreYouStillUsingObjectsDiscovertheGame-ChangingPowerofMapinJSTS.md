---
title: "JavaScript와 TypeScript에서 Map 활용 방법"
description: ""
coverImage: "/assets/img/2024-08-13-AreYouStillUsingObjectsDiscovertheGame-ChangingPowerofMapinJSTS_0.png"
date: 2024-08-13 11:17
ogImage: 
  url: /assets/img/2024-08-13-AreYouStillUsingObjectsDiscovertheGame-ChangingPowerofMapinJSTS_0.png
tag: Tech
originalTitle: "Are You Still Using Objects Discover the Game-Changing Power of Map in JS TS"
link: "https://medium.com/javascript-in-plain-english/are-you-still-using-objects-discover-the-game-changing-power-of-map-in-js-ts-3600a6c28b60"
isUpdated: true
updatedAt: 1723863049095
---



![이미지](/assets/img/2024-08-13-AreYouStillUsingObjectsDiscovertheGame-ChangingPowerofMapinJSTS_0.png)

개발자로서 초창기에는 JavaScript와 TypeScript에서 키-값 쌍의 컬렉션을 다루기 위해 두 번째 생각 없이 객체를 사용했었습니다.

그러나 언어가 발전함에 따라 우리의 도구들도 함께 발전하고 있습니다. 그 중 하나인 Map이 상당한 인기를 얻고 있습니다.

객체는 자리가 있지만, Map은 우리의 코드를 더 효율적이고 표현력 있게 만들어줄 다양한 이점을 제공합니다.


<div class="content-ad"></div>

이 기사에서는 Map의 주의 사항, 장점 및 단점에 대해 살펴보고, 객체 대신에 Map을 사용하는 시점에 대해 논의할 것입니다.

# 왜 Map을 사용해야 하나요? 🌈

Map은 키-값 쌍을 저장할 수 있는 내장 객체입니다. 일반적으로 키가 문자열이나 심볼인 객체와 달리 Map은 객체, 함수 및 기본 유형을 포함한 모든 유형의 키를 허용합니다.

## Maps 사용의 장점

<div class="content-ad"></div>

- 유연한 키 유형: 객체보다 Map의 가장 중요한 장점 중 하나는 키 유형에 대한 유연성입니다. 객체에서는 키가 문자열이나 심볼이어야 합니다. Map에서는 키가 프리미티브 값부터 복잡한 객체까지 모든 유형이 될 수 있습니다.

![이미지](/assets/img/2024-08-13-AreYouStillUsingObjectsDiscovertheGame-ChangingPowerofMapinJSTS_1.png)

- 순서 유지: Map은 항목의 입력 순서를 유지하므로 추가된 순서가 보존됩니다. 이는 예측 가능한 반복 순서가 필요할 때 특히 유용합니다.

![이미지](/assets/img/2024-08-13-AreYouStillUsingObjectsDiscovertheGame-ChangingPowerofMapinJSTS_2.png)

<div class="content-ad"></div>

- 자주 추가/제거할 때 성능이 향상됨: Map은 key-value 쌍을 자주 추가하거나 제거할 때 최적화되어 있습니다. 반면에 객체는 특히 많은 항목이 있는 경우 속성을 자주 추가 또는 제거할 때 성능 문제가 발생할 수 있습니다.
- Size Property: Map에는 맵에 포함된 요소 수를 제공하는 size 속성이 있습니다. 이는 Object.keys(obj).length를 사용하여 객체의 크기를 계산하는 것보다 효율적이며, 이는 전체 키 세트를 훑어야 하기 때문입니다.

![이미지](/assets/img/2024-08-13-AreYouStillUsingObjectsDiscovertheGame-ChangingPowerofMapinJSTS_3.png)

내장 메서드: Map에는 set, get, has, delete, clear와 같은 내장 메서드가 함께 제공되어 키-값 쌍을 직관적이고 간편하게 조작할 수 있습니다.

![이미지](/assets/img/2024-08-13-AreYouStillUsingObjectsDiscovertheGame-ChangingPowerofMapinJSTS_4.png)

<div class="content-ad"></div>

# 맵 사용이 항상 최 우선 사항이 아닌 이유 ⚓

맵은 내부 구조와 추가 기능으로 인해 일반 객체와 비교해서 약간 더 많은 오버헤드를 가지고 있습니다.

성능이 중요하고 컬렉션이 작고 간단한 경우에는 이 오버헤드가 불필요할 수 있습니다.

![이미지](/assets/img/2024-08-13-AreYouStillUsingObjectsDiscovertheGame-ChangingPowerofMapinJSTS_5.png)

<div class="content-ad"></div>

일반 객체와 달리, Map은 JSON으로 직접 직렬화할 수 없습니다. 데이터를 직렬화해야 한다면, 먼저 Map을 객체나 배열로 변환해야 합니다.

## 이럴 때 필요합니다

- 복잡한 키 타입: 문자열이나 심볼이 아닌 키가 필요할 때, Map을 사용하면 좋습니다.

![이미지](/assets/img/2024-08-13-AreYouStillUsingObjectsDiscovertheGame-ChangingPowerofMapinJSTS_6.png)

<div class="content-ad"></div>

- 자주 추가 및 제거되는 항목: 항목이 자주 추가 또는 제거되는 컬렉션의 경우, Maps가 더 나은 성능을 제공합니다.
- 순서가 중요한 컬렉션: 항목의 순서가 중요한 경우, Maps는 객체와 달리 삽입 순서를 유지합니다.

## 다음 상황에서는 필요하지 않습니다

단순하고 정적인 컬렉션: 소규모이거나 정적인 데이터 컬렉션의 경우, 일반 객체가 더 직관적이고 오버헤드가 덜합니다.

![이미지](/assets/img/2024-08-13-AreYouStillUsingObjectsDiscovertheGame-ChangingPowerofMapinJSTS_7.png)

<div class="content-ad"></div>

직렬화 필요: 데이터를 JSON으로 자주 직렬화해야 하는 경우, Map은 추가 변환을 필요로하기 때문에 객체가 더 편리할 수 있습니다.

# 인터뷰에서 Map 사용의 가치, 특히 구글과 같은 회사에서 🚀

기술 인터뷰의 세계에서, 특히 구글과 같은 최고 수준의 회사에서, 언어 기능에 대한 심층적인 이해를 보여주고 이를 언제 사용해야 하는지 알면 다른 지원자들과 차별화될 수 있습니다.

여기 Map을 효과적으로 사용하는 방법이 있습니다:

<div class="content-ad"></div>

- 고급 기능 이해를 보여줍니다: Map을 활용하면 현대 JavaScript 및 TypeScript 기능에 정통하다는 것을 보여줍니다. 이는 기본을 넘어서 언어의 더 고급 측면을 이해한다는 것을 시사합니다.
- 문제 해결 능력을 나타냅니다: Map을 성능상의 이점이나 복잡한 키 유형을 처리할 수 있는 능력 때문에 선택할 때, 업무에 맞는 적절한 도구를 선택할 수 있는 능력을 반영합니다. 이러한 의사 결정 과정은 면접 중 문제 해결 시나리오에서 중요합니다.

![image](/assets/img/2024-08-13-AreYouStillUsingObjectsDiscovertheGame-ChangingPowerofMapinJSTS_8.png)

- 성능 인식을 강조합니다: 성능이 중요한 응용 프로그램에서, Map이 객체보다 자주 추가 및 제거하는 경우 더 나은 성능을 제공한다는 것을 알고 있다면 이는 상당한 장점일 수 있습니다. 면접에서 이러한 성능 이점을 논의하면 최적화 기술에 대한 인식을 보여줍니다.
- 데이터 구조에 대한 지식을 설명합니다: 데이터 구조를 효과적으로 이해하고 구현하는 것은 모든 소프트웨어 엔지니어에게 중요한 기술입니다. 필요한 경우 Map을 사용함으로써 본인의 필수 데이터 구조에 대한 능숙함과 효율적인 코드를 작성하기 위해 그것들을 이용하는 능력을 강조합니다.
- 시스템 설계 논의를 준비합니다: Google과 같은 회사에서, 시스템 설계 인터뷰가 일반적입니다. Map을 사용할 때 알면 이러한 논의에서 유용할 수 있으며, 거대한 데이터셋과 다양한 키 유형을 처리해야하는 시스템을 설계해야 할 수도 있는 상황에서 유용합니다.

# 2024년에 Map의 인기 💎

<div class="content-ad"></div>

2024년 현재, Maps는 TypeScript 및 JavaScript 커뮤니티 전반에서 점점 더 인기를 얻고 있습니다. 성능, 유연성, 그리고 내장 메서드 측면에서의 장점으로 인해 많은 개발자들에게 선호되는 선택지가 되었습니다.

![맵 이미지](/assets/img/2024-08-13-AreYouStillUsingObjectsDiscovertheGame-ChangingPowerofMapinJSTS_9.png)

커뮤니티는 또한 Maps를 활용하는 여러 라이브러리와 도구들을 제작했으며, 현대적인 개발 방법론에서 그들의 위치를 더욱 확고히 하고 있습니다.

# 결론

<div class="content-ad"></div>

자바스크립트와 타입스크립트의 기본 요소인 객체들은, 맵은 키-값 쌍을 다루는 강력한 대안을 제공합니다.

맵은 유연성을 제공하며, 동적 컬렉션에 대한 성능을 높여주며, 코드를 간편하게 만들어주는 직관적인 메소드들을 제공합니다. 키-값 쌍을 저장하기 위해 객체에 접근할 때, 한 번 Map이 더 잘 맞지는 않을지 생각해보세요.

맵을 받아들이면 보다 깨끗하고 성능이 우수하며 표현력이 높은 코드를 작성할 수 있어, 2024년 이후로 더욱 능숙한 개발자가 될 수 있을 겁니다.

건배 🍺

<div class="content-ad"></div>

# 평이한 영어 커뮤니티의 일원이 되어 주셔서 감사합니다! 떠나시기 전에:

- 저자를 박수로 응원하고 팔로우해주세요 👏️️
- 팔로우하기: X | LinkedIn | YouTube | Discord | Newsletter
- 다른 플랫폼 방문하기: CoFeed | Differ
- 더 많은 콘텐츠: PlainEnglish.io