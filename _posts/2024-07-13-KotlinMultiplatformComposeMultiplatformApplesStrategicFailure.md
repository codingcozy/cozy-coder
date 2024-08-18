---
title: "애플 전략 실패 Kotlin Multiplatform과 Compose Multiplatform의 영향"
description: ""
coverImage: "/assets/img/2024-07-13-KotlinMultiplatformComposeMultiplatformApplesStrategicFailure_0.png"
date: 2024-07-13 00:39
ogImage:
  url: /assets/img/2024-07-13-KotlinMultiplatformComposeMultiplatformApplesStrategicFailure_0.png
tag: Tech
originalTitle: "Kotlin Multiplatform, Compose Multiplatform: Apple’s Strategic Failure"
link: "https://medium.com/better-programming/kmp-cm-apples-strategic-failure-cb758c24f824"
isUpdated: true
---

![Kotlin Multiplatform](/assets/img/2024-07-13-KotlinMultiplatformComposeMultiplatformApplesStrategicFailure_0.png)

"Kotlin Multiplatform 기술은 여러 플랫폼 프로젝트 개발을 간편화하기 위해 설계되었습니다. 서로 다른 플랫폼용으로 동일한 코드를 작성하고 유지하는 데 드는 시간을 줄이면서 네이티브 프로그래밍의 유연성과 혜택을 유지합니다." - Kotlin.org

모바일 애플리케이션 개발자들 - 그리고 어디에서나 관리자들 - 의 신성한 그라아일입니다.

한 번 코드를 작성하고, 어떤 플랫폼에서라도 실행할 수 있는 능력.

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

There are a few reasons for wanting this:

- Having a common code base helps to minimize inconsistencies across different platforms.
- It decreases the need for extensive testing.
- It leads to quicker market entry.
- It reduces development costs.

However, the most crucial point is probably the last one.

In reality, mobile app development can be quite costly. Hiring iOS and Android developers doesn't come cheap, and management and executives have always been hesitant about investing money in developing the same app twice.

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

한 번은 iOS 용으로, 한 번은 Android 용으로.

Kotlin Multiplatform (KMP)과 Compose Multiplatform (CM)은 이러한 문제들을 해결하기 위한 최신 시도 중 일부를 나타낸다. 함께 하여 그들은 밝고 반짝이는 희망을 그린다…

그리고 Apple에 대해서는, 그것들은 가장 높은 등급의 중요한 전략적 실패를 상징한다.

하지만 먼저, 성배부터. 🌟

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

# 성배를 찾는 여정

성배는 언제나 손에 잡히지 않는다. 그것이 성배의 본성이다.

하지만 우리는 오랜 시간 동안 성배를 찾는 것뿐만 아니라 성배 자체인 것을 주장하는 여러 회사와 기술들로부터 다양한 발표를 들어왔다.

초창기의 Java와 Flash부터 Cordova와 Ionic, JavaScript/React에 이르기까지, 그리고 Xamarin과 Flutter와 같은 최근 기술들까지.

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

모든 것이 왕인 자리를 주장했습니다. 우리가 당신입니다. 한 번 작성하고 어디서나 실행해보세요.

하지만 현실에서 멀티 플랫폼 개발은 어렵습니다.

플랫폼은 다른 언어와 API를 사용합니다. 다른 개발 환경과 IDE를 갖고 있습니다. 다른 사용자 인터페이스와 컨트롤을 갖고 있으며, 아마도 더 중요한 것은 그들이 다르게 작동한다는 것입니다.

# 외관과 느낌

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

후반부 내용은 이해하기 쉽고, 사용자 애플리케이션을 바라볼 때 가장 중요한 부분입니다.

내 iOS 앱은 iOS 앱 같이 느껴지나요? 내 Android 앱은 Android 앱처럼 작동하나요? Apple의 사용자 인터페이스 가이드 라인이나 Google의 Material 가이드 라인에 따르나요?

어딘가... 이상하게 느껴지는 게 있나요?

이전에 언급한 대로, 대부분 플랫폼의 컨트롤과 사용자 인터페이스 요소를 모방하고 Skia를 사용해 자체 버전을 렌더링하는 Flutter와 같은 솔루션의 주요 문제입니다.

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

그리고 Compose Multiplatform도 문제가 있습니다. CM은 Skiko를 사용하는데, Skiko는 다시 Skia를 그래픽 렌더링 API로 사용합니다.

