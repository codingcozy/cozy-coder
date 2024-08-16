---
title: "구글 매직 이레이저의 비밀 공개"
description: ""
coverImage: "/assets/img/2024-07-13-RevealingtheSecretsofGooglesMagicEraser_0.png"
date: 2024-07-13 22:35
ogImage: 
  url: /assets/img/2024-07-13-RevealingtheSecretsofGooglesMagicEraser_0.png
tag: Tech
originalTitle: "Revealing the Secrets of Google’s Magic Eraser!"
link: "https://medium.com/gitconnected/revealing-the-secrets-of-googles-magic-eraser-fb232c83723b"
isUpdated: true
---





작년에 구글은 '매직 이레이서'를 모든 Google One 사용자에게 제공하기 시작했습니다 (이전에는 Pixel 폰에서만 사용 가능했습니다). 매직 이레이서를 사용하면 사진에서 원하지 않는 부분을 쉽게 제거할 수 있습니다. 주변 환경을 기반으로 적합한 객체로 공간을 채우는 AI를 사용하여 전체 이미지가 더 자연스럽게 보이게 됩니다!

이 기술은 마법처럼 보일 수 있으며, 원하는 대로 이미지를 변경할 수 있게 해줍니다. 일부 사람들은 이것이 많은 수학 지식과 컴퓨터 성능을 필요로 한다고 생각할 수도 있습니다. 그러나 사실, '이미지 인페인팅'인 매직 이레이서는 그리 복잡하지 않을 뿐더러 컴퓨터에서도 할 수 있습니다.

오늘날 인기 있는 방법 중에는 확산, GANs (생성적 적대 신경망), 그리고 주의 메커니즘을 기반으로 한 모델이 포함됩니다. 이 글에서는 이미지 인페인팅을 위한 GAN 기반 접근 방법에 초점을 맞출 것입니다. 시작하기 전에 먼저 구글의 매직 이레이서가 어떻게 작동하는지 짧은 비디오를 보겠습니다.

## 목차

<div class="content-ad"></div>

**조건부 이미지 생성**

이전 글에서는 이미지 생성을 위해 인공신경망을 사용하는 방법을 소개했습니다. 이미지 생성이란 본질적으로 모델이 가짜 데이터 분포 Pmodel(x)를 생성하도록 하는 것입니다. 목표는 이 분포를 가능한한 실제 분포 Pdata(x)에 가깝게 만들어 새로운 이미지를 생성하는 것입니다.

![이미지](/assets/img/2024-07-13-RevealingtheSecretsofGooglesMagicEraser_0.png)

<div class="content-ad"></div>

이미지 인페인팅에서 우리의 목표는 이미지의 누락된 부분을 채우는 것입니다. 이것은 이미지에 '조건'을 도입하는 것으로 생각할 수 있으며, 우리 모델의 접근 방식을 일반 분포 Pmodel(x)에서 조건부인 Pmodel(x | 조건)로 변경합니다.

이 프로세스는 일반 이미지 생성과 유사하지만, 조건을 모델에 통합하는 추가 단계가 있습니다. 나중에 더 자세히 설명하겠습니다.

## 이미지 인페인팅의 단계

![Image Inpainting Step](/assets/img/2024-07-13-RevealingtheSecretsofGooglesMagicEraser_1.png)

<div class="content-ad"></div>

- 랜덤 가리개를 생성합니다.
- 실제 이미지와 가리개를 결합하여 (가리개가 씌인 이미지) 생성합니다.
- 가리개가 씌인 이미지와 가리개를 신경망에 입력으로 제공하여 가짜 이미지를 생성합니다.
- 실제 이미지와 가짜 이미지 사이의 유사성(손실)을 계산합니다.

# 협조 조절 생성 적대 신경망

협조 조절 생성 적대 신경망, 즉 CoModGAN은 이미지 완성을 위해 설계된 생성 모델의 일종입니다. 이 모델은 이미지의 큰 부분이 누락된 작업을 처리하는 데 특히 효과적입니다.

![이미지](/assets/img/2024-07-13-RevealingtheSecretsofGooglesMagicEraser_2.png)

