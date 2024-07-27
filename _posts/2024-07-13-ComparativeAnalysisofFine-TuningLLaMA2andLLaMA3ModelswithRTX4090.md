---
title: "RTX 4090을 사용한 LLaMA 2와 LLaMA 3 모델의 비교 분석"
description: ""
coverImage: "/assets/img/2024-07-13-ComparativeAnalysisofFine-TuningLLaMA2andLLaMA3ModelswithRTX4090_0.png"
date: 2024-07-13 23:04
ogImage: 
  url: /assets/img/2024-07-13-ComparativeAnalysisofFine-TuningLLaMA2andLLaMA3ModelswithRTX4090_0.png
tag: Tech
originalTitle: "Comparative Analysis of Fine-Tuning LLaMA 2 and LLaMA 3 Models with RTX 4090"
link: "https://medium.com/towards-artificial-intelligence/comparative-analysis-of-fine-tuning-llama-2-and-llama-3-models-b476a06c7879"
---


LLM 작업을 시작할 때 중요한 것은 어떤 모델을 사용할 지에 대한 질문입니다. LLaMA 모델의 팬으로서, LLaMA 3이 반드시 LLaMA 2보다 나은지 궁금해졌습니다. 이 분석은 두 모델의 실제 성능을 세밀한 조정 작업에서 비교하며, 특히 제한된 vRAM과 예산과 같은 제약 조건 하에서의 성능을 살펴봅니다.

## 설정

내 PC 환경은 Intel(R) Core(TM) i7–14700KF 3.40 GHz 프로세서와 NVIDIA GeForce RTX 4090 GPU가 장착된 Alienware R16를 사용하고 있습니다. 이전에는 RTX 3070을 사용했지만 너무 느리고 vRAM 문제에 취약하다는 점을 발견했습니다. 제 NVIDIA-SMI 버전은 550.76.01이며, 드라이버 버전은 552.44이며, CUDA 버전은 12.4입니다.

검토 중인 2가지 모델은 LLaMA 2와 LLaMa 3입니다. LLaMA 2는 Hugging Face에서 meta-llama/Llama-2–7b에서 확인할 수 있습니다. 이 모델은 7b 모델입니다. LLaMa 3는 meta-llama/Meta-Llama-3–8B에서 확인할 수 있습니다. 이 모델은 80억 개의 파라미터를 가지고 있습니다.

<div class="content-ad"></div>

Luca Massaron’s notebook on Kaggle를 기반으로 한 스크립트를 참고하여 수정했어요. RTX 4090에서 로컬에서 실행할 수 있도록 하고 두 모델을 수용하도록 변경했어요.

## 방법론

금융 감성 분석을 위해 모델을 세밀하게 조정했어요. 사용한 데이터셋은 FinancialPhraseBank 데이터셋인데, 이는 소매 투자자의 관점에서 금융 뉴스 헤드라인과 감성 분류 레이블의 포괄적인 컬렉션입니다. 데이터는 여기에서 찾을 수 있어요: [takala/financial_phrasebank](https://github.com/takala/financial_phrasebank) Hugging Face의 Datasets에 있어요.

전체 데이터셋에서 900개의 예시를 학습용으로, 900개를 테스트용으로 샘플링했어요. 원래 4840개의 문장으로 구성된 영어 금융 뉴스가 감성에 따라 분류되어 있어요. 학습용과 테스트용 세트에 있는 예시들은 균형을 이루고 있어요. 즉, 긍정적, 중립적, 부정적인 샘플 수가 동일하다는 뜻이에요.

<div class="content-ad"></div>

우선, 두 모델은 처음에 그대로 평가되었습니다. 그런 다음 다른 매개변수를 사용해 미세 조정하여 대상 모듈과 epoch에 집중했습니다. 감정은 Positive, Neutral, Negative 세 가지 범주로 나누어지며, 각각을 각각 2, 1, 0에 매핑하고 있습니다. 출력이 없을 경우 중립으로 매핑되며 1입니다.

## 1. 기준 성능

### 1.1 LLaMA 2

금융 감성 분석 작업을 위해 미세 조정하기 전 LLaMA 2 모델의 초기 성능은 아래의 분류 보고서에 요약되어 있습니다. 이 모델의 성능 지표는 300개의 샘플을 포함하는 각 범주 (0, 1, 2)를 통해 평가되었습니다.

<div class="content-ad"></div>

### 모델의 종합 정확도는 37%로, 모델이 인스턴스의 37%를 올바르게 분류한다는 것을 나타냅니다. 매크로 및 가중 평균은 모델의 모든 클래스에 대한 성능을 종합적으로 보여줍니다. 긍정적 및 부정적 감정에 대한 정밀도는 비교적 높지만, 재현율과 F1-점수는 낮습니다. 이는 모델이 어떤 클래스에 대해 정밀도는 높지만 각 클래스의 실제 인스턴스를 올바르게 식별하는 데 잘하지 못한다는 중요한 불균형을 강조합니다.

### LLaMA 3

![LLaMA 3 이미지](/assets/img/2024-07-13-ComparativeAnalysisofFine-TuningLLaMA2andLLaMA3ModelswithRTX4090_1.png)

<div class="content-ad"></div>

***안녕하세요!***

금융 감성 분석 작업에 대한 세부 분류 보고서에 따르면, LLaMA 3 모델의 세밀 조정 이전 초기 성능을 아래 분류 보고서에 요약해 드리겠습니다.

해당 모델의 전체적인 정확도는 36%로, 이는 이전 모델보다 1% 낮은 성과를 보입니다. 정밀도는 보통 수준이지만, 리콜과 F1 점수는 낮아서 모델이 일부 클래스를 다른 것보다 더 잘 예측하는 불균형을 보여주고 있습니다.

재미있는 점은 부정 클래스에 대한 정밀도와 리콜인데요. 각각 1과 0.02로 나타나는데요. 정밀도 1.00은 부정 감성으로 예측된 모든 사례가 실제로 부정이었으며, 거짓 양성이 없다는 것을 의미합니다. 그러나 0.02의 리콜은 모델이 실제 부정 감성 사례의 전체 2%만 올바르게 식별했음을 의미하여 낮은 F1 점수를 초래하고 있습니다. 이는 매우 바람직하지 않습니다.

기본 설정에서 LLaMA 2는 전체적인 정확도가 37%로 LLaMA 3보다 약간 우수한 성과를 보입니다.***

<div class="content-ad"></div>

# 2. 섬세한 조정 결과 비교

LLaMA 2 및 LLaMA 3와 같은 언어 모델을 로우랭크 적응(Low-Rank Adaptation) 기술을 사용하여 세밀하게 조정하는 맥락에서 target_modules 매개변수는 트레이닝 중에 어떤 모델 레이어가 조정되는 지를 지정합니다. target_modules의 선택은 섬세한 조정 프로세스의 효율에 상당한 영향을 미칠 것이며, 이것이 트랜스포머 모델의 어텐션 메커니즘에서 중요한 요소인 q와 v만 튜닝 프로세스에 포함시키는 이유입니다.

2.1 LLaMA 2 — target_modules=["q_proj", "v_proj"]

![이미지](/assets/img/2024-07-13-ComparativeAnalysisofFine-TuningLLaMA2andLLaMA3ModelswithRTX4090_2.png)

<div class="content-ad"></div>

LLaMA 2 모델을 2번의 에포크 설정으로 세밀하게 조정하고 q_proj 및 v_proj 모듈을 대상으로 하여, 금융 감성 분석 작업의 성능 지표가 크게 향상되었습니다. 정확도가 77%로 증가했으며, 정밀도, 재현율 및 F1 점수에 대한 균형 잡힌 Marco 및 Weighted 평균이 나타났습니다. 이 세밀한 조정 접근은 모델이 부정적, 중립적 및 긍정적 감정을 식별하는 능력을 향상시켰습니다. LLaMA 3에 비해 더 균형 잡힌 f1 점수는 모델의 2개의 중요한 레이어를 조정해도 LLaMA 2가 보다 나은 결과를 얻을 수 있다는 것을 보여줍니다.

2.2 LLaMA 3— target_modules=["q_proj", "v_proj"]

![Comparative Analysis of Fine-Tuning LLaMA 2 and LLaMA 3 Models with RTX4090](/assets/img/2024-07-13-ComparativeAnalysisofFine-TuningLLaMA2andLLaMA3ModelswithRTX4090_3.png)

위 차트에서 볼 수 있듯이, LLaMA 3 모델을 2번의 에포크 설정으로 세밀하게 조정하고 q_proj 및 v_proj 모듈을 대상으로 한 결과, 전반적인 성능이 크게 향상되었습니다. 정확도는 75%로 증가했으며, 정밀도, 재현율 및 F1 점수에 대한 균형 잡힌 Marco 및 Weighted 평균이 나타났습니다. 하지만 클래스 1을 식별하는 능력은 아직도 만족스럽지 않습니다.

<div class="content-ad"></div>

이제 2개의 중요한 레이어가 아닌 모든 레이어로 모델을 파인튜닝하는 것이 모델 최종 성능에 어떤 영향을 미치는지 살펴봅시다.

![이미지](/assets/img/2024-07-13-ComparativeAnalysisofFine-TuningLLaMA2andLLaMA3ModelswithRTX4090_4.png)

2개의 에포크로 LLaMA 2 모델을 파인튜닝하고 모든 선형 레이어를 대상으로 설정한 결과, 베이스라인 모델과 대비해 전체 성능이 상당히 향상되었습니다. 정확도가 80%로 증가했으며, precision, recall 및 F1 스코어에 대한 균형 잡힌 마크로 및 가중치 평균을 보여줍니다. 각 클래스의 F1 스코어가 향상되어 전체 정확도의 80%를 기여합니다.

<div class="content-ad"></div>

2.4 LLaMA 3— target_modules=["all-linear"]

![LLaMA Image](/assets/img/2024-07-13-ComparativeAnalysisofFine-TuningLLaMA2andLLaMA3ModelswithRTX4090_5.png)

Looking at the image above, we can observe that fine-tuning both LLaMA 2 and LLaMA 3 models with an epoch setting of 2 and targeting all linear layers has notably enhanced their performance. Among the two, LLaMA 3 stands out with a more balanced precision score and higher F1-score across various metrics, positioning it as the preferred choice for financial sentiment analysis tasks.

Considering that fine-tuning with all linear layers has yielded better outcomes in both models so far, we will continue to implement target_modules=["all-linear"] in all subsequent tests and only adjust the number of epochs.

<div class="content-ad"></div>

2.5 LLaMA 2 - epoch=3

에포크(epochs) 수는 파인튜닝 과정에서 중요한 초매개변수(hyperparameter)입니다. 적절하게 설정하는 것은 학습 시간, 자원 활용, 그리고 모델 성능 사이의 균형을 맞추는 작업을 포함하고 있습니다. 일반적으로 vRAM 제한이 있는 경우, 이는 먼저 하향 조정할 파라미터 중 하나입니다. 보통 목표는 모델이 좋은 일반화를 달성하면서도 과도한 학습 시간이나 자원 소비를 초래하지 않는 '적절한 괜찮은 지점'을 찾는 것입니다.

Markdown 형식으로 변환하면 다음과 같습니다:

![image](/assets/img/2024-07-13-ComparativeAnalysisofFine-TuningLLaMA2andLLaMA3ModelswithRTX4090_6.png)

상기 그래프에서, 타겟 모듈 'all_linear'로 LLaMA 2 모델을 3 에포크 동안 파인튜닝한 결과, 모든 감성 클래스에서 성능이 더욱 향상되었습니다. 모델은 이제 높은 정확도, 정밀도, 재현율, 그리고 F1-스코어를 보여줌으로써, 재무 감성 분석에 대한 효과적인 분류 능력을 갖추고 있다는 것을 보여줍니다. 이 개선은 파인튜닝을 통해 모델이 텍스트의 감정을 올바르게 식별하고 분류하는 능력을 향상시키는 데 효과적임을 강조합니다. 전체적인 정확도는 지금 82%로, 이전 epoch=2 결과에 비해 상당한 개선을 보입니다.

<div class="content-ad"></div>

**2.6 LLaMA 3— 시대=3**

![Comparative Analysis of Fine-Tuning LLaMA2 and LLaMA3 Models with RTX4090_7](/assets/img/2024-07-13-ComparativeAnalysisofFine-TuningLLaMA2andLLaMA3ModelswithRTX4090_7.png)

이 모델의 전체적인 정확도는 86%로, LLaMA 3의 이전 결과(83%)와 비교했을 때 감정을 정확히 분류하는 능력이 상당히 향상되었습니다. 모든 클래스를 대상으로 한 macro와 weighted 평균은 정밀도, 재현율, 그리고 F1 점수에서 균형 잡힌 높은 성능을 보여주었습니다.

동일한 매개변수 설정으로 LLaMA 2의 결과와 비교했을 때, LLaMA 3는 모든 클래스에서 더 높고 균형 잡힌 점수를 보여줍니다.

<div class="content-ad"></div>

2.7 LLaMA 2 - epoch=5

Hi there, it seems like the model might be overfitting after epoch 2 or 3. The noticeable drop in training loss along with the increase in validation loss post-epoch 2 indicates that the model is becoming too focused on fitting the training data precisely, rather than performing well on the validation set. For this scenario, the ideal number of epochs would probably be around 2 or 3, where the validation loss was at its lowest before starting to rise. 

![Comparative Analysis of Fine-Tuning LLaMA 2 and LLaMA 3 Models with RTX 4090](/assets/img/2024-07-13-ComparativeAnalysisofFine-TuningLLaMA2andLLaMA3ModelswithRTX4090_8.png)

![Comparative Analysis of Fine-Tuning LLaMA 2 and LLaMA 3 Models with RTX 4090](/assets/img/2024-07-13-ComparativeAnalysisofFine-TuningLLaMA2andLLaMA3ModelswithRTX4090_9.png)

<div class="content-ad"></div>

에포크 5에서 오버핏이 관찰되었지만, LLaMA 2 모델을 5 에포크 동안 파인튜닝한 결과, 3 에포크 대비 모든 감정 클래스에서 성능이 약간 향상되어 84%의 정확도를 달성했습니다. 이 향상은 모델이 텍스트의 감정을 올바르게 식별하고 분류하는 능력을 향상시키는 데 확장된 파인튜닝의 효과를 강조합니다. 하지만 효율성 측면에서는 2 또는 3 에포크에서 중지하는 것이 더 나았을지도 모릅니다.

2.8 LLaMA 3— epoch=5

![image1](/assets/img/2024-07-13-ComparativeAnalysisofFine-TuningLLaMA2andLLaMA3ModelswithRTX4090_10.png)

![image2](/assets/img/2024-07-13-ComparativeAnalysisofFine-TuningLLaMA2andLLaMA3ModelswithRTX4090_11.png)

<div class="content-ad"></div>

이전에 본 LLaMA 2와 유사하게, 훈련 레코드는 에포크를 거칠수록 훈련 손실이 일정하게 감소하는 모습을 보여주는 반면, 검증 손실은 처음에 약간 감소한 후 다시 증가하는 양상을 보입니다. 이는 잠재적으로 오버피팅을 시사하는 것으로 생각됩니다.

모델의 전체 정확도는 계속하여 86%를 유지하고 있으며, 훈련을 계속할 경우 모델이 감정을 올바르게 분류하는 능력에 도움이 되지 않을 것으로 나타납니다. 5번의 에포크 이후, LLaMA 3는 여전히 LLaMA 2와 비교했을 때 더 나은 정확도를 유지하고 있습니다.

# 3. 결론

![Comparative Analysis of Fine-Tuning LLaMA 2 and LLaMA 3 Models with RTX4090](/assets/img/2024-07-13-ComparativeAnalysisofFine-TuningLLaMA2andLLaMA3ModelswithRTX4090_12.png)

<div class="content-ad"></div>

모든 실험을 거친 결과, 제한된 자원을 사용할 때 LLaMA 2가 특정 레이어만 세밀하게 조정할 때 좋은 성능을 보인다는 것을 발견했습니다. 그러나 자원이 더 많으며 확장된 세밀 조정에 사용 가능할 때 LLaMA 3는 더 높은 정확도와 균형 잡힌 성능을 보여줍니다.

초기 질문으로 돌아가 보면, LLaMA 3이 LLaMA 2보다 나은지는 자원과 제한에 따라 다릅니다. 예산이 촉박한 경우 LLaMA 2는 튼튼한 선택이지만, 자원이 더 많이 확보된다면 LLaMA 3가 우수한 성능을 제공합니다.

이러한 비교 분석에 대한 당신의 생각은 무엇인가요?

나중에 노트북을 제공할 예정입니다.

<div class="content-ad"></div>

감사합니다! 이 글을 좋아하셨다면 데이터 과학 친구들과 공유해주시고, 저를 팔로우해주세요. 커뮤니티에 기여를 계속할 동기가 됩니다.