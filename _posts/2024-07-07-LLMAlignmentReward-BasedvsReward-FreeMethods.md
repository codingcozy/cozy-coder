---
title: "LLM 정렬 보상 기반 vs 비보상 방식 비교"
description: ""
coverImage: "/assets/img/2024-07-07-LLMAlignmentReward-BasedvsReward-FreeMethods_0.png"
date: 2024-07-07 14:10
ogImage:
  url: /assets/img/2024-07-07-LLMAlignmentReward-BasedvsReward-FreeMethods_0.png
tag: Tech
originalTitle: "LLM Alignment: Reward-Based vs Reward-Free Methods"
link: "https://medium.com/towards-data-science/llm-alignment-reward-based-vs-reward-free-methods-ef0c0f6e8d88"
isUpdated: true
---

## LLM 정렬을 위한 최적화 방법

# 맥락

언어 모델은 사용자가 제시한 프롬프트를 기반으로 다양한 매력적인 텍스트를 생성하는 놀라운 능력을 보여 주었습니다. 그러나 "좋은" 텍스트가 무엇을 의미하는지 정의하는 것은 어려운 일이며 종종 개인의 선호도와 특정 맥락에 따라 달라집니다. 예를 들어, 이야기에서는 창의성이 핵심이며, 정보를 제공하는 콘텐츠를 만들 때는 정확성과 신뢰성이 중요하며, 코드를 생성할 때는 올바르게 실행되는 것이 중요합니다. 따라서 "LLM 정렬 문제"는 대형 언어 모델(LLMs)이 인간의 가치, 의도 및 선호에 일관되게 반응하는 방식을 보장하는 도전을 의미합니다.

창의성, 정확성 또는 실행 가능성과 같은 우리가 텍스트에서 가치 있는 다양한 특성들을 포착하는 손실 함수를 설계하는 것은 매우 복잡하고 종종 실용적이지 않습니다. 이러한 개념들은 미분 가능하지 않으며, 따라서 역전파되지 않고 단순한 다음 토큰 생성을 통해 훈련될 수 없습니다.

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

만약 우리가 인간 피드백을 활용하여 생성된 텍스트의 품질을 평가하거나, 더 나아가 그 피드백을 가이드로 사용해서 모델 성능을 향상시키는 것이 가능하다면 어떨까요? 이 개념이 인간 피드백으로부터 강화 학습(RLHF)을 하는 데에 근간을 두고 있습니다. 강화 학습 기술을 적용함으로써, RLHF는 우리에게 직접적인 인간 피드백을 기반으로 언어 모델을 세밀하게 조정할 수 있게 해주며, 모델을 미묘한 인간 가치와 기대에 더 가깝게 일치시킵니다. 이 방식은 보다 반응성이 높을 뿐만 아니라 인간의 선호도의 복잡성과도 더욱 일치하는 언어 모델을 훈련하는 새로운 가능성을 열어주었습니다.

아래에서는 보상 기반 및 보상 무관 방법을 통해 RLHF에 대해 좀 더 알아보려고 합니다.

# 인간 피드백을 통한 강화 학습 (RLHF)의 보상 기반 체계는 무엇일까요?

Reinforcement learning through human feedback (RLHF)를 보상 기반 시스템으로 살펴봅시다. 3개의 주요 단계로 구성되어 있습니다:

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

## 지도학습 미세 조정

RLHF는 높은 품질의 데이터 세트에서 이미 미세 조정된 사전 훈련 모델입니다. 여기서의 목표는 간단합니다. 즉, 입력(프롬프트)을 받으면 출력을 생성합니다. 여기서의 궁극적인 목표는 이 모델을 더욱 세밀하게 조정하여 인간의 선호에 따라 출력을 생성하는 것입니다. 따라서 이것을 참조를 위한 기본 모델이라고 부르겠습니다. 현재 이 모델은 어떠한 인간의 선호도에 대해 인식하지 못한 바닐라 기본 모델입니다.

## 보상 모형화 단계

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

