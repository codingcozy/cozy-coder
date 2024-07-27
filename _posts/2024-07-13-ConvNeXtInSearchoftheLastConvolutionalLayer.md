---
title: "ConvNeXt 마지막 컨볼루션 레이어를 찾아서"
description: ""
coverImage: "/assets/img/2024-07-13-ConvNeXtInSearchoftheLastConvolutionalLayer_0.png"
date: 2024-07-13 22:57
ogImage: 
  url: /assets/img/2024-07-13-ConvNeXtInSearchoftheLastConvolutionalLayer_0.png
tag: Tech
originalTitle: "ConvNeXt: In Search of the Last Convolutional Layer"
link: "https://medium.com/gitconnected/convnext-in-search-of-the-last-convolutional-layer-da801d9f123b"
---


이미지 링크를 Markdown 형식으로 변경해주세요:
![image1](/assets/img/2024-07-13-ConvNeXtInSearchoftheLastConvolutionalLayer_0.png)

2010년대는 딥 러닝의 발전에 의해 특징 지어졌습니다. Convolutional Neural Networks의 부흥으로 특징화되는 시대였어요.

역전파 학습이 적용된 ConvNets의 발명은 1980년대로 거슬러 올라가며 발표되었습니다.
Handwritten Digit Recognition with a Back-Propagation Network라는 학술 논문에서 소개되었어요.

![image2](/assets/img/2024-07-13-ConvNeXtInSearchoftheLastConvolutionalLayer_1.png)

<div class="content-ad"></div>

비록 알고리즘은 1989년에 소개되었지만, 실제 시각적 특징 학습에 대한 참된 가능성이 ImageNet 대규모 시각 인식 챌린지(ILSVRC)에서 2012년 말에 드러났습니다.

Alex Krizhevsky, Ilya Sutskever, 그리고 Geoffrey Hinton이 개발한 AlexNet 구조는 이 분야를 혁신적으로 바꾸었습니다. 전통적인 알고리즘을 능가하여 최상위 5개 에러율이 15.3%로 달성되었는데, 이는 최고 경쟁자의 26.2%보다 훨씬 낮았습니다.

이후 이 분야는 빠르게 발전되었고, 대표적인 ConvNet 구조들이 개발되었습니다:

- VGGNet
- Inception
- ResNeXt
- DenseNet
- MobileNet
- EfficientNet
- RegNet

<div class="content-ad"></div>

ConvNeXt 논문에서 언급한 내용이에요:

## Inductive bias

![Image](/assets/img/2024-07-13-ConvNeXtInSearchoftheLastConvolutionalLayer_2.png)

합성곱 신경망(CNNs)은 아래와 같은 귀납 바이어스를 가지고 있어요:

<div class="content-ad"></div>

- 현지성: ConvNets는 이미지의 지역 영역이 이미지 내용을 이해하는 데 더 중요하다고 가정합니다. 따라서 이미지 위를 컨벌루션하는 작은 필터(커널)를 사용합니다. 이러한 필터는 가장자리, 질감, 색상과 같은 지역 패턴을 포착합니다. 이는 높은 수준의 기능의 기본 구성 요소입니다.
- 변환 동질성: 이미지 내 객체가 움직이면 그 피쳐 맵에서의 표현도 같은 방식으로 이동합니다. 이를 통해 CNN은 객체의 위치와 상관없이 객체를 감지할 수 있습니다.
- 계층 구조: 합성곱 신경망은 계층적으로 기능을 학습하도록 설계되었습니다. 첫 번째 레이어는 낮은 수준의 기능(가장자리와 같은)을 검색하고, 구조의 깊숙이 들어가면 보다 복잡하고 추상적인 특성을 포착합니다.
- 가중치 공유와 공간 불변성: 동일한 필터가 이미지 전체에 적용되어 매개 변수의 양을 줄이고 네트워크를 더 효율적으로 만듭니다. 본 가정은 이미지의 다른 부분에 유용한 기능(가장자리, 질감 또는 패턴)이 나타날 수 있다고 합니다.