일부 응용 프로그램에서는 이러한 불일치가 중요하지 않을 수 있습니다. 그러나 다른 경우에는 그렇지 않을 수 있습니다. 특정 플랫폼의 사용자는 일반적으로 해당 플랫폼에 의해 그 플랫폼을 선택했으며, 응용 프로그램이 일관되고 예측 가능한 방식으로 작동하고 해당 기기의 다른 응용 프로그램 및 서비스와 원활하게 통합되기를 원합니다.

이로써 우리는 미술의 새로운 시작을 향해 나아가게 됩니다.

# Kotlin Multiplatform

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

여기 인용한 웹사이트 내용을 보면요:

애플리케이션의 네트워킹, 데이터 저장 및 유효성 검사, 분석, 계산 및 다른 로직에 대한 단일 코드베이스를 유지하는 것이 아이디어입니다.

그런 다음 Android에서 해당 로직을 사용하거나 iOS로 내보내서 임베드된 라이브러리로 사용합니다.

양 플랫폼은 각각 Compose(Android)와 SwiftUI(iOS)와 같은 자체 인터페이스 라이브러리를 사용하여 해당 코드를 사용합니다.

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

이것은 회사와 사용자 양쪽에게 최상의 이점을 제공한다고 알려져 있어요. 특정 플랫폼에서 일관된 사용자 인터페이스 동작과 여러 플랫폼에서 일관된 논리적 동작이 있어요.

사용자 인터페이스는 두 번 작성되었지만, 핵심 논리, 애플리케이션의 두뇌부는 한 번만 작성되고 확인되었으며 테스트되었어요.

![image](/assets/img/2024-07-13-KotlinMultiplatformComposeMultiplatformApplesStrategicFailure_1.png)

이것은 강력하고 은밀한 개념이에요.

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

하지만 말했듯이, 이는 애플 및 iOS 소프트웨어 개발 미래 비전에 대한 중대한 전략적 실패를 대표하는 것이기도 합니다.

그 이유는 무엇일까요?

**Android 우선 개발**

당연한 일이지만 최대의 문제는 Android 우선 개발이라고 부르는 상황으로 이어진다는 것입니다.

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

All of the core business logic, validation, and networking tasks are handled in Kotlin within Android Studio. Given that converting the library for use in Xcode on iOS involves a time-consuming process, it's more efficient to focus on development and testing primarily on the Android platform.

Staying within the Android ecosystem speeds up the compile-run-test cycle significantly.

Once the functionality is confirmed on Android, the library is exported for integration by iOS developers into their user interface.

This approach results in an application structure and code optimized for Android, with interfaces, services, and APIs written in Kotlin and tailored for a Compose environment.

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

그 라이브러리가 iOS에 도착하면 어떻게 되는지 궁금하신가요?

우선, SwiftUI에서 뷰를 재배치하고 구성하는 것이 훨씬 더 어려워집니다. 왜냐하면 모델과 뷰 모델 구조가 Android용으로 설계되었기 때문이죠.

게다가 iOS 전용 엔티티를 지원하기 위해 해당 구조를 변경하는 것도 어려울 것입니다. 왜냐하면 Android 코드가 이미 테스트되고 완료된 상태이기 때문에 의미 있는 변경 요청은 거부당할 가능성이 높습니다.

그래서 경영진은 iOS 팀에게 어떤 메시지를 전달할까요?

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

"이건 해결해야 할 문제야."

## 언어와 플랫폼의 정체

개발자들과 애플에게 제일 큰 걱정거리인 두 번째 문제는 스위프트나 iOS나 SwiftUI에 추가된 멋진 변화나 기능이 무슨소용이 없다는 거야.

무슨소용이 없다는 이유는 그 변화나 기능이 안드로이드에 존재하지 않기 때문에 iOS 개발자들은 그걸 사용할 수 없기 때문이지.

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

Swift에 async/await를 추가하고 싶나요? Kotlin에서는 그 형태로 존재하지 않기 때문에 completion callbacks으로 사용해야 합니다.

Swift에 macro나 model 지원을 추가하고 싶나요? 좋아요. 그러나 모든 모델과 뷰 모델은 Objective-C 기반의 Kotlin에서 복사된 중간 클래스이기 때문에 그것들을 사용할 수 없습니다.

