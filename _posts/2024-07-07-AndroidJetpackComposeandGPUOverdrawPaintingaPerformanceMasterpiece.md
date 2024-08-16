---
title: "Android Jetpack Compose와 GPU 오버드로우 성능 향상을 위한 최적화 방법 "
description: ""
coverImage: "/assets/img/2024-07-07-AndroidJetpackComposeandGPUOverdrawPaintingaPerformanceMasterpiece_0.png"
date: 2024-07-07 23:18
ogImage: 
  url: /assets/img/2024-07-07-AndroidJetpackComposeandGPUOverdrawPaintingaPerformanceMasterpiece_0.png
tag: Tech
originalTitle: "Android Jetpack Compose and GPU Overdraw: Painting a Performance Masterpiece !"
link: "https://medium.com/@hadawalepranav/reducing-gpu-overdraw-in-android-jetpack-compose-5d0920a81958"
isUpdated: true
---





### 소개
Android Jetpack Compose은 안드로이드 애플리케이션을 위한 사용자 인터페이스를 구축하는 개발자들의 방식을 혁신했습니다. 이는 UI 개발에 선언적이고 현대적인 접근 방식을 제공하여 아름답고 반응적인 앱을 쉽게 만들 수 있게 합니다. 그러나 강력한 도구이기 때문에 Jetpack Compose를 사용할 때 부드럽고 효율적인 렌더링을 보장하기 위해 주의 깊게 주의해야 합니다.

Jetpack Compose를 사용할 때 개발자들이 종종 직면하는 일반적인 성능 이슈 중 하나가 GPU 오버드로우입니다. 오버드로우는 GPU가 동일한 픽셀을 여러 번 그리는 경우 발생하며, 이는 자원을 낭비하고 앱의 성능에 영향을 줄 수 있습니다. 이 기사에서는 GPU 오버드로우가 무엇이고 왜 중요한지, 그리고 Jetpack Compose를 사용할 때 어떻게 줄일 수 있는지 알아보겠습니다.

### GPU 오버드로우란 무엇인가요?
GPU 오버드로우는 GPU가 그린 픽셀이 나중에 다른 픽셀에 의해 가려지는 경우 발생합니다. 이는 그려진 픽셀이 최종적으로 사용자에게 표시되는 프레임에서 보이지 않음을 의미합니다. 이 불필요한 렌더링은 추가 GPU 리소스를 소비하여 전력 소모를 증가시키고 휴대폰 등 모바일 장치의 배터리 수명을 단축시킬 수 있습니다.

<div class="content-ad"></div>

GPU Overdraw이 무엇을 걱정하게 만드는지 알고 계십니까? 과도한 GPU 오버드로우는 애플리케이션에 여러 부정적인 결과를 초래할 수 있습니다:

- 성능 저하: 오버드로잉은 GPU에 부담을 줄 수 있어 프레임 속도 하락을 초래하고 앱이 느리고 반응이 둔해 보이게 할 수 있습니다.
- 전력 소비 증가: 불필요한 GPU 렌더링은 더 많은 전력을 소모하게 하여 배터리 수명이 단축되고 기기 열이 증가하게 합니다.
- 시각적 아티팩트: 오버드로우는 시각적 아티팩트나 장애물을 유발할 수 있어 전반적인 사용자 경험과 앱의 지각된 품질에 영향을 줄 수 있습니다.

오버드로우를 식별하는 방법:

![이미지](/assets/img/2024-07-07-AndroidJetpackComposeandGPUOverdrawPaintingaPerformanceMasterpiece_1.png)

<div class="content-ad"></div>

안드로이드 스튜디오에는 오버드로우를 시각화하는 Layout Inspector라는 내장 도구가 제공됩니다. 사용 방법은 다음과 같아요:

- 안드로이드 기기에서 개발자 옵션을 활성화합니다. 개발자 옵션에서 "Debug GPU Overdraw"를 검색하고 "Show overdraw areas"를 선택합니다.
- Layout Inspector를 열고 앱의 실행 중인 인스턴스를 선택합니다.
- 빨강, 분홍, 파랑, 초록으로 강조된 영역을 확인합니다.

- 참색(하이라이트 없음): 해당 픽셀에 오버드로우가 없음을 나타냅니다. 픽셀이 한 번만 그려집니다.
- 파랑(한 번 오버드로우): 파랑 하이라이트는 같은 프레임 내에서 특정 픽셀이 두 번 그려졌음을 나타냅니다. 이는 UI 요소의 겹침이나 비효율적인 렌더링 때문일 수 있습니다.
- 초록(두 번 오버드로우): 초록 하이라이트는 같은 프레임 내에서 픽셀이 세 번 그려졌음을 나타냅니다. 더 높은 수준의 오버드로우를 시사하며 추가 최적화가 필요합니다.
- 분홍(세 번 이상 오버드로우): 분홍으로 강조된 픽셀은 같은 프레임 내에서 네 번 이상 그려졌음을 나타냅니다. 이는 상당한 오버드로우를 나타내며 성능 향상을 위해 즉각적인 조치가 필요합니다.
- 빨강(가장 많은 오버드로우): 빨강으로 강조된 영역은 오버드로우로 가장 심각하게 영향을 받았습니다. 이 픽셀은 다른 영역과 비교하여 가장 많은 수의 그림을 그렸습니다.
# Jetpack Compose에서 GPU 오버드로우를 줄이는 전략

<div class="content-ad"></div>

GPU 오버드로우를 줄이려면 앱의 렌더링 파이프라인을 이해하고 최적화하기 위한 최상의 실천 방법을 적용해야 합니다. Jetpack Compose를 사용할 때 GPU 오버드로우를 최소화하기 위해 사용할 수 있는 몇 가지 전략을 소개합니다:

![image](/assets/img/2024-07-07-AndroidJetpackComposeandGPUOverdrawPaintingaPerformanceMasterpiece_2.png)

- Composable 계층 최적화
불필요하게 많은 Composable을 중첩하는 것은 피하세요. 각 중첩된 레이어는 오버드로우에 기여합니다. 대신 Composable 계층을 펼치는 방법으로 Composable 재사용, 사용자 정의 수정자 활용 또는 Jetpack Compose에서 제공하는 내장 Composable을 활용해 계층을 최적화하세요.

![image](/assets/img/2024-07-07-AndroidJetpackComposeandGPUOverdrawPaintingaPerformanceMasterpiece_3.png)

<div class="content-ad"></div>

**이미지 첨부**

2. 효율적인 컴포저블 사용하기
젯팩 콤포즈는 성능을 최적화한 여러 내장 컴포저블을 제공합니다. 예를 들어, 요소를 배치할 때 여러 Column과 Row 컴포저블을 중첩하는 대신에 Box를 사용하면 컴포저블과 레이어의 수를 줄여 오버드로우를 줄일 수 있습니다.

**이미지 첨부**

**이미지 첨부**

<div class="content-ad"></div>

3. 불필요한 배경 제거하기
뷰 계층 구조를 평가해보세요. 다른 뷰들에 의해 완전히 가려지는 배경이 있는 뷰들이 있나요? 그렇다면 해당 배경을 제거하거나 여러 뷰에 대해 단일 배경을 사용하는 것을 고려해보세요.

![image](/assets/img/2024-07-07-AndroidJetpackComposeandGPUOverdrawPaintingaPerformanceMasterpiece_7.png)

4. 나태한 레이아웃
긴 목록이나 스크롤 가능한 콘텐츠에 대해 LazyColumn이나 LazyRow를 활용해보세요. 이러한 컴포저블은 보이는 항목만 렌더링하여 화면 밖 요소들의 오버드로우를 최소화합니다.

![image](/assets/img/2024-07-07-AndroidJetpackComposeandGPUOverdrawPaintingaPerformanceMasterpiece_8.png)

<div class="content-ad"></div>

## 5. 커스텀 드로어블 최적화
만약 커스텀 드로어블이나 그래픽을 사용하고 있다면, 불필요한 픽셀 렌더링을 줄이기 위해 최적화하세요. 안드로이드의 Vector Asset Studio나 서드파티 도구와 같은 도구를 사용하여 효율적인 벡터 그래픽을 생성하여 오버드로우를 최소화하세요.

## 6. 투명도 줄이기
투명도는 시각적 흥미를 더할 수 있지만, 과도한 사용은 오버드로우를 발생시킬 수 있습니다. 반투명 오버레이나 투명도 설정이 가능한 벡터 드로어블과 같은 대안을 고려해보세요.

<div class="content-ad"></div>

### 7. 고도에 주의하세요
그림자와 고도는 깊이를 더해주지만, 오버드로우에 기여할 수도 있습니다. 이를 분별해서 사용하고 시각적 위계를 달성하는 대안적 방법을 고려해보세요:

- 안쪽 여백과 배경색: 약간의 안쪽 여백과 배경색을 추가하여 고도에만 의존하지 않고 미묘한 구분 효과를 만들어보세요.
- 선 또는 외곽선: 선 또는 외곽선 효과를 사용하여 요소들을 시각적으로 차별화시키며 그림자가 필요 없는 추가적인 레이어를 도입하세요.
- Z-인덱스: 필요하다면 Modifier.zIndex()를 사용하여 요소들의 적절한 레이어 순서를 제어하고 올바로 가려진 레이어가 오버드로우에 기여하지 않도록 보장하세요.

<div class="content-ad"></div>

**8. 클리핑**
컴포저블의 그리기 영역을 제한하기 위해 클립 수정자를 사용하세요. 이렇게 하면 정의된 클립 경계 밖에는 불필요한 렌더링이 방지됩니다.

**9. 상태 없는 컴포저블 우선**
상태 없는 컴포저블은 일반적으로 재구성이 더 빠르고 오버드로우에 기여할 가능성이 적은 편이에요.


<div class="content-ad"></div>

**10. 컴포저 로직 재사용하기**
UI를 재사용 가능한 컴포저로 분해하여 특정 기능을 캡슐화하세요. 이는 코드 유지보수를 촉진하고 중복 렌더링 가능성을 줄입니다.

![이미지1](/assets/img/2024-07-07-AndroidJetpackComposeandGPUOverdrawPaintingaPerformanceMasterpiece_16.png)

![이미지2](/assets/img/2024-07-07-AndroidJetpackComposeandGPUOverdrawPaintingaPerformanceMasterpiece_17.png)

<div class="content-ad"></div>

11. derivedStateOf를 사용하세요
derivedStateOf는 다른 상태에 의존하는 상태를 만드는 데 도움이 되는 유틸리티 함수입니다. 이 함수를 사용하면 리컴포즈를 최적화할 수 있어, 해당 파생 상태가 전체 상태 객체가 아닌 특정 상태가 변경될 때에만 리컴포즈되도록 할 수 있습니다.

![image](/assets/img/2024-07-07-AndroidJetpackComposeandGPUOverdrawPaintingaPerformanceMasterpiece_18.png)

이러한 관행을 채택함으로써, Jetpack Compose에서 성능이 우수하고 시각적으로 매력적인 UI를 만들어 부드럽고 반응적인 사용자 경험을 제공할 수 있습니다. 기억하세요, 성능 최적화는 지속적인 프로세스이며, 이러한 지침은 시작하는 데 튼튼한 기반이 될 것입니다.

컴포즈를 즐기세요!