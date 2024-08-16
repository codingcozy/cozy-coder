---
title: "이미지 분류를 위한 합성곱 신경망의 역사 1989년  현재"
description: ""
coverImage: "/assets/img/2024-07-06-TheHistoryofConvolutionalNeuralNetworksforImageClassification1989-Today_0.png"
date: 2024-07-06 03:02
ogImage: 
  url: /assets/img/2024-07-06-TheHistoryofConvolutionalNeuralNetworksforImageClassification1989-Today_0.png
tag: Tech
originalTitle: "The History of Convolutional Neural Networks for Image Classification (1989 - Today)"
link: "https://medium.com/towards-data-science/the-history-of-convolutional-neural-networks-for-image-classification-1989-today-5ea8a5c5fe20"
isUpdated: true
---





## 딥 러닝과 컴퓨터 비전 분야에서 가장 큰 혁신들을 시각적으로 살펴보기

CNN 이전에 이미지를 분류하기 위해 신경망을 훈련시키는 표준 방법은 이미지를 픽셀 목록으로 평탄화시키고 피드포워드 신경망을 통해 이미지의 클래스를 출력하는 것이었습니다. 이미지를 평탄화하는 문제는 이미지에 있는 필수적인 공간 정보가 버려진다는 것이었습니다.

1989년, 얀 르쿤과 그의 조직이 합성곱 신경망을 소개했고 이후 15년동안 컴퓨터 비전 연구의 중추적인 역할을 해왔습니다! 피드포워드 신경망과는 달리 CNN은 이미지의 2차원 특성을 보존하며 정보를 공간적으로 처리할 수 있습니다!

이 글에서는 이미지 분류 작업을 위한 CNN의 역사를 특히 살펴보겠습니다. 90년대 초기 연구로부터 시작하여 2010년대 중반의 황금 시대에 이르기까지, 가장 뛰어난 딥 러닝 구조들이 탄생했던 시기를 살펴보고, 이제는 주목과 비전 트랜스포머와 경쟁하면서 CNN 연구의 최신 동향을 논의해보겠습니다.

<div class="content-ad"></div>

이 기사에서 시각적으로 애니메이션과 함께 모든 개념을 설명하는 유튜브 비디오를 확인해보세요. 특별히 언급되지 않는 한, 이 기사에서 사용된 모든 이미지와 일러스트레이션은 비디오 버전을 만들 때 제가 직접 생성했습니다.

![The History of Convolutional Neural Networks for Image Classification 1989-Today](/assets/img/2024-07-06-TheHistoryofConvolutionalNeuralNetworksforImageClassification1989-Today_0.png)

# 합성곱 신경망의 기본

CNN의 핵심은 합성곱 연산입니다. 필터를 이미지 위로 스캔하고 각 겹치는 위치에서 필터와 이미지의 내적을 계산합니다. 이 결과 출력은 특성 맵이라고 하며, 이미지에서 필터 패턴이 얼마나 많이 있으며 어디에 있는지를 포착합니다.

<div class="content-ad"></div>

