---
title: "주요 엔지니어가 되기 위해 알아야 할 10가지 소프트웨어 디자인 패턴"
description: ""
coverImage: "/assets/img/2024-08-18-10SoftwareDesignPatternsyoushouldknowtobecomePrincipalEngineer_0.png"
date: 2024-08-18 11:03
ogImage:
  url: /assets/img/2024-08-18-10SoftwareDesignPatternsyoushouldknowtobecomePrincipalEngineer_0.png
tag: Tech
originalTitle: "10 Software Design Patterns you should know to become Principal Engineer"
link: "https://medium.com/javascript-in-plain-english/10-software-design-patterns-you-should-know-to-become-principal-engineer-b2f070564516"
isUpdated: false
---

![Image](/assets/img/2024-08-18-10SoftwareDesignPatternsyoushouldknowtobecomePrincipalEngineer_0.png)

# 소개

처음에 주니어 개발자가 되었을 때, 당신의 코드는 플레이도 뱀처럼 보일 것입니다. 그러나 시니어 개발자로 발전하고 소프트웨어 디자인 패턴에 대해 배우면, 당신의 코드는 시스틴 성당처럼 보일 것입니다.

그러나 그 후에 주요 엔지니어로 발전하면, 사람들이 어리석은 웹사이트를 위해 시스틴 성당을 유지하길 원하지 않는다는 것을 깨닫고 플레이도 뱀을 만들기 위해 돌아갈 것입니다.

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

이번 기사에서는 소프트웨어 디자인 패턴 10가지에 대해 배우게 되며 사용하는 장단점에 대해 이야기할 것입니다. 이는 매우 주관적이며 논란이 될 수 있습니다.

프로그래밍 역사상 가장 영향을 끼친 책 중 하나는 C++ 엔지니어 네 명으로 이루어진 "갱 오브 포"에 의한 디자인 패턴입니다.

프로그래머가 직면하는 반복적인 문제를 해결하기 위한 23가지 다양한 접근 방식을 제시하며, 이는 생성 패턴(객체 생성 방식), 구조 패턴(객체 간의 관계) 및 행동 패턴(객체 간의 통신 방식)으로 구분됩니다.

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

프로 소프트웨어 엔지니어가 되는 것은 프로그래밍 언어의 구문을 외우는 것이 아니라 문제를 해결하는 능력에 관한 것입니다. 이 기사를 끝까지 따라가면 여러 가지 문제 해결 패턴을 이해하게 될 것입니다, 이것은 현대 앱 개발자들과 관련하여 어떻게 적용되는 지에 대한 것입니다.

# 소프트웨어 디자인 패턴이란?

디자인 패턴은 정말 흥미로운데, 스택 오버플로우에서 복사하여 붙여넣을 수 있는 알고리즘과는 다릅니다. 실제로 구현하기 위해 머리를 써야 합니다.

이를 모든 곳에 구현하려는 유혹을 느낄 수 있지만, 부적절하게 사용하면 코드베이스에 추가 복잡성과 보일러플레이트를 더할 수 있습니다.

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

그 책은 성서가 아니며, 디자인 패턴을 인식하는 것이 프로그래머로서 능력을 향상시키는 데 도움이 된다는 점을 알고 있더라도 그 책에 대한 많은 비평이 있습니다.

# 싱글톤

첫 번째로 살펴볼 패턴은 싱글톤입니다. 이것은 매우 쉽게 이해할 수 있는 유형의 오브젝트로, 한 번만 인스턴스화될 수 있는 오브젝트입니다.

TypeScript에서 전역 앱 설정 데이터를 표현하기 위해 싱글톤 클래스인 settings를 구현할 수 있습니다. 이 클래스에는 정적 인스턴스 속성이 있을 것이며, 생성자를 비공개로 만들어 new 키워드로 인스턴스화할 수 없도록 할 것입니다.

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

그런 다음, 이미 생성된 인스턴스를 확인하고 생성되지 않았다면 새로 생성하는 정적 get instance 메서드를 만듭니다. 이 메서드는 오직 하나의 객체만 생성될 수 있도록 보장합니다. 이것이 모두 멋지지만, 여기서 조금 미묘해지는 부분이 있습니다.

JavaScript에서는 객체 리터럴을 사용합니다. 또한 전역 데이터와 객체의 개념은 참조로 전달됩니다. 이 패턴(아래에 코드 제공)은 전역 객체를 생성함으로써 동일한 기본 특성을 갖게 됩니다.

이 패턴 자체는 실제로 필요 없는 추가적인 보일러플레이트일 뿐입니다. C++에서는 전혀 다른 이야기가 되지만, 핵심은 어떤 멋진 디자인 패턴을 구현하기 전에 언어의 내장 기능에 의존하는 것입니다.