<div class="content-ad"></div>

이 모델은 GAN (Generative Adversarial Network) 형식의 모델이라는 것을 알 수 있어요. 이는 주로 두 부분으로 구성되어 있습니다: 이미지를 생성하는 Generator와 이러한 이미지를 평가하여 실제인지 아닌지를 결정하는 Discriminator가 그 주요 구성 요소입니다.

![image of GAN model](/assets/img/2024-07-13-RevealingtheSecretsofGooglesMagicEraser_3.png)

CoModGAN의 또 다른 중요한 측면에 집중해봅시다: 'co-modulation'. 아래 이미지는 조절의 다양한 접근 방식을 보여줍니다: (E: 조건부 인코더, D: 디코더, M: 매핑 네트워크)

![image of modulation approaches](/assets/img/2024-07-13-RevealingtheSecretsofGooglesMagicEraser_4.png)

<div class="content-ad"></div>

- 모델 (a)은 이미지 생성을 위한 무조건적인 모델을 나타내며, 원래 StyleGAN에서 소개되었습니다. 주요 특징에는 수정 및 매핑 네트워크가 포함됩니다.
- 모델 (b) 및 (c)는 인코더-디코더 구조의 전형적인 예시로, 인코더는 이미지를 잠재 공간으로 변환하고, 디코더는 원래 이미지를 재구성합니다. (예: Pix2Pix, CycleGAN...)
- 모델 (d)는 모델 (b) 및 (c)의 구조를 기반으로 하며, StyleGAN의 (a) 매핑 네트워크도 통합하여 잠재 벡터 Z를 분리합니다. 더불어, 생성된 이미지의 스타일을 더 잘 제어하기 위해 E 및 M의 출력을 D의 추가 입력으로 결합합니다. 이것이 바로 코-모듈레이션(co-modulation)으로 언급되는 이유입니다. 💪

이제 코딩을 위한 시간이에요~ 😄

# 데이터 전처리

머신 러닝에서 첫 번째이자 가장 중요한 단계는 데이터 전처리 단계입니다. 이 구현에서는 CelebA-HQ 데이터셋과 직사각형 마스크를 사용하여 모델을 훈련할 것입니다. 게다가, 자유 형식의 마스크를 사용하거나 직접 마스크를 만드는 것도 권장됩니다 ~~

<div class="content-ad"></div>

이미지 태그를 마크다운 형식으로 변경해주세요.

# Model Architecture

![Model Architecture](/assets/img/2024-07-13-RevealingtheSecretsofGooglesMagicEraser_5.png)

위 다이어그램은 CoModGAN의 생성기 구조를 보여줍니다. 생성기는 인코더 네트워크와 디코더 네트워크로 구성됩니다. 인코더는 비교적 직관적이며, 입력을 인코딩하기 위해 VGG와 같은 심층 컨볼루션 네트워크를 쉽게 사용할 수 있습니다.

<div class="content-ad"></div>

이제 디코더 부분에 집중해봅시다. 코-모듈레이션은 인코더와 매핑 네트워크에서 가져온 두 개의 다른 잠재 벡터를 결합하여 스타일GAN의 합성 네트워크와 유사하게 디코더의 여러 레이어로 통합합니다.

## StyleGAN 이미지 품질 향상

![이미지](/assets/img/2024-07-13-RevealingtheSecretsofGooglesMagicEraser_7.png)

위의 이미지는 StyleGAN에 의해 생성되었습니다. 이미지의 모서리에 물방울이 있는 것을 볼 수 있습니다. 이 현상은 64x64 해상도에서 모든 특징 맵에서 시작되고 더 높은 해상도에서 점차적으로 강해집니다.

<div class="content-ad"></div>

수정된 StyleGAN인 StyleGAN2(C 및 D)를 제안하여 물방울 문제를 해결합니다. 저자는 이 문제가 AdaIN 레이어로 인해 발생한다고 봅니다. AdaIN 레이어는 각 특징 맵의 평균과 분산을 별도로 정규화하여, 특징 간 상대적인 크기에 관련된 정보를 파괴할 수 있습니다.

