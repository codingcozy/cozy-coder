---
title: "분산 스케일링의 저주 이해해야 할 5가지 핵심 포인트"
description: ""
coverImage: "/assets/img/2024-07-07-TheCurseofVarianceScaling_0.png"
date: 2024-07-07 02:56
ogImage: 
  url: /assets/img/2024-07-07-TheCurseofVarianceScaling_0.png
tag: Tech
originalTitle: "The Curse of Variance Scaling"
link: "https://medium.com/@plalindia01/the-curse-of-variance-scaling-53ad2db51283"
---


이 컨텐츠를 통해 나는 왜 자기 주의가 '스케일드 닷 프로덕트 어텐션'이라고 불리는지에 대한 이유를 특히 타겟팅하고 싶어합니다.

기억해주세요: 이것은 튜토리얼이 아니라, 그저 제가 이해한 바를 담은 것입니다.

## 변이의 성격

변이는 수학적 양성의 많은 고통스러운 점이 되는 간단한 이동과 스케일링 속성을 갖고 있습니다.

<div class="content-ad"></div>

- 변화는 변화의 영향을 받지 않습니다. 예를 들어:
var('1,2,3,4') = 1
var('2,3,4,5') = 1 [1씩 이동한 데이터 포인트]
- 스케일링은 다음과 같이 분산에 영향을 줍니다:
var('1,2,3,4') = 1
var('3,6,9,12') = 9 [1로 스케일 조정된 데이터 포인트]

Var(aX + b) = a²Var(X)

고차원 벡터는 더 높은 분산을 얻고, 저차원 벡터는 더 낮은 분산을 얻습니다.

## 문제

<div class="content-ad"></div>

동적이고 스마트하며 컨텍스트에 맞는 단어 임베딩을 만드는 과정에서 솔프 어텐션을 사용합니다. 이때 고차원 벡터, 이러한 벡터의 행렬 및 그들의 내적과 관련된 매트릭스를 다룹니다. 

이러한 고차원 벡터는 높은 분산의 행렬을 생성합니다. SoftMax 함수로 전달할 때, 낮은 극단의 데이터 포인트는 더욱 소멸됩니다. 이로 인해 학습이 상위 극단으로 편향되며, 하위 극단의 데이터 포인트는 무시되어 가중치가 업데이트되지 않는(전형적인 소실 그라디언트 문제) 문제가 발생합니다.

## 해결책

우리는 스케일링을 합니다!

1차원 벡터 X에 대해, 분산이 var(X)라고 가정해봅시다.

<div class="content-ad"></div>

`2차원 벡터 Y에 대한 분산은 2.var(Y)입니다. 이제 1/sqrt(2)로 스케일을 조정한 2차원 벡터 Y에 대한 분산은 var(Y)가 됩니다. 따라서 self-attention에서는 차원의 제곱근으로 점곱을 스케일링하여 다루기 쉽고 훈련을 가능한 한 안정적으로 유지합니다. 그래서 이제 self-attention의 방정식인 Attention(Q, K, V) = SoftMax(QK`)V는 다음과 같이 표현됩니다:`

<div class="content-ad"></div>

![The Curse of Variance Scaling](/assets/img/2024-07-07-TheCurseofVarianceScaling_0.png)