![CNN](https://miro.medium.com/v2/resize:fit:1400/1*4ZBZhhftMDbMYnsdxzzPfg.gif)

In a convolutional layer, we train multiple filters to extract various feature maps from the input image. By stacking multiple convolutional layers in sequence with some non-linearity, we create a convolutional neural network (CNN).

Each convolutional layer performs two main tasks simultaneously:
1. Spatial filtering by applying convolution operation between images and kernels.
2. Combining multiple input channels to produce a new set of output channels.

It's fascinating that around 90 percent of research in CNNs aims to modify or enhance these two fundamental principles.

<div class="content-ad"></div>

**1989 논문**

이 1989 논문은 역전파를 사용하여 처음부터 비선형 CNN을 훈련하는 방법을 알려주었습니다. 16x16 그레이스케일 손글씨 숫자 이미지를 입력으로 사용하고, 5x5 크기의 12개 필터를 가진 두 개의 합성곱 레이어를 거쳐갑니다. 필터는 이동 거리가 2인 스트라이드로 스캔 중입니다. 스트라이드 합성곱은 입력 이미지를 다운샘플링하는 데 유용합니다. 합성곱 레이어 후에 출력 맵은 평탄화되어서 두 개의 완전 연결 네트워크를 거쳐 숫자 10개에 대한 확률을 출력합니다. 소프트맥스 크로스 엔트로피 손실을 사용하여 네트워크는 손글씨 숫자의 올바른 레이블을 예측하도록 최적화됩니다. 각 레이어 이후에는 탄젠트 하이퍼볼릭 함수(tanh) 비선형도 사용되어 학습된 특징 맵이 더 복잡하고 표현력이 풍부해집니다. 9760개의 파라미터만 사용된 이 네트워크는 오늘날의 수억 개 파라미터를 포함하는 네트워크에 비하면 매우 작은 규모였습니다.

<div class="content-ad"></div>

## 유도 편향

유도 편향은 기계 학습에서의 개념으로, 우리가 모델을 일반화에서 멀리하고 인간과 유사한 이해를 따르는 해결책 쪽으로 더 많이 이끌기 위해 학습 과정에 특정 규칙과 제한을 의도적으로 도입하는 것을 말합니다.

사람이 이미지를 분류할 때도 여러 표현을 형성하기 위해 일반적인 패턴을 찾기 위해 공간 필터링을 하고 그것들을 결합시켜 예측을 만들곤 합니다. CNN 아키텍처는 정확히 그것을 복제하도록 설계되었습니다. 피드포워드 네트워크에서는 각 픽셀이 독립적인 특징으로 다루어지지만, CNN에서는 동일한 필터가 전체 이미지를 스캔하기 때문에 매개변수 공유가 더 많습니다. 유도 편향은 네트워크 설계 때문에 CNN을 더 많은 데이터가 필요로 하지 않게 만들어 줍니다. 그러나 피드포워드 네트워크는 필요한 패턴 인식을 일일이 학습해야 하는 반면에 CNN에서는 그것을 무료로 받아들일 수 있습니다.

# Le-Net 5 (1998)

<div class="content-ad"></div>

1998년, 얀 르쿤과 그의 팀이 Le-Net 5를 발표했습니다. 이 모델은 더 깊고 큰 7층 CNN(합성곱 신경망) 네트워크입니다. 그들은 또한 맥스 풀링을 사용했는데, 이는 이미지를 2x2 슬라이딩 윈도우에서 최댓값을 추출하여 다운샘플링하는 방법입니다.

## 지역 수용장

3x3 합성곱 레이어를 훈련할 때 주목해보세요. 각 뉴런은 원래 이미지에서 3x3 영역에 연결됩니다. 이것이 뉴런의 지역 수용장입니다. 즉, 뉴런이 패턴을 추출하는 이미지 영역입니다.

<div class="content-ad"></div>

이 feature map을 다른 3x3 레이어를 통과시킬 때, 새로운 feature map은 원본 이미지로부터 더 큰 5x5 영역의 수용 영역을 간접적으로 만듭니다. 게다가, 이미지를 max-pooling이나 strided-convolution을 통해 다운샘플링할 때도, 수용 영역이 커지며 — 보다 깊은 레이어가 입력 이미지에 접근하는 범위가 전역적으로 증가합니다.

그 이유로 CNN의 초기 레이어들은 특정 가장자리나 모서리와 같은 낮은 수준의 세부 정보만 추출하는 반면, 후반 레이어들은 보다 분산된 전역 수준의 패턴을 식별합니다.

# 건조한 시기 (1998-2012)

Le-Net-5가 얼마나 인상적인지라도, 2000년대 초반 연구자들은 여전히 신경망이 계산적으로 매우 비용이 많이 들고 많은 데이터가 필요하다고 생각했습니다. 과적합 문제도 있었는데, 복잡한 신경망은 훈련 데이터세트 전체를 단순히 암기하고 새로운 보이지 않는 데이터세트에서의 일반화에 실패했습니다. 대신 연구자들은 작은 데이터세트에서 더 나은 성능을 보이며 훨씬 적은 계산 요구를 가진 SVM과 같은 전통적인 기계학습 알고리즘에 집중했습니다.

<div class="content-ad"></div>

## ImageNet Dataset (2009)

The ImageNet dataset was open-sourced in 2009 — it contained 3.2 million annotated images at the time covering over 1000 different classes. Today it has over 14 million images and over 20,000 annotated different classes. Every year from 2010 to 2017 we got this massive competition called the ILSVRC where different research groups will publish models to beat the benchmarks on a subset of the ImageNet dataset. In 2010 and 2011, traditional ML methods like Support Vector Machines were winning — but starting from 2012 it was all about CNNs. The metric used to rank different networks was generally the top-5 error rate — measuring the percentage of times that the true class label was not in the top 5 classes predicted by the network.

### AlexNet (2012)

AlexNet, introduced by Dr. Geoffrey Hinton and his team was the winner of ILSVRC 2012 with a top-5 test set error of 17%. Here are the three main contributions from AlexNet.

<div class="content-ad"></div>

## 1. Multi-scaled Kernels

알렉스넷은 224x224 RGB 이미지로 훈련되었으며 네트워크에서 여러 커널 크기를 사용했습니다 — 11x11, 5x5, 3x3 커널이 있었습니다. 르넷5와 같은 모델은 5x5 커널만 사용했습니다. 큰 커널은 더 많은 가중치를 훈련하기 때문에 계산 비용이 더 많이 소요되지만, 더 많은 전역적인 패턴을 이미지에서 포착합니다. 이러한 큰 커널로 인해 알렉스넷은 6,000만 개 이상의 훈련 가능한 매개변수를 가졌습니다. 이 모든 복잡성은 과적합으로 이어질 수 있습니다.

![image](/assets/img/2024-07-06-TheHistoryofConvolutionalNeuralNetworksforImageClassification1989-Today_4.png)

## 2. Dropout

<div class="content-ad"></div>

과적합을 줄이기 위해 AlexNet은 Dropout이라 불리는 정규화 기술을 사용했습니다. 훈련 중에 각 계층의 일부 뉴런을 무효화시킵니다. 이렇게 함으로써 네트워크가 특정 뉴런이나 뉴런 그룹에 지나치게 의존하지 않도록 하고 대신 모든 뉴런이 분류에 유용한 일반적인 의미 있는 특징을 학습하도록 장려합니다.

## 3. RELU

AlexNet은 또한 tanh 비선형성을 ReLU로 교체했습니다. RELU는 음수 값을 제로로 만들고 양수 값을 그대로 유지하는 활성화 함수입니다. tanh 함수는 x값이 너무 높거나 너무 낮아지면 기울기가 낮아져 최적화를 느리게 만들기 때문에 깊은 네트워크에서 포화하는 경향이 있습니다. RELU는 tanH보다 약 6배 빠르게 네트워크를 훈련시키기 위한 안정적인 기울기 신호를 제공합니다.

AlexNet은 또한 지역 응답 정규화 개념과 분산된 CNN 훈련 전략을 소개했습니다.

<div class="content-ad"></div>

# GoogleNet / Inception (2014)

2014년에 GoogleNet 논문은 ImageNet의 상위 5위 오류율이 6.67%가 나왔어요. GoogLeNet의 핵심 요소는 inception 모듈이에요. 각 inception 모듈은 서로 다른 필터 크기(1x1, 3x3, 5x5)와 맥스 풀링 레이어가 있는 병렬 컨볼루션 레이어로 구성돼 있어요. Inception은 이러한 커널을 동일한 입력에 적용하고 그 결과를 연결하여 낮은 수준과 중간 수준 특징을 결합해요.

![image](/assets/img/2024-07-06-TheHistoryofConvolutionalNeuralNetworksforImageClassification1989-Today_5.png)

## 1x1 Convolution

<div class="content-ad"></div>

1x1 컨볼루션 레이어를 사용합니다. 각 1x1 커널은 먼저 입력 채널을 조정한 다음 결합합니다. 1x1 커널은 각 픽셀을 고정된 값으로 곱하기 때문에 포인트와이즈(convolutions)로도 불립니다.

3x3 및 5x5와 같은 큰 커널은 공간 필터링과 채널 결합을 모두 수행하지만, 1x1 커널은 채널 혼합에만 유용하며 적은 수의 가중치로 효율적으로 수행합니다. 예를 들어 1x1 컨볼루션 레이어의 3x4 그리드는 (1x1 x 3x4 =) 12개의 가중치만 훈련하며, 3x3 커널이었다면 (3x3 x 3x4 =) 108개의 가중치를 훈련해야 합니다.

## 차원 축소

GoogleNet은 1x1 컨볼루션 레이어를 사용하여 낮은 차원 피쳐 맵에서 3x3 및 5x5 컨볼루션을 실행하기 전에 채널 수를 줄이는 차원 축소 방법으로 사용합니다. 이를 통해 AlexNet에 비해 훈련 가능한 가중치 수를 줄일 수 있습니다.

<div class="content-ad"></div>

# VGGNet (2014)

The VGG Network claims that we don't necessarily need larger kernels like 5x5 or 7x7 networks; all we require are 3x3 kernels. A 2-layer 3x3 convolutional layer provides the same receptive field as a single 5x5 layer. Similarly, three 3x3 layers offer the same receptive field as a single 7x7 layer.

Deep 3x3 Convolution Layers can capture the same receptive field as larger kernels but with fewer parameters!

A single 5x5 filter trains 25 weights, whereas two 3x3 filters train only 18 weights. Moreover, one 7x7 filter trains 49 weights, while three 3x3 filters train just 27. Training with deep 3x3 convolution layers has been the norm in CNN architectures for a considerable period of time.

<div class="content-ad"></div>

## Batch Normalization (2015)

딥 신경망은 훈련 중 "내부 공변량 이동"이라는 문제에 시달릴 수 있습니다. 네트워크의 초기 레이어는 계속해서 훈련되기 때문에 후속 레이어는 이전 레이어로부터 수시로 변동하는 입력 분포에 지속적으로 적응해야 합니다.

Batch Normalization은 이 문제를 극복하기 위해 각 레이어의 입력을 정규화하여 훈련 중에 평균이 0이고 표준 편차가 1이 되도록 하는 것을 목표로 합니다. Batch Normalization 레이어는 모든 합성곱 레이어 뒤에 적용될 수 있습니다. 훈련 중에는 미니배치 차원을 따라 특성 맵의 평균을 빼고 표준 편차로 나눕니다. 이렇게 하면 훈련 중 각 레이어가 더 안정적인 단위 가우시안 분포를 볼 수 있게 됩니다.

<div class="content-ad"></div>

## 배치 정규화의 장점

- 수렴 속도가 약 14배 빨라집니다.
- 더 높은 학습률을 사용할 수 있게 해줍니다.
- 신경망의 초기 가중치에 강건하게 만들어 줍니다.

# ResNets (2016)

## 딥 네트워크의 식별 매핑 어려움

<div class="content-ad"></div>

상상해보세요. 분류 작업에 뛰어난 정확도를 갖는 얕은 신경망이 있다고 합시다. 이 신경망 위에 100개의 새로운 컨볼루션 레이어를 추가한다면, 모델의 학습 정확도가 낮아질 수 있다는 사실이 밝혀졌습니다!

이는 상당히 직관에 반하는데, 모든 이러한 새로운 레이어들이 해야 하는 일은 각 레이어에서 얕은 신경망의 출력을 복사하는 것뿐입니다. 적어도 원래의 정확도와 일치해야 합니다. 실제로 딥 네트워크를 훈련하는 것이 어렵다는 것은 널리 알려져 있습니다. 많은 레이어를 거쳐 백프로파게이션하는 동안 그레이디언트가 포화되거나 불안정해질 수 있기 때문입니다. 우리는 Relu와 배치 정규화를 사용하여 22층의 깊은 CNN을 훈련할 수 있었고, 마이크로소프트의 엔지니어들은 2015년 ResNets를 소개하여 150층의 CNN을 안정적으로 훈련할 수 있게 했습니다. 그들은 무엇을 했을까요?

## 잔차 학습

입력은 일반적으로 하나 이상의 CNN 레이어를 거치지만, 마지막에 원래 입력이 최종 출력에 추가됩니다. 이러한 블록을 잔차 블록이라고 부르는 이유는 이들이 전통적인 의미에서 최종 출력 피쳐 맵을 학습할 필요가 없기 때문입니다. 단지 입력을 복사해서 최종 피쳐 맵을 얻기 위해 더해져야 하는 잔차 피쳐일 뿐입니다. 중간 레이어의 가중치가 자신을 0으로 바꾼다면, 잔차 블록은 단순히 항등 함수를 반환할 것입니다. 즉, 입력 X를 쉽게 복사할 수 있게 될 것입니다.

<div class="content-ad"></div>

## 쉬운 그래디언트 흐름

백프로파게이션 중에 그래디언트는 이러한 바로 가기 경로를 통해 직접 빠르게 모델의 초기 레이어에 도달하여 그래디언트 소멸 문제를 방지하는 데 도움을 줍니다. ResNet은 많은 이러한 블록들을 함께 쌓아서 정확도 손실 없이 깊은 네트워크를 형성합니다!

<div class="content-ad"></div>

그 놀라운 발전 덕분에 ResNets는 이전 모든 기록을 뛰어넘는 Top-5 오류율을 가진 152층 모델을 훈련시키게 되었어요!

## DenseNet (2017)

![image](/assets/img/2024-07-06-TheHistoryofConvolutionalNeuralNetworksforImageClassification1989-Today_9.png)

댄스넷(Dense-Nets)은 네트워크의 이전 레이어들과 나중 레이어들을 연결하는 shortcut paths를 추가했어요. DenseNet 블록은 일련의 컨볼루션 레이어들을 훈련시키는데, 각 레이어의 출력은 블록 내 이전 각 레이어들의 피쳐 맵들과 연결되어 다음 레이어로 전달되기 전에 합쳐지게 돼요. 각 레이어는 이미지가 네트워크를 통과할 때 네트워크의 “총합 지식”에 새로운 피쳐 맵을 작성하게 돼요. 각 레이어는 손실 함수로부터 그라디언트에 직접 접근할 수 있기 때문에 DenseNets는 네트워크 전체를 통해 정보와 그라디언트가 향상된 흐름을 갖게 돼요.

<div class="content-ad"></div>

## 스퀴즈 앤 익스사이테이션 네트워크 (2017)

SEN-NET은 ILSVRC 대회의 최종 우승자로, CNN에 스퀴즈 앤 익스사이테이션 레이어를 소개했습니다. SE 블록은 특성 맵의 모든 채널 간의 종속성을 명시적으로 모델링하도록 설계되었습니다. 일반 CNN에서 특성 맵의 각 채널은 서로 독립적으로 계산됩니다. SEN-Net은 각 특성 맵의 채널이 입력 이미지의 전역 속성에 대해 문맥적으로 인식되도록 self-attention과 유사한 방식을 적용합니다. SEN-Net은 2017년 ILSVRC의 최종 우승을 차지했으며, 154층 SenNet + ResNet 모델 중 하나는 우스꽝스러운 4.47%의 탑-5 오류율을 기록했습니다.

<div class="content-ad"></div>

## 스퀴즈 작업

스퀴즈 작업은 입력 특성 맵의 공간 차원을 전역 평균 풀링을 사용하여 채널 디스크립터로 압축합니다. 각 채널에는 이미지의 지역 속성을 캡처하는 뉴런이 포함되어 있기 때문에 스퀴즈 작업은 각 채널에 대한 전역 정보를 누적합니다.

## 활성화 작업

활성화 작업은 스퀴즈 작업에서 얻은 채널 디스크립터를 사용하여 입력 특성 맵을 채널별 곱셈으로 재조정합니다. 이를 통해 전역 수준의 정보가 각 채널로 전파되어 각 채널을 특성 맵의 다른 채널과 맥락화합니다.

<div class="content-ad"></div>

프렌드리한 톤으로 번역을 해보겠습니다:

이미지 태그를 Markdown 형식으로 변경해보세요.


![MobileNet (2017)](/assets/img/2024-07-06-TheHistoryofConvolutionalNeuralNetworksforImageClassification1989-Today_12.png)

Convolution 레이어는 두 가지를 수행합니다 - 1) 공간 정보 필터링 및 2) 채널별 결합. MobileNet 논문은 Depthwise Separable Convolution을 사용하며, 이 기술은 이 두 작업을 두 개의 다른 레이어로 분리합니다 - 필터링을 위한 Depthwise Convolution 및 채널 결합을 위한 pointwise convolution.

