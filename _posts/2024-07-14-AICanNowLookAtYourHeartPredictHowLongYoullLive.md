---
title: "AI 기술로 심장을 분석해 수명을 예측하는 방법"
description: ""
coverImage: "/assets/img/2024-07-14-AICanNowLookAtYourHeartPredictHowLongYoullLive_0.png"
date: 2024-07-14 01:53
ogImage:
  url: /assets/img/2024-07-14-AICanNowLookAtYourHeartPredictHowLongYoullLive_0.png
tag: Tech
originalTitle: "AI Can Now Look At Your Heart , Predict How Long You’ll Live."
link: "https://medium.com/gitconnected/ai-can-now-look-at-your-heart-predict-how-long-youll-live-23770010be0b"
isUpdated: true
---

AI가 본격적으로 사용되어 복잡한 의료 문제를 해결하는 데 이제껏 없던 방법으로 사용되고 있어요.

런던 임페리얼 칼리지의 연구팀이 놀라운 AI 모델을 제안했어요. 바로 심장의 복잡한 3D 이미지를 ‘살아있는 심장’으로 볼 수 있는 모델이 사람의 생존을 예측할 수 있다는 거예요.

이야기 속으로 들어가 보죠. 이런 일이 가능하게 된 과정을 깊게 파헤쳐 보도록 할 거에요.

이 AI 모델의 실제적인 세부 사항에 대해 들어가기 전에, 인간의 심장이 어떻게 작동하는지 조금 배워볼까요?

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

# 인간 심장 소개

심장은 몸 전체에 혈액을 펌핑하는 큰 근육 기관입니다.

심장은 네 개의 수축성 실을 갖고 있어요 —

- 좌우에 있는 두 개의 상부 작은 실인 심방 (단수형은 심방)
- 좌우에 있는 두 개의 하부 큰 실인 심실

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

각 쪽 복심방이 각각 심방과 심실로 이어져요.

![이미지](/assets/img/2024-07-14-AICanNowLookAtYourHeartPredictHowLongYoullLive_0.png)

여기서 전체 시스템을 통해 혈액이 흐르는 방법을 알려 드릴게요.

- 심장의 왼쪽 심실이 산소가 풍부한 혈액을 신체 모든 장기로 펌핑해요.
- 이 장기들은 혈액 산소를 사용하고, 산소가 없어진 혈액은 다시 심장의 오른쪽 방으로 돌아가요.
- 이 혈액은 오른쪽 심실을 통해 폐로 이동해서 더 많은 산소가 첨가돼요.
- 이 산소가 풍부해진 혈액은 왼쪽 복심방과 심실을 통해 최종적으로 신체 모든 장기로 도착해요.

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

![Pulmonary Hypertension](/assets/img/2024-07-14-AICanNowLookAtYourHeartPredictHowLongYoullLive_1.png)

# 폐고혈압이란?

폐의 혈관이 손상되고 좁아지면 폐동맥압이 높아지는 일이 가끔 발생합니다.

이 상황을 폐고혈압이라고 합니다.

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

다음은 폐고혈압의 일반적인 원인 몇 가지입니다:

- 유전적 돌연변이
- 약물 및 독소
- 폐에서 '뒤쪽'으로 압력이 쌓이도록 하는 심장의 왼쪽 부분 질병
- 혈관염과 같은 염증성 장애
- 폐 혈관에 혈전
- 원인을 찾을 수 없는 특발성

폐고혈압은 혈액이 폐를 통과하기 어렵게 만들어서 결과적으로 심장(우심실)이 해당 혈관을 통해 혈액을 펌핑하기 위해 더욱 열심히 일해야 하는 상황을 만들어냅니다.

이로 인해 우심실의 크기가 증가하게 되어 그 추가 작업을 지원하기 위해 증가합니다.

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