상태 관리를 위한 연관 타입이 있는 Enumerations? 죄송합니다.

더 얘기할 수 있지만, 아이디어를 받아들였을 거라 믿습니다. Android First Development 세계에서, iOS는 2급 시민이며, 최소 공통 분모 애플리케이션이 규칙입니다.

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

안녕하세요! 오늘은 타로 카드 전문가가 여러분을 찾아왔어요!

안드로이드가 먼저 나왔죠. 📱👾

# 애플의 해결책?

![이미지 설명](/assets/img/2024-07-13-KotlinMultiplatformComposeMultiplatformApplesStrategicFailure_2.png)

그럼 이 상황에 애플은 어떤 대책을 내놨을까요? 🍏🤔

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

카드를 뒤집어보니, 현재 상황에 대해 자세히 말씀드릴게요.

WWDC 2023는 왔다가 지나갔지만, 배송 또는 준비 중인 제품과 관련된 언급은 전혀 없었어요. Apple Vision Pro와 같은 새로운 제품은? 물론 들었죠. 그것에 대해서는 자세히 알려졌어요.

Swift 및 SwiftUI에 새로운 추가 기능이 있었나요? 물론이죠. 두 가지 모두 많은 멋진 새로운 기능들이 있어요... 하지만 대부분의 개발자들이 2024년이나 2025년까지 사용할 수 없을 거에요!

제가 "애플은 또 다시 그랬다"라고 썼던 것처럼, 애플은 Swift와 SwiftUI에 새로운 기술을 추가했고... 그런 다음 그 기술을 iOS 17과 최신 버전의 iPadOS, macOS 등에서만 실행되도록 제한했어요. (그들이 "일치하는 플랫폼"이라고 애정을 담아 부르는 것과 비슷하게요.)

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

애플은 자신들의 플랫폼에서도 사용자 인터페이스 코드를 역호환성 있게 구현하지 못하는데, 여러 플랫폼은 걱정하지 않으셔도 됩니다!

그런데, 혹시 Compose는 어떨까요? SwiftUI vs. Jetpack Compose: 안드로이드가 확실히 이기는 이유에 대해 이야기를 했습니다.

왜 구글과 젯브레인은 이전 버전들을 지원할 수 있는가요?

사실, 경쟁사 플랫폼도 지원할 수 있는 방법이 있다니까요... 애플만이 이것을 못하는 게 이상하지 않나요? ✨

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

이 질문에는 간단한 대답이 있어요.

## 플랫폼

![이미지](/assets/img/2024-07-13-KotlinMultiplatformComposeMultiplatformApplesStrategicFailure_3.png)

애플의 세계에서, 크로스 플랫폼 개발은 여러분의 iOS 앱과 코드를 iPad에서 실행하는 것을 의미해요. 아니면 애플 워치에서도 실행되거나, 애플의 생태계에서 다른 장치에서 실행되는 것을 말해요.

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

하지만 안드로이드에 대해서는 언급이 아예 없었던 것 같아요. 전체 WWDC 키노트 중 Android 플랫폼에 대한 언급이 단 한 번도 없었을 정도입니다.

메타에 대한 언급은 있었나요? 아니요.

VR에 대해서 언급되었나요? 아니요.

AI에 대해서도요? 아니요.

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

애플의 세계에서는 다른 기술과 플랫폼이 단순히 존재하지 않는다.

하지만 문제는, 물론 그들이 존재한다는 것이다.

## 생태계

![2024-07-13-KotlinMultiplatformComposeMultiplatformApplesStrategicFailure_4.png](/assets/img/2024-07-13-KotlinMultiplatformComposeMultiplatformApplesStrategicFailure_4.png)

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

애플은 자사의 생태계를 자랑하기를 좋아합니다.

비전 프로 발표 도중, 애플은 해당 기기 출시 시 iOS 및 iPad 앱이 사용 가능하다고 여러 차례 언급했습니다.

이는 오늘날 대부분의 앱이 iOS 전용으로 개발되어 iOS 툴킷과 플랫폼 도구를 사용했기 때문에 가능한 일입니다.

하지만 만약 다른 생태계에서 허용하는 것에 구애받는다면 어떻게 될까요? 다른 플랫폼의 도구와 기능 제한 때문에 어려움을 겪는다면 어떻게 될까요?

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

### iOS-First 세상에서 개발자들이 개발을 중단하면 어떻게 될까요?