보상 모델 혁신: 새로운 혁신이 시작되는 곳입니다. 보상 모델은 RLHF에 어떻게 통합되는지에 대한 새로운 아이디어입니다. 보상 모델 뒤에 있는 아이디어는 새로운 LLM 모델이 사람의 선호도 점수를 생성할 수 있다는 것입니다. 이 모델이 대규모 언어 모델과 비슷한 이유는, 출력이 사람이 선호하는지 여부를 평가하기 전에 이 모델도 언어 의미론을 이해해야 하기 때문입니다. 보상이 스칼라이기 때문에, 우리는 LLM 위에 선형 레이어를 추가하여 인간의 선호도에 따른 스칼라 점수를 생성합니다.

데이터 수집 단계: 이는 기본 모델이 주어진 텍스트에 대해 2개의 출력을 생성하도록 지도 미세 조정 단계에서 수행됩니다. 예: 입력 토큰 x에 대해 기본 모델이 y1과 y2 두 출력 토큰을 생성합니다. 이러한 출력은 사람의 평가자에게 보여주어 각각의 출력에 대해 사람의 선호도가 기록됩니다.

훈련 단계: 데이터 수집 단계에서 수집한 데이터 샘플이 있다면, 보상 모델은 다음 프롬프트로 훈련됩니다. "다음 입력이 주어졌을 때: `x`, LLM이 `y` 출력을 생성했습니다. 출력의 성능을 평가할 수 있나요?". 모델은 보상 r을 출력하고, 우리는 이미 데이터 수집 단계에서 보상 r1의 실제 값을 알고 있습니다. 이제 손실 함수와 함께 이를 역전파하여 모델을 훈련할 수 있습니다. 아래는 모델이 역전파를 통해 최적화하는 목표 손실 함수입니다:

![이미지](/assets/img/2024-07-07-LLMAlignmentReward-BasedvsReward-FreeMethods_0.png)

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

Notation:

- rΦ(x, y): 리워드를 추정하는 Φ로 파라미터화된 리워드 모델입니다. 파라미터화된 모델은 실제 값이 아니고 위 식에서 최적화되어야 하는 것을 의미합니다. 이것이 LLM 모델의 리워드 자체입니다. 대부분의 경우, LLM 파라미터는 여기서 동결되어 있고 몇몇 파라미터만 변경됩니다. 가장 중요한 레이어는 맨 위에 추가된 선형 레이어입니다. 이 레이어가 대부분의 학습을 수행하여 출력의 점수를 평가합니다.
- Ɗ: 입력(x), 이긴 출력(yw), 진 출력(yl)의 튜플로 이루어진 데이터셋입니다.
- σ: 리워드의 차이를 확률(0~1)로 매핑하는 시그모이드 함수입니다.
- ∑(x, y,w yl) ~Ɗ : x, yw, yl이 Ɗ에서 샘플링된다는 것을 의미합니다.

예시 시나리오: 응답을 평가하기 위한 리워드 모델을 훈련 중이라고 상상해보세요. 주어진 프롬프트에 대한 응답 쌍이 있고 인간 피드백을 통해 어떤 응답이 더 좋은지 알 수 있습니다. 예를 들어, x("프랑스의 수도는 무엇입니까?")에 대한 yw("프랑스의 수도는 파리입니다.")가 이기고 yl("프랑스의 수도는 베를린입니다.")이 지는 경우입니다. 리워드 모델은 "프랑스의 수도는 파리입니다." 출력에 더 높은 리워드를 주도록 학습해야합니다. 입력으로 "프랑스의 수도는 무엇입니까?"가 주어졌을 때 "프랑스의 수도는 파리입니다." 출력에 대해 "프랑스의 수도는 베를린입니다." 출력보다 더 높은 리워드를 부여해야 합니다.

## 강화 학습 세밀 조정 단계

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