# Prototype

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

이제 프로토타입 패턴에 대해 알아보겠습니다. 이는 클론을 위한 멋진 용어에 불과합니다. 객체 지향 프로그래밍을 수행한 적이 있다면 상속에 익숙할 것입니다. 클래스를 서브클래스로 확장할 수 있는 상속의 개념입니다.

상속의 한 문제는 복잡한 코드 계층 구조로 이어질 수 있다는 점입니다. 프로토타입 패턴은 상속을 구현하는 대안적인 방법으로, 클래스의 기능을 상속받는 대신 이미 생성된 객체에서 기능을 가져옵니다.

이는 객체 간 기능을 공유하기가 훨씬 쉬워지도록 단순한 프로토타입 체인을 생성합니다. 특히 자바스크립트와 같이 동적 언어에서 프로토타입 상속을 기본적으로 지원하는 경우에 유용합니다. 상상해보세요. 우리에게는 "zombie"라는 객체가 있다고 가정합니다. 이것이 우리의 프로토타입입니다. 하지만 이제 이를 기반으로 새 객체를 만들고 싶습니다.

이 새 객체에도 "이름"이라는 속성이 있기를 원합니다. 우리는 새로운 속성들을 추가할 때, 해당 "zombie"를 프로토타입으로 전달하고 새 객체의 이름과 같은 추가 속성들을 지정해주면 됩니다. 흥미로운 점은 이 객체를 로그에 남길 때, 이름만이 아니라 "뇌 먹기" 메소드가 아니라는 것입니다.

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

그러나 해당 메소드를 호출하려고 하면 여전히 작동합니다. 이것은 자바스크립트가 모든 메소드나 속성을 찾기 위해 프로토타입 체인을 따라 루트까지 이동하기 때문입니다.

프로토타입은 _proto_ 속성을 사용하여 객체로부터 언제든지 가져올 수 있지만, 이것은 현대적인 Best Practice가 아니며 대신에 Object.getPrototypeOf(chad)을 사용해야 합니다.

자바스크립트의 클래스에 대해 이야기할 때 prototype은 해당 생성자를 의미하며, 이는 원하는 경우 클래스를 추가 함수로 확장할 수 있다는 것을 의미합니다. 그러나 일반적으로 이것도 좋지 않은 실천으로 간주됩니다.

# Builder

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

그럼, 이제 빌더 패턴으로 전환해 봅시다. 핫도그 가게를 운영 중이라고 상상해봅시다. 고객이 주문할 때 샌드위치에 무엇을 넣을지를 말해야 하는데, 기존의 생성자에 모든 옵션을 알려야 하는 것은 조금 복잡하고 모든 단계를 나중에 처리할 수도 있습니다.

빌더 패턴을 사용하면 생성자 대신에 메서드를 사용하여 단계별로 객체를 만들 수 있고, 빌드 로직을 완전히 다른 클래스로 위임할 수도 있습니다.

JavaScript에서는 각 메서드가 this를 반환하게 되는데, 이는 메서드 체이닝을 구현할 수 있도록 객체 인스턴스에 대한 참조입니다.

그 다음 체이닝된 메서드가 작동하지만, 항상 반환 값으로 객체를 받습니다. 이 패턴은 주로 jQuery와 같은 라이브러리에서 빈번히 사용되었지만, 최근 몇 년간은 약간 시대에 뒤떨어진 스타일이 되어갔습니다.

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

# 팩토리

새 키워드 대신 객체를 인스턴스화하는 데 함수나 메서드를 사용하는 팩토리라는 패턴을 만날 수도 있습니다. 사소한 것처럼 들릴 수 있지만, 실제 예시를 보겠습니다.

모바일 앱을 개발 중이라고 상상해보죠. iOS와 안드로이드에서 모두 실행되는 크로스 플랫폼 앱을 만들고 있습니다. 두 플랫폼은 동일한 인터페이스를 가지고 있지만, 코드 내에서 어느 버튼을 표시할지 결정하기 위해 많은 조건을 확인하고 있는 상황입니다. 이는 유지보수하기 어려운 방식입니다.

대신, 어떤 객체를 인스턴스화해야 할지를 결정하는 서브 클래스나 함수를 만들어 사용할 수 있습니다. 이제 반복 로직을 사용하는 대신, 팩토리를 사용하여 렌더링될 버튼을 결정할 수 있습니다.

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

# 파사드

이제 첫번째 구조용 패턴 파사드를 살펴볼 준비가 되었어요. 파사드는 건물의 얼굴이에요. 건물 안에는 사용자가 알 필요가 없는 각종 꿍꿍한 일들이 발생하는데요. 파사드는 코드베이스에서 다른 저수준 세부 사항을 숨기기 위한 간단화된 API입니다.

