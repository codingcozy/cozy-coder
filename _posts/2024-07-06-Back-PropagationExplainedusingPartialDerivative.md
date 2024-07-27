---
title: "부분 도함수를 사용한 역전파 쉽게 이해하기"
description: ""
coverImage: "/assets/img/2024-07-06-Back-PropagationExplainedusingPartialDerivative_0.png"
date: 2024-07-06 11:00
ogImage: 
  url: /assets/img/2024-07-06-Back-PropagationExplainedusingPartialDerivative_0.png
tag: Tech
originalTitle: "Back-Propagation Explained using Partial Derivative"
link: "https://medium.com/@sagar4999/back-propagation-explained-using-partial-derivative-25279e52931c"
---


#### 백프로파게이션: 심층학습의 기본 컴포넌트

백프로파게이션은 신경망의 심층학습에서 핵심 구성 요소입니다. 오늘날 우리가 보는 신경망의 기초로 볼 수 있습니다. 백프로파게이션의 원래 형태는 1970년대에 소개되었지만, 루멜하트, 힌튼, 윌리엄스에 의해 1988 논문 "오류 역전파를 통한 표현 학습"에서 더 깊은 신경망 교육에 더 적합하고 더 빠른 알고리즘을 개발할 수 있었습니다.

백프로파게이션을 구글로 검색하면 정확히 무엇인지와 어떻게 작동하는지 설명하는 많은 좋은 기사들을 찾을 수 있습니다. 그래서 여기서는 조금 다른 접근 방식으로 백프로파게이션의 작동을 부분 미분과 수동 계산을 통해 설명하려고 합니다. 본 기사는 당신이 신경망과 미분에 대한 기본적인 이해를 가지고 있다고 가정합니다.

백프로파게이션 알고리즘은 두 단계로 구성됩니다: -

<div class="content-ad"></div>

**1. Forward pass**

이제 우리는 훈련 예제를 네트워크를 통과시켜 예측을 만듭니다.

**2. Backward pass**

가중치의 그래디언트를 계산하기 위해 오차를 네트워크를 통해 역방향으로 전파합니다.

<div class="content-ad"></div>

이해하기 위해 간단한 네트워크를 고려해 보겠습니다. 숨겨진 레이어가 없는 네트워크입니다.

![Network Diagram](/assets/img/2024-07-06-Back-PropagationExplainedusingPartialDerivative_1.png)

X₁,X₂는 두 개의 입력값입니다. W₁, W₂는 두 개의 가중치이며 b는 편향값입니다.

출력 레이어가 시그모이드 활성화 함수를 사용한다고 가정해 봅시다.

<div class="content-ad"></div>

/assets/img/2024-07-06-Back-PropagationExplainedusingPartialDerivative_2.png

우리가 조사하고 있는 것은 각 입력과 해당 가중치의 곱의 합인 s입니다.

S = X₁×W₁ + X₂×W₂ + b — — — — — (여기서 b는 편향)

간단한 훈련 예시를 보겠습니다.

<div class="content-ad"></div>

그 리딩을 시작해 볼까요?

X₁= 0.1, X₂= 0.3, 원하는 결과 = 0.03

시작 가중치,

W₁ = 0.5, W₂ = 0.2, b = 1.83

전방 전달(pass)

<div class="content-ad"></div>

S = X₁×W₁ + X₂×W₂ + b

S = 0.1 × 0.5 + 0.3 × 0.2 + 1.83

S = 1.94

그런 다음 시그모이드 활성화 함수를 적용하면,

<div class="content-ad"></div>

이렇게하면 위에서 언급한대로 임의로 초기화된 가중치와 편향으로 학습 예제를 전달하면 0.8743이 예측 값으로 나옵니다.

이제 오차 함수가 제곱된 오차라고 가정해 봅시다.

![이미지](/assets/img/2024-07-06-Back-PropagationExplainedusingPartialDerivative_4.png)

<div class="content-ad"></div>

**뒤로 걸어가기**

이제, 이 오차를 최소화하기 위해 변경할 수 있는 유일한 매개변수는 가중치와 편향입니다. 역전파는 각 가중치가 오차와 어떻게 관련되는지를 알려주며, 그 방법은 편미분입니다. 뒤로 가기는 기본적으로 각 가중치가 전체 오차에 얼마나 기여했는지를 결정하고 이에 맞게 조정합니다. 순방향과 역방향을 반복적으로 수행함으로써, 네트워크는 가중치를 조정하여 정확한 예측을 할 수 있는 능력을 향상시킵니다.

왜 편미분을 사용해야 할까요?

함수의 편미분(두 개 이상 변수의 경우)은 다른 변수들을 모두 상수로 유지하면서 한 변수에 대한 도함수입니다.

<div class="content-ad"></div>

