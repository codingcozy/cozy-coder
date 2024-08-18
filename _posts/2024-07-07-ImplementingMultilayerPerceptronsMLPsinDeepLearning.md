---
title: "딥러닝에서 멀티레이어 퍼셉트론MLP 구현하는 방법"
description: ""
coverImage: "/assets/img/2024-07-07-ImplementingMultilayerPerceptronsMLPsinDeepLearning_0.png"
date: 2024-07-07 19:51
ogImage:
  url: /assets/img/2024-07-07-ImplementingMultilayerPerceptronsMLPsinDeepLearning_0.png
tag: Tech
originalTitle: "Implementing Multilayer Perceptrons (MLPs) in Deep Learning"
link: "https://medium.com/@ckekula/implementing-multilayer-perceptrons-a-mathematical-guide-c528ea89704d"
isUpdated: true
---

이전 글을 따라오셨다면, 신경망이 어디에서 시작되었는지, 그 아키텍처가 무엇이며 출력을 어떻게 계산하는지 알고 있을 겁니다. 또한 역전파 알고리즘에 대해서도 배웠을 거예요. 그렇다면 정확히 이것들로 어떤 일을 할 수 있을까요?

# 회귀 MLPs

첫째, MLPs는 회귀 작업에 사용될 수 있습니다. 하나의 값을 예측하고 싶다면(예를 들어, 집의 가격을 여러 기능을 통해 예측하는 경우), 하나의 출력 뉴런만 필요합니다: 출력은 예측된 값입니다. 다변량 회귀(즉, 한 번에 여러 값 예측)의 경우, 각 출력 차원마다 하나의 출력 뉴런이 필요합니다.

예를 들어, 이미지상에서 개체의 중심을 찾기 위해 2D 좌표를 예측해야 한다면, 두 개의 출력 뉴런이 필요합니다. 개체 주위에 바운딩 박스를 두고 싶다면, 두 개의 숫자가 더 필요합니다: 개체의 너비와 높이입니다. 따라서 4개의 출력 뉴런이 필요하게 됩니다. 일반적으로, 회귀를 위한 MLP를 생성할 때,

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

- 출력 뉴런에는 활성화 함수를 사용하고 싶지 않을 수 있습니다. 그렇게 하면 값을 자유롭게 출력할 수 있습니다. 하지만 출력이 항상 양수임을 보장하고 싶다면 ReLU 활성화 함수나 출력 레이어에서 소프트플러스 활성화 함수를 사용할 수 있습니다.
- 예측값이 특정 범위 내에 떨어지도록 보장하려면 로지스틱 함수나 쌍곡 성형 탄젠트를 사용하고 레이블을 해당 범위에 맞게 스케일링하면 됩니다: 로지스틱 함수의 경우 0에서 1까지, 쌍곡 성형 탄젠트의 경우 -1에서 1까지의 범위로 스케일링합니다.
- 훈련 중에 사용해야 할 손실 함수는 일반적으로 평균 제곱 오차를 사용하지만, 훈련 세트에 이상치가 많은 경우 평균 절대 오차를 사용하는 것이 좋습니다. 또는 두 가지를 결합한 퓨버 손실을 사용할 수도 있습니다.

## 분류 MLPs

MLPs는 분류 작업에도 사용될 수 있습니다. 이진 분류 문제의 경우 로지스틱 활성화 함수를 사용하는 단일 출력 뉴런만 있으면 됩니다: 출력은 0에서 1 사이의 숫자가 되며, 이를 긍정 클래스의 추정 확률로 해석할 수 있습니다. 분명히, 부정 클래스의 추정 확률은 그 수에서 뺀 값입니다.

MLPs는 멀티 레이블 이진 분류 작업도 쉽게 처리할 수 있습니다. 예를 들어 각 수신 이메일이 햄(정상)인지 스팸인지 예측하고 동시에 긴급 또는 비긴급 이메일인지 예측하는 이메일 분류 시스템이 있을 수 있습니다. 이 경우 로지스틱 활성화 함수를 사용하는 두 개의 출력 뉴런이 필요합니다: 첫 번째는 이메일이 스팸일 확률을 출력하고 두 번째는 긴급일 확률을 출력합니다.

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

일반적으로, 각 긍정적인 클래스에 대해 하나의 출력 뉴런을 할당합니다. 출력 확률이 반드시 1의 합이 되지 않는다는 점을 유의하세요. 이렇게 하면 모델이 레이블의 어떤 조합이든 출력할 수 있습니다: 비긴급 햄, 긴급 햄, 비긴급 스팸, 아마도 긴급 스팸일 수도 있어요 (비록 그것이 아마 오류일 것입니다) 😅.

각 인스턴스가 3개 이상의 가능한 클래스 중 하나에만 속할 수 있는 경우(예: 0부터 9까지의 클래스가 있는 이미지 분류), 각 클래스당 하나의 출력 뉴런이 필요하며 전체 출력 레이어에 소프트맥스 활성화 함수를 사용해야 합니다:

![image](/assets/img/2024-07-07-ImplementingMultilayerPerceptronsMLPsinDeepLearning_0.png)

