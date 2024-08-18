---
title: "논문 리뷰 Chronos - 시계열 데이터 언어 학습의 최신 연구"
description: ""
coverImage: "/assets/img/2024-07-09-PaperReviewChronosLearningtheLanguageofTimeSeries_0.png"
date: 2024-07-09 23:51
ogImage:
  url: /assets/img/2024-07-09-PaperReviewChronosLearningtheLanguageofTimeSeries_0.png
tag: Tech
originalTitle: "Paper Review: Chronos: Learning the Language of Time Series"
link: "https://medium.com/@artgor/paper-review-chronos-learning-the-language-of-time-series-edd79dd4fc96"
isUpdated: true
---

## 논문 리뷰

논문 링크

![이미지](/assets/img/2024-07-09-PaperReviewChronosLearningtheLanguageofTimeSeries_0.png)

Chronos는 시간 시리즈 데이터에 대해 확률적 모델을 사전학습하는 프레임워크로, 이 값을 토크나이징하여 T5와 같은 트랜스포머 기반 모델에 사용합니다. 이는 고정 어휘로 확장하고 양자화하여 이루어지며, 가우시안 프로세스를 통해 생성된 공개 및 합성 데이터셋에서 교육됩니다. 20M부터 710M 매개변수를 가지는 모델로, Chronos 모델은 보다 전통적이고 심층적 학습 모델보다 더 나은 성능을 보여주며, 기존 데이터셋에 대해 제로샷 성능이 경쟁력이 있거나 우수합니다.

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

# The Approach

![Image](/assets/img/2024-07-09-PaperReviewChronosLearningtheLanguageofTimeSeries_1.png)

## Tokenization

When preparing time series data for transformer-based language models, we follow two essential steps: scaling and quantizing. Scaling brings the data into a standard range through mean scaling, where each point is adjusted by the mean of the absolute values within the historical context. After scaling, quantization converts the real-valued series into discrete tokens by dividing the data range into bins, each bin represented by a token. The authors advocate for uniform binning over quantile binning to accommodate the variability seen across different datasets. One potential limitation is that the prediction range is confined by the predefined minimum and maximum values of the bins. Additionally, special tokens for padding and sequence ending are introduced.

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

## 목적 함수

Chronos은 시계열 데이터에서 예측을 분류 문제로 처리하여 훈련되었습니다. 이 과정에서 카테고리별 교차 엔트로피 손실 함수를 사용합니다. 모델은 양자화된 시계열 데이터를 나타내는 토큰화 어휘에 대한 분포를 예측하고, 이 분포와 실제 그라운드 트루스의 분포 간의 이격을 최소화합니다. 거리를 고려하는 메트릭과 달리, 이 접근 방식은 구간 간의 근접성을 직접적으로 고려하지 않고, 모델이 데이터로부터 구간 간의 관계를 학습하도록 의존합니다. 이 접근 방식은 기존 언어 모델 아키텍처 및 훈련 방법과 매끄럽게 통합되는 장점을 제공하며, 모델 구조나 훈련 목표를 변경하지 않고도 임의의 다중 모드 출력 분포를 학습할 수 있는 능력을 제공합니다. 다양한 도메인에서 다재다능하게 사용할 수 있습니다.

Chronos 모델은 미래 시간 단계에 대한 예측 토큰 분포에서 자기회귀적으로 샘플링함으로써 확률적 예측을 생성합니다. 이후 생성된 토큰들은 양자화 함수와 역 스케일링을 이용하여 실제 값으로 변환됩니다.

# 데이터 확대

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

TSMix: 시간 시리즈 믹스업. TSMix는 이미지 분류를 위해 개발된 Mixup 데이터 증가 개념을 시계열 데이터로 확장한 것으로, 학습 데이터셋에서 시간 길이가 다른 여러 데이터 포인트를 결합합니다. 무작위로 학습 데이터셋에서 다양한 길이의 시계열을 선택하고 이를 스케일링하여 볼록 조합을 생성합니다. 이 볼록 조합의 가중치는 대칭 디리클레 분포에서 무작위로 추출됩니다.

KernelSynth: 가우시안 프로세스를 활용한 합성 데이터 생성. KernelSynth은 가우시안 프로세스를 사용하여 합성 시계열 데이터를 생성하는 방법입니다. KernelSynth은 GP 커널을 조합하여 새로운 시계열을 생성하며, 추세, 부드러운 변동, 계절성과 같은 일반적인 시계열 패턴을 위한 기본 커널 뱅크를 활용합니다. 이러한 커널을 무작위로 선택하고 덧셈 또는 곱셈을 통해 결합함으로써 다양한 시계열 데이터를 생성합니다.

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

