---
title: "Jetpack Compose 실수 10가지 아름다운 UI를 올바르게 만드는 방법"
description: ""
coverImage: "/assets/img/2024-07-13-10JetpackComposeMistakesBuildingBeautifulUIstheRightWay_0.png"
date: 2024-07-13 00:31
ogImage: 
  url: /assets/img/2024-07-13-10JetpackComposeMistakesBuildingBeautifulUIstheRightWay_0.png
tag: Tech
originalTitle: "10 Jetpack Compose Mistakes🤥: Building Beautiful UIs the Right Way"
link: "https://medium.com/@niranjanky14/10-jetpack-compose-mistakes-building-beautiful-uis-the-right-way-464844a1381d"
isUpdated: true
---





**아래는 Compose 개발을 더 부드럽고 효율적으로 만들기 위해 피해야 할 10가지 흔한 함정이 있습니다:**

1. **과도한 재구성:**

리스트에 사용자 이름과 아바타를 표시하는 항목이 있다고 상상해보세요. 리스트 내에서 발생하는 작은 변경이 모든 항목의 전체 재구성을 유발한다면 성능 문제로 이어질 수 있습니다.

<div class="content-ad"></div>

💁🏻‍♂️ 피하세요! 효율적인 스크롤링 리스트를 위해 LazyColumn이나 LazyRow와 같은 기술을 활용해보세요. 특정 데이터 변경에 따라 다시 구성을 최적화하기 위해 remember나 derivedStateOf와 같은 기술도 고려해볼 가치가 있어요. 아래 예시를 확인해보세요:

![Jetpack Compose Mistakes](/assets/img/2024-07-13-10JetpackComposeMistakesBuildingBeautifulUIstheRightWay_1.png)

## 2. 상태 관리 잘못 사용하기:

Jetpack Compose은 UI 상태를 관리하기 위한 다양한 방법을 제공해요 (예: mutableStateOf, viewModel). 적절하지 않은 방식을 선택하면 복잡성과 예상치 못한 동작을 야기할 수 있어요.

<div class="content-ad"></div>

💁🏻‍♂️Dodge it: 간단한 로컬 상태에는 mutableStateOf를 사용하세요. UI 전체에 걸쳐 복잡한 상태 관리를 위해서는 ViewModel이나 MutableStateFlow 기반의 라이브러리와 같은 상태 관리 솔루션을 고려해보세요. 아래는 예시입니다:

![Jetpack Compose Mistakes](/assets/img/2024-07-13-10JetpackComposeMistakesBuildingBeautifulUIstheRightWay_2.png)

## 3. Composable Constraints를 무시하기:

Composable은 부모 레이아웃으로부터 받은 제약을 고려해야 합니다. 이를 무시하면 예상치 못한 크기 조정이나 레이아웃 문제가 발생할 수 있습니다.

<div class="content-ad"></div>

💁🏻‍♂️Dodge it: Your composables should pay attention to the constraints given and utilize modifiers like size or fillMaxSize to establish their size within the layout. Let me show you an example:

## 4. Overusing Modifiers:

Even though modifiers are effective for styling UIs, excessive use can lead to cluttered and difficult-to-maintain code.

💁🏻‍♂️Dodge it: Strive for clarity and brevity. Think about creating custom composables to encompass common styling patterns. Here’s an example:

<div class="content-ad"></div>

## 5. 컴포저블 함수에서의 부작용:

컴포저블 함수는 주로 UI를 설명하는 데 초점을 맞춰야 합니다. 네트워크 호출이나 데이터베이스 상호작용과 같은 부작용을 직접 컴포저블 내에 넣는 것은 예상치 못한 동작과 테스트의 어려움으로 이어질 수 있습니다.

💁🏻‍♂️피하기: LaunchedEffect나 SideEffect와 같은 기술을 사용하여 컴포저블 내에서 부작용을 다루세요. 이를 이용하면 부작용이 적절하게 트리거되고 UI 렌더링 로직과 분리되어 있음을 보장할 수 있습니다. 아래는 예시입니다:

![image](/assets/img/2024-07-13-10JetpackComposeMistakesBuildingBeautifulUIstheRightWay_3.png)

<div class="content-ad"></div>

## 6. 접근성을 무시합니다:

접근성 있는 UI를 만드는 것은 포용성을 위해 매우 중요합니다. Jetpack Compose는 UI가 모두에게 사용 가능하도록 하는 도구를 제공합니다.

💁🏻‍♂️피하기: contentDescription, 의미론적 요소, 그리고 플랫폼별 접근성 가이드라인을 따르는 등의 접근성 기능을 사용하세요. 다음은 예시입니다:

[이미지](/assets/img/2024-07-13-10JetpackComposeMistakesBuildingBeautifulUIstheRightWay_4.png)

<div class="content-ad"></div>

## 7. 리스트에 대한 키 사용을 잊지 마세요:

Compose에서 리스트를 작업할 때 각 항목에 고유한 키를 사용하는 것이 효율적인 업데이트와 애니메이션에 중요합니다.

💁🏻‍♂️피해 가기: 항상 각 항목에 고유한 키를 제공하세요. 이렇게 하면 Compose가 어떤 항목이 변경되었는지 식별하고 UI를 효율적으로 업데이트할 수 있습니다. 예시는 다음과 같습니다:

![Compose Example](/assets/img/2024-07-13-10JetpackComposeMistakesBuildingBeautifulUIstheRightWay_5.png)

<div class="content-ad"></div>

### 8. 레이아웃 구성을 위해 수식어를 오용하는 것:

수식어는 기본 레이아웃 작업에 사용될 수 있지만, 복잡한 레이아웃은 Row, Column 또는 Box와 같은 전용 레이아웃 조합 요소로 처리하는 것이 더 나을 수 있습니다.

💁🏻‍♂️ 피해 가기: UI의 전반적인 구조를 정의할 때는 레이아웃 조합 요소를 사용하세요. 수식어는 스타일링 및 소량의 조정에 더 적합합니다. 다음은 예시입니다:

![이미지](/assets/img/2024-07-13-10JetpackComposeMistakesBuildingBeautifulUIstheRightWay_6.png)

<div class="content-ad"></div>

### 9. 사전에 구축된 Composables 활용하지 않기:

Jetpack Compose는 공통 UI 요소(버튼, 텍스트 필드 등)를 위한 풍부한 사전 구축 Composables 집합을 제공합니다. 시간을 절약하고 일관성을 확보하기 위해 이를 활용하세요.

💁🏻‍♂️피할 수 있어: 모든 것을 빈틈없이 구축하기 전에 사용 가능한 사전 구축 Composables를 탐색하세요. 여기에 예시가 있습니다:

### 10. 문서화 및 테스트 소홀히 하기:

<div class="content-ad"></div>

적절한 문서화와 테스트는 Compose 코드베이스를 유지하고 개선하는 데 중요합니다.

💁🏻‍♂️피하기: Composable을 명확하게 문서화하여 목적, 사용법 및 동작을 설명하세요. Composable 함수에 대한 유닛 테스트를 작성하여 다양한 입력 상태에 따라 올바르게 렌더링되는지 확인하세요.

참고: 이러한 실수를 이해하고 이러한 팁을 채택함으로써 Jetpack Compose 마에스트로가 되는 길에 한 발짝 더 나아갈 수 있을 겁니다! 계속 연습하고 새로운 기능을 탐색하며 아름답고 기능적인 사용자 인터페이스를 구축하세요.🌟