요약하자면:
ConvNets는 컨볼루션 필터를 통해 지역 패턴을 포착하며, 변환 동질성과 공간 불변성을 통해 위치와 관계없이 객체를 인식할 수 있으며, 단순한 기능에서 복잡한 기능으로 계층적으로 학습하며, 효율성을 위해 가중치 공유를 사용합니다.

![image](/assets/img/2024-07-13-ConvNeXtInSearchoftheLastConvolutionalLayer_3.png)

## Vision Transfomers (ViTs)의 부상

<div class="content-ad"></div>

약 2010년대에는 자연어 처리(NLP)용 신경망 설계가 컴퓨터 비전과 매우 다른 방향을 취했습니다. "트랜스포머"라는 새로운 모델 세대가 완전히 순환 신경망(RNN)과 그 변형을 대체했습니다.

![image description](/assets/img/2024-07-13-ConvNeXtInSearchoftheLastConvolutionalLayer_4.png)

언어와 비전 분야의 차이에도 불구하고, 두 분야는 놀랍게도 2020년에 수렴했습니다.

컴퓨터 비전 분야를 완전히 변혁시킨 "ViT"는 글로벌 컨텍스트를 포착하는 능력, 대규모 데이터셋과의 확장성, 그리고 다양한 입력 크기를 다루는 유연성 덕분에 여러 작업에서 CNN을 능가할 수 있었습니다.

<div class="content-ad"></div>

![image](/assets/img/2024-07-13-ConvNeXtInSearchoftheLastConvolutionalLayer_5.png)

이미지의 다양한 부분의 중요성을 평가하기 위해, 비전 트랜스포머는 복잡한 관계를 인식할 수 있도록 자기 주의 메커니즘을 사용합니다.

더 큰 전체 그림을 이해하는 데 중요한 영역에서는, 로컬 CNN 초점과는 달리 이러한 전역적 관점이 도움이 될 것입니다.

또한, ViT는 다양한 해상도에 적응할 수 있으며, 해상도가 더 높은 이미지에서 더 효과적으로 사용될 수 있습니다.

<div class="content-ad"></div>

![ConvNeXtInSearchoftheLastConvolutionalLayer_6.png](/assets/img/2024-07-13-ConvNeXtInSearchoftheLastConvolutionalLayer_6.png)

## ViT와 CNN의 통합 (계층적 트랜스포머)

ViT 내에서 가장 큰 문제는 입력 크기의 제곱 복잡성을 가진 글로벌 어텐션 디자인입니다 (입력 이미지의 크기가 증가함에 따라 계산량이 기하급수적으로 증가합니다).

ImageNet 분류 작업에는 문제가 되지 않을 수 있지만, 고해상도 입력에서는 주목할 만한 문제가 될 수 있습니다.

<div class="content-ad"></div>

Hierarchical Transformers were created to close this gap through a combination of methods.

Think of it like a core moving through the image; however, instead of executing convolution operations, it utilizes self-attention processes at each stage.

[Link to the image](/assets/img/2024-07-13-ConvNeXtInSearchoftheLastConvolutionalLayer_7.png)

The Swin Transformer stands out as a significant accomplishment in the progression of hierarchical ViTs, showcasing that transformers can excel in various computer vision assignments beyond just image classification.

<div class="content-ad"></div>

Swin Transformer의 성공과 신속한 채택은 한 가지 사실을 드러내었습니다:

# ConvNeXt 디자인

ConvNeXt의 디자인 프로세스는 한 가지 주요 질문에 의해 주도되었습니다:

이 전략은 원본 논문에서 다음과 같이 정의되어 있습니다:

<div class="content-ad"></div>

그러니까, 전략은 일부 ViT (예: Swin) 특성을 통합하여 전통적인 ResNet 아키텍처를 수정하는 것이었습니다.

ResNet-50 ResNet은 잔여 연결로 중요한 발전을 이룬 컴퓨터 비전의 기본 모델이었습니다.

ResNet-50과 같은 표준 ResNet 모델로 시작하는 선택은 저자들이 전통적이고 잘 이해된 아키텍처를 현대화하는 것이 최근 Transformer 기반 모델과 유사한 향상을 이끌 수 있는지를 탐구할 수 있도록 했습니다.