이에 따라 스타일 평균을 제거함으로써 물방울 아티팩트가 완전히 사라지는 것을 발견할 수 있습니다. (참고: 이제 AdaIN은 이미지 Xi 대신 CNN의 가중치(w2, ...)에 직접 작동합니다)

<div class="content-ad"></div>


이미지 태그를 다음과 같이 변경해 보세요:

![Image](/assets/img/2024-07-13-RevealingtheSecretsofGooglesMagicEraser_10.png)

또 다른 수정 사항은 Instance Normalisation(Norm std)을 Demod 레이어로 대체하는 것입니다. Normalisation의 목적은 레이어의 출력을 단위 표준 편차로 다시 되돌리는 것이므로, 이 작업은 레이어의 가중치를 정규화함으로써 더 직접적으로 수행할 수 있습니다. 이를 'Demodulation'이라고 합니다.

# 생성기 블록

매핑 네트워크에서 EqualLinear을 사용하여 레이어 출력을 학습 중 동적으로 재조정합니다. 제 실험에서 매핑 네트워크에서 EqualLinear을 사용하는 것은 일반적인 선형 레이어에 비해 결과에 큰 영향을 미치지 않았습니다. torch.nn.Linear을 직접 사용하는 것을 권장합니다.

<div class="content-ad"></div>

이런 다양한 선택사항들 중에서 tRGB와 fRGB 아키텍처를 비교해봤어. 내 실험에서는 (b)를 주 구조로 사용해.

# 생성기

![Generator](/assets/img/2024-07-13-RevealingtheSecretsofGooglesMagicEraser_12.png)

<div class="content-ad"></div>

# 판별자

![Discriminator](/assets/img/2024-07-13-RevealingtheSecretsofGooglesMagicEraser_13.png)

제너레이터를 논의한 후 이제는 판별자로 넘어가 보겠습니다. 판별자의 주요 역할은 이미지가 실제인지 가짜인지를 구별하는 것이기 때문에, 임의의 이미지 분류 모델을 판별자로 사용할 수 있습니다.

여기서는 VGG와 비슷한 구조를 사용하여 간단히 구현해 보겠습니다! 👍

<div class="content-ad"></div>

# 손실 함수

![image](/assets/img/2024-07-13-RevealingtheSecretsofGooglesMagicEraser_14.png)

- G: 생성기
- D: 판별자 (0 ~ 1 사이의 값 출력, 가짜: 0, 진짜: 1)
- X: 입력 이미지, 형태 [B, C, H, W]
- Y: 마스크, 형태 [B, 1, H, W]
- Z: 잠재 벡터 (가우시안 분포에서 무작위 샘플링)

손실 함수는 클래식 GAN과 동일하며, 생성기와 판별자에 서로 다른 목표를 설정하여 그들 사이에 경쟁을 만들어냅니다.

<div class="content-ad"></div>

디스크리미네이터는 진짜 이미지와 생성기가 만든 가짜 이미지를 잘 구별하도록 더 나아지려고 노력합니다. 생성기의 목표는 디스크리미네이터를 속이는 것이며, 디스크리미네이터가 하는 것과 정반대를 수행합니다.

# 훈련 루프 & 게으른 정규화

스타일 갠에서 저자는 디스크리미네이터를 위한 R1과 생성기를 위한 Path Length 정규화 두 가지 유형의 정규화를 사용하여 모두에게 제약 조건을 부과합니다.

또한, 저자는 모든 가중치 업데이트마다 정규화를 적용할 필요가 없다는 것을 발견했습니다. 대신, 매 16번의 업데이트마다 디스크리미네이터에 대해 정규화를 적용하고, 매 8번의 업데이트마다 생성기에 대해 적용합니다. 이것을 "게으른 정규화"라고 합니다.

<div class="content-ad"></div>

## R1 정규화

![image](/assets/img/2024-07-13-RevealingtheSecretsofGooglesMagicEraser_15.png)

R1 정규화는 실제 데이터의 기울기만을 벌점을 주어 판별자가 데이터 매니폴드에서 벗어난 비제로 기울기를 생성하는 것을 방지하려는 목적을 가지고 있습니다.

