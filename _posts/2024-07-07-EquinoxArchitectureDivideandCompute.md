---
title: "분할과 계산 Equinox 아키텍처의 비밀"
description: ""
coverImage: "/assets/img/2024-07-07-EquinoxArchitectureDivideandCompute_0.png"
date: 2024-07-07 13:29
ogImage: 
  url: /assets/img/2024-07-07-EquinoxArchitectureDivideandCompute_0.png
tag: Tech
originalTitle: "Equinox Architecture: Divide and Compute"
link: "https://medium.com/@DakshishSingh/equinox-architecture-divide-and-compute-99c555ac08d6"
---


# Abstract:

Equinox Architecture introduces the concept of “Divide and Compute”, which allows for sequential processing and generation in O(n) time complexity, and in O(log n) when parallelized. It uses a tree model and neural networks to compute sequences.

Hello, I am a student and there may be errors in my work. I would appreciate it if you could review it and let me know if you find anything valuable. Feel free to email me at: dakshishsingh12@gmail.com

I am planning to write a paper and would greatly appreciate your help. If you discover something valuable or have any questions, please don’t hesitate to contact me. You can email me at the address above.

<div class="content-ad"></div>

# 장난감 문제:

'n'을(를) 합해봅시다 (여기서는 8) 숫자를 신경망을 사용하여. 두 숫자를 더할 수 있는 단일 신경망을 훈련합니다. 우리는 시퀀스를 2개씩 짝을 이루도록 나누고 해당 숫자들을 네트워크에 입력하여 더 작은 시퀀스를 생성하게 할 것입니다. 우리는 문제를 해결할 때까지 이 프로세스를 반복할 것입니다 (여기서는 숫자들의 합이어야 하는 단일 숫자를 얻게 됩니다).

![equinoxarchitecturedivideandcompute_0.png](/assets/img/2024-07-07-EquinoxArchitectureDivideandCompute_0.png)

여기에서 원은 두 숫자를 추가할 수 있는 신경망을 나타냅니다. 그 신경망과 개념을 사용하여 2^n 숫자를 합할 수 있습니다.

<div class="content-ad"></div>

저희는 이 개념을 이용해서 LLM을 훈련하는 데 적용할 수 있어요.

![Equinox Architecture Divide and Compute](/assets/img/2024-07-07-EquinoxArchitectureDivideandCompute_1.png)

Q) 왜 이것이 동작해야 할까요?

A) 신경망의 역할은 단어 단위로 읽는 대신 시퀀스를 쌍으로 읽을 수 있는 방법을 찾는 것입니다. 여기에 장점이 있는데, 모든 토큰 간의 거리는 2 * log n이며, 재귀 신경망은 선형적으로 거리를 둡니다. 이는 정보를 더 좋은 맥락에서 교환할 수 있도록 할 수 있습니다. 이론적으로 하나의 신경망을 훈련해야 하므로 매우 큰 신경망을 쉽게 훈련할 수 있습니다.

<div class="content-ad"></div>


Q) 왜 작동하지 않아야 하는 건가요?

A) 언어가 토큰 쌍으로 처리되지 않는 것은 불가능한 일일 수도 있습니다. 만약 이 문제가 존재한다면, 더 큰 네트워크가 다중 헤드, 다중 레이어, MLP 접근 방식을 사용하여 문제를 해결할 수 있을 것입니다. 하지만 아마도 신경망은 해결책을 찾을 것입니다.

2^n 개의 토큰이 없다면, 특수 토큰이나 0 벡터를 제공하여 이 경우를 처리하도록 하면 됩니다. 그러면 신경망이 이를 다루는 방법을 배우게 될 것입니다.

![Equinox Architecture Divide and Compute](/assets/img/2024-07-07-EquinoxArchitectureDivideandCompute_2.png)

<div class="content-ad"></div>

신경망은 4, 8, 16 등의 벡터를 입력으로 받아 1,2,3,4 등의 벡터를 출력으로 생성할 수 있습니다.

![Equinox Architecture](/assets/img/2024-07-07-EquinoxArchitectureDivideandCompute_3.png)

## 다음 토큰을 예측하는 학습 없이 LLM을 훈련하기:

이제 순차 프로세서로 Equinox Architecture를 사용하는 LLM을 훈련할 것입니다. 이 프로젝트에서는 중요한 변화를 가합니다: 가우우 여러분이 아시다시피, Equinox Architecture의 하나의 신경망이 2개의 벡터를 입력으로 받아 1개의 벡터로 인코딩하는 오토인코더로 훈련될 것입니다. 여기서 벡터란 우리의 인코딩된 토큰 길이를 말해요. 각 레이어마다 다른 네트워크가 있겠지만, 레이어마다 시퀀싱 작업을 처리하는 단일 네트워크가 있을 것입니다. 그래서 우리의 Equinox 부분은 트랜스포머처럼 동작하는 것이 아니라 압축기로 작용할 거에요.

<div class="content-ad"></div>