## Depthwise Convolution


<div class="content-ad"></div>

입력 특징 맵 세트와 M채널이 주어졌을 때, 먼저 M개의 3x3 합성곱 커널을 훈련하는 깊이별 합성곱 레이어를 사용합니다. 보통의 합성곱 레이어가 모든 특징 맵에서 합성곱을 수행하는 것과는 달리, 깊이별 합성곱 레이어는 각각 하나의 특징 맵에서만 합성곱을 수행하는 필터를 훈련합니다. 두 번째로, 1x1 포인트별 합성곱 필터를 사용하여 이러한 모든 특징 맵을 섞습니다. 이렇게 필터링과 결합 단계를 분리함으로써 경량화된 모델을 만들 수 있습니다. 무게의 수를 엄청나게 줄이면서도 성능을 유지할 수 있습니다.

# MobileNetV2 (2019)

<div class="content-ad"></div>

2018년에는 MobileNetV2가 MobileNet 아키텍처를 개선했습니다. 이는 Linear Bottlenecks와 Inverted residuals를 도입하여 더욱 혁신적으로 나아갔습니다.

## Linear Bottlenecks

MobileNetV2는 차원 축소를 위해 1x1 포인트별 합성곱을 사용하며, 공간 필터링을 위해 깊이별 합성곱 레이어를 이어서 채널을 다시 확장하기 위한 또 다른 1x1 포인트별 합성곱 레이어를 사용합니다. 이 병목은 RELU를 거치지 않고 선형으로 유지됩니다. RELU는 차원 축소 단계에서 나온 모든 음수 값을 제로화하며, 이는 특히 이러한 낮은 차원 하위 공간의 대부분이 음수였을 경우, 네트워크가 중요한 정보를 잃을 수 있습니다. 선형 레이어는 병목에서 과도한 정보 손실을 방지합니다.

