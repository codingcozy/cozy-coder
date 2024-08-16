---
title: "Jetpack Compose를 사용한 Android 애플리케이션에서 BFF와 동적 UI 활용법"
description: ""
coverImage: "/assets/img/2024-07-06-LeveragingBFFandDynamicUIinAndroidApplicationswithJetpackCompose_0.png"
date: 2024-07-06 03:19
ogImage: 
  url: /assets/img/2024-07-06-LeveragingBFFandDynamicUIinAndroidApplicationswithJetpackCompose_0.png
tag: Tech
originalTitle: "Leveraging BFF and Dynamic UI in Android Applications with Jetpack Compose"
link: "https://medium.com/@basaransuleyman/leveraging-bff-and-dynamic-ui-in-android-applications-with-jetpack-compose-27d81edb7c7c"
isUpdated: true
---





기사를 시작하기 전에 BFF와 Dynamic UI에 대해 간단히 소개한 후, 안드로이드용 Jetpack Compose를 사용하여 사용 및 장점에 대해 논의하고, 마지막으로 코드 도전 과제를 다룰 것입니다.

Backend for Frontend (BFF)는 모바일 애플리케이션의 고유한 요구 사항을 충족시키기 위해 특별히 설계된 디자인 패턴입니다. 모바일 요구 사항에 최적화된 백엔드 레이어를 제공함으로써, BFF는 보다 성능이 탁월하고 확장 가능한 애플리케이션의 개발을 용이하게 합니다.

Dynamic UI 또는 Remote Configured UI로도 알려진 것은 전통적인 백엔드의 기능을 확장하여 데이터뿐만 아니라 UI를 어떻게 렌더링해야 하는지 제어하는 것입니다. 이 접근 방식은 Google Play와 같은 플랫폼에서 애플리케이션의 새 릴리스 없이 백엔드 업데이트를 통해 UI 변경을 허용하는 등 의미 있는 장점을 제공합니다.


![이미지](/assets/img/2024-07-06-LeveragingBFFandDynamicUIinAndroidApplicationswithJetpackCompose_0.png)


<div class="content-ad"></div>

# BFF와 Dynamic UI의 이점

**승인**: BFF와 Dynamic UI 패턴을 결합하면 Jetpack Compose가 안드로이드 애플리케이션의 개발 경험과 기능을 크게 향상시킬 수 있습니다.

## - 단순화된 개발

Jetpack Compose는 보일러플레이트 코드를 줄여 개발자들이 비즈니스 로직에 더 집중할 수 있게 해줍니다. BFF와 결합하면 백엔드 레이어가 모바일 앱의 요구에 특별히 맞춰져 데이터 처리가 더 효율적으로 이루어집니다.

<div class="content-ad"></div>

## - 데이터 처리 최적화

젯팩 콤포즈를 사용하면 앱이 필요한 데이터만 검색하고 처리하여 처리 시간을 최소화합니다. 그 효율적인 재구성 메커니즘은 업데이트된 부분만 다시 그리기 때문에 성능을 더욱 향상시킵니다.

## - 유연성 및 반응성 강화

동적 UI는 응용 프로그램을 다시 배포할 필요 없이 사용자 인터페이스를 실시간으로 업데이트할 수 있습니다. 이는 빠른 프로토타이핑, A/B 테스팅 및 사용자 피드백 구현에 특히 유용합니다. 젯팩 콤포즈의 선언적인 특성을 활용하여 백엔드에서 모바일로 변경 사항을 통합하는 과정이 원활해집니다.

<div class="content-ad"></div>

## - 코드 유지 보수성

백엔드가 데이터와 UI 구성을 관리하고, 모바일 앱이 UI를 렌더링하는 데 집중하는 관심사 분리는 젯팩 콤포즈의 컴포넌트 기반 아키텍처와 잘 어울려 있어 코드의 유지 보수성을 향상시킵니다.

## - 콘텐츠 관리

BFF와 젯팩 콤포즈를 사용하면 백엔드에서 UI 컴포넌트의 속성을 동적으로 관리할 수 있어 앱을 재배포하지 않고도 UI 변경이 가능해집니다. 백엔드에서 가한 변경사항이 즉시 앱에 반영되어 사용자 경험을 지속적으로 업데이트할 수 있습니다. 이 접근 방식은 콘텐츠 관리, 동적 폼, 그리고 다양한 사용자 그룹을 위해 사용자 지정 UI를 제공하는 데 특히 적합합니다.