배관 시스템과 전기 시스템을 위한 클래스가 있다고 상상해봅시다. 이와 같은 시스템 안에는 압력과 전압과 같은 복잡한 것들이 많은데, 집에 사는 사람들은 이러한 저수준 세부 사항에 접근할 필요가 없어요.

그래서 우리는 저수준 시스템을 의존성으로 포함하고, 그러나 그들의 작업을 단순화하는 파사드 클래스를 만들어요. 예를 들어, 전기 및 배관 세부 사항을 단일 메서드로 결합하여 사용자는 단일 메서드로 전기와 배관을 간단히 켜거나 끌 수 있게 해줄 수 있어요.

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

거의 모든 JavaScript로 설치하는 패키지는 특정한 방식으로 패싸드로 간주할 수 있으며 jQuery 같은 것은 더 귀찮은 하위 수준의 JavaScript 기능을 위한 패싸드의 훌륭한 예입니다.

# Proxy

다음 구조적 패턴은 우리가 살펴볼 프록시이며, 학교에서 대체 교사가 있는 것처럼, 이것은 대체물을 위한 멋진 단어일 뿐입니다.

프로그래밍에서는 대상 객체를 프록시로 대체할 수 있지만, 그렇게 하려면 왜 원하는 걸까요? 음, Vue.js의 반응성 시스템이 좋은 사례 연구입니다.

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

Vue에서는 데이터를 생성하지만 프레임워크 자체가 데이터를 가로채서 데이터가 변경될 때 UI를 업데이트할 수 있는 방법이 필요합니다. 이를 처리하는 방법은 원래 객체를 프록시로 대체하는 것입니다.

프록시는 원래 객체를 첫 번째 인수로 취하고 두 번째 인수로 핸들러를 취합니다. 핸들러 내에서 get 및 set과 같은 메서드를 재정의할 수 있어서 객체의 속성이 액세스되거나 변경될 때마다 코드를 실행할 수 있습니다.

예를 들어, set 내에서 우리는 프레임워크에 다시 렌더링하도록 지시하고 original 객체의 데이터를 업데이트하기 위해 반영(reflect)를 사용할 수 있습니다. 최종 사용자는 이제 프록시를 원래 객체처럼 사용할 수 있지만, 이러한 부작용을 트리거할 수 있습니다.

프록시는 또한 메모리를 복제하는 데 비용이 많이 드는 매우 큰 객체를 다룰 때 흔히 사용됩니다.

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

# Iterator

이제 행동 패턴 중 일부를 살펴볼 준비가 되었습니다. 먼저 이터레이터부터 시작해 보겠습니다. 이터레이터 패턴을 사용하면 객체 모음을 횡단할 수 있습니다.

현대 언어는 이미 이터프리터 패턴을 위한 추상화를 제공합니다. 배열 항목을 순회할 때 for 루프를 사용하면 이터레이터 패턴을 사용하게 됩니다.

하지만 JavaScript에 대해 제가 실망하는 한 가지는 원하는 시간 간격마다 쉽게 열 번 반복하고 싶은 범위 함수가 내장되어 있지 않다는 것입니다. 실제로 우리 자신의 이터레이터 패턴을 구현하여 이를 비교적 쉽게 수행할 수 있습니다.

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

JavaScript에서는 다음 메서드가 있는 객체를 정의함으로써 그 작업을 수행할 수 있습니다. 해당 함수는 루프에서 현재 값을 나타내는 값을 반환해야 하며 종료 기준을 알기 위해 done 속성을 가져야 합니다.

이 경우 시작 값이 끝 값보다 작은 경우 계속 다음 단계로 이동할 것이지만 각 반복마다 시작 값을 단계적으로 증가시켜주어야 합니다. 결국 시작 값이 끝 값보다 커지면 done 속성이 true로 설정된 객체를 반환할 수 있고, 이는 JavaScript에게 반복 작업을 중단할 시점을 알려줍니다.

여기서 멋진 기술은 이 객체에 심볼 이터레이터를 추가하여, 결국 반복에 있어 그것을 일반적인 for...of 루프에 사용할 수 있게 할 수 있다는 점입니다. 요컨대, 반복 과정에서는 집합에서 시작하여 어떻게 처음부터 끝까지 이동할지 결정하는 코드를 작성합니다.

# 옵서버

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

다음으로 살펴볼 패턴은 옵저버입니다. 옵저버는 푸시 기반 시스템입니다. 옵저버 패턴은 다른 객체에 의해 방송된 이벤트에 여러 개의 객체가 구독할 수 있게 합니다. 이것은 일대다 관계입니다.