예를 들어, 생성자가 진짜 데이터 분포를 생성하고, 판별자의 출력이 데이터 매니폴드에서 0과 동일할 때(일반적으로 판별자가 진짜와 가짜 데이터를 구별할 수 없게 되는 것을 의미합니다), 기울기 벌점은 판별자가 비제로 기울기를 생성할 수 없도록 하여 손실 증가를 방지합니다.

<div class="content-ad"></div>

## 경로 길이 규제

![Link](/assets/img/2024-07-13-RevealingtheSecretsofGooglesMagicEraser_16.png)

경로 길이 규제는 잠재 공간 'w'에서 고정 크기의 변경을 가했을 때, 그 변경이 이미지에서 무시할 수 없는 크기로 유지되도록 하는 것을 목적으로 합니다.

생성자 출력의 'w'에 대한 미분을 계산하고 이를 임의의 노이즈 'y'와 곱합니다. 이 결과에 L2 노름을 적용한 뒤 지수 이동 평균 'a'를 뺍니다.

<div class="content-ad"></div>

안녕하세요! 오늘은 CoModGAN의 교육 루프에 대해 이야기해볼게요! 이 규칙화는 숨겨진 벡터 Z의 변화가 생성된 이미지에 급격한 변화를 유발하지 않도록 할 수 있습니다.

마지막으로, CoModGAN의 교육 루프를 확인해보세요! 👏

# 결과

맨 위의 이미지는 CoModGAN에 의해 생성되었고, 맨 아래는 실제 이미지 데이터입니다.

<div class="content-ad"></div>

이미지 결과물과 실제 데이터 간에는 작은 차이가 있지만, 상당히 합리적인 이미지를 생성한다고 생각되어요! CoModGAN의 성능은 이미 충분히 수준에 도달한 것 같아요~ 🎊 🎊

![image](/assets/img/2024-07-13-RevealingtheSecretsofGooglesMagicEraser_17.png)

![image](/assets/img/2024-07-13-RevealingtheSecretsofGooglesMagicEraser_18.png)

![image](/assets/img/2024-07-13-RevealingtheSecretsofGooglesMagicEraser_19.png)

<div class="content-ad"></div>

마침내, 이 기사를 즐겁게 읽어 주셨기를 바랍니다. AI와 관련된 더 많은 기사를 작성할 예정이며, 그들의 기본 원리 설명과 구현 방법을 포함하고 있습니다.

관심이 있다면, 언제든지 제 팔로우해 주세요. 👏 😁

나의 미디엄: [링크](Medium 링크)

나의 링크드인: [링크](LinkedIn 링크)

<div class="content-ad"></div>

![2024-07-13-RevealingtheSecretsofGooglesMagicEraser_20.png](/assets/img/2024-07-13-RevealingtheSecretsofGooglesMagicEraser_20.png)

## 참고 자료

- Zhao, S., Cui, J., Sheng, Y., Dong, Y., Liang, X., Chang, E. I., & Xu, Y. (2021). 대규모 이미지 보완을 통한 상호 조절 생성적 적대 신경망. arXiv 사전 인쇄: arXiv:2103.10428.
- Goodfellow, I., Pouget-Abadie, J., Mirza, M., Xu, B., Warde-Farley, D., Ozair, S., ... & Bengio, Y. (2014). 생성 적대적 네트워크. 신경 정보 처리 시스템의 발전, 27.
- Karras, T., Laine, S., & Aila, T. (2019). 생성 적대적 네트워크를 위한 스타일 기반 생성기 아키텍처. 컴퓨터 비전 및 패턴 인식에 관한 IEEE/CVF 컨퍼런스 논문집 (pp. 4401-4410).
- Karras, T., Laine, S., Aittala, M., Hellsten, J., Lehtinen, J., & Aila, T. (2020). Stylegan 이미지 품질 분석 및 개선. 컴퓨터 비전 및 패턴 인식에 관한 IEEE/CVF 컨퍼런스 논문집 (pp. 8110-8119).