<div class="content-ad"></div>


![Image 1](/assets/img/2024-07-06-LeveragingBFFandDynamicUIinAndroidApplicationswithJetpackCompose_1.png)

Let’s focus on the Home page, a vital component across all applications in the industry. Visualize the top of the image, featuring a banner area at the top, followed by Horizontal Slidable, Vertical Scrollable, and Flex-box components.

![Image 2](/assets/img/2024-07-06-LeveragingBFFandDynamicUIinAndroidApplicationswithJetpackCompose_2.png)

Imagine we have successfully communicated with the backend for BFF and Dynamic UI integration. We then receive a response like this:


<div class="content-ad"></div>

![image](/assets/img/2024-07-06-LeveragingBFFandDynamicUIinAndroidApplicationswithJetpackCompose_3.png)

## 응답의 장점들

백엔드 유연성: 응답에서 유형과 다른 속성을 변경함으로써, 모바일 UI를 동적으로 조정하여 민첩성과 반응성을 제공할 수 있습니다.

구체적인 섹션 알 필요 없음: 클라이언트는 각 섹션의 세부 내용을 알 필요가 없습니다. 각 섹션을 그들의 유형에 따라 처리하면 됩니다.

<div class="content-ad"></div>

유형 기반 처리: 클라이언트는 JSON 응답에서 제공된 유형에 따라 섹션을 동적으로 처리합니다. 이는 클라이언트가 새로운 섹션 유형을 백엔드에서 코드를 변경하지 않고 추가할 수 있음을 의미합니다. 클라이언트가 새로운 유형을 처리하는 방법을 알고 있으면 됩니다.

섹션 데이터를 위한 단일 객체: 각 섹션 데이터는 단일 객체 (sectionData)로 캡슐화됩니다. 이는 클라이언트 측 로직이 일관된 데이터 구조를 처리하므로 단순화됩니다.

![Image](/assets/img/2024-07-06-LeveragingBFFandDynamicUIinAndroidApplicationswithJetpackCompose_4.png)

**중요: 백엔드 서비스를 개발하고 반환된 JSON을 직접 만들었기 때문에 null 검사가 없습니다.**

<div class="content-ad"></div>

우리는 테이터 레이어와 도메인 레이어에서 데이터를 다루는 부분에 대한 자세한 내용은 이전 글에서 설명했기 때문에 이를 넘어가겠습니다. 대신, 이동하는 데이터의 응답에 대한 동적인 변화를 강조하고 모바일에서의 데이터 응답 클래스를 공유하겠습니다. 이미지의 제목에서 메시지를 확인하고 Jetpack Compose에 초점을 맞출 것입니다. 

*참고: 객체 내부의 필드에 대해 고민할 필요는 없습니다. 이는 가짜 json입니다.

API를 통해 운반된 데이터는 데이터 레이어의 데이터 소스, 저장소, 도메인 레이어의 유즈 케이스와 매퍼를 거쳐 아래와 같이 프리젠테이션에 도달하며, 뷰 모델에서 처리됩니다.

![이미지](/assets/img/2024-07-06-LeveragingBFFandDynamicUIinAndroidApplicationswithJetpackCompose_5.png)

<div class="content-ad"></div>

# 홈 화면 구성

컴포저블 화면에서 뷰 모델 상태가 수집되며, 데이터가 컨텐츠로 전송되며 빨간 화살표로 표시됩니다.

![이미지](./assets/img/2024-07-06-LeveragingBFFandDynamicUIinAndroidApplicationswithJetpackCompose_6.png)

콘텐츠 레이어를 다음과 같이 보여주고 데이터 전송을 더 잘 이해할 수 있도록 했습니다:

<div class="content-ad"></div>

## 홈 내용

![이미지](/assets/img/2024-07-06-LeveragingBFFandDynamicUIinAndroidApplicationswithJetpackCompose_7.png)