소프트맥스 함수는 모든 추정된 확률을 0부터 1 사이로 보장하고 이들이 1의 합이 되도록(클래스가 서로 배타적인 경우 필요) 합니다. 이를 다중 클래스 분류라고 합니다.

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

## 소프트맥스 회귀

소프트맥스 회귀는 로지스틱 회귀 모델의 일반화된 형태로, 복수의 클래스를 바로 지원할 수 있게끔 만들어진 모델입니다. 이를 통해 여러 개의 이진 분류기를 훈련하고 결합할 필요 없이 바로 다중 클래스를 지원할 수 있습니다.

아이디어는 꽤 단순합니다: 주어진 인스턴스 x가 있을 때, 소프트맥스 회귀 모델은 먼저 각 클래스 k에 대한 점수 sk(x)를 계산한 후, 이 점수에 소프트맥스 함수(정규화된 지수 함수로도 불립니다)를 적용하여 각 클래스에 대한 확률을 추정합니다.

인스턴스 x에 대해 각 클래스의 점수를 모두 계산한 후, 이를 소프트맥스 함수에 적용하여 해당 인스턴스가 클래스 k에 속할 확률 pk를 추정할 수 있습니다: 이 함수는 각 점수의 지수값을 계산한 후 이를 정규화(모든 지수값의 합으로 나누기)합니다. 이 점수들은 일반적으로 로짓이라고 부릅니다.

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

비선형 회귀 분석에서 사용되는 Softmax 회귀 분류기는 최고의 예측 확률을 갖는 클래스를 예측합니다 (최고 점수를 갖는 클래스).

손실 함수에 대해선, 우리가 확률 분포를 예측하기 때문에, 교차 엔트로피 (로그 손실이라 불리는)가 일반적으로 좋은 선택지입니다. 이는 모델이 목표 클래스에 대해 낮은 확률을 추정할 때 패널티를 부여하기 때문입니다. 교차 엔트로피는 추정된 클래스 확률이 목표 클래스와 얼마나 잘 일치하는지를 측정하는 데 자주 사용됩니다.

## 요약

회귀 MLPS:

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

Classification MLPs:

![Image](/assets/img/2024-07-07-ImplementingMultilayerPerceptronsMLPsinDeepLearning_2.png)

# Keras로 MLPs 구현하기

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

Keras는 간단하게 다양한 신경망을 만들고 훈련하고 평가하며 실행할 수 있는 고수준 딥러닝 API입니다. 자세한 내용은 https://keras.io에서 확인하실 수 있어요. 현재 세 가지 인기있는 오픈 소스 딥러닝 라이브러리 중에서 선택할 수 있어요: TensorFlow, Microsoft Cognitive Toolkit (CNTK), 또는 Theano.

더불어 TensorFlow 자체에는 tf.keras라는 Keras 구현이 번들로 제공돼요. 이 구현은 TensorFlow만을 백엔드로 지원하지만, 매우 유용한 추가 기능을 제공하는 장점이 있어요.

예를 들어, TensorFlow의 Data API를 지원하여 데이터를 효율적으로 로드하고 전처리하는 것이 꽤 쉬워졌어요.

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

## 순차적 API

이것은 하나의 층 스택으로만 구성된 신경망을 위한 가장 간단한 종류의 케라스 모델입니다.

## 함수형 API

순차적이지 않은 신경망의 한 예는 Wide & Deep 신경망입니다. 이 신경망 아키텍처는 2016 논문에서 소개되었습니다. 입력의 모든 부분이나 일부가 출력 층에 직접 연결됩니다.

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

![Implementing Multilayer Perceptrons in Deep Learning](/assets/img/2024-07-07-ImplementingMultilayerPerceptronsMLPsinDeepLearning_4.png)

This architecture allows the neural network to grasp both complex patterns (via the deep path) and simple rules (through the short path). Unlike a regular MLP where data flows through all layers, potentially distorting simple patterns, this design provides a more nuanced approach to learning.

**Subclassing API**

Both the Sequential API and the Functional API are declarative: you outline the layers and their connections before training or inferring. This method offers numerous benefits:

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

- 모델은 쉽게 저장, 복제, 공유할 수 있고, 구조를 표시하고 분석할 수도 있습니다. 프레임워크는 모양을 유추하고 유형을 확인하므로 오류를 일찍 잡을 수 있습니다 (예: 어떠한 데이터도 모델을 통과하기 전에). 또한 전체 모델이 레이어를 갖는 정적 그래프이기 때문에 디버깅하기도 상대적으로 쉽습니다.

하지만 그의 반대편이 정적이라는 점입니다. 일부 모델은 루프, 변동 형태, 조건 분기 및 기타 동적 행위를 포함합니다. 이러한 경우나 좀 더 명령형 프로그래밍 스타일을 선호하는 경우 Subclassing API가 적합합니다.

MLP에 대한 내용은 여기까지입니다! 다음 글에서는 심층 신경망에 대해 이야기해보겠습니다. 읽어 주셔서 감사합니다 🎉