![이미지](/assets/img/2024-07-13-ConvNeXtInSearchoftheLastConvolutionalLayer_8.png)

<div class="content-ad"></div>

Swin Transformer
ViTs 분야에서 현대적인 접근을 대표하는 Swin Transformer는 ResNet-50과 크기와 성능에서 유사성을 갖추어 참조 모델로 선택되었습니다.

Swin-T는 Swin Transformer의 소형 버전으로, ResNet-50과 유사한 GFLOPS를 제공하지만 더 높은 정확성을 제공하여 비교를 위한 이상적인 모델입니다.

![이미지](/assets/img/2024-07-13-ConvNeXtInSearchoftheLastConvolutionalLayer_9.png)

FLOPs란 무엇인가요?
FLOPs(Floating Point Operations per Second)는 컴퓨터가 1초에 수행할 수있는 부동 소수점 연산의 수를 측정합니다.
부동 소수점 연산이란 덧셈, 뺄셈, 곱셈 또는 나눗셈과 같은 수학적 계산을 말합니다.

<div class="content-ad"></div>

GFLOPs(기가 부동 소수점 연산/초)는 특정 규모(기가 GFLOPs)를 나타내는 보다 구체적인 용어입니다. 이 용어는 일반적으로 서로 다른 알고리즘이 높은 규모에서의 성능을 비교하는 데 사용됩니다.

(예: 모델이 3.7 GFLOPs를 갖고 있다고 말할 때, 이는 모델의 계산이 실행되는 각 초에 37억 개의 연산을 포함한다는 것을 의미합니다).

# 모델 개발

아래 차트는 원래의 ResNet-50에 적용된 모든 변환을 나타냅니다.

<div class="content-ad"></div>

각 단계마다 모델의 정확도와 GFLOPs가 변합니다. 최종 단계 이후 Swin-T 모델과 비교가 이루어집니다.

(결과는 간단함을 위해 ResNet-50에서 보여줍니다. ResNet-200 결과는 원본 논문의 부록 C를 확인해주세요).

![이미지](/assets/img/2024-07-13-ConvNeXtInSearchoftheLastConvolutionalLayer_10.png)

## 1.- 매크로 디자인

<div class="content-ad"></div>

**1.1.- 단계 비율**

ResNet의 디자인에서는 계산 분배가 주로 경험적입니다. 즉, 이론적 모델보다 아키텍처 개발은 실험 데이터와 관찰에 기반을 두었습니다.

전통적인 ResNet은 (3:4:6:3)의 비율을 갖습니다.

반면, 트랜스포머는 계산 자원을 분배하기 위한 더 명확한 비율을 갖습니다. Swin-T의 경우 1:1:3:1 같이 각 단계가 균형을 이루지만 세 번째 단계만 더 무겁습니다.

<div class="content-ad"></div>

![Image](/assets/img/2024-07-13-ConvNeXtInSearchoftheLastConvolutionalLayer_11.png)

1.2.- 줄기 세포

ResNets에서 시작 레이어는 일반적으로 이미지 크기를 줄이고 초기 특징을 추출하는 합성곱 레이어입니다.

반면에 Vision Transformers는 이미지를 작은 패치로 나누어 다운샘플링하는 "패치화" 단계로 시작합니다.

<div class="content-ad"></div>

![ConvNeXt Model](/assets/img/2024-07-13-ConvNeXtInSearchoftheLastConvolutionalLayer_12.png)

ConvNeXt 모델은 4×4 크기와 스트라이드 4를 사용하여 구현된 패치 레이어를 사용함으로써 이 방법을 채택합니다.

## 2.- ResNeXt-ify

ResNeXt는 '그룹화된 합성곱' 개념을 통합함으로써 잔차 네트워크의 원리를 확장한 신경망 아키텍처입니다.

<div class="content-ad"></div>

세계 여러분, 안녕하세요! 오늘은 ConvNeXt 아키텍처의 특별한 부분, Inverted Bottleneck에 대해 이야기해보려고 해요.

ConvNeXt는 깊이별 합성곱(depthwise convolutions)을 사용하여 이 구조를 특별하게 만들었어요. 이는 그룹화된 합성곱의 특수한 경우로, 아래 이미지에서 살펴볼 수 있어요.