여기서는 SectionList Compose로 넘어가기 전에 UI 모델이 어떻게 처리되는지 살펴보겠습니다. HomeSectionAdapterItem 클래스는 위에서 언급한 다양한 유형 및 섹션을 처리하기 위해 작성된 추상 클래스입니다. 이 클래스를 상속하는 클래스들은 각 섹션 유형에 대해 필요한 데이터 및 viewType를 정의합니다. BFF로부터 받은 데이터는 여기에 채워지고 Composables로 전달됩니다.

viewType: 각 섹션 유형에 대한 BFF에서 핸드셰이크 식별자인 정수 값입니다.

<div class="content-ad"></div>

## UI Model

![UI Model](/assets/img/2024-07-06-LeveragingBFFandDynamicUIinAndroidApplicationswithJetpackCompose_8.png)

마침내, 저희는 이 구조를 Compose에서 다루는 것에 대해 이야기해 볼까요? 이것은 이 글에서 가장 중요한 부분 중 하나입니다. 예전 시스템을 이어가야 한다면, 메인 XML 파일, 각 섹션을 위한 XML 파일, 어댑터, 그리고 각 섹션에 오는 데이터를 위한 뷰 홀더를 작성해야 합니다. 이는 대략 세 배 더 많은 클래스를 생성해야 한다는 것을 의미합니다.

## Compose 섹션

<div class="content-ad"></div>


![Image](/assets/img/2024-07-06-LeveragingBFFandDynamicUIinAndroidApplicationswithJetpackCompose_9.png)

- **onEvent**: UI 이벤트를 처리하는 람다 함수입니다.
- **items**: 각 섹션마다 목록 항목을 생성하는 함수입니다.
- **key**: 각 항목에 고유한 키를 설정합니다. 이는 Compose가 각 요소를 정확하게 관리하는 데 도움이 됩니다.

`when` 구조를 사용하여 섹션 유형별로 키를 결정하면 Compose가 각 요소를 정확하게 관리하고 성능을 향상시킬 수 있습니다. LazyColumn 콘텐츠의 두 번째 `when` 구조는 우리가 이전에 매핑한 섹션 중 어느 것이든 UI에서 BFF 순서로 그려지도록 합니다.

그리고 마지막으로, 각 어댑터 및 뷰 홀더와 그들의 XML 대신에 샘플 섹션 구조가 있습니다.


<div class="content-ad"></div>

P.S. 오류를 최소화하려면 키를 자체 구조에 따라 처리할 수 있습니다.

## 섹션 예시


![VerticalItemCard](/assets/img/2024-07-06-LeveragingBFFandDynamicUIinAndroidApplicationswithJetpackCompose_10.png)

위 섹션의 내용인 VerticalItemCard를 자신만의 디자인에 맞게 구성할 수 있습니다.

<div class="content-ad"></div>

**P.S.** `foreach`를 사용한 이유는 JSON을 개발한 사람이기 때문에 데이터의 수를 알고 있다는 것입니다.

## 동적 UI

그렇다면, 동적 UI를 어떻게 다루어야 하는지 알아봅시다. Jetpack Compose를 사용하여 동적 폼을 만드는 방법을 살펴보겠습니다. JSON 응답에 텍스트 크기, 텍스트 색상, 테두리 색상 및 선택적 아이콘이 있는 동적 속성이 포함되어 있다고 가정하겠습니다. 이러한 속성을 사용하여 동적 카드를 만들어보겠습니다.

우선, 동적 속성을 포함하도록 JSON 구조를 확장해봅시다.

<div class="content-ad"></div>

# 카드 공유 방법

우리는 이러한 속성들을 컴포저블 함수에서 사용하여 동적 카드를 만들 것입니다:

![Image](/assets/img/2024-07-06-LeveragingBFFandDynamicUIinAndroidApplicationswithJetpackCompose_11.png)

<div class="content-ad"></div>

# 결론

이 방법은 개발을 단순화하는 것뿐만 아니라 실시간으로 업데이트할 수 있는 동적이고 유연한 사용자 인터페이스를 제공합니다. BFF, Dynamic UI 및 Jetpack Compose를 함께 활용함으로써, 개발자는 더 민첩하고 유지보수가 용이하며 효율적인 안드로이드 애플리케이션을 만들 수 있습니다.

자세한 구현 및 코드 예제는 GitHub 저장소를 방문해 주세요.

![이미지](/assets/img/2024-07-06-LeveragingBFFandDynamicUIinAndroidApplicationswithJetpackCompose_12.png)