![이미지](/assets/img/2024-07-06-TheHistoryofConvolutionalNeuralNetworksforImageClassification1989-Today_15.png)

<div class="content-ad"></div>

## 역전된 잔차

두 번째 혁신은 역전된 잔차라고 불립니다. 일반적으로, 잔차 연결은 가장 높은 채널을 가진 레이어 간에 발생하지만, 저자들은 병목 레이어 사이에 바로 가기를 추가합니다. 병목은 저차원 잠재 공간 내에서 관련 정보를 포착하며, 이러한 레이어 간의 정보 및 그래디언트의 자유로운 흐름이 가장 중요합니다.

![Image](/assets/img/2024-07-06-TheHistoryofConvolutionalNeuralNetworksforImageClassification1989-Today_16.png)

# Vision Transformers (2020)

<div class="content-ad"></div>

Vision Transformers 또는 ViTs는 트랜스포머가 이미지 분류에서 최신 CNN을 이길 수 있다는 것을 입증했습니다. 트랜스포머와 어텐션 매커니즘은 시퀀스를 모델링하기 위한 고도로 병렬화 가능하고 확장 가능하며 일반적인 아키텍처를 제공합니다. 신경망 어텐션은 딥러닝의 완전히 다른 영역이며, 이 기사에서는 다루지 않겠지만 이 주제에 대해 더 배우고 싶다면 이 YouTube 비디오를 참고해보세요.

