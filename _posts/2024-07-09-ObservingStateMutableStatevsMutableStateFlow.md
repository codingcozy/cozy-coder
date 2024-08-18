---
title: "상태 관찰 MutableState와 MutableStateFlow 비교"
description: ""
coverImage: "/assets/img/2024-07-09-ObservingStateMutableStatevsMutableStateFlow_0.png"
date: 2024-07-09 10:39
ogImage:
  url: /assets/img/2024-07-09-ObservingStateMutableStatevsMutableStateFlow_0.png
tag: Tech
originalTitle: "Observing State: MutableState vs MutableStateFlow"
link: "https://medium.com/@gtxtreme/observing-state-mutablestate-vs-mutablestateflow-abaf73b36021"
isUpdated: true
---

이 기사에서는 가볍고 간결하게 읽고 배운 내용을 전해 드리겠습니다.

우리가 사용할 수 있는 많은 observable 패러다임을 지원하는 클래스와 라이브러리들 때문에 때로는 어떤 무기가 전쟁에서 우리를 승리로 이끌어 줄지 혼란스러울 수 있습니다.

MutableState

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

만약 신중히 살펴보면, 이름이 시사하는 대로, 이는 변경 가능한 상태를 나타냅니다. 여기서 상태는 Compose에서 데이터의 기본 단위로 이해될 수 있습니다. 그렇다면, Compose와 매우 밀접한 연관이 있는 클래스이고, ViewModel에서 사용하기 시작하면, 내일 새로운 프레임워크가 나와도 이를 이동하기가 어려워질 것입니다. 많은 사람들이 종종 도메인과 같은 다른 레이어에서도 사용합니다. Kotlin Multiplatform과 같은 도구들의 출현으로 안드로이드 특정 또는 Compose 특정 종속성을 제거하기가 어려워질 것입니다. 그렇다면 어떻게 해야 할까요?

MutableStateFlow

MutableStateFlow는 flow 패키지의 일부로 제공되며 Kotlin 언어의 일부입니다. 그래서 위에서 언급한 문제가 해결됩니다. 보통 UI 프레임워크가 변경될 수 있거나, 실행 중인 하드웨어가 변경될 수도 있지만, 앱을 이동할 때 사용하는 언어는 변경되지 않을 것입니다. MutableStateFlow는 스레드 안전하며, 여러 스레드가 흐름 상태를 업데이트할 수 있고, 이를 우아하게 처리할 것입니다. 현실에서 경주에서 이기는 것은 좋지만, 앱 내에서 경주가 시작되면 그것을 추적하는 것은 매우 어려울 수 있습니다. 게다가 collectAsStateWithLifecycle 메서드를 사용할 수 있어서, 앱이 백그라운드에 있을 때 작업이 우아하게 중지되고, 다시 백그라운드로 돌아오면 다시 시작될 것입니다. 이에 대해 더 알아보려면 여기를 읽어보세요. 또한 리포지토리나 도메인 레이어와 같은 다른 레이어에서도 사용할 수 있으며, 많은 라이브러리가 흐름과 함께 작동하는 것을 지원합니다.

그래서 이 기사를 정리해 드리겠습니다.

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

- MutableStateFlow은 라이프사이클을 고려해 만들어져 있어서, ViewModel에서 UI로 전송되는 데이터에 좋은 선택지가 될 수 있어요. MutableState는 다소 사용자 친화적이지 않아요.
- MutableStateFlow는 순수한 코틀린 Flow 기반 API로, 모든 레이어에서 안전하게 사용할 수 있어요. MutableState를 사용하려면 UI 레이어가 항상 Compose여야 함을 보장해야 해요.
- MutableState는 UI 레이어 자체에 선언된 일회성 상태 변수에 좋은 옵션일 수 있어요.

보너스

LiveData를 기억하시나요?
LiveData는 훌륭한 API였지만, 앱이 순수하게 코틀린으로 작성되었고 Java가 관여되지 않은 경우, ViewModel에 LiveData가 필요하지 않아요. LiveData는 다른 복잡성을 도입하며 UI를 Android로 가져와야 한다는 문제가 발생할 수도 있어요. LiveData를 Compose 호환 MutableState로 변환할 수 있는 좋은 상호 운용 솔루션이 있어요. 왜냐하면 우리는 예전 친구들을 완전히 버리면 안 되기 때문이에요

![image](/assets/img/2024-07-09-ObservingStateMutableStatevsMutableStateFlow_1.png)

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

그게 다야, 친구들.
새로운 것을 배웠으면 좋겣다. 🌟
