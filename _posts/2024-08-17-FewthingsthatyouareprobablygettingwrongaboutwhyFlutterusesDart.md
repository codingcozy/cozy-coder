---
title: "반드시 알고 있어야 하는 왜 Flutter가 Dart를 사용하는 이유들 당신이 잘못 알고 있는 몇 가지"
description: ""
coverImage: "/assets/img/2024-08-17-FewthingsthatyouareprobablygettingwrongaboutwhyFlutterusesDart_0.png"
date: 2024-08-17 00:19
ogImage: 
  url: /assets/img/2024-08-17-FewthingsthatyouareprobablygettingwrongaboutwhyFlutterusesDart_0.png
tag: Tech
originalTitle: "Few things that you are probably getting wrong about why Flutter uses Dart"
link: "https://medium.com/@slavaolenin/few-things-that-you-are-probably-getting-wrong-about-why-flutter-uses-dart-463ae327abbc"
isUpdated: false
---


<img src="/assets/img/2024-08-17-FewthingsthatyouareprobablygettingwrongaboutwhyFlutterusesDart_0.png" />

越来越多时候，我会收到关于为什么人们认为Google选择Dart用于Flutter的电子邮件。似乎，这些大多数文章都是由刚刚加入Flutter群体并没有从一开始关注Dart发展的人所写的。因此，作为一个从一开始就关注的人，我决定写几行关于为什么我认为Google更愿意选择Dart而不是其他任何替代品。

# 1. Dart是为UI开发而设计的，这就是为什么它被选择用于Flutter。

错误。Dart是作为JavaScript的替代方案来运行在浏览器中开发的，因此，主要目标是：

<div class="content-ad"></div>

- 페이지를 빠르게 컴파일할 수 있도록: 브라우저가 페이지를로드 할 때 JavaScript (또는 우리의 경우 Dart)이 완전히로드되고 해석/컴파일 될 때까지 어떤 스크립팅된 작업도 수행할 수 없습니다. 특히 거대한 기업 응용 프로그램의 경우 이는 매우 중요합니다. 따라서 Dart는 JavaScript 외에도 이러한 목표로 설계된 유일한 언어입니다. 이것이 Flutter가 우수한 "핫 리로딩" 기능을 가질 수 있는 이유입니다.
- JavaScript만큼 성능이 우수하도록: 옛날에는 JavaScript가 이미 충분히 빠르기 때문에 빠르게 만드는 것이 중요한 목표가 아니었습니다. 그럼에도 불구하고 항상 환영받는 것입니다. 사실, Dart의 매우 첫 번째 버전은 동적으로 타입이 지정되었으며 C와 같은 언어에서 알고 있는 타입들은 코드 가독성과 IDE 지원을 위해서만 선택적으로 필요했습니다. 개발자들은 그들을 사용하지 않아도 되었고 실제로 사용하더라도 이러한 타입들은 런타임에서 확인될 것입니다.
- Dart는 Java와 같은 객체지향 언어에서 영감을 받았지만 당시에는 대부분 JavaScript와 어느 정도의 CoffeeScript를 사용하던 웹 개발 커뮤니티에 쉽게 채택될 수 있도록 설계되었습니다. 중요한 목표는 그러한 개발자들을 위해 사용하기 편리하게 만드는 것이었으며 Dart에는 UI에 특화된 요소가 없었습니다.
- "new" 연산자 제거나 배열에서의 조건과 같은 모든 이러한 기능은 Flutter가 소개된 후 추가되었습니다. 이는 Flutter 위젯 코드의 순수 가독성을 해결하기 위해 추가된 것입니다. 솔직히 말하자면, XML 및 HTML 또는 JSX와 같은 파생물은 UI를 위해 설계된 유일한 "언어"입니다.

<div class="content-ad"></div>

# 3. AOT은 JIT보다 빠릅니다.

잘못된 정보입니다: AOT 컴파일된 코드는 우수한 실행 성능을 보장하지 않습니다. 빠른 시작 속도? 네 — 미리 컴파일할 필요가 없습니다. 적은 메모리 필요성? 아마도 그렇습니다 — VM을 메모리에 계속 보관할 필요가 없습니다. 하지만 실행 속도는 아닙니다. 사실, 이론적으로 JIT가 더 빠를 수도 있습니다. 왜냐하면 컴파일이 대상 하드웨어에서 발생하고 사용자가 실행하는 환경에 맞춰 VM이 일부 최적화를 수행할 수 있기 때문입니다. 예를 들어, 자바와 C# 코드는 대부분의 경우 AOT로 컴파일된 Dart 코드보다 성능이 우수합니다.

# 4. 구글은 플러터에 Kotlin, Go 또는 다른 언어를 사용해야 했을까요?

구글이 플러터에 작업을 시작할 때에는 그 당시에는 실제 대안이 없었습니다. Kotlin이 아직 유명해지지 않았으며, Java와 기타 플랫폼은 "핫 리로딩" 기능을 제공할 수 없었습니다. 그러나 더 중요한 것은, 구글은 해당 언어들을 소유하고 있지 않기 때문에 원하는 대로 언어를 발전시키지 못할 것입니다. 그래서 플러터의 맥락에서 다른 언어와 Dart를 비교하는 것은 의미가 없습니다. 애플에게 Swift 대신 Java를 사용하지 않는다고 불평하지 않는 것과 같습니다. 그들은 그들의 플랫폼에 가장 적합한 도구를 제작합니다.

<div class="content-ad"></div>

# 결론

Google이 Dart를 사용하기로 결정한 다른 이유도 있습니다. 예를 들어, Google은 Dart 및 그 도구를 만드는 데 상당한 자원을 투자했습니다. 그들에 따르면, 플러터 이전부터 이미 Google에서 사용되는 상위 다섯 가지 언어 중 하나인 Dart를 사용하면서 만족스러웠다고 합니다. 하지만 저는 기술적인 관점에서 나열한 그 이유들이 가장 중요하다고 생각합니다.

원래 플러터를 만든 사람의 피드백을 받으면 정말 좋을 것 같습니다. 왜냐하면 위에 나열한 것들은 제 추측일 뿐이고, 그것들을 확인하거나 부정할 수 있어 기쁠 것 같습니다.

항상 읽어 주셔서 정말 감사합니다!!!