강화학습 아이디어: 이제 베이스 모델과 보상 모델이 훈련을 받았으니, 아이디어는 보상 모델 점수를 활용하고 베이스 모델 매개변수를 업데이트하여 인간의 선호를 반영하는 것입니다. 보상 모델은 스칼라 점수를 출력하며 미분 가능하지 않기 때문에 간단한 역전파(back-propagation)를 사용하여 베이스 모델 매개변수를 업데이트할 수 없습니다. 따라서 베이스 모델을 업데이트하기 위해 다른 기술이 필요합니다. 이것이 강화학습이 필요한 이유이며, 이를 통해 베이스 모델이 보상 모델 점수를 통해 매개변수를 조정하게 됩니다. 이는 PPO(Proximal Policy Optimization)를 통해 이루어집니다. PPO의 핵심 아키텍처를 이해하는 것은 이 개념을 파악하는 데 필수적이지 않으므로 여기서는 다루지 않겠습니다. 그러나 일반적으로, PPO는 스칼라 점수를 사용하여 베이스 모델 매개변수를 업데이트할 수 있습니다. 이제 베이스 모델이 인간의 선호를 학습하는 데 어떻게 베이스 및 보상 모델이 결합되는지 이해해 봅시다.

강화학습 세부튜닝 아이디어: 강화학습에서는 행동, 공간 및 보상이 있습니다. 아이디어는 어떤 행동 에이전트가 보상을 극대화하는 공간에서 취할 수 있는 정책을 만드는 것입니다. 이는 다소 복잡해지지만 간단하게 말하면 π는 정책으로, 즉 베이스 LLM 모델입니다. Πref는 베이스 모델을 의미하며, ΠӨ는 생성하려는 다른 LLM 최적 모델을 나타냅니다. 우리는 인간이 선호하는 출력을 제공하는 ΠӨ(베이스 모델의 신경망 가중치가 섬세하게 조정될 것)를 찾아야 합니다. 우리가 ΠӨ를 모르기 때문에 이 최적 모델을 찾는 것이 아이디어입니다.

강화학습 훈련 및 피드백 루프 단계: 입력 x를 2개의 정책 모델, Πref(기준 모델) 및 ΠӨ(생성하려는 최적 모델)에 제공합니다. 초기에 두 모델은 동일하게 유지됩니다. x를 두 모델에 각각 제공하면 각각 두 출력을 얻습니다. ΠӨ 모델의 출력은 또한 보상 모델에 전달되어(입력: x, 출력: y; 위에서 설명한 대로) 보상 점수를 출력하라고 요청합니다. 이제 기준 모델의 출력, 최적 모델의 출력 및 최적 모델부터의 보상 점수가 있습니다. 우리가 여기서 최적화하려는 것은 보상을 극대화하는 것과 모델이 인간의 선호에 가능한 한 가까워져야 한다는 것 두 가지입니다. 보상을 최대화하는 것은 이미 스칼라량이기 때문에 간단하지만 기준 및 최적 모델 간의 차이를 최소화하는 방법은 무엇일까요. 여기서 "Kullback–Leibler 발산"을 사용하여 2개의 연속 확률 분포 사이의 차이를 추정합니다. 이 목적 손실 함수에 대해 더 자세히 알아봅시다.

![LLMAlignmentReward-BasedvsReward-FreeMethods](/assets/img/2024-07-07-LLMAlignmentReward-BasedvsReward-FreeMethods_1.png)

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

**주의사항:**

- rΦ(x, y): 최적 모델의 입력 x와 출력 y에 대한 스칼라 값입니다. 명확히 하자면, 최적 모델의 출력이 보상 모델로 전달됩니다.
- Dkl (ΠӨ (y | x) || Πref (y | x)): 이것은 2개의 확률 분포 사이의 쿨백-라이블러 발산을 계산합니다. 각 모델의 각 토큰이 확률 분포입니다. KL은 분포가 얼마나 서로 다른지를 추정합니다.
- β: 최적 모델이 기준 모델에 가까워지는 것이 얼마나 중요한지를 결정하는 하이퍼파라미터입니다.

