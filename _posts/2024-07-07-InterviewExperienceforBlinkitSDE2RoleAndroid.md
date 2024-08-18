---
title: "Blinkit SDE2 안드로이드 개발자 면접 경험 2024 최신"
description: ""
coverImage: "/milky-road.github.io/assets/no-image.jpg"
date: 2024-07-07 20:04
ogImage:
  url: /milky-road.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "Interview Experience for Blinkit SDE2 Role (Android)"
link: "https://medium.com/@rajagupta07042000/interview-experience-for-blinkit-sde2-role-android-b35907ff0821"
isUpdated: true
---

**Blinkit SDE2 역할 면접** 대화가 친근한 간단한 소개로 시작됐어요. 인터뷰어가 처음부터 편안하게 느끼게 해줬더니 처음에 느낀 긴장감도 풀리는 느낌이었어요.

### 활동 및 프래그먼트 라이프사이클

인터뷰는 먼저 활동 라이프사이클에 대한 심도 깊은 토론으로 시작됐어요. 인터뷰어가 화면 회전 변경과 여러 활동 간 상호 작용과 같은 다양한 시나리오에 대해 물어보셨어요. 그 후에는 프래그먼트 라이프사이클에 대한 철저한 검토가 이어졌는데, 라이프사이클 메서드와 다양한 상황에서의 동작에 대한 다수의 질문이 있었어요.

### 프래그먼트 작업

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

다음은 조각 추가 및 교체 작업에 대한 것이었습니다. 면접관은 이러한 작업의 지식과 fragment 트랜잭션을 관리하는 addToBackStack 메서드의 중요성을 이해하고 싶어했습니다.

## ViewModel

이어서 ViewModel에 대해 자세히 논의했습니다. ViewModel이 구성 변경 사이에 상태를 보존하는 방법, 액티비티에서 인스턴스를 얻는 방법 및 ViewModelProvider의 내부 작업에 중점을 뒀습니다.

## 아키텍처 패턴 및 LiveData

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

The interviewer delved into the distinctions between MVVM and MVP architectural patterns, as well as the significance of LiveData in Android development, emphasizing how these frameworks are practically applied and the advantages they offer.

### Coroutines and Concurrency

Next, we delved into the realm of Kotlin coroutines and concurrency. Our discussion touched on a multitude of angles, such as suspend functions, diverse dispatcher types, SupervisorScope, runOnUiThread, and the disparities between launch and async. Strategies for executing sequential and parallel tasks using coroutines were also explored.

### Advanced Kotlin Concepts

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

인터뷰어는 확장 함수, 인라인 함수, 동반 객체, lazy 및 lateinit와 같은 고급 Kotlin 개념에 대해 이야기했어요. 그리고 확장 함수와 동반 객체 내의 함수를 호출하는 방법의 차이에 대해서도 물었어요.

## 레이아웃

안드로이드에서의 다양한 종류의 레이아웃에 대해 질문했어요. 레이아웃의 성능과 작동 방식에 중점을 두며, 어떤 레이아웃이 가장 빠른지, 그리고 LinearLayout, RelativeLayout, ConstraintLayout, FrameLayout을 효과적으로 사용하는 방법에 대해 논의했어요.

## 프로젝트 토론

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

인터뷰는 내 프로젝트에 대한 상세한 토론으로 마무리되었습니다. 이 주제에 대해 약 30분 정도를 소요했는데, 내가 참여한 프로젝트와 직면한 어려움, 그리고 내가 구현한 해결책에 대해 설명했습니다. 그녀는 특히 내 문제 해결 접근 방식과 기술적 지식을 실제 상황에 어떻게 적용하는지에 대해 흥미를 가졌던 것 같았습니다.

# 요약

인터뷰는 안드로이드 개발 지식을 철저히 검토하는 것으로 진행되었습니다. 핵심 개념 및 실제 시나리오를 다루며, 안드로이드 컴포넌트와 Kotlin 기능의 이론적 측면과 실제 구현의 중요성을 강조했습니다. 인터뷰어는 지식이 풍부하고 인내심이 많았는데, 내 사고 과정을 설명하고 토론 내내 가치 있는 피드백을 제공해주었습니다.

## 팁: 내부 구현에 대해 깊이 파고들어 보세요

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

이러한 면접에서 뛰어나기 위해서는 안드로이드 구성 요소와 코틀린 기능의 내부 구현에 깊이 파고들어야 합니다. 근본적인 메커니즘을 이해하면 복잡한 질문에 대답하고 주제에 대한 강력한 지식을 입증하는 데 도움이 됩니다.