나는 2세대의 LLM을 훈련시켰어요. 이 두 LLM과 distil-gpt는 모두 GPT-2의 어휘와 인코딩을 사용했어요. 우리는 우리의 모델을 distil-gpt와 비교할 거에요. 두 세대는 모두 PTB-text-only로 훈련되었어요. 내 LLM과 distil-gpt는 모두 PTB-text-only 테스트 세트에서 시험을 받았어요. Distil-gpt와 내 LLM은 동일한 어휘와 인코딩 체계를 사용했고, 우리의 모델(40-45 백만 개 파라미터)은 distil-gpt(88 백만 개)보다 파라미터 수가 절반 정도 작아요.

우리 모델의 구조는 이렇게 생겼어요:

![equinox-architecture](/assets/img/2024-07-07-EquinoxArchitectureDivideandCompute_4.png)

트랜스포머 블록은 임베딩을 가져와 다음 토큰의 임베딩을 출력해요; 이것은 단순한 신경망이에요. 프리딕터 블록은 생성된 다음 토큰의 임베딩을 사용해 다음 토큰을 예측하려고 해요. 모든 부분은 차례로 개별적으로 훈련되어요: 먼저 에퀴녹스 블록, 그 다음에는 트랜스포머 블록, 그리고 마지막으로 프리딕터 블록이에요.

<div class="content-ad"></div>

제 1 세대 결과:

n번째 토큰 예측의 혼란과 부정적 가능성:

3번째 토큰: 142.448708 (4.95)

5번째 토큰: 1715.15373 (7.44)

<div class="content-ad"></div>

## 9번 토큰: 109365.979 (11.60)

모델의 크기:

예측 시: 이퀴녹스의 매개변수 수 디코더의 매개변수 수

• 3번 토큰 1,180,416 40,492,801

<div class="content-ad"></div>

**5번 토큰 2,360,832 40,492,801**

**- 9번 토큰 3,541,248 40,492,801**

두 번째 세대:

두 번째 세대에서는 한 번에 2개 이상의 토큰을 처리할 수 있는 네트워크를 실험하고 더 나은 방식으로 훈련시키는 것을 시도했어요. 모델의 크기는 그리 많이 변하지 않았어요.

<div class="content-ad"></div>

결과는 저희 모델의 내용입니다:

![모델 결과](/assets/img/2024-07-07-EquinoxArchitectureDivideandCompute_5.png)

여기서 열은 에쿼녹스 블록의 단일 신경망이 받는 벡터의 수를 나타냅니다. 첫 번째 행은 에쿼녹스 블록 내에 있는 레이어의 개수를 보여줍니다. 따라서 LLM으로 공급되는 토큰의 수는 열 번호 ^ 행 번호로 확인할 수 있습니다. 두 번째 열과 두 번째 행 네트워크는 16(4²)개의 토큰을 입력으로 받게 됩니다.

<div class="content-ad"></div>

어느 수신하는 곳에서도 흥미로운 발견을 할 수 있어요. 문맥의 길이가 증가할수록 토큰의 수도 늘어나면서 당황스러워할 수 있어요. 이는 오토인코더로 훈련된 것과 모델을 학습하는 방식 때문일 수도 있어요. 

distil-gpt의 다양한 토큰 개수에 따른 당황도는 다음과 같아요:

문맥 길이 -` 당황도

64 -` 58.5089, 32 -` 63.2031, 16 -` 73.8640, 8 -` 101, 4 -` 185.0610, 2 -` 419.9217

<div class="content-ad"></div>

Gen 2 모델은 낮은 문맥 길이에서 예측 성능이 우수합니다. 즉, 2, 4에서 뛰어나며 8토큰에서 예측 성능이 비슷합니다. 반 파라미터 수로 단문맥에서 더 나은 성능을 발휘하는 것으로 보입니다. 또한, 진정한 LLM으로 훈련되지 않았음에도 불구하고 이 모델이 뛰어납니다.

첫 번째 열 모델: [링크](https://drive.google.com/drive/folders/11fE1ec5iEmlA_7ZzUaaROp_8QwmZ4LiF?usp=drive_link)

두 번째 열 모델: [링크](https://drive.google.com/drive/folders/1HtH3FGtrEesBqy5USY7IU1FGrqiwJ0dv?usp=drive_link)

세 번째 열 모델: [링크](https://drive.google.com/drive/folders/1Isii4X9nqoU4pF2m0Uv9jgkndU2jPfym?usp=drive_link)

<div class="content-ad"></div>

**4th column model:** [Link to the 4th column model](https://drive.google.com/drive/folders/1YZzLj61ImeYP_gsXJnrKKHiLgCSCSBIL?usp=drive_link)

**5th column model:** [Link to the 5th column model](https://drive.google.com/drive/folders/12T4QrfRi0iXyRGURQD-_04y5ykEprLl7?usp=drive_link)

# How it could be applied to image generation:

<div class="content-ad"></div>

When it comes to image generation, the potential is truly remarkable. We can start by inputting the image we wish to generate into a neural network. The network will then produce two versions of the image, representing different parts (top and bottom, for example). By repeating this process, we can generate versions for each pixel or group of pixels. Additionally, utilizing another neural network, we could determine the pixel values. 

![EquinoxArchitectureDivideandCompute_6](/assets/img/2024-07-07-EquinoxArchitectureDivideandCompute_6.png)