## ViTs는 패치 임베딩과 셀프 어텐션을 사용합니다

입력 이미지는 먼저 일정 크기의 패치 시퀀스로 나누어집니다. 각 패치는 독립적으로 CNN을 통해 또는 선형 계층을 통과하여 고정 크기의 벡터로 임베딩됩니다. 이러한 패치 임베딩과 위치 인코딩들은 그리고 그들은 패치 간의 관계를 모델링하고 전체 이미지를 컨텍스트적으로 고려한 새로운 업데이트된 패치 임베딩을 출력합니다.

![이미지](/assets/img/2024-07-06-TheHistoryofConvolutionalNeuralNetworksforImageClassification1989-Today_17.png)

<div class="content-ad"></div>

## 귀납적 편향 vs 일반성

CNN은 이미지에 대한 여러 가지 귀납적 편향을 소개하는 반면, 트랜스포머는 반대로 한다. — 로컬리제이션 없음, 커널 슬라이딩 없음 — 모든 이미지 패치 간의 관계를 모델링하기 위해 일반성과 원시 컴퓨팅에 의존한다. 셀프 어텐션 레이어는 이미지의 모든 패치 간의 전역 연결성을 제공하며 공간적으로 얼마나 떨어져 있는지에 관계없이 작동한다. 귀납적 편향은 작은 데이터셋에서는 효과적이지만, 트랜스포머의 잠재력은 대규모 훈련 데이터셋에 있다. 일반적인 프레임워크는 결국 CNN이 제공하는 귀납적 편향을 능가할 것이다.