안타깝게도, 이 크기의 증가(우심실 비대증)는 단단해진 심장과 혈류의 난류로 이어져 환자가 이 병으로 사망할 위험이 높아지는 상태로 이끌 수 있습니다.

일반적으로, 환자의 전망과 치료 선택은 다음과 같이 개인맞춤형으로 결정됩니다:

- 임상 위험 요인(나이, 성별, 6분 보행 거리 등)
- 심장 영상검사에서 파생된 용적 지표(우심실 축출계수, 최종 수축기 용적, 최종 이완기 용적 등)

하지만 이러한 요소들은 가장 정확하지 않기 때문에, 예후 정확도를 향상시키는 방법은 이 질환으로 살아가는 사람들의 삶을 개선할 수 있을 것입니다.

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

그리고 이 연구자들이 인공지능의 도움을 받아 이룩한 결과가 여기 있어요.

## 생존 예측 AI 모델 개요

우선, 폐고혈압 진단을 받은 다수의 환자로부터 심장 MRI 이미지가 얻어졌어요.

이 영상 기술에 익숙하지 않은 분이라면, 이 기술로 얻어진 이미지가 얼마나 자세한지 깜짝 놀랄 거예요!

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

아래를 한 번 확인해보세요.

![Tarot Image](https://miro.medium.com/v2/resize:fit:916/0*c2IC2QFLPLHrd07U.gif)

(만약 MRI 작동 방식에 대해 궁금하다면, 이 주제에 대한 이전 글 중 하나를 확인해보세요.)

그 다음, 심장 MRI 이미지를 처리하여 분할을 사용하여 심장의 심방의 3D 움직임을 식별하고 추적했습니다.

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

At this stage, a Fully Convolutional Network (FCN) was employed for image segmentation.

Next, the 3D motion models were fed into another neural network known as "4DSurvival," which is essentially an altered version of a Supervised Denoising Autoencoder (DAE).

The objective of this model was to understand concealed representations of heart motion and forecast a patient's survival based on this data.

Remarkably, this model surpassed the traditional clinical method in terms of accuracy when it came to predicting survival.

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

**The prediction accuracy was quantified using Harrell’s C-index, a popular metric used to evaluate risk models in survival analysis.**

**The model achieved a remarkable C-index of 0.75, surpassing the human benchmark of 0.59!**

# _Embarking on a Journey into the Survival Prediction AI Model_

_Join us as we uncover the story behind developing this cutting-edge AI model from the ground up._

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

첫 번째로, 폐 고혈압과 오른심 기능 이상이 진단된 302명 환자의 심장 MRI 영상을 획득했습니다.

# 분할 네트워크

Fully Convolutional Network (FCN)이 설정되었고 수동으로 레이블된 심장 MRI 영상을 입력으로 받았습니다.

네트워크 구조는 아래 이미지에 나와 있습니다.

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

해당 모델은 이미지 내 중요한 해부학적 랜드마크를 학습하고 이미지를 오른쪽과 왼쪽 두 개의 심방(vetricles)으로 분할하려고 했습니다.

표준 아틀라스에서 사전 정의된 심장의 3D 형상이 이어서 네트워크의 분할 능력을 정교화하는 데 사용되었습니다.

전체 과제는 각 이미지의 픽셀이 특정 세그먼트(심방)에 속하고 특정 해부 랜드마크를 포함하도록 분류되는 Multi-task Classification 문제로 다뤄졌습니다.

모델 훈련 중 전반적인 손실 함수는 Stochastic Gradient Descent (SGD)를 사용하여 최소화되었습니다.

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

![Image](/assets/img/2024-07-14-AICanNowLookAtYourHeartPredictHowLongYoullLive_2.png)

오늘은 로스 함수 L(W)에 대해 이야기해보려고 해요. 이 함수에는 다음과 같은 변수들이 포함돼 있어요 —

- LS(W)와 LD(W)는 이미지의 분할 맵을 예측하기 위해 네트워크가 최소화하는 지역 관련 손실값들이에요
- LL(W)는 이미지에서 해부적 랜드마크 위치를 예측하기 위해 사용하는 랜드마크 관련 손실값이에요
- α, β, 그리고 γ는 각각의 개별 손실값의 중요성을 결정하는데 사용되는 가중치 계수들이에요
- ∥W∥²F는 네트워크가 과적합 되는 것을 막기 위한 가중치 감쇠 용어에요

이러한 로스 함수들에 대한 자세한 내용은 여기에서 확인할 수 있어요.

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

This network's outputs are further refined to create temporal segmentation maps as shown below.

![Cardiac Cycle](/assets/img/2024-07-14-AICanNowLookAtYourHeartPredictHowLongYoullLive_3.png)

Next, displacement vectors are created for the trajectory of each point in the right ventricle to understand how it moves throughout the cardiac cycle.

These displacement vectors are concatenated to create a representation of the motion of the entire right ventricle across the cardiac cycle.

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

**"4DSurvival: The Prediction Network"**

This network, called "4DSurvival," utilizes a combination of a Denoising Autoencoder (DAE) and a Cox proportional hazards model. The features extracted from the cardiac imaging data serve as inputs to this innovative prediction system.

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

먼저 DAE가 어떻게 작동하는지 배워봅시다.

## Denoising Autoencoder (DAE)

이 네트워크는 서로 구조적으로 대칭인 인코더와 디코더로 구성되어 있습니다.

인코더의 첫 번째 레이어는 Dropout을 사용하여 입력 데이터를 확률적으로 손상시킵니다.

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

해당된 입력값들은 암호화된(숨겨진) 레이어를 거쳐 입력 데이터의 압축/잠재된 버전을 나타내는 중앙 레이어로 전달됩니다.

이 잠재 표현은 이후 다층 디코더로 전달되어 원본 표현으로 업샘플링하려고 합니다.

원본 입력과 디코더의 재구성된 표현 사이의 차이는 단순히 아래에 나와있는 평균 제곱 오차로 페널티를 주는 재구성 손실(L(r))을 사용합니다.

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

여기에서:

- n은 샘플 크기를 나타냅니다
- x는 원본 또는 손상되지 않은 입력을 나타냅니다
- ψ(ϕ(x))는 디코더의 재구성된 출력입니다

이 네트워크의 목적은 심장 영상 입력의 중요한 잠재 표현을 학습함으로써 노이즈에 강하고 정확한 생존 예측에 사용할 수 있는 것입니다.

인코더의 잠재 표현은 디코더에 연결된 것뿐만 아니라 선형 Cox 비례 위험 모델에 기반한 별도의 예측 분기에도 공급됩니다.

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

이 모델이 무엇인지 배워봅시다.

## Cox Proportion Hazards 회귀 모델

이 모델은 특정 이벤트가 발생하는 시간에 연결된 변수를 가진 데이터를 분석하는 데 사용되는 통계 모델입니다 (질병 재발, 사망 또는 기타 종점 이벤트와 관련된).

이러한 데이터를 생존 데이터 또는 시간-이벤트 데이터라고 합니다.

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

현실 세계에서 생존 데이터는 종종 관심 사건에 대한 불완전한 정보를 가지고 옵니다. 이것은 억제로 표현됩니다. 그 종류 중 두 가지는 다음과 같습니다 —

- 왼쪽 억제: 이는 연구에 참여한 개인이 연구가 시작되기 전에 이미 사건을 겪은 경우에 발생합니다. 따라서 사건 발생 시간이 정확히 알려지지 않습니다.
- 오른쪽 억제: 이는 연구의 종료 시점까지 관심 사건이 발생하지 않은 경우나 개인이 추적을 중단한 경우에 발생합니다.

이러한 억제 형태는 현실 세계의 생존 데이터에 매우 일반적이며 Cox 모델은 이러한 억제된 데이터를 다루기 위해 설계되었습니다.

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

### Cox 모델에 대한 이해

**수식은 다음과 같이 정의됩니다:**

**여기서:**

- **h(t)**는 시간 t에서의 위험(hazard) 또는 시간 t에서 사건이 발생할 위험/가능성으로 정의됩니다. 아직 일어나지 않은 경우를 가정합니다.

- **h₀(t)**는 기준 위험(hazard)입니다. 모든 공변량(covariates)이 제로일 때의 위험율을 설명합니다.

- **X1, X2, ... , Xp**는 위험율에 영향을 미치는 공변량 또는 변수입니다. 나이, 치료 그룹(사례/대조), 성별 등이 여기에 해당합니다.

- **β1, β2, … , βp**는 각 공변량에 대한 계수(coefficients)입니다.

Cox 모델과 관련된 주요 용어 중 하나는 Proportional Hazards Assumption입니다.

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

이것은 단순히 연구 중 두 명의 개인 간의 위험 비 (상대 위험 또는 h(t) / h0(t))가 연구 기간 동안 일정함을 가정하는 것입니다.

다음으로, 공변량과 관련된 계수인 βp가 콕스 비례위험부분우도함수의 최대화를 통해 추정됩니다 (로그함수는 아래에 표시되어 있음).

![Image](/assets/img/2024-07-14-AICanNowLookAtYourHeartPredictHowLongYoullLive_6.png)

여기서:

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

- z는 공변량 변수를 나타냅니다 (앞서 언급된 X와 동일합니다)
- δ는 사건 발생 여부를 나타내는 이벤트 인디케이터로, 이벤트(예: 사망)가 발생하면 1 또는 연구 기간 종료까지 사건을 경험하지 않고 생존한 경우에는 0을 나타냅니다
- β는 공변량에 대한 계수를 나타냅니다
- R(t(i))는 시간 t(i)에서의 위험 집합을 나타냅니다

여기까지가 꽤 혼란스러울 수 있으니, 실제 예시를 들어 설명해 드리겠습니다.

다음은 췌장암 환자 100명을 대상으로 진행되고 있는 새로운 약물 X의 효과를 평가하기 위한 연구에 참여한 모의 데이터셋입니다.

| 환자ID | 생존기간 | 치료 | 나이 | 암단계 | 이벤트 상태 |
| ------ | -------- | ---- | ---- | ------ | ----------- |
| 1      | 56.31    | 0    | 53.2 | 4      | 1           |
| 2      | 361.21   | 1    | 62.3 | 3      | 1           |
| 3      | 158.01   | 0    | 62.9 | 3      | 0           |
| 4      | 109.55   | 1    | 52.9 | 4      | 1           |
| 5      | 20.35    | 1    | 78.7 | 3      | 0           |

...

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

이 문제에서 변수는 다음과 같습니다:

- 환자 ID
- 생존 시간 (일 단위)
- 치료 여부는 환자가 약을 받았으면 1, 대조군이면 0
- 환자의 나이
- 암 진단 분류 (3과 4는 진행성 질병을 나타냄)
- 사건 상태는 환자가 생존하지 못했으면 1, 검열된 데이터인 경우 0

저는 이 생존 데이터를 분석하기 위해 lifelines Python 라이브러리를 사용하고 있는데, 이에 대한 과정은 이 Google Colaboratory 노트북에 문서화되어 있습니다.

분석 결과는 여기에 표시되어 있습니다 —

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

coef exp(coef) se(coef) coef lower 95% coef upper 95% exp(coef) lower 95% exp(coef) upper 95% cmp to z p -log2(p)
Treatment -0.113527 0.892680 0.265144 -0.633199 0.406146 0.530891 1.501022 0.0 -0.428170 0.668528 0.580941
Age 0.001061 1.001062 0.014768 -0.027884 0.030006 0.972501 1.030461 0.0 0.071850 0.942722 0.085096
Cancer_Stage -0.341446 0.710742 0.272649 -0.875828 0.192936 0.416517 1.212805 0.0 -1.252328 0.210450 2.248449

When interpreting the Tarot cards, we focus our energy on the following mystical powers —

- **Hazard Ratios (exp(coef)):** This magical ratio reveals secrets of risks. A number above 1 unveils increased risk, while a number below 1 whispers about a lowered risk.

The mystical power of the hazard ratio (0.892680) for the card 'Treatment' shows a sacred message — it is less than 1, indicating that it holds the power to decrease the risk of a sudden departure from this earthly realm. Trust in its divine influence. 🌟🔮✨

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

- 계수(β): 각 공변량이 위험률/효과에 미치는 방향과 영향의 크기를 나타냅니다.

'Treatment'의 음의 β값은 위험률을 줄이거나 생존기간을 연장시킨다는 것을 의미합니다.

'Age'의 양의 β값은 위험률을 증가시키거나 생존율이 낮다는 것을 나타냅니다.

- p-값: 효과가 통계적으로 유의한지 여부를 알려줍니다.

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

In our data, all of the results are not statistically significant since their p-values are greater than 0.05.

**Confidence Intervals (CI):** A 95% CI is expected to include the true value of the hazard ratio 95% of the time.

A narrow CI suggests a more accurate estimate of the hazard ratio, while a wider CI indicates less precision.

Furthermore, if the 95% CI does not include the value 1 (known as the null or no-effect value), then the results are considered statistically significant.

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

저희 데이터를 보면, 모든 exp(coef)의 95% CI는 결과가 통계적으로 유의하지 않음을 나타냅니다.

이해가 되셨나요? 이제 AI 모델로 돌아가 봅시다.

신경망의 예측 부문에는 생존 손실 (L(s))으로 Cox 모델이 채택되었습니다. 아래 그림과 같이 —

![image](/assets/img/2024-07-14-AICanNowLookAtYourHeartPredictHowLongYoullLive_7.png)

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

W`은 잠재 표현 ϕ(x)에 적용된 가중치 벡터로, 주어진 주체 i에 대한 생존 예측이나 로그 위험 비율 추정을 나타내는 스칼라 값(W`ϕ(x(i)))을 생성합니다.

