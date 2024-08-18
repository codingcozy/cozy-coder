---
title: "Google의 시간 시계열 예측을 위한 TimesFM 소개"
description: ""
coverImage: "/assets/img/2024-07-12-TimesFMGooglesFoundationModelForTime-SeriesForecasting_0.png"
date: 2024-07-12 23:30
ogImage:
  url: /assets/img/2024-07-12-TimesFMGooglesFoundationModelForTime-SeriesForecasting_0.png
tag: Tech
originalTitle: "TimesFM: Google’s Foundation Model For Time-Series Forecasting"
link: "https://medium.com/towards-data-science/timesfm-googles-foundation-model-for-time-series-forecasting-593a332dd08d"
isUpdated: true
---

![image](/assets/img/2024-07-12-TimesFMGooglesFoundationModelForTime-SeriesForecasting_0.png)

안녕하세요! 구글이 시계열 예측용 재단 모델 경쟁에 참여했어요.

2023년 8월, 시계열 커뮤니티는 Nixtla의 첫 번째 시계열 예측을 위한 재단 모델인 TimeGPT 출시로 혼란스러워졌어요.

TimeGPT를 따르며, 여러 재단 예측 모델이 출시되었지만, 하나는 눈에 띄었어요. 최근에 구글은 현격한 결과를 보여주는 혁신적인 시계열 모델인 TimesFM을 공개했어요.

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

To be able to forecast accurately, a firm understanding of time series is crucial. It is a versatile tool used in various fields such as retail, energy demand, economics, healthcare, and more. A solid foundation in time series modeling can significantly improve predictive accuracy, just like how GPT-4 revolutionized text prediction.

In this blog post, we will delve into the following topics:

- The unique challenges faced by foundational models in time series compared to NLP (Natural Language Processing).
- How TimesFM tackles these obstacles effectively.
- How TimesFM operates and why it stands out as a robust model.
- Results of benchmark tests conducted on TimesFM.
- The promising future of foundational models in the realm of time-series forecasting.

Excited to explore further? Let's dive in!

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

# Foundation Models for Time Series: Why They Pose a Challenge

The emergence of foundation models has been a significant development in NLP, exemplified by the introduction of GPT-2 in the pivotal work "Language Models are Unsupervised Multitask Learners." However, when it comes to time series data, the task of constructing a foundation model is far from simple. Here are some of the key challenges:

- **Dataset Scarcity:** While text data is abundant in NLP, time-series datasets are not as readily accessible.
- **Unpredictable Format:** Unlike language models that operate on structured grammars and vocabularies, time-series data can vary significantly in format, spanning from sparse sales figures to volatile financial data.
- **Different Granularities:** Time series models are typically designed to work at specific granularities (e.g., hourly, weekly, monthly). Adapting a foundation model to perform effectively across all granularities presents a notable challenge.
- **Variable Contexts and Horizons:** NLP models are optimized for predicting the next word within a defined context length. Conversely, a universal time-series model must contend with variable context lengths and prediction horizons.

Exploring the realms of time series modeling unveils a rich tapestry of complexities and intricacies awaiting solutions.

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

# TimesFM란 무엇인가요?

TimesFM은 더 구체적으로 다음과 같습니다:

- 200M개의 파라미터가 있습니다.
- 1000억 개의 실제 시간 지점에서 훈련되었습니다.
- 추가 공변량을 특징으로 사용할 수 있습니다.
- 인과적 셀프 어텐션과 잔여 블록을 활용합니다.
- 다른 최첨단 모델들보다 제로샷 예측에서 우수한 성과를 보여줍니다.

다음으로, TimesFM이 시계열 모델의 기초를 구축하는 과제를 어떻게 극복하는지 살펴보겠습니다.

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

# A) 거대한 양의 데이터 찾기

공개 시계열 데이터셋은 드물다.

Monash 리포지토리와 같은 표준 벤치마크(예: 관광, 전력 데이터 등을 포함하고 있음)로만으로는 한계가 있습니다.

보편적 시계열 모델을 위한 이상적인 데이터셋은 아래와 같은 특징을 갖춰야 합니다:

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

- 수백만 개의 데이터가 포함된 대규모 데이터
- 여러 도메인을 대표하는 다양한 데이터
- 매번 시간 간격(일일, 주간 등)

TimesFM의 저자들은 공개 시계열 데이터에서 3가지 추가 자원을 유리하게 활용했습니다:

- Google Trends: 저자들은 Google Trends 검색 관심을 시간별 시퀀스로 재활용했습니다.
- Wiki Pageviews: 이들은 모든 위키미디어 페이지의 시간별 조회수를 캡처했습니다.
- 합성 데이터: 저자들은 ARMA 프로세스를 사용하여 계절성, 주파수 및 트렌드가 혼합된 시계열 데이터 코퍼스를 생성했습니다.

결과는 1000억 데이터포인트로 이루어진 데이터셋이었습니다.

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

100B 데이터 포인트도 LLaMa의 1조 토큰에 비하면 작지만, 대규모 시계열 데이터셋을 만드는 좋은 시작입니다.

# B) 변수 컨텍스트와 시야

지금까지 여러 도메인과 시간 단위에서 데이터 포인트를 포함한 다양하고 대규모 데이터셋을 보유하고 있습니다.

대규모 트랜스포머가 범용 시간 패턴을 학습할 수 있을 것입니다. 하지만 컨텍스트 길이와 시야에 대해 어떤 가정을 해야 할까요?

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

수평 길이의 연구 (예측 길이)
LLMs는 다음 단어를 예측하는 데 최적화되어 있습니다 (자기회귀).

시계열 모델에 대해 두 가지 문제가 발생합니다:

- [3] [4]에서의 연구에 따르면 장기 예측에서는 다중단계 자기회귀 접근 대신 완전한 수평을 직접 예측하는 것이 더 나은 결과를 보인다고 합니다.
- 그러나 우리의 경우(고정 향상도 예측)에는 수평 길이가 사전에 알려져 있지 않은 경우 가능하지 않습니다. 범용 모델은 모든 수평 길이를 예측할 수 있어야 합니다.

TimesFM은 다른 성공적인 모델인 PatchTST에 의해 인기 있는 기술인 패칭을 활용하여 중간 지점을 찾습니다.

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

**패칭의 작동 방식**

TimesFM은 한 번에 하나의 데이터 포인트를 예측하지 않으며 전체 수평 길이를 예측하지도 않습니다. 대신, 컨텍스트와 수평 길이 모두를 패치로 나눕니다.

만약 우리가 다음과 같은 것을 갖고 있다고 가정해 봅시다:

- 크기 L의 컨텍스트 길이,
- 그리고 크기 p의 패치,
- 그러면 우리는 입력을 N = L/p 개의 패치로 분할합니다. 이러한 것들을 입력 패치라고 합니다.
- 또한 수평 길이가 h인 출력 패치가 있습니다.
- 출력 패치 크기 ` 입력 패치 크기를 허용함으로써, 저자들은 TimesFM이 어떠한 길이의 수평을 더 빠르고 정확하게 예측할 수 있게 될 수 있다는 것을 발견했습니다.

Figure 1은 TimesFM 아키텍처가 교육 중에 어떻게 보이는지 보여줍니다.

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

**이미지 첨부**

먼저, input_patch_size에 대한 값을 p로 선택하고, output_patch_size에 대한 값을 h로 선택하세요. 모델은 그런 다음 입력을 N = L/p 개의 입력 패치로 나눕니다.

첫 번째 훈련 복호화 단계에서:

- 첫 번째 패치는 입력 잔차 블록에서 처리됩니다.
- 결과는 위치 부호화 벡터에 추가됩니다.
- 단계 2의 출력은 쌓인 Transformer에 공급됩니다. 거기서 인과적 self-attention이 적용되어 각 출력 토큰이 자신 이전의 입력 토큰에만 참조할 수 있습니다.
- 마찬가지로 단계 3의 출력은 출력 잔차 블록으로 전달됩니다. 따라서 예측적인 수평선인 출력 패치가 생성됩니다. 이 패치는 손실을 계산하기 위해 실제 데이터와 비교됩니다.
- 다음 복호화 단계에서 모델은 다음 두 입력 패치를 처리하고 두 번째 출력 패치를 출력할 것입니다.

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

그러므로 결론을 내릴 수 있습니다:

여기에 그것이 있습니다! 우리는 TimesFM의 아키텍처를 개요로 설명했습니다.

## 평가 기준

마지막으로, 저자들은 TimesFM을 다른 SOTA 예측 모델(통계적인, 트리 기반, 딥러닝)과 비교하여 평가했습니다.

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

Also, the authors compared llmtime with another successful TS foundational model, using GPT-3 and LLaMa-2 with tailored modifications for time-series forecasting.

Here are the benchmark parameters:

- Scaled MAE is utilized - each model's MAE is adjusted by the MAE of a basic naive model due to varying dataset scales.
- All models undergo evaluation on a separate hold-out dataset.
- TimesFM is zero-shot in this comparison - it was not pre-trained on the held-out data.
- In contrast, all other models were explicitly trained and fine-tuned using the held-out data.
- The hold-out dataset comprises time series from three datasets: The Monash Archive, the Darts data benchmark, and the Informer benchmark. The same datasets were used to assess llmtime.

The evaluation results are displayed below:

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

**이미지 추가:**

- [TimesFMGooglesFoundationModelForTime-SeriesForecasting_2.png](/assets/img/2024-07-12-TimesFMGooglesFoundationModelForTime-SeriesForecasting_2.png)
- [TimesFMGooglesFoundationModelForTime-SeriesForecasting_3.png](/assets/img/2024-07-12-TimesFMGooglesFoundationModelForTime-SeriesForecasting_3.png)

**결과에 대한 참고사항:**

여기서 TimesFM이 분명한 승자입니다.

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

TimesFM은 모든 벤치마크에서 상위 3개 모델 중 하나로 선정되었습니다. 또한 llmtime도 능가하고 있습니다.

TimesFM은 zero-shot 추론을 수행하는데, 즉 TimesFM은 학습 중에 해당 데이터를 전혀 본 적이 없다는 것을 의미합니다! 반면에 다른 모델들은 hold-out 세트의 시계열 데이터를 개별적으로 학습했습니다.

또한, 세 벤치마크 모두 다양한 도메인, 크기, 세분성 및 수평 길이를 다루는 데이터세트로 구성되어 있습니다. 다양하고 보지 못한 데이터에 대해 경쟁력 있는 결과를 달성하는 것은 인상적인 성취입니다. 이러한 추세는 TimeGPT 벤치마크에서도 관찰되고 있습니다.

파인튜닝의 영향

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

TimesFM 논문은 세세한 세부사항이 부족합니다.

제로샷 추론은 기본 모델을 평가하는 훌륭한 방법입니다. 그렇지만 평가 데이터 일부에 대해 TimesFM을 파인튜닝했다면 결과는 어떻게 개선되었을까요?

게다가, 파인튜닝은 자연어 처리에서 표준적인 실천 방법입니다. 재미있게도 TimesGPT는 어떠한 파인튜닝 훈련 세부사항도 공유하지 않지만, 해당 API를 통해 직접 데이터셋을 파인튜닝할 수 있습니다.

# 스케일링 법칙

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

TimesFM은 본질적으로 큰 사전 훈련된 디코더 전용 트랜스포머 모델이기 때문에 모델 크기를 기준으로 성능을 평가하는 것이 타당합니다. 간단히 말하면:

스케일링 법칙은 LM(언어 모델)의 매개 변수 크기, 토큰(데이터셋 크기), 훈련 시간 및 성능 사이의 관계를 설명하는 경험적인 규칙입니다.

스케일링 법칙은 처음에는 [5]에서 소개되었지만 나중에 [6]에 의해 다듬어졌으며 이를 치치야 논문이라고도 합니다.

당연히 TimesFM 저자들은 사전 훈련 데이터셋을 동일하게 사용하고 유사한 횟수의 반복까지 시도하여 17M, 70M 및 200M 매개 변수 크기의 3개의 TimesFM 모델을 훈련한 초기 스케일링 연구를 수행했습니다.

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

결과는 아래의 이미지에서 확인할 수 있어요:

![Figure 4](/assets/img/2024-07-12-TimesFMGooglesFoundationModelForTime-SeriesForecasting_4.png)

모델이 더 많은 매개변수와 함께 확장되는 것을 명백하게 보여주며, 스케일링 법칙이 변환자 예측 모델에도 적용된다는 것을 입증했어요 ([7] 참조).

게다가 저자들은 데이터셋 크기가 모델 성능에 미치는 영향을 조사했어요. 작은 데이터셋(M4, Electricity, Traffic, and Weather)에서만 훈련시킨 결과를 큰 데이터셋(Google Trends 등) 및 합성 데이터셋과 비교했어요. 결과는 아래의 이미지에서 확인할 수 있답니다:

![Figure 5](/assets/img/2024-07-12-TimesFMGooglesFoundationModelForTime-SeriesForecasting_5.png)

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

안녕하세요!

오늘은 또 다른 흥미로운 주제에 대해 이야기해보려고 해요. 연구에서 더 많은 세부사항과 함께 다른 관계들도 포함되었다면 더 흥미로웠을 텐데요. 특히 성능 대 데이터셋 크기 및 성능 대 학습 시간 관계가 좀 더 자세히 다뤄졌다면 좋겠네요.

마지막으로, 저자들은 자신들의 구조적 선택이 어떤 영향을 미치는지 측정하기 위해 Ablation study를 실시했습니다.

새로운 정보들이 늘어나면서 지식을 더욱 풍부하게 만들어 가는 과정이 정말 흥미롭죠! 함께 공부하고 성장하는 것은 늘 즐거운 일이에요. 좋은 하루 되세요! 🌟

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

They mostly focus on the relationships between the performance of TimesFM and the sizes of the input and output patches.

Remember, the choice of breaking the input into patches and allowing the output patches to be larger is the key design choice of TimesFM. If we didn’t have input patches (e.g., the model consumed the full input in a single step), TimesFM would essentially function as an encoder-decoder model, not decoder-only.

Therefore, the authors measured how the performance and the size of input/output patches are related. The analysis is shown in Figure 6 and Figure 7:

![Figure 6](/assets/img/2024-07-12-TimesFMGooglesFoundationModelForTime-SeriesForecasting_6.png)

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

![TimesFM Model Image](/assets/img/2024-07-12-TimesFMGooglesFoundationModelForTime-SeriesForecasting_7.png)

양쪽 그림 모두 더 작은 입력 패치 (하지만 매우 작지는 않음)와 더 큰 출력 패치를 선택하도록 정당화합니다 — 효율적으로 TimesFM을 디코더 전용 모델로 만들었습니다.

# 마무리 맺음

TimesFM은 Google의 놀라운 시계열 모델로서 — 시계열 기초 모델 카테고리에 중요한 추가입니다.

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

현재 모델은 비공개 베타 상태에 있지만 구글은 이를 Google Cloud Vertex AI에서 이용할 수 있도록 계획 중입니다. 그러나 모델이나 훈련 데이터세트는 아직 공개되지 않았습니다 (구글은 모델을 오픈 소스로 공개할지 아직 고민 중입니다).

다행히 TimesFM 논문은 Nixtla의 TimesGPT 논문보다 훨씬 정보가 풍부합니다. 어떤 기초 예측 모델이어야 하는지에 대한 많은 통찰을 제공합니다.

그래도 대중적인 시계열 데이터세트의 부재를 고려하면 TS 기초 모델에 대한 연구는 아직도 멀은 길을 가야 합니다. 예를 들어 2019년에 T5가 출시되었을 때 CommonCrawl에서 파생된 750GB의 정제된 텍스트 데이터인 C4 데이터세트가 이미 사용 가능했었습니다.

내 의견으로는, 앞으로 TS 기초 모델에 대한 막대한 잠재력이 있다고 생각합니다!

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

# 읽어주셔서 감사합니다!

- Linkedin에서 저를 팔로우해 주세요!
- AI Horizon Forecast 뉴스레터를 구독해 주세요!

# 참고문헌

[1] Das et al., A decoder-only foundation model for time-series forecasting [2023]

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

[2] RadFord et al., 언어 모델은 감독되지 않은 다중 작업 학습자입니다

[3] Zhou et al., Informer: 장기 시퀀스 시계열 예측을 위한 효율적인 트랜스포머 넘어서

[4] Makridakis et al., 통계, 기계 학습 및 심층 학습 예측 방법: 비교 및 전망 (2022년 8월)

[5] Jared Kaplan et al., 신경망 언어 모델의 확장 법칙들

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

**(6) Jordan Hoffmann et al. Training Compute-Optimal Large Language Models**

**(7) Manuel Kunz et al. Deep Learning based Forecasting: a case study from the online fashion industry**