![ConvNeXtInSearchoftheLastConvolutionalLayer_14.png](https://www.example.com/assets/img/2024-07-13-ConvNeXtInSearchoftheLastConvolutionalLayer_14.png)

이런 방식으로 Inverted Bottleneck은 ConvNeXt 아키텍처의 중요한 구성 요소 중 하나에요. 자세한 내용은 블로그에서 확인해주세요! 감사합니다! 🌟

<div class="content-ad"></div>

표준 병목현상의 경우, 입력 채널의 수가 먼저 줄어들고 네트워크 블록 내에서 확장됩니다.

그러나 역병목현상은 더 좁은 입력으로 시작하여 중간 처리를 위해 더 넓은 내부 차원으로 확장한 다음 다시 더 좁은 출력으로 압축합니다.

![이미지](/assets/img/2024-07-13-ConvNeXtInSearchoftheLastConvolutionalLayer_15.png)

트랜스포머는 종종 중간 단계 처리를 위해 내부 차원인 MLP(다층 퍼셉트론 블록)이 입력 차원보다 훨씬 큰 역병목 디자인을 사용합니다.

<div class="content-ad"></div>

이 디자인은 MobileNetV2와 같은 아키텍처에서 볼 수 있는 ConvNets와 유사점이 있습니다. 이 아키텍처에서 네트워크 레이어의 내부 차원을 먼저 증가시킨 후에 다시 압축하는 것이 일반적인 기능이 되었습니다.

## 4. 대형 커널

이것은 가장 흥미로운 부분 중 하나인데요. 컴퓨터 비전에서 커널의 진화에 대해 더 잘 이해하기 위해 잠시 되짚어보겠습니다. 

![Image](/assets/img/2024-07-13-ConvNeXtInSearchoftheLastConvolutionalLayer_16.png)

<div class="content-ad"></div>

초반에는 큰 커널이 흔히 있었으며, 네트워크가 입력 이미지에서 다양한 특징을 포착할 수 있게 했습니다.

그러나 VGGNet의 등장으로 작은 (3×3) 커널이 여러 층에 더 많이 사용되면서 실제로 더 효과적이고 효율적이라는 것이 입증되었습니다.

하지만 비전 트랜스포머(ViTs)의 등장으로 시야 변환기(ViT)가 등장하면서 적어도 (7×7)의 창 크기로 큰 커널이 다시 도입되었습니다.

이를 통해 모델이 전역 맥락 정보를 포착할 수 있게 되었으며, 트랜스포머의 자기 주의 메커니즘의 넓은 수용 영역과 유사해졌습니다.

<div class="content-ad"></div>

![ConvNeXtInSearchOfTheLastConvolutionalLayer_17](/assets/img/2024-07-13-ConvNeXtInSearchoftheLastConvolutionalLayer_17.png)

이전 단계와 마찬가지로 ConvNeXt는 ViT 특징(큰 커널)을 선택했습니다.

연구자들은 다양한 커널 크기에 실험을 해보았고, 더 큰 커널 크기의 이점이 7×7에서 포화점에 도달함을 관찰했습니다. 그러나 5와 같은 더 작은 커널 크기보다 성능이 더 우수함을 확인했습니다.

CNN의 맥락에서 '깊이별 컨볼루션 레이어를 올리는 것'은 레이어가 입력 데이터에 적용되는 순서를 변경하는 것을 의미합니다.

<div class="content-ad"></div>

표준 아키텍처에서는, 깊이별 합성곱 레이어가 다른 유형의 레이어 뒤에서 더 깊게 나타날 수 있습니다. 그러나, 저희는 깊이별 합성곱 레이어를 프로세스 초기에 배치하기로 결정했습니다. 

깊이별 합성곱 레이어를 위로 옮김으로써, 계산 복잡한 연산들(크기가 큰 커널 합성곱)이 더 작은 채널 수에서 작동하게 됩니다. 그리고 1x1 합성곱과 같은 더 효율적인 연산들이 늘어난 채널을 다루며, 이는 더 많은 계산 작업이 필요한 부분입니다.

이 재배치는 전체 계산 부하(GFLOPs)를 줄이고, 네트워크의 효율성을 향상시키는 것을 목표로 합니다.

## 5.- 미크로 디자인

<div class="content-ad"></div>

**이미지 1**

- **ReLU를 GELU로 대체하는 이유**: ConvNets에서 ReLU가 일반적이지만, 보다 부드러운 GELU 변형은 Transformer 모델에서 자주 사용됩니다. ConvNets에 GELU를 통합해도 정확도는 변하지 않지만, GLOPS가 0.1 감소했습니다.

**이미지 2**

- **활성화 함수 감소**: Transformer 모델은 ResNets에 비해 적은 활성화를 사용합니다. GELU 활성화를 줄이는 것은 이를 모방하며 성능이 개선되었습니다 (80.6에서 81.3).
- **정규화 레이어 감소**: Transformer와 유사하게 BatchNorm 레이어를 줄이는 것도 성능을 향상시켰습니다(0.1 증가).

<div class="content-ad"></div>

![이미지](/assets/img/2024-07-13-ConvNeXtInSearchoftheLastConvolutionalLayer_20.png)

- BN을 LN으로 대체: 배치 정규화 대신 레이어 정규화를 사용하는 것은 트랜스포머에서도 흔히 사용되는 방법으로, 정확도를 약간 향상시켰습니다. 0.1만큼 향상되었어요.

![이미지](/assets/img/2024-07-13-ConvNeXtInSearchoftheLastConvolutionalLayer_21.png)

새로운 다운샘플링 레이어:

<div class="content-ad"></div>

- ResNet에서는 각 단계의 시작 부분에 잔차 블록에 의해 공간 다운샘플링이 이루어집니다.
- Swin 트랜스포머에서는 각 단계 사이에 별도의 다운샘플링 레이어가 추가됩니다.

ResNet 방법 대신 Swin 트랜스포머의 다운샘플링 방식을 채택하는 것이 의외로 정확도 향상에 상당한 영향을 미쳤습니다 (81.5에서 82로).

# 모델 테스트

![모델 테스트](/assets/img/2024-07-13-ConvNeXtInSearchoftheLastConvolutionalLayer_22.png)

<div class="content-ad"></div>

학습만큼이나 테스트 부분도 중요합니다. 모델이 잘 작동하는지 확인하고, 평가 방법으로 계속 발전하지 않으면 안 됩니다.

일반적으로 이러한 모델은 거대한 데이터셋이나 프레임워크에서 테스트됩니다. 여기서 알고리즘의 다양한 능력을 평가할 수 있습니다.

1. ImageNet 분류
ConvNeXt-T는 Swin Transformers를 넘어서며 82.1%의 top-1 정확도를 달성했습니다. 가장 큰 버전인 ConvNeXt-L도 인상적인 결과를 보여 85.5%를 기록하며 이 벤치마크에서 최고 모델 중 하나가 되었습니다.

![ConvNeXtInSearchoftheLastConvolutionalLayer_23](/assets/img/2024-07-13-ConvNeXtInSearchoftheLastConvolutionalLayer_23.png)

<div class="content-ad"></div>

테이블 상단에 있는 세 가지 구조물(RegNet 및 EffNet 버전)은 나열된 구조물 중에서 정확도와 계산 요구 사항의 최상의 조합을 갖고 있습니다.

그러나 전체 그림을 시각화하는 것이 중요합니다. 일반적으로 모델은 작은 및 큰 데이터 세트로 평가되며, 그들이 어떻게 확장되는지 확인합니다.

이전에 논의한 대로, ConvNeXt의 장점 중 하나는 아키텍처에 transformer 기능이 있어 데이터 양이 많아질수록 적응할 수 있는 능력입니다.

이미지넷 22k 데이터 세트에서 모델의 성능을 살펴보겠습니다:

<div class="content-ad"></div>

![ConvNeXtInSearchoftheLastConvolutionalLayer](/assets/img/2024-07-13-ConvNeXtInSearchoftheLastConvolutionalLayer_24.png)

결과는 매우 흥미로웠어요. 이번에는 ConvNeXt가 ConvNeXt-XL로 가장 높은 정확도(87.8%)를 달성했습니다. 그러나 파라미터와 FLOPS는 여전히 EffNet V2-XL과 비교했을 때 상당히 높은 수준에 있습니다.

그러나 ConvNeXt의 다른 버전들은 정확도와 FLOPS 사이에서 더 나은 균형을 이루었습니다. 명확한 예시는 ConvNeXt-L입니다. 이 모델은 벤치마크에서 두 번째로 높은 정확도(87.5%)를 달성했지만, 파라미터 수가 현저히 적으며(198M), FLOP는 101.0G로 EffNetV2-XL(94.0G) 수준 정도입니다.

작은 버전(ConvNeXt-T)은 상당히 잘 확장되었고, 정확도와 FLOPs 사이에서 인상적인 균형을 제공했습니다(82.9% / 4.5G).

<div class="content-ad"></div>

2.- COCO Object Detection

The COCO (Common Objects in Context) dataset is commonly utilized in the assessment of model performance across tasks such as object detection, segmentation, and captioning.

Object detection revolves around the recognition and localization of various objects within a series of images.

![COCO Object Detection](/assets/img/2024-07-13-ConvNeXtInSearchoftheLastConvolutionalLayer_25.png)

<div class="content-ad"></div>

분석하기 전에 중요한 점은 다음 메트릭을 이해하는 것입니다:

- AP_bbox: 다양한 IoU(IoU, 교차하는 영역의 비율) 임계값에서 바운딩 박스 검출에 대한 평균 정밀도입니다. AP_50과 AP_75는 각각 50% 및 75% IoU 임계값에서의 정밀도를 나타냅니다.
높은 AP 값은 더 정확한 검출을 나타내며, 더 나은 결과를 의미합니다.
- AP_mask: AP_bbox와 유사하지만 세그멘테이션 마스크에 대한 것입니다.

![image](/assets/img/2024-07-13-ConvNeXtInSearchoftheLastConvolutionalLayer_26.png)

ConvNeXt-T는 Swin-T보다 FLOPs가 약간 낮고(FLOPs 714G), FPS(FPS 13.5)가 더 높은 것으로 나타납니다. 따라서 더 효율적인 모델일 수 있으나 AP 점수를 통해 비교 가능한 정확도를 유지하고 있습니다.

<div class="content-ad"></div>

### 3. Efficiency

![Efficiency Benchmark](/assets/img/2024-07-13-ConvNeXtInSearchoftheLastConvolutionalLayer_27.png)

This benchmark is where the model truly shines, as it has outperformed all versions of the Swin Transformer. The throughput column reflects the number of images per second that the model can process.

In general, ConvNeXt's throughput exceeds that of the Swin Transformer by approximately 40%, showcasing a significant enhancement in efficiency while also maintaining or even improving accuracy.

<div class="content-ad"></div>

Here's the primary focus of ConvNeXt:

### References:

- Liu, Z., Mao, H., Wu, C.-Y., Feichtenhofer, C., Darrell, T., & Xie, S. (2022). A ConvNet for the 2020s. arXiv.
- He, K., Zhang, X., Ren, S., & Sun, J. (2016). Deep Residual Learning for Image Recognition. arXiv.

<div class="content-ad"></div>

- **3.** Liu, Z., Lin, Y., Cao, Y., Hu, H., Wei, Y., Zhang, Z., Lin, S., & Guo, B. (2021). "Swin Transformer: Hierarchical Vision Transformer using Shifted Windows." arXiv.

- **4.** Xie, S. (2017). "Aggregated Residual Transformations for Deep Neural Networks." arXiv.

- **5.** Sandler, M., Howard, A., Zhu, M., Zhmoginov, A., & Chen, L. (2019). "MobileNetV2: Inverted Residuals and Linear Bottlenecks." arXiv.

- **6.** Simonyan, K., & Zisserman, A. (2015). "Very Deep Convolutional Networks for Large-Scale Image Recognition." arXiv.

<div class="content-ad"></div>

친구들, 이 글 읽어주셔서 감사해요! 만약 이 글이 마음에 드시면 박수(최대 50번!)를 보내주세요. 또 저의 매체 팔로우를 통해 새 게시물이 올라올 때마다 업데이트 받으세요. 🌟