# 실험 결과

![이미지](/assets/img/2024-07-09-PaperReviewChronosLearningtheLanguageofTimeSeries_4.png)

도메인 내 결과입니다. 대형 Chronos-T5 모델(Base 및 Large)은 기본 모형을 현저히 능가하며 우수한 확률적 및 포인트 예측 능력을 보여줍니다. 이러한 모델들은 AutoETS 및 AutoARIMA와 같은 지역 통계 모델뿐만 아니라 PatchTST 및 DeepAR과 같은 작업별 딥 러닝 모델도 능가합니다. 더 작은 Chronos 변형 및 Chronos-GPT2도 대부분의 기준선을 능가하지만 일부 경우에는 PatchTST가 더 강력한 결과를 보입니다. 계절적 순진 모델의 경쟁력 있는 성능은 에너지 및 운송 부문을 중심으로 한 이러한 데이터셋이 강한 계절적 추세를 가지고 있다는 것을 나타냅니다.

![이미지](/assets/img/2024-07-09-PaperReviewChronosLearningtheLanguageofTimeSeries_5.png)

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

영점 결과. Chronos 모델은 지역 통계 모델과 대부분의 과제별 모델보다 확률적 예측에서 우수한 성과를 보였습니다. Chronos-T5 Large 모델은 점 예측에서 세 번째로 랭크되었습니다. 심지어 ForecastPFN (영점 예측기)와 GPT4TS (GPT2의 파인튜단 모델)보다도 뛰어나며, 종합적인 시계열 예측 모델로서 주목할만한 잠재력을 나타냈습니다.

![Chronos Learning the Language of Time Series](/assets/img/2024-07-09-PaperReviewChronosLearningtheLanguageofTimeSeries_6.png)

작은 모델의 파인튜닝은 상당한 성능 향상을 나타내어, 영점 설정에서 더 큰 Chronos 변형 모델들과 최고의 과제별 모델들을 능가하는 결과를 보였습니다.

![Chronos Learning the Language of Time Series](/assets/img/2024-07-09-PaperReviewChronosLearningtheLanguageofTimeSeries_7.png)

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

**전문가 팁:**

- 대형 모델이 더 좋다는 것은 예상가능한 일입니다.
- 무작위 가중치 초기화가 LLM 가중치 사용보다 나을 수 있습니다. 시계열 예측에 관련성이 없을 수 있기 때문입니다.
- TSMix는 제로샷 학습을 향상시켰지만 도메인 내에서는 그렇지 않았습니다.
- 약 10%의 합성 데이터를 사용하는 것이 최적입니다.

# 토론

이 연구는 다양한 데이터셋에서 Chronos의 제로샷 예측 능력을 입증하며, 저랭크 어댑터 또는 과제별 보정을 위한 형식 조정 기법과 같은 정밀 조정 기술을 통해 과제별 모델을 능가할 잠재력을 시사합니다. LightGBM과 같은 모델과 함께 태스크별 어댑터 또는 스태킹 앙상블을 사용하여 공변량을 추가하고 다변량 예측에 적용할 수 있습니다.

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

![Chronos Model](/assets/img/2024-07-09-PaperReviewChronosLearningtheLanguageofTimeSeries_8.png)

안녕하세요! Chronos 모델의 성능을 높이려면 몇 가지 기술적인 접근 방식이 있습니다.

Chronos 모델의 종류 중 대형 모델은 특정 작업용 딥 러닝 모델보다 추론 속도가 느릴 수 있지만, 실제로는 실용적인 배포에 지장을 주는 정도는 아닙니다. Chronos 모델의 장점은 각각의 작업별 특정 훈련이 필요하지 않는 다양한 데이터셋 특성에서의 유연성에 있습니다. 또한 최적화된 컴퓨팅 커널, 양자화 및 더 빠른 디코딩 방법과 같은 기술은 Chronos에도 적용되어 추론 속도와 예측 품질을 향상시킬 수 있습니다.

긴 컨텍스트 데이터를 처리하는 방법은 Chronos의 성능을 향상시킬 수 있고, 높은 주파수 데이터셋에서는 NLP 기법을 적용하여 추정의 효율성과 정확도를 개선할 수 있습니다. 추가적인 실험과 탐구를 통해 Chronos 모델을 개선하는 방안을 고려해 보세요. 번창하세요! 🌟
