---
title: "선형대수학 Part-1 2차원 실수 좌표 공간 이해하기"
description: ""
coverImage: "/assets/img/2024-07-09-LinearAlgebraPart-12-DRealCoordinateSpace_0.png"
date: 2024-07-09 10:25
ogImage: 
  url: /assets/img/2024-07-09-LinearAlgebraPart-12-DRealCoordinateSpace_0.png
tag: Tech
originalTitle: "Linear Algebra (Part-1): 2-D Real Coordinate Space"
link: "https://medium.com/@amitsubhashchejara/linear-algebra-part-1-2-d-real-coordinate-space-e5d51a0f4034"
---


![Image](/assets/img/2024-07-09-LinearAlgebraPart-12-DRealCoordinateSpace_0.png)

## 서문

시작되는 새로운 시리즈인 선형 대수에 대해 안내 드리겠습니다. 이 시리즈에서는 대학 수준의 선형 대수를 다루며, 일반적으로 유용하며 머신 러닝에도 도움이 됩니다. 이 시리즈는 초보자를 대상으로 하지는 않습니다. 따라서 고등학교 수준의 선형 대수에 익숙해야 합니다. 저는 선형 대수에 대한 배움과 이해를 공유할 것이며, 여러분이 이 개념을 잘 이해하고 이 시리즈에서 가치를 찾기를 바랍니다. 더 인터랙티브하게 만들기 위해 댓글에 참여하여 토론에 참여해주시기 바랍니다. 그 토론에 함께 참여하고 싶습니다. 이제 여정을 시작해봅시다!

<div class="content-ad"></div>

## 2차원 실 좌표 공간이란?

2차원 실 좌표 공간의 공식적인 정의는 다음과 같습니다:
모든 가능한 실수값 2-튜플의 집합을 2차원 실 좌표 공간이라고 합니다.

하지만 2-튜플이 무엇인가요?
튜플은 숫자의 순서가 정해진 목록이며, 2-튜플은 두 개의 숫자로 이루어진 순서가 정해진 목록입니다.

그러므로 2차원 실 좌표 공간은 실수 집합에서 두 숫자의 가능한 조합을 선택한 것으로 생각할 수 있습니다. 실제로 예를 들어, 우리가 익숙한 2차원 좌표 공간은 실수의 조합인 점들로 이루어져 있습니다. 아래 그림을 보고 2차원 실 좌표 공간을 시각화해 보세요.

<div class="content-ad"></div>

![Image](/assets/img/2024-07-09-LinearAlgebraPart-12-DRealCoordinateSpace_1.png)

여기서 "Space"는 수학적 공간을 가리키는 것이지 물리적 공간을 의미하는 것이 아님을 기억하세요.

실수 좌표 공간은 ℝⁿ으로 표현되며, n은 공간의 차원을 나타냅니다. 따라서 ℝⁿ은 n차원의 실수 좌표 공간을 나타냅니다. 여기서 우리는 2차원 좌표 공간을 다루고 있으므로 n은 2이며, 따라서 2차원 좌표 공간은 ℝ²로 표현됩니다.

#선의 매개변수 표현

<div class="content-ad"></div>

2차원 실 좌표 공간에서 선 L을 아래 그림과 같이 생각해 봅시다.
주의: 벡터는 굵은 이탤릭체로 표시됩니다.

![선 L과 tv](/assets/img/2024-07-09-LinearAlgebraPart-12-DRealCoordinateSpace_2.png)

위 그림에서, L은 한 선이고 tv는 서로 공선이며 선 L에 평행한 무한한 수의 벡터 그룹입니다. 벡터 x를 tv에 더하면 삼각 벡터 합의 법칙을 사용하여 선 L상의 한 점에 도달할 수 있습니다.

따라서, 선 L의 매개 방정식은 다음과 같이 쓸 수 있습니다.
L = ' x + tv | t ∈ ℝ '

<div class="content-ad"></div>

이 식을 이해하는 것은 꽤 간단합니다. 실수로 이루어진 't'의 모든 가능한 값에 대해 tv 벡터가 있으며, tv에 상수 벡터 x를 더하면 우리는 선 L상의 한 점에 도착합니다. 따라서 't'의 가능한 모든 값에 대해 우리는 선 L상의 한 점에 도착하며, 이러한 방식으로 우리는 선 상의 모든 점에 도착할 수 있습니다.

이 정의는 모든 차원에서 작동합니다.

선의 매개 방정식을 이해하면 n 차원의 평면 방정식을 이해하는 데 도움이 될 것입니다. 이것으로 첫 번째 부분이 마무리되었습니다. 함께 배우는 과정을 즐겁게 여기셨기를 바랍니다.