예시 시나리오: "프랑스의 수도는 무엇입니까?"라고 묻는다고 상상해보세요. Πref (기준 모델)는 "프랑스의 수도는 베를린입니다."라고 말하고, ΠӨ (최적 모델)은 "3개의 수도가 있으며, 파리, 베르사유, 리옹이 있지만 파리가 공식적으로 수도로 간주됩니다."라고 합니다. 이제 rΦ("x: 프랑스의 수도는 무엇인가요?", "y: 3개의 수도가 있습니다.")는 인간 선호도가 낮기 때문에 낮은 점수를 줘야 합니다. 뿐만 아니라 (ΠӨ (y | x) || Πref (y | x))의 쿨백-라이블러 발산이 높아야 합니다. 왜냐하면 각각의 개별적인 출력에 대한 확률 분포 공간이 서로 다르기 때문입니다. 그러므로 손실은 두 용어에서 모두 높아져야 합니다. 우리는 모델이 보상만 최적화하는 것이 아니라 기준 모델에 더 가까워지도록 하기를 원하기 때문에 두 용어를 사용하여 보상을 최적화합니다. 다음 학습 반복에서 ΠӨ (최적 모델)이 "프랑스의 수도는 델리입니다"라고 말한다면, 모델은 Πref (기준 모델)에 더 가깝게 유지하고 기준 모델에 가까운 형식으로 출력하지만 보상 성분은 여전히 낮을 것입니다. 세 번째 학습 반복에서는 ΠӨ (최적 모델)이 "프랑스의 수도는 파리입니다"라는 결과를 학습하고 출력하여 보상이 상승하고 모델 출력이 기준 모델과 밀접하게 일치할 것입니다.

아래 다이어그램은 이 논리를 설명하는데 도움을 줍니다. 또한 hugging face의 RLHF 링크를 참고하시기를 강력히 권장드립니다.

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

![RLHF](/assets/img/2024-07-07-LLMAlignmentReward-BasedvsReward-FreeMethods_2.png)

# Reinforcement Learning Through Human Feedback (RLHF)를 통한 보상 없는 방법은 무엇인가요?

보상 있는 방법을 사용하는 RLHF에 대해 이야기했으니, 이제 보상 없는 방법으로 넘어가 봅시다. 논문에 따르면 "보상 함수로부터 최적 정책으로의 분석적 매핑을 활용함으로써 보상 함수에 대한 손실 함수를 정책에 대한 손실 함수로 변환할 수 있는 핵심적인 통찰이다. 이러한 변수 변환 접근 방식을 통해 명시적으로 독립적인 보상 모델을 맞추는 것을 피하면서도 여전히 인간 선호 모델의 존재하는 모델 하에서 최적화할 수 있다." 이해하기 어려운 내용이긴 하지만, 다음 섹션에서 간단히 설명해 보겠습니다.

보상 없는 방법의 핵심 아이디어: RLHF에서는 비용이 많이 들고 유지 비용이 큰 별도의 새로운 보상 모델을 훈련합니다. 새로운 보상 모델을 훈련하는 것을 피하고 기존 기본 모델을 사용하여 새로운 최적 모델을 달성하는 메커니즘이 있을까요? 바로 보상 없는 방법이 하는 일인데요. 이 방법은 새로운 보상 모델을 훈련하지 않으며 결과적으로 DPO(직접 선호 최적화)의 손실 함수에서 보상 모델 용어가 없어지도록 방정식을 변경합니다. 이를 이해하는 한 가지 방법은 기본 모델(Πref)에서 최적 모델 정책(ΠӨ)에 도달해야 한다는 것입니다. 최적 모델 정책에 도달하는 데는 보상 함수 공간을 최적화해 최적 모델 정책에 도달하는 데 도움을 주는 프록시를 구축하거나 보상에서 정책으로의 매핑 함수를 직접 학습하고 정책 자체를 최적화할 수 있습니다. 이 점에 대해 논문 저자들이 지적한 내용입니다. 손실 함수에서 보상 함수 구성 요소를 제거하고 직접 모델 정책 매개변수로 대체함으로써 이러한 점을 시도한 것입니다. "보상 함수로부터 최적 정책으로의 분석적 매핑을 활용하여 정책에 대한 손실 함수로 전환한다"고 말할 때 이를 의미한 것이며, 이것이 이 논문의 핵심 혁신입니다.

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