![2024-07-06-TheHistoryofConvolutionalNeuralNetworksforImageClassification1989-Today_18](/assets/img/2024-07-06-TheHistoryofConvolutionalNeuralNetworksforImageClassification1989-Today_18.png)

# ConvNext — 2020년대를 위한 컨브넷 (2022)

<div class="content-ad"></div>

이 기사에 포함하면 좋을만한 선택은 Swin Transformers인데, 이건 다른 날에 다루도록 하죠! CNN 기사니까 CNN 논문 중 하나를 마지막으로 살펴보도록 합시다.

![이미지](/assets/img/2024-07-06-TheHistoryofConvolutionalNeuralNetworksforImageClassification1989-Today_19.png)

## VIT와 같이 이미지 패치 모듈화

ConvNext의 입력은 Vision Transformers에서 영감을 받은 패치 전략을 따릅니다. 스트라이드가 4인 4x4 컨볼루션 커널이 다운샘플링된 이미지를 생성하고, 이 이미지가 네트워크의 나머지 부분에 입력됩니다.

<div class="content-ad"></div>

## Depthwise Separable Convolution

Inspired by MobileNet, ConvNext adopts depthwise separable convolution layers. The authors also suggest that depthwise convolution resembles the weighted sum operation in self-attention, as it blends information within each channel in the spatial dimension. Additionally, the 1x1 pointwise convolutions are akin to the channel combination steps in Self-Attention.