현실 세계에서는 라디오 타워가 신호를 보내고 동시에 수신기가 수신하는 것을 생각해 볼 수 있습니다.

이 패턴은 앱 개발에서 많이 사용되며, 파이어베이스 같은 곳에서 데이터가 서버에서 변경되면 모든 클라이언트 앱이 해당 데이터를 구독하고 자동으로 최신 데이터로 업데이트됩니다.

우리의 코드에서는 RXJS 라이브러리를 가져와 이 패턴을 간단히 보여줄 것입니다. 이 라이브러리는 우리가 청취하고 싶은 데이터인 서브젝트 클래스를 제공합니다.

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

이제 주제를 가지게 되면 해당 주제에 여러 가입을 추가할 수 있습니다. 주제는 모든 이러한 구독을 추적하고 데이터가 변경될 때마다 콜백 함수를 호출할 것입니다.

나중에 어느 시점에서는 next 메서드를 호출하여 주제에 새 값을 추가할 수 있습니다. 그럴 때마다 모든 구독이 알림을 받을 것입니다. 개인적으로 시간의 차원에 걸쳐 펼쳐지는 루프로 생각하는 것을 좋아합니다.

# 중재자

이제 중재자 패턴으로 넘어가 봅시다. 중재자는 매개자나 브로커와 같습니다. 비행기와 샐도로를 위한 클래스가 있다고 상상해보세요.

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

여러 개의 활주로와 비행기가 있을 수 있고, 어떻게 하면 특정 활주로에 비행기가 착륙해도 좋은지 확인해야 합니다.

현재, 이를 수행하려면 모든 객체들이 서로 통신해야 합니다. 우리는 많-대-많 관계를 가지고 있는데, 실제로나 프로그래밍에서 상당히 위험합니다.

해결책은 활주로와 비행기 사이에 위치한 항공 교통 관제사처럼 중재자를 만드는 것입니다. 이 중재자는 조정과 통신을 제공합니다.

다음은 express.js 웹 프레임워크의 더 실용적인 예입니다. 미들웨어 시스템을 사용하여 들어오는 요청과 나가는 응답을 처리하며 미들웨어가 중간에 위치합니다.

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

그것은 비행기처럼 모든 요청을 가로채서 적절한 형식으로 응답을 위한 런웨이로 변환합니다. 이것은 관심사의 분리를 제공하며 코드 중복을 제거합니다.

# 상태

그리고 이것은 우리를 열 번째이자 마지막 디자인 패턴 상태로 이끕니다. 여기서 객체는 유한한 상태에 따라 다르게 동작합니다.

코드에서는 상태나 데이터에 따라 여러 가능성을 처리하기 위해 조건 로직 또는 스위치 문을 사용해 왔을 것입니다.

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

이런 식의 코드는 일반적으로 잘 확장되지 않습니다. 이 상태 패턴을 사용하면 하나의 기본 클래스로 시작하여 내부 상태에 따라 다양한 기능을 제공할 수 있습니다.

이 아이디어는 유한 상태 기계 및 Xstate와 같은 라이브러리와 관련이 있습니다. 목표는 오브젝트의 동작을 그것의 기본 상태에 따라 예측 가능하게 하는 것입니다.

이 예에서는 사람 클래스를 가지고, 그의 기분에 따라 다르게 생각하게 합니다. 현재는 switch 문을 사용하여 그렇게 하고 있지만, 다른 방법은 각 클래스 내에서 가능한 상태마다 별도의 클래스를 만드는 것일 수 있습니다. 동작이 다른 동일한 메소드가 있을 것입니다.

이제 사람 클래스에서 상태를 속성으로 설정하고, 그 메소드가 호출될 때마다 현재 상태로 위임합니다. 이는 상태가 변경될 때마다 객체가 완전히 다른 방식으로 동작하지만 동시에 API를 변경할 필요가 없거나 다수의 조건부 로직을 사용할 필요가 없다는 것을 의미합니다.

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

# 크레딧

이 게시물을 작성하게 된 동기는 Youtube 채널 Fireship의 비디오였기 때문에 마음에서 우러나온 감사와 크레딧을 전합니다. 비디오를 시청하고 싶다면 [여기](링크)를 클릭해주세요.

# 팁 주기

# 요약하여 말하면 🚀

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

인 플레인 잉글리쉬 커뮤니티에 참여해 주셔서 감사합니다! 떠나시기 전에:

- 반드시 박수 치고 작가를 팔로우하세요 👏
- 팔로우하세요: X | LinkedIn | YouTube | Discord | 뉴스레터
- 다른 플랫폼 방문하기: CoFeed | Differ
- PlainEnglish.io에서 더 많은 콘텐츠를 만나보세요
