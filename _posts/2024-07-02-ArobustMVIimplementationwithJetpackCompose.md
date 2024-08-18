---
title: "Jetpack Compose로 강력한 MVI 구현 방법"
description: ""
coverImage: "/assets/img/2024-07-02-ArobustMVIimplementationwithJetpackCompose_0.png"
date: 2024-07-02 23:08
ogImage:
  url: /assets/img/2024-07-02-ArobustMVIimplementationwithJetpackCompose_0.png
tag: Tech
originalTitle: "A robust MVI implementation with Jetpack Compose"
link: "https://medium.com/proandroiddev/a-robust-mvi-with-jetpack-compose-e08882d2c4ff"
isUpdated: true
---

# 왜 이를 하고 있는 걸까?

나는 학습 곡선이 낮고 모든 개발자들에게 인식되는 견고한 아키텍처를 개발하기 위해 노력해왔습니다.

목표는 프로젝트에 독립적인 것을 가지는 것이었습니다. 간단히 말해, 우리는 모든 개발자가 아키텍처에 익숙해져 다른 프로젝트에 쉽게 기여할 수 있도록 하고 싶었습니다. 그들이 필요할 때 다른 프로젝트에 더 쉽게 참여할 수 있도록.

아키텍처를 구축하는 데에는 시간이 걸립니다. 나는 대부분의 시간을 안드로이드 생태계와 그 최고의 실천 방법들, 그리고 이전 권장 사항에서 새로운 것들로의 전환 등을 배우는 데에 할애했습니다.

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

## 내가 어떻게 할 것인가?

코드 한 줄도 작성하기 전에, 우리가 너무 다양한 방향으로 헤딩하지 않도록 몇 가지 사항을 확인해야 합니다.

- 좋은 컵 `선호하는 음료`을 준비한다.
- MVI가 실제로 무엇인지 연구한다.
- 목표 행동을 정의한다.
- 실제 구현을 코드화한다.
- 실제 예제에서 결과를 테스트한다.

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

# 연구 단계

만약 여러분이 MVI가 무엇을 의미하는지 알고 있다고 가정한다면 (또는 지금까지 그것을 피해 왔다면), 간단히 모두를 속도에 맞게 잡아보겠습니다.

MVI는 Model-View-Intent의 약자입니다. 이 아키텍처는 MVVM 및 기타 모델과 함께 MV\* 패밀리의 일환입니다. 이것의 핵심 원칙은 입력 Intent를 받아들이는 상태 기계를 가지고 있는데, 이 기계는 UI의 기저 상태를 나타내는 View State를 생성합니다. 이 모든 것을 통해 MVI는 MVVM과는 달리 이전 버전인 MVVM와 다르게 SSoT (Single Source of Truth) 원칙을 존중합니다.

![이미지](/assets/img/2024-07-02-ArobustMVIimplementationwithJetpackCompose_0.png)

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

# 목표 행동

먼저, 구현의 기초로 제공하는 스크린이 필요합니다. 이를 위해 이 곳에서 찾을 수있는 Now In Android 애플리케이션을 사용할 것입니다. 첫 번째 스크린샷에만 집중할 것이며(ForYouScreen), 다양한 데이터 요소를 표시하고 상호 작용할 수 있습니다.

이제 MVI의 핵심 개념인 Reducer를 설명해야 합니다! Reducer는 어떤 의미에서 UI와 ViewModel 사이의 계약입니다. 이는 두 부분으로 나뉘어집니다:

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

**상태(State), 이벤트(Event) 및 효과(Effect) 인터페이스**

- **reduce** 함수 (수학적 reduce 연산자와 혼돈하지 말아야 함)

## ViewState

이것은 UI의 표현입니다. 이론적으로, Compose 화면을 표시하는 데 필요한 모든 것이 포함되어 있어야 합니다. 이는 각각이 화면의 다른 상태를 나타내는 여러 미리 보기를 쉽게 만드는 장점이 있습니다.

## ViewEvent

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

이 문구는 MVI의 핵심으로, 모든 사용자 상호작용과 더불어 약간의 더 많은 기능을 포함합니다. 이것이 ViewModel에 의해 상태 변경을 트리거하는 데 사용될 것입니다.

**ViewEffect**

이것은 ViewEvent의 특별한 유형입니다. 그 역할은 ViewModel에 의해 UI로 발화되는 것입니다. 네비게이션, 스낵바/토스트 메시지 표시와 같은 동작을 포함합니다.