DPO 교육 및 피드백 루프 단계: Πref (기준 모델)을 사용하여, 입력 x가 주어지고 2개의 출력 (y1 및 y2)을 생성하도록 요청됩니다. 모든 x, y1 및 y2는 인간 평가자들에 의해 이기는 yw와 지는 yl을 결정하는 데 사용됩니다. 오프라인 데이터 세트는 삼중 정보 'x, yw 및 yl'로 수집됩니다. 이 정보를 통해, 이기는 (인간이 선호하는) 답변과 지는 (인간이 선호하지 않는) 답변을 알 수 있습니다. 이제, 동일한 입력 x가 두 정책 (모델) Πref (기준 모델)과 ΠӨ (최적 모델)에 제공됩니다. 초기에는 두 모델 모두 학습 목적을 위해 동일하게 유지됩니다. 두 모델에 각각 독립적으로 입력 x가 주어지면 각각 두 출력이 생성됩니다. 우리는 "쿨백-라이블러 발산"을 통해 기준 및 최적 모델에서 출력이 이기는 및 지는 답변으로부터 얼마나 떨어져 있는지 계산합니다. 목적 손실 함수를 자세히 살펴보겠습니다.

방정식

![Equation](/assets/img/2024-07-07-LLMAlignmentReward-BasedvsReward-FreeMethods_3.png)

- ΠӨ (yw | x) - 주어진 x(입력)에 대해, 모델의 해당 출력인 youtput이 이기는 출력 yw로부터 얼마나 떨어져 있는지. 출력 youtput과 yw는 확률 분포이며 둘 사이의 차이는 "쿨백-라이블러 발산"을 통해 계산됩니다. 이는 스칼라 값이 될 것입니다. 또한 이는 Πref (yw | x), Πref (yl | x), ΠӨ (yw | x) 및 ΠӨ (yl | x)의 서로 다른 조합에 대해 각 모델에 대해 계산됩니다.
- β: 최적 모델이 기준 모델에 가깝도록 하는 것이 얼마나 중요한지를 결정하는 하이퍼파라미터입니다.

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

![LLM Alignment](/assets/img/2024-07-07-LLMAlignmentReward-BasedvsReward-FreeMethods_4.png)

# 결론

- 자연스럽게, 보상 기반 방법을 사용하는 PPO 또는 보상 없는 방법을 사용하는 DPO를 통해 RLHF를 하는 것이 더 나은지에 관한 질문이 나옵니다. 이 질문에 대해 정답은 없습니다. 최근 논문은 "LLM 정렬에 대해 DPO가 PPO보다 우수한가"를 비교하고, PPO가 일반적으로 DPO보다 우수하며, DPO가 확실히 out-of-distribution 데이터로부터 더 많이 피해를 입는다고 결론내렸습니다. "Out-of-distribution" 데이터란 인간의 선호 데이터가 기준으로 훈련된 데이터와 다른 것을 의미합니다. 이는 기본 모델 훈련이 어떤 데이터 집합에서 이루어지고 선호 출력이 어떤 다른 데이터 집합에서 이루어질 경우 발생할 수 있습니다.
- 전반적으로, 어떤 것이 더 나은지에 대한 연구는 여전히 진행 중이지만, OpenAI, Anthropic, Meta와 같은 기업들이 PPO와 DPO를 통한 RLHF를 LLM 정렬 도구로 활용하고 있음을 보았습니다.

# 참고문헌

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

- 디렉트 프리퍼런스 최적화: 당신의 언어 모델은 고반 스코어 모델입니다: [링크](https://arxiv.org/pdf/2305.18290)
- LLM 정렬을 위해 DPO가 PPO보다 우월한가? 포괄적인 연구: [링크](https://arxiv.org/pdf/2404.10719)
- Hugging Face RLHF 기사: [링크](https://huggingface.co/blog/rlhf)