멀티변수 함수인 Y = X²Z + H에 대해, X의 변화가 Y에 미치는 영향은 무엇인가요? 이를 위해 X에 대한 Y의 편미분을 가져와야 합니다. 이때 다른 모든 변수를 일정한 값으로 유지합니다.

![image](https://miro.medium.com/v2/resize:fit:632/1*Tvp0RVn_nupnJrMqCceJ6w.gif)

신경망의 경우, 에러 함수가 가중치에 의존하기 때문에 목표는 오차에 대한 W₁ 및 W₂의 영향을 계산하는 것입니다.

![image](https://miro.medium.com/v2/resize:fit:206/1*dh9ycx6HdraOA4G8VKFE2A.gif)

<div class="content-ad"></div>

하지만 예측 오차(E)와 가중치(W₁ 및 W₂)가 동시에 나타나는 직접적인 방정식이 없으므로 우리는 곱셈 연쇄 법칙을 사용합니다.

![image](https://miro.medium.com/v2/resize:fit:538/1*JAPK7MCIgiS13aVdlHDpSg.gif)

원하는 항은 일정하며,

![image](https://miro.medium.com/v2/resize:fit:348/1*YFZnx28fBV2Yrfg3rvh5Qg.gif)

<div class="content-ad"></div>

s = X₁×W₁ + X₂×W₂ + b

편미분을 이용한 s의 변화 is 0, 상수이기 때문에 생략할 수 있어요. Bias는 학습률만을 이용해 업데이트돼요.

![Back-PropagationExplainedusingPartialDerivative_5](/assets/img/2024-07-06-Back-PropagationExplainedusingPartialDerivative_5.png)

그래서 W₁와 W₂의 가중치 변화가 예측 오차에 미치는 영향을 계산하려면, 위에서 언급한 세 개의 중간 편미분 값을 계산한 후 체인 곱셈 규칙을 적용해야 해요. 그래서 공식은 다음처럼 변해요,

<div class="content-ad"></div>

여기 한 장씩 계산해 보겠어요.

![이미지1](/assets/img/2024-07-06-Back-PropagationExplainedusingPartialDerivative_7.png)

![이미지2](/assets/img/2024-07-06-Back-PropagationExplainedusingPartialDerivative_8.png)

<div class="content-ad"></div>

/assets/img/2024-07-06-Back-PropagationExplainedusingPartialDerivative_9.png

So, by utilizing equations 1 and 2,

![](https://miro.medium.com/v2/resize:fit:620/1*UeDiMFyppD0TqPOefk1Q1Q.gif)

![](https://miro.medium.com/v2/resize:fit:620/1*b-En22be-NcgPQL7rZZ8rQ.gif)

<div class="content-ad"></div>

Derivatives showing a positive trend indicate that boosting the weights can also elevate the error.

When we adjust the weights and bias, we initiate a significant shift in the neural network's performance and efficiency. Here are the images detailing backpropagation explained through partial derivatives:

![Image 1](/assets/img/2024-07-06-Back-PropagationExplainedusingPartialDerivative_10.png)

![Image 2](/assets/img/2024-07-06-Back-PropagationExplainedusingPartialDerivative_11.png)

<div class="content-ad"></div>

아래는 새 가중치와 바이어스를 기반으로 새 예측과 오차를 계산할 것입니다.

![Back-PropagationExplainedusingPartialDerivative_13](/assets/img/2024-07-06-Back-PropagationExplainedusingPartialDerivative_13.png)

![Back-PropagationExplainedusingPartialDerivative_14](/assets/img/2024-07-06-Back-PropagationExplainedusingPartialDerivative_14.png)

<div class="content-ad"></div>

새로운 오류가 조금 이전 것보다 나아졌어요. 오류의 개선은 학습 속도가 작기 때문에 작지만, 높은 학습 속도가 더 나은 것은 아니라는 뜻이에요. 학습 속도는 모델이 문제에 얼마나 빨리 적응하는지를 제어해요. 작은 학습 속도는 가중치에 가해지는 변화가 적기 때문에 더 많은 훈련 epoch를 필요로 하지만, 큰 학습 속도는 빠른 변화를 가져와 더 적은 훈련 epoch를 필요로 해요. 너무 큰 학습 속도는 모델이 너무 빨리 지나치게 최적해에 수렴하게 할 수 있고, 너무 작은 학습 속도는 과정을 멈출 수 있어요.

그래서 이 간단한 예시를 통해 역전파 과정이 어떻게 진행되는지 이해했어요. 여러분은 이 연습을 반복하려면 다양한 오차 함수와 활성화 함수를 사용할 수 있어요.

끝으로

기본적인 역전파 과정을 이해하는 것은 재발/폭발 그래디언트 문제를 이해하는 데 유용하답니다. 또한 역전파는 오차 함수가 볼록해야 하며, 비볼록인 경우 국소 최적해에서 멈출 수 있어요. 오차 함수와 활성화 함수는 모두 미분 가능해야 해요.