## Larger Kernel Sizes

Despite ConvNets traditionally utilizing 3x3 kernels since VGG, ConvNext introduces larger 7x7 filters to encompass a broader spatial context, aiming to approach the all-encompassing global context captured by ViTs, while still preserving the localization principles of CNNs.

<div class="content-ad"></div>

ConvNext 아키텍처의 나머지 부분을 이루는 MobileNetV2 영강화된 인버티드 보텔넥, GELU 활성화, 배치 정규화 대신 레이어 정규화 등이 있습니다.

## 확장성

ConvNext는 depthwise separable convolutions를 사용해 계산 효율적인 방법으로, 시퀀스 길이에 따라 제곱적으로 확장하는 Self-Attention 대신 Convolution을 사용하므로 고해상도 이미지에서 Transformer보다 확장성이 더 뛰어납니다.

![Convolutional Neural Networks for Image Classification](/assets/img/2024-07-06-TheHistoryofConvolutionalNeuralNetworksforImageClassification1989-Today_20.png)

<div class="content-ad"></div>

# 마무리 맺으며!

CNN의 역사는 딥 러닝, 유도 편향, 그리고 컴퓨팅의 본질에 대해 많은 것을 알려줍니다. 최종적으로 어떤 것이 승리할지 - ConvNets의 유도 편향 또는 Transformers의 일반성 - 끝까지 흥미롭게 살펴볼 만합니다. 시각적인 투어를 원하신다면 해당 기사의 동영상 컨텐츠를 확인해보세요. 또한 아래 목록에 있는 개별 논문들도 살펴보세요.

## 참고 문헌

CNN with Backprop (1989): [논문 링크](http://yann.lecun.com/exdb/publis/pdf/lecun-89e.pdf)

<div class="content-ad"></div>

Here are some resources for you to explore different neural network architectures:

- LeNet-5: [LeNet-5 Paper](http://vision.stanford.edu/cs598_spring07/papers/Lecun98.pdf)
- AlexNet: [AlexNet Paper](https://proceedings.neurips.cc/paper_files/paper/2012/file/c399862d3b9d6b76c8436e924a68c45b-Paper.pdf)
- GoogleNet: [GoogleNet Paper](https://arxiv.org/abs/1409.4842)
- VGG: [VGG Paper](https://arxiv.org/abs/1409.1556)

Feel free to delve into these groundbreaking works and deepen your understanding of neural networks!

<div class="content-ad"></div>

Batch Norm: [Link](https://arxiv.org/pdf/1502.03167)

ResNet: [Link](https://arxiv.org/abs/1512.03385)

DenseNet: [Link](https://arxiv.org/abs/1608.06993)

MobileNet: [Link](https://arxiv.org/abs/1704.04861)

<div class="content-ad"></div>

MobileNet-V2: [arXiv link](https://arxiv.org/abs/1801.04381)

Vision Transformers: [arXiv link](https://arxiv.org/abs/2010.11929)

ConvNext: [arXiv link](https://arxiv.org/abs/2201.03545)

Squeeze-and-Excitation Network: [arXiv link](https://arxiv.org/abs/1709.01507)

<div class="content-ad"></div>

Swin Transformers: [Link](https://arxiv.org/abs/2103.14030)