**reduce 함수**

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

**타로 전문가**입니다. 제도로 도와 드리겠습니다.

### 감소 함수

*감소 함수*는 **ViewState**와 **ViewEvent**를 사용하여 새로운 **ViewState**와 제공된 이벤트와 연결된 _ViewEffect_(사용자 효과)를 생성합니다.

이제 *Reducer*가 준비되었으니, 모든 *ViewModels*에 의해 구현될 *BaseViewModel*을 정의해야 합니다:

### 구현

베이스 구조가 설정되고 준비된 상태이므로 이제 화면을 위해 이를 실제로 구현하는 시간입니다!

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

## 리듀서

먼저 ViewState를 정의하여 우리가 일어날 이벤트를 쉽게 식별할 수 있도록 합시다.

이제 ViewState가 생겼으니, 사용자 상호작용을 처리하고 그에 따라 상태를 업데이트하기 위해 ViewEvent를 정의할 수 있습니다.

위의 내용은 ViewState와 함께 명백하다고 생각합니다. 그러나 마지막 3개의 이벤트는 그렇지 않을 수 있습니다.

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

위의 텍스트를 한국어로 번역하겠습니다:

이미지 태그를 Markdown 형식으로 변경합니다.

화면의 클릭 가능한 요소에 다음과 같은 방식으로 연결됩니다:

- UpdateTopicIsFollowed은 녹색 윤곽 요소에 연결됩니다.
- UpdateNewsIsSaved는 주황색 윤곽 요소에 연결됩니다.
- UpdateNewsIsViewed는 보라색 윤곽 요소에 연결됩니다.

이제 MVI 아키텍처의 마지막 부분인 ViewEffect를 정의해야 합니다! 다행히도 이 화면에서는 가능한 효과가 두 가지밖에 없으므로 비교적 간단합니다.

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

마침내 우리에겐 구현해야 할 reduce 함수가 있습니다. 대부분의 경우, reduce 함수는 매우 간단할 것이며 입력 ViewEvent 데이터를 수정된 ViewState로 매핑할 것입니다.

여기서 주목해야 할 중요한 점(특히 마지막 3개의 ViewEvent 경우)은 실제 프로덕션 애플리케이션에서 Reducer에서 표시되는 실제 데이터를 직접 수정하는 경우는 매우 드물다는 것입니다.

## ViewModel

이제 우리에겐 Reducer가 있으니, 다음과 같이 보이는 ViewModel에서 사용해야 합니다:

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

저희 ViewModel을 보시면, 코드 양이 매우 적다는 것을 알 수 있을 겁니다. 그것은 바로 우리가 사용할 주요 기능을 포함하는 BaseViewModel을 사용하기 때문입니다.

## 화면

마침내, 이 모든 것을 화면에 적용하여 UI에서 발생할 수 있는 다양한 경우를 처리하는 것이 얼마나 간단한지 확인할 수 있을 겁니다.

# 여기까지입니다!

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

우리가 한 모든 작업으로 인해, 이제 SSoT 원칙을 준수하면서도 확실히 정의된 MVI 아키텍처와 UI를 직접 나타내는 ViewState를 활용하는 것이 가능해 졌습니다!

아마도 여러분 중 일부는 이미 시험되고 검증된 기존 아키텍처가 있는데 왜 이 모두를 겪고 있는지 궁금해할지도 모르겠습니다. 간단히 말해서, 필요했기 때문입니다. 여러 프로젝트에서 작업하고, 여러 프로젝트 매니저 아래에서 다른 개발자들과 함께 작업합니다.

이러한 프로젝트들에서 리드 개발자로서 종종 PR들을 검토해야 합니다 (또는 GitLab 사용자들에게는 MR들을). 종종 작은 아키텍처적 실수에 대한 주석을 지적해야 할 때가 많습니다. 이런 실수들은 해당 아키텍처에 대한 보다 깊은 이해를 통해 피할 수 있는 일들입니다.

그래서 내가 일하는 모든 프로젝트와 호환되며 내 동료들에게 널리 알려지고 인정받는 아키텍처를 상상한다면, 전체적인 검토 과정이 모든 관련 당사자들에게 빠르고 쉽게 진행되며 전반적인 개발 속도를 높일 수 있습니다!

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

다음 링크에서 전체 프로젝트를 찾을 수 있어요:

MVI에 대한 많은 다른 훌륭한 기사들이 있어요. 관심이 있다면 몇 개를 소개해 드릴게요:
