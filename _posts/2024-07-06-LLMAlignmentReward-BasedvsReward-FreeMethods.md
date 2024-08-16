---
title: "LLM 정렬 보상 기반 방법 vs 보상 없는 방법 비교"
description: ""
coverImage: "/assets/img/2024-07-06-LLMAlignmentReward-BasedvsReward-FreeMethods_0.png"
date: 2024-07-06 03:42
ogImage: 
  url: /assets/img/2024-07-06-LLMAlignmentReward-BasedvsReward-FreeMethods_0.png
tag: Tech
originalTitle: "LLM Alignment: Reward-Based vs Reward-Free Methods"
link: "https://medium.com/towards-data-science/llm-alignment-reward-based-vs-reward-free-methods-ef0c0f6e8d88"
isUpdated: true
---





## LLM 정렬을 위한 최적화 방법

# 문맥

언어 모델은 사용자가 제공한 프롬프트를 기반으로 다양한 매력적인 텍스트를 생성하는 놀라운 능력을 보여주고 있습니다. 그러나 "좋은" 텍스트를 정의하는 것은 어렵습니다. 개인의 취향과 구체적인 맥락에 따라 달라지기 때문입니다. 예를 들어, 이야기를 하는 경우 창의성이 중요하고, 정보를 전달하는 내용을 작성할 때는 정확성과 신뢰성이 중요하며, 코드를 생성할 때는 올바르게 실행되는지 확인하는 것이 필수적입니다. 따라서 "LLM 정렬 문제"는 대규모 언어 모델 (LLM)이 인간의 가치관, 의도 및 선호도와 일치하는 방식으로 작동하는 것을 보장하는 과제를 가리킵니다.

문자, 정확성 또는 실행 가능성과 같은 다양한 텍스트 속성을 포착하는 손실 함수를 설계하는 것은 매우 복잡하고 종종 비현실적입니다. 이러한 개념은 미분 가능하지 않으므로 역전파되지 않으며 간단한 다음 토큰 생성으로 훈련할 수 없습니다.

<div class="content-ad"></div>

만약 우리가 인간 피드백을 활용해 생성된 텍스트의 품질을 평가하거나, 더 나아가 그 피드백을 모델 성능 향상을 위한 유도하는 손실 함수로 사용할 수 있다면 얼마나 좋을까요? 이 개념이 Reinforcement Learning from Human Feedback(RLHF)의 핵심입니다. RLHF는 강화 학습 기술을 적용하여 우리에게 직접적인 피드백을 기반으로 언어 모델을 세밀하게 조정할 수 있게 해줍니다. 이를 통해 모델을 미묘한 인간의 가치와 기대에 더 밀접하게 조정할 수 있습니다. 이 접근법은 민감하게 반응하면서도 인간의 선호도의 복잡성과 더 잘 일치하는 언어 모델을 훈련하는 새로운 가능성을 열어주었습니다.

이제, 보상 중심 학습을 통한 RLHF와 보상이 없는 방법을 통한 RLHF에 대해 자세히 살펴보겠습니다.

# 보상 중심 시스템을 통한 인간 피드백을 통한 Reinforcement Learning이 무엇인가요 (RLHF)?

Reinforcement Learning through Human Feedback(RLHF)를 살펴보겠습니다. 이는 세 가지 주요 단계로 구성됩니다:

<div class="content-ad"></div>

- 지도된 파인튜닝
- 보상 모델링 단계
- 강화학습 파인튜닝 단계

**지도된 파인튜닝**

RLHF는 이미 고품질 데이터셋에서 사전 훈련된 모델입니다. 그 목적은 간단합니다. 즉, 입력(프롬프트)을 주면 출력을 생성합니다. 여기서의 궁극적인 목표는 이 모델을 더욱 세밀하게 조정하여 인간의 선호에 따라 출력을 생성하는 것입니다. 따라서 이를 참조용 기본 모델이라고 부를 수 있습니다. 현재 이 모델은 어떤 인간의 선호도에 대해 인식하지 못하는 바닐라 기본 모델입니다.

**보상 모델링 단계**

<div class="content-ad"></div>

보상 모델 혁신: 새로운 혁신의 시작, 보상 모델이 RLHF에 어떻게 통합되는지에 대한 것이다. 보상 모델의 아이디어는, 위에서 언급한 기본 모델과 동일할 수 있는 새로운 LLM 모델이 인간 선호 점수를 생성할 수 있는 능력을 갖게 된다는 것이다. 이 모델이 대형 언어 모델과 유사한 이유는, 이 모델도 출력이 인간이 선호하는지 아닌지 평가하기 전에 언어 의미론을 이해해야하기 때문이다. 보상이 스칼라이기 때문에, LLM 위에 선형 레이어를 추가하여 인간의 선호에 따른 스칼라 점수를 생성한다.

데이터 수집 단계: 이는 지도된 세밀한 조정 단계에서 수행된다. 여기서 기본 모델은 주어진 텍스트에 대해 2개의 출력을 생성하도록 요청된다. 예시: 입력 토큰 x에 대해 기본 모델에 의해 y1과 y2 두 출력 토큰이 생성된다. 이러한 출력은 인간 평가자에게 보여지고, 각각의 출력에 대한 인간 선호가 기록된다.

훈련 단계: 데이터 수집 단계에서 수집된 데이터 샘플이 있으면, 보상 모델은 다음 프롬프트로 훈련된다. “다음 입력이 주어졌을 때: `x`, LLM이 `y` 출력을 생성했습니다. 출력의 성능을 평가할 수 있나요?”. 이 모델은 r(보상)을 출력하고, 이미 데이터 수집 단계에서 보상 r1의 실제 값이 알려져 있다. 이제 이를 손실 함수와 함께 역전파하여 모델을 훈련시킬 수 있다. 아래는 모델이 역전파를 통해 최적화하는 목적 손실 함수이다:

![이미지](/assets/img/2024-07-06-LLMAlignmentReward-BasedvsReward-FreeMethods_0.png)

<div class="content-ad"></div>

Notation:

- rΦ(x, y): a reward model parameterized by Φ which estimates the reward. Here, "parameterized" means we do not know the actual value and it needs to be optimized from the above equation. This model represents the reward LLM model itself. Typically, the LLM parameters are fixed, and only a few parameters are left to be adjusted. The most critical layer is the linear layer added at the top, responsible for learning to rate the output score.
- Ɗ: A dataset of triplets (x, yw, yl) where x: input, yw: the winner output, and yl: the loser output.
- σ: the sigmoid function that maps the difference in reward to a probability (0–1).
- ∑(x, y, w, yl) ~ Ɗ means x, yw, yl are all sampled from Ɗ.

Let's consider an example scenario: suppose you are training a reward model to evaluate responses. You have pairs of responses to a given prompt, and human feedback helps you determine which response is better. For instance, with x("What is the capital of France?"), you have yw("The capital of France is Paris.") as the winner and yl("The capital of France is Berlin.") as the loser. The reward model is expected to learn to assign a higher reward to the output "The capital of France is Paris." compared to "The capital of France is Berlin." given the input "What is the capital of France?"

## RL fine-tuning phase

<div class="content-ad"></div>

강화 학습 아이디어: 이제 베이스 모델과 보상 모델이 훈련을 받았는데, 아이디어는 보상 모델 점수를 활용하고 베이스 모델 매개변수를 업데이트하여 인간의 선호를 반영하는 것입니다. 보상 모델이 스칼라 점수를 출력하고 미분 가능하지 않기 때문에 간단한 역전파를 사용하여 베이스 모델 매개변수를 업데이트할 수 없습니다. 따라서 베이스 모델을 업데이트하기 위해 다른 기술이 필요합니다. 이것이 바로 강화 학습이 나오는 곳인데, 이것은 베이스 모델이 보상 모델 점수를 통해 매개변수를 조정하는 데 도움을 주는 것입니다. 이는 PPO(Proximal Policy Optimization)를 통해 수행됩니다. PPO의 핵심 아키텍처를 이해하는 것은 이 개념을 파악하는 데 꼭 필요한 것은 아니므로 여기서 다루지는 않겠습니다. 그러나 일반적으로 PPO는 스칼라 점수를 사용하여 베이스 모델 매개변수를 업데이트할 수 있습니다. 이제 베이스 및 보상 모델이 어떻게 결합되어 베이스 모델이 인간의 선호도를 학습하는 데 도움이 되는지 알아보겠습니다.

강화 학습 미세 조정 아이디어: 강화 학습에서는 행동, 공간 및 보상이 있습니다. 아이디어는 공간에서 어떤 행동 대리인이 최대 보상을 창출하는 정책을 찾는 것입니다. 이것은 꽤 복잡해질 수 있지만 단순화된 의미에서 π는 정책으로, 이것이 우리의 베이스 LLM 모델입니다. Πref은 베이스 모델을 의미하고 ΠӨ는 생성하려는 다른 LLM 최적 모델을 나타냅니다. 우리는 인간의 선호 출력을 제공하는 ΠӨ를 찾아야 합니다. 우리가 ΠӨ를 모르고 있을 뿐이며, 이 최적 모델을 찾는 것이 아이디어입니다.

강화 학습 훈련 및 피드백 루프 단계: 입력 x는 2개의 정책 모델, Πref(기준 모델) 및 ΠӨ(생성하려는 최적 모델)에 제공됩니다. 초기에 두 모델은 동일합니다. 개별적으로 두 모델에 x를 입력하면 각각 두 개의 출력이 생성됩니다. ΠӨ 모델의 출력은 보상 모델에도 전달됩니다 (입력: x, 출력: y; 위에서 논의한 대로) 그리고 보상 점수인 rΦ(x, y)를 출력하도록 요청됩니다. 이제 우리에게는 2가지가 생겼습니다. 

-기준 모델에서의 출력
-최적 모델에서의 출력
-최적 모델의 보상 점수
마지막으로 우리는 2가지를 최적화하는데 노력합니다. 보상을 최대화하기 원하며 결국 모델이 인간의 선호와 가까워지기를 원하며 다른 하나는 기준 모델과 최적 모델 사이의 발산을 최소화하는 것입니다. 보상을 최대화하는 것은 이미 스칼라 양이므로 쉽지만 기준 모델과 최적 모델 간의 발산을 어떻게 최소화해야 할까요? 여기서는 "Kullback–Leibler 발산"을 사용하여 2개의 연속 확률 분포 사이의 차이를 추정합니다. 객체 함수의 목표 손실 함수에 대해 좀 더 자세히 살펴보겠습니다.

<div class="content-ad"></div>

**주의 사항:**

- rΦ(x, y): 최적 모델의 입력 x와 출력 y에 대한 스칼라 값입니다. 명확하게 하기 위해, 최적 모델의 출력은 보상 모델에 공급됩니다.
- Dkl (ΠӨ (y | x) || Πref (y | x)): 이것은 두 확률 분포 사이의 쿨백-라이블러 발산을 계산합니다. 각 모델의 각 토큰은 확률 분포입니다. KL은 분포가 얼마나 서로 다른지를 추정합니다.
- β: 최적 모델을 기준 모델에 가깝게 가져야 하는 중요성을 결정하기 위해 사용되는 하이퍼파라미터입니다.

예시 시나리오: "프랑스의 수도는 어디입니까?" 라고 묻는다고 상상해보세요. Πref (기준 모델)는 "프랑스의 수도는 베를린입니다."라고 말하고, ΠӨ (최적 모델)은 "3개의 수도가 있으며, 파리, 베르사유, 리옹이 있지만, 파리가 공식 수도로 간주됩니다." 라고 합니다. 이제 rΦ("x: 프랑스의 수도는 어디...","y: 3개의 수도가 있습니다...")는 인간 선호도가 덜한 결과이므로 낮은 점수를주어야하며, (ΠӨ(y | x)의 Πref(y | x)의 Kullback-라이블러 발산)는 두 개별 출력에 대해 확률 분포 공간이 서로 다률 떨어져 있기 때문에 높아야합니다. 따라서 손실이 두 용어에서 높아집니다. 모델이 보상만을 최적화하도록 하지 않도록하고, 기준 모델에 더 가깝게 유지하도록하기 위해 두 용어가 모두 사용되어 보상을 최적화합니다. 학습과정의 다음 반복에서, ΠӨ (최적 모델)이 "프랑스의 수도는 델리입니다."라고 말한다면, 모델은 Πref (기준 모델)에 더 가까이 있도록 배우고 기본 모델에 더 가까운 형식의 출력을 제공하지만 보상 요소는 여전히 낮을 것입니다. 바람직하게는 세 번째 반복에서 ΠӨ (최적 모델)이 "프랑스의 수도는 파리입니다."라는 것을 배우고 더 높은 보상으로 모델 출력이 기준 모델과 밀접하게 일치하도록하기를 희망합니다.

아래 다이어그램이 이 개념을 설명하는 데 도움이 됩니다. Hugging Face의 RLHF 링크도 적극 권합니다.

<div class="content-ad"></div>

/assets/img/2024-07-06-LLMAlignmentReward-BasedvsReward-FreeMethods_2.png

# 보상 없는 방법을 통한 인간 피드백을 통한 강화 학습이란?

보상 기반 방법을 사용하는 RLHF를 염두에 두고, 보상 없이 방법으로 이동해 봅시다. 논문에 따르면: "저희의 주요 이해는 보상 함수에서 최적 정책으로의 분석적 매핑을 활용함으로써 보상 함수에 대한 손실 함수를 정책에 대한 손실 함수로 변환할 수 있게 되었으며, 이 변수 변경 접근 방식은 명시적으로 단독 보상 모델을 맞추는 것을 피할 수 있게 해줍니다. 그러면서도 사람들의 선호도에 대한 기존 모델 아래 최적화를 수행할 수 있습니다." 이해하기 복잡하지만, 다음 섹션에서 간단한 단계로 나누어 이해해 봅시다.

보상 없는 방법의 주요 아이디어: RLHF에서는 비용이 많이 들고 유지하기 어려운 별도의 새로운 보상 모델이 훈련됩니다. 새로운 보상 모델을 훈련시키지 않고 기존 베이스 모델을 사용하여 새로운 최적 모델을 도달할 수 있는 메커니즘이 있을까요? 바로 보상 없는 방법이 하는 일입니다. 즉, 새로운 보상 모델을 훈련시키는 것을 피하고, 그 결과로 DPO(Direct preference optimization)의 손실 함수에 보상 모델 용어가 없도록 방정식을 변경합니다. 이를 이해하는 한 가지 방법은 베이스 모델(Πref)에서 최적 모델 정책(ΠӨ)에 도달해야 한다는 점입니다. 이것은 최적 모델 정책에 도달하기 위해 보상 함수 공간을 최적화하거나 보상에서 정책으로의 매핑 함수를 직접 학습하고 이를 통해 정책 자체로 최적화하는 두 가지 방법으로 달성될 수 있습니다. 이것이 저자가 "보상 함수에서 최적 정책으로의 분석적 매핑을 활용하여 손실 함수를 정책에 대한 손실 함수로…."라고 말할 때 의미하는 것입니다. 이것이 이 논문의 핵심 혁신입니다.

<div class="content-ad"></div>

DPO 교육 및 피드백 루프 단계: Πref (기준 모델)을 사용하여, 입력 x가 제공되고 2개의 출력 (y1과 y2)을 생성하도록 요청됩니다. 모든 x, y1 및 y2는 인간 평가자들에 의해 이기는 yw와 지는 yl을 결정하는 데 사용됩니다. 오프라인 데이터 집합은 삼쌍 정보 'x, yw 및 yl'를 수집합니다. 이 정보를 통해 우승하는(인간이 선호하는) 및 패배하는(인간이 선호하지 않는) 답변을 알 수 있습니다. 이제 동일한 입력 x가 2개의 정책(모델) Πref (기준 모델)과 ΠӨ (최적 모델)에 제공됩니다. 초기에는 교육 목적으로 두 모델이 동일하게 유지됩니다. 두 모델에 각각 입력 x를 제공하면 각각 두 가지 출력을 얻게 됩니다. "Kullback–Leibler 발산"을 통해 기준 및 최적 모델에서 출력이 이기는 및 지는 답변으로부터 얼마나 먼지 계산합니다. 목적 손실 함수를 자세히 살펴보겠습니다.

방정식

![Equation](/assets/img/2024-07-06-LLMAlignmentReward-BasedvsReward-FreeMethods_3.png)

- ΠӨ (yw | x) -' 주어진 x(입력)로 모델의 해당 출력 youtput이 이기는 출력 yw로부터 얼마나 떨어져 있는가. 출력 youtput 및 yw는 확률 분포이며 두 분포 사이의 차이는 "Kullback–Leibler 발산"을 통해 계산됩니다. 이것은 스칼라 값이 될 것입니다. 또한 이는 Πref (yw | x), Πref (yl | x), ΠӨ (yw | x) 및 ΠӨ (yl | x)의 서로 다른 조합에 대해 두 모델 모두로 계산됩니다.
- β: 최적 모델이 기준 모델에 가까운 것이 얼마나 중요한지를 결정하는 데 사용되는 초모수입니다.

<div class="content-ad"></div>


![image](/assets/img/2024-07-06-LLMAlignmentReward-BasedvsReward-FreeMethods_4.png)

# 결론

- 자연스럽게, 보상 기반 방법을 사용하는 PPO 또는 DPO를 사용한 무보상 방법을 통해 RLHF를 수행하는 것 중 어느 것이 더 나은지에 대한 질문이 제기됩니다. 이 질문에 대한 정답은 없습니다. 최근 논문은 "LLM 정렬을 위해 DPO가 PPO보다 우수한가"를 비교하고 (논문 링크) PPO가 일반적으로 DPO보다 우수하며, DPO가 분포에서 벗어난 데이터에 더 많이 영향을 받는다는 결론을 내립니다. "분포 밖" 데이터란 인간의 선호 데이터가 기준 훈련 데이터와 다른 경우를 의미합니다. 이는 베이스 모델 훈련이 어떤 데이터셋에서 이루어지고 선호 출력이 다른 데이터셋에서 이루어지는 경우에 발생할 수 있습니다.
- 전반적으로, 어느 쪽이 더 나은지에 대한 연구는 아직 미정이지만, OpenAI, Anthropic, Meta와 같은 회사들이 RLHF를 위해 PPO와 DPO 모두를 LLM 정렬 도구로 활용하고 있다는 점은 확인할 수 있습니다.

# 참고문헌


<div class="content-ad"></div>

- Direct Preference Optimization: 이미지를 해석하여 나중에 표시할 수 있도록 디스크에 저장합니다. [2305.18290](https://arxiv.org/pdf/2305.18290)
- LLM 정렬을 위해 DPO가 PPO보다 우수한가요? 포괄적인 연구 [2404.10719](https://arxiv.org/pdf/2404.10719)
- Hugging Face RLHF 기사 [블로그](https://huggingface.co/blog/rlhf)