## 기업 채택

맨 위에 썼던 것처럼, KMP 프로젝트는 대부분 기업들에 의해 주도될 것입니다. 이들은 모두를 통치하는 하나의 반지에 기대할 욕망을 가진 기업들일 것입니다. 그들은 "비용 절감"과 "이익 극대화"가 그들의 본성이기 때문이죠.

저는 이런 식으로 제 글에 댓글을 남기는 것을 읽었습니다:

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

그리고 넷플릭스가 코틀린 멀티플랫폼을 공식으로 도입했다는 큰 소식도 있죠.

제 기관에서도 세계에서 가장 큰 금융 기관 중 하나인 고객 중 하나가 이미 KMP를 기반으로 한 현실 성능 검증을 진행 중이라는 사실을 알고 있어요.

그뿐만 아니라 KMP/CM을 실제로 구현하는 것이 예상보다 쉽지 않다는 노력들에 대해 읽어봤어요. 여기서, 여기서, 그리고 여기서 해당 경험에 대해 더 읽을 수 있어요.

개발자들은 모든 통합 문제에 불평할 거예요. 그들은 대부분의 초기 시간을 절약할 수 있다고 설명할 차단된 특정 버그의 위치를 찾기 위해 시스템을 추적하려 하면서 나중에 소진될 것이라고 시도할 거예요.

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

현재 시스템에는 더 많은 주요 종속성이 있다는 점을 지적할 겁니다. 이는 도구 체인을 더욱 취약하게 만들죠. 새로운 iOS 및 iPadOS 버전으로 이동하기를 억제하는데, 도구 체인이 애플의 출시 일정에 따라 따라잡을 때까지.

또는 아직 릴리스되지 않은, 안정성이 보장되지 않은, 큰 지원 커뮤니티가 없는 써드 파티 라이브러리와 개발 프로세스 및 응용 프로그램이 연결되어 있다는 사소한 사실에 대한 언급이 있을지 모르겠어요. 그리고 그 라이브러리는 결국 언제든지 버려질 수도 있죠.

하지만 중요한 건, 성배의 매력이 강하다는 것이죠.

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

만약 애플이 선두에 서지 않는다면, 누군가 다른가에서 이끌 것입니다.

미국에서 애플이 주요 기술 플랫폼인 것은 중요하지 않습니다.

미국에서 대부분의 사용자 장치가 주로 iOS인 것도 중요하지 않습니다.

기업이 iOS 장치를 만들고 고객을 두류 시민으로 취급해 비용을 절약할 수 있다고 생각한다면, 그것이 이루어질 것입니다.

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

만약 CIO에게 모바일 예산을 30%나 40% 또는 심지어 50%까지 줄일 수 있다고 말한다면, 그들이 진지하게 고려하지 않을까요?

물론 그 정도로는 아닐 것이지만...

성밖의 유혹은 강력합니다.

Swift는 다른 플랫폼에서도 실행되며, 그 고유한 디자인 덕분에 SwiftUI도 진정한 크로스 플랫폼 툴킷이 될 수 있었을 텐데요.

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

우리는 iOS 선도 세계에서 살았을 수도 있었습니다.

애플은 여기서 선도할 기회가 있었습니다.

하지만 그들은 포기했습니다.

# 완료된 블록

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

This is a message that signals a deep contemplation.

You hold strong faith in iOS development and the tools provided by Apple. Your belief in SwiftUI as a top-tier user interface library is unwavering.

However, recurrent obstacles placed by Apple have muddled the path for developers. Since WWDC 2020, there has been a trend of inhibiting access to the newly introduced tools, APIs, and advancements.

The current situation is a reflection of the choices and policies adopted by Apple. If you delve deeper into the matter, you may find clarity and insight.

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

더구나 그들이 그것이 여기 있다는 걸 알고 있는지조차 잘 모르겠어.

의견이 있으신가요? 아래에 남겨주세요.

특히, KMP 분야에 발을 담가보려는 경우에는 제게 꼭 알려주세요.

그렇다면, 어떻게 생각하시나요? 적중한가요? 불일치인가요? 제가 놓친 부분이 있나요?

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

언제나 댓글로 알려주세요.

진짜로요. Medium은 홍보와 보상 알고리즘을 변경했기 때문에 박수와 댓글을 남기는 것이 엄청난 차이를 만들어냅니다. 감사합니다!