재구성 손실과 생존 손실을 모두 결합하면 4DSurvival 모델에서 사용되는 전체 손실(L(hybrid))이 생성됩니다.

![Image](/assets/img/2024-07-14-AICanNowLookAtYourHeartPredictHowLongYoullLive_8.png)

이 손실 함수는 L1 정칙화와 Adam 최적화기를 사용하여 최소화됩니다. 계산된 그래디언트를 사용하여 손실 함수를 최적화하는 백프로퍼게이션 기법이 적용됩니다.

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

이 네이버스러멜 모델의 전체 구조가 아래에 나와 있어요.

# 파티클 스왐 최적화(PSO)로 하이퍼파라미터 튜닝

모델의 하이퍼파라미터는 파티클 스왐 최적화(PSO)라고 불리는 기술을 통해 조정되었어요.

이건 새들의 사회적 먹이 찾기 행동에서 영감을 받은 흥미로운 최적화 방법이에요.

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

이것은 Swarm Intelligence의 원리에 기반한 작동 방식을 가지고 있습니다. 이는 관심 문제에 대한 다양한 솔루션(입자라고 불림)으로 구성된 모집단(떼로 불림)이 참여하는 것을 의미합니다.

이 입자들 또는 솔루션들은 수학적 공식에 기반하여 탐색 공간에서 움직입니다.

이들의 움직임은 입자 자체 및 전체 떼의 최적 위치에 의해 이뤄지며, 가장 좋은 가능한 해답을 찾아내기 위해 유도됩니다.

한 가지 간단한 예시를 상상해보세요. 새떼가 어떤 지역에서 먹이를 찾고 있는 모습을 상상해보십시오.

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

PSO를 사용하면 새들은 자신의 경험을 바탕으로 음식이 어디에 있을지 추정하기 위해 동료 새들과 소통할 것입니다.

그에 따라 음식이 위치한 곳에 도달할 때까지 비행을 조정할 것입니다.

![image](https://miro.medium.com/v2/resize:fit:1120/0*UwZRI9DJmM2c_zB1.gif)

마찬가지로, 모델에서는 PSO를 사용하여 미리 정의된 값 범위에서 최적의 하이퍼파라미터 세트를 선택했습니다.

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

# 하렐의 콘코던스 지수에 기반한 결과

해당 모델의 예측 정확도는 하렐의 콘코던스 지수(C-index)를 사용하여 평가되었습니다. 이 지수는 오른쪽에 있으며 절단된 시간-이벤트 데이터를 위한 ROC 곡선 아래 영역(AUC)의 확장이다.

0.5의 C-index는 무작위 추측을 나타내며, 1.0의 값은 모델의 완벽한 예측 능력을 의미합니다.

그러므로 1에 가까운 값은 모델의 강력한 예측 정확도를 시사합니다.

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

모델을 사용하여 우심실 기능이 저하된 환자의 생존을 추정하는 데 사용된 전통적 체적 지수와 비교하기 위해 다른 Cox 비례위험 모델이 사용되었습니다.

모델의 C-지표는 0.73으로, 전통적 모델의 C-지표가 0.59임에 비해 통계적으로 유의미한 결과를 보였습니다.

# 라플라스 의미지를 사용한 잠재 공간 시각화

더 잘 이해하기 위해, 모델에 의해 생성된 잠재 공간은 비선형 차원 축소 기술인 라플라스 의미지를 사용하여 시각화되었습니다.

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

이 기술은 높은 차원의 잠재 공간을 두 차원으로 축소시켜, 다른 데이터 포인트 간의 고유한 기하학적 관계를 보존합니다 (PCA(주성분 분석)와 같은 전통적인 선형 방법과는 달리)

(유사한 차원 축소 기술에 대해 이전 글 중 하나에 설명이 있습니다. 여기를 클릭하세요.)

![image](/assets/img/2024-07-14-AICanNowLookAtYourHeartPredictHowLongYoullLive_9.png)

# 생존 예측에 대한 RV의 다양한 영역의 효과 연구

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

연구자들은 우측 심방의 각 지점의 평균 변위를 측정하고, 이를 예측된 생존 위험과 결합하여 선형 회귀 모델을 사용했습니다.

각 지점의 획득된 회귀 계수는 해당 심방 영역의 운동이 환자의 생존에 어떤 기여를 하는지를 정량화합니다.

이러한 계수의 로그값을 우측 심방의 3D 구조에 배치하여, 서로 다른 심방 영역이 생존에 미치는 영향을 보여주는 3D 중요도 지도를 만들었습니다.

![이미지](/assets/img/2024-07-14-AICanNowLookAtYourHeartPredictHowLongYoullLive_10.png)

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

# 더 읽을 거리

- "인간 생존 예측을 위한 딥 러닝 심장 운동 분석"이라는 제목의 연구 논문의 사전 인쇄 버전이 Arxiv에 게재되었습니다.
- "인간 생존 예측을 위한 딥 러닝 심장 운동 분석"이라는 제목의 연구 논문이 Nature Machine Intelligence 저널에 게재되었습니다.

AI 기술은 건강 관리 분야를 혁신하고 사람들이 더 오래 살 수 있도록 도와주고 있습니다.

더 중요한 연구나 현재 진행 중인 연구에 대해 더 알고 싶거나 진행 중이신 연구가 있다면 아래 댓글로 알려주세요!

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

## 이곳이 내 메일링 리스트 링크야. 내 작업과 연결 유지하고 싶으면 클릭해봐!
