---
title: "Part-1 과학 컴퓨팅  왜 트랜스포머는 과학 컴퓨팅에서 부족한가"
description: ""
coverImage: "/assets/img/2024-07-09-Part-1ScientificComputingWhyTransformersFallShortinScientificComputing_0.png"
date: 2024-07-09 10:17
ogImage:
  url: /assets/img/2024-07-09-Part-1ScientificComputingWhyTransformersFallShortinScientificComputing_0.png
tag: Tech
originalTitle: "Part-1 (Scientific Computing) ~ Why Transformers Fall Short in Scientific Computing"
link: "https://medium.com/autonomous-agents/part-1-scientific-computing-why-transformers-fall-short-in-scientific-computing-812c64c5c149"
isUpdated: true
---

LLM(Large Language Model)과 같은 Transformer 기반 모델들은 자연어 처리 작업에서 놀라운 능력을 보여주고 있습니다. 그러나 이러한 모델들은 과학 계산에 적용될 때 한계점이 드러납니다. 특히 네비에-스토크스(Navier-Stokes) 방정식을 해결하는 경우에는 그 한계가 더욱 분명해집니다. 유체 역학에 기초하는 이 방정식들은 복잡한 편미분 방정식(PDEs)을 해결해야 하는데, Transformer 모델들은 이러한 작업을 처리할 수 있는 능력이 없기 때문에 한계가 발생합니다. 이 블로그에서는 Transformer 모델이 직면하는 한계를 설명하고, 이를 탐구하며 수학적 및 개념적 도전에 대해 살펴봅니다.

![Image](/assets/img/2024-07-09-Part-1ScientificComputingWhyTransformersFallShortinScientificComputing_0.png)

## Transformer 모델이 실패하는 추론 측면

추상적 개념화: Transformer 모델들은 진정한 개념을 형성하는 능력이 부족하기 때문에 추상적 추론에 어려움을 겪습니다. 그들의 작동은 진정한 이해 대신 훈련 데이터 내의 통계적 상관 관계에 기반하고 있습니다. 이 한계는 데이터에 명시적으로 인코딩되지 않은 추상적 개념을 이해하는 능력을 제약합니다. (추상적 사고는 다중 척도 사고 능력과 데이터 도메인 분포 외의 생각을 요구합니다. 어텐션 모델은 극히 약한 귀납 편향을 나타냅니다.)

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

Counterfactual Reasoning: 카운터팩추얼 고찰은 실제 사건에서 벗어난 "만약에" 시나리오를 고려하는 것을 의미합니다. 트랜스포머는 이 영역에서 취약한 편이며, 알려진 데이터 패턴에서 벗어나는 가상 시나리오를 시뮬레이션하는 데 어려움을 겪습니다. (플래닝 기능이 부족한 트랜스포머는 카운터팩추얼 고찰을 통해 가상 시나리오를 고려하고 특정 조건이나 사건이 다르게 진행되었을 때 어떻게 될지를 고려해야 합니다. 이는 DAGs를 구축하는 것을 필요로 하며, 이를 위해서는 다양한 가상 시나리오를 얽어나가는 방법과 여러 규모에서 계층적으로 구성하는 방법이 필요합니다.)

Causal Inference: 상관 관계로부터 인과 관계를 결정하는 것은 트랜스포머의 중요한 약점입니다. 그들은 상관 관계를 식별할 수 있지만, 상관성과 인과 관계를 구별하는 능력이 부족하기 때문에 원인 추론을 필요로 하는 작업에는 믿을 수 없습니다. (원인과 결과 관계를 그리기 위한 인과 베이지안 그래프를 설명하기 위해서도 플래닝 능력이 요구됩니다.)

Generalization to Novel Contexts: 트랜스포머는 훈련 데이터의 범위 내에서는 일반화할 수 있지만, 새로운 전혀 보지 못한 환경에 배운 지식을 적용하는 데 자주 실패합니다. 이 한계는 근본적인 원리를 깊이 이해하는 대신 패턴 인식에 의존하기 때문에 발생합니다.

Meta-Reasoning: 트랜스포머는 메타-추론 능력이 부족하여 자신의 추론 과정에 대해 추론할 수 없습니다. 이 결핍으로 인해 결과의 타당성이나 타당성을 독립적으로 판단할 수 없어서 종종 잘못된 결과에 대해 오만함을 느끼게 됩니다.

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

직관적 물리와 상식: 트랜스포머는 직관적인 물리와 상식적 추론에 능숙하지 않습니다. 이러한 것들에는 물리 법칙과 일상 경험에 대한 기본적인 이해가 필요합니다. 그들은 그럴듯한 대답을 만들어 낼 수 있지만, 실제 세계적 추론 과제에서 종종 실패합니다.

다단계 논리 추론: 복잡한 다단계 논리 추론은 여전히 어렵습니다. 트랜스포머는 간단한 논리 추론을 처리할 수 있지만, 추론 체인의 복잡성과 길이에 따라 성능이 저하되어 얕은 논리 처리가 반영됩니다.

# 과학 컴퓨팅에서의 트랜스포머의 한계

## 이산 불변성

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

이산 불변성은 시스템이 이산화 변화에도 특성을 유지하는 속성을 말합니다. 과학적 계산에서 수치적 방법은 서로 다른 이산화 체계 하에서도 불변해야 합니다. 변압기(Transformer)는 이 불변성이 없어서, 다양한 이산화 그리드에 직면했을 때 일관성 없는 결과를 가져올 수 있습니다.

수학적 예시: 수치 적분

구간 [a, b]에서 함수 f(x)의 적분을 고려해 보세요.

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

특정 이산화 방법은 이 적분을 근사화하기 위해 이산적인 점에서 함수 값들을 합산합니다. 특정 이산화 체계(예: 사다리꼴 규칙)에 훈련된 변환기는 다른 체계(예: 심슨 규칙)에 잘 일반화되지 않아 부정확한 적분 근사화로 이어질 수 있습니다.

## 유한 차원 벡터 공간과 무한 함수 정의역

변환기는 유한 차원의 벡터 공간에서 작동하여 유한 입력 벡터를 유한 출력 벡터로 매핑합니다. 과학적 문제는 종종 무한 차원의 함수 공간 간의 매핑을 필요로 하는데, 이는 변환기가 효과적으로 처리할 수 없습니다.

수학적 정식화

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

과학 컴퓨팅에서는 종종 함수 공간과 관련된 문제들을 자주 만납니다. 여기서 L²(Ω)은 정의역 Ω 상의 제곱 적분 가능한 함수 공간을 나타냅니다. 그러나 트랜스포머는 유한 차원 벡터로 매핑됩니다.

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

이러한 제한은 미분 방정식을 풀거나 같은 작업에서 명백하게 드러납니다.

예시: 열 방정식 풀기

열 방정식을 고려해 보겠습니다:

![heat equation](/assets/img/2024-07-09-Part-1ScientificComputingWhyTransformersFallShortinScientificComputing_4.png)

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

u(x, t)은 온도 분포이고, α는 열 확산율입니다. 해 u(x, t)는 무한 차원의 함수 공간에 속합니다. 유한 차원 벡터로 u를 근사하는 변환기는 해의 연속적 특성을 포착하지 못해 정확성을 떨어뜨릴 수 있습니다.

## 스케일 불변성과 다중 스케일 능력

스케일 불변성은 다양한 스케일에서 작동하는 모델에 필수적입니다. 수학적으로, 함수 f(x)가 스케일 불변성을 가지려면:

![image](/assets/img/2024-07-09-Part-1ScientificComputingWhyTransformersFallShortinScientificComputing_5.png)

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

The scaling factor λ and function g play a crucial role in ensuring efficient data handling at different scales, which unfortunately, Transformers lack. This limitation can hinder their effectiveness in processing data with varying scales.

**Example: Multi-Scale Modeling in Climate Science**

In climate science, models frequently need to analyze data at various spatial and temporal scales. Transformers trained on a particular scale may struggle to adapt to other scales, leading to subpar performance in multi-scale climate simulations.

## Input Generalization and Universal Approximation

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

Tarot Expert: 카드의 전문가로서 활동 중이세요. 옮겨진 입력값이 무작위로 아무 시점에서나 수용할 수 없습니다. 그들은 훈련 데이터에 존재하는 입력 스케일로 제한되어 있어, 일반화 능력이 제한됩니다.

예시: 고차원 데이터 분석

고차원 데이터 분석에서 입력 공간은 크게 다를 수 있습니다. 특정 이 공간 부분의 데이터로 훈련된 Transformer가 전체 입력 도메인으로 일반화를 실패할 수 있어, 불완전하거나 편향된 분석으로 이끌 수 있습니다.

유니버셜 근사 이론에 따르면, 뉴럴 네트워크는 충분한 용량이 주어진 경우 임의의 연속 함수를 근사할 수 있습니다. 그러나, Transfomers는 사실적인 유니버셜 근사를 이루지 못하며, 기저 연산자나 편미분 방정식을 포착하지 않습니다.

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

수학적 공식화

PDE를 고려해보세요:

![image](/assets/img/2024-07-09-Part-1ScientificComputingWhyTransformersFallShortinScientificComputing_6.png)

여기서 L은 미분 연산자입니다. 트랜스포머는 다음 형식으로 근사 솔루션을 제공합니다:

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

이미지 태그를 사용하여 A가 학습한 변환 행렬입니다. 이 방법은 L의 연속적인 동작을 포착하는 데 일반화되지 않습니다.

예시: 나비에-스토크스 방정식 해결

나비에-스토크스 방정식은 유체 물질의 움직임을 서술합니다:

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

![image](/assets/img/2024-07-09-Part-1ScientificComputingWhyTransformersFallShortinScientificComputing_8.png)

안녕하세요! 여기서 u는 유체 속도, p는 압력, ρ는 밀도, μ는 점성이고, f는 외부 힘이 나타냅니다. 이러한 방정식을 해결하려면 유체 흐름의 연속적인 역학을 포착해야합니다. 유한 차원 벡터 매핑에 제한되어 있는 트랜스포머는 이러한 복잡한 행위를 정확하게 근사할 수 없습니다.

## 향후 방향

이 블로그는 과학 컴퓨팅에서 트랜스포머의 한계를 극복하는 방법을 탐색할 일련의 시리즈 중 첫 번째입니다. 향후 부분은 Fourier Neural Operators (FNO), Physics-Informed Neural Networks (PINNs), Hamiltonian Neural Networks (HNNs), Denoising Diffusion Probabilistic Models (DDPS), Score-Based Generative Models (SDE), Variational Diffusion Models (VDM) 등과 같은 고급 기술을 심층적으로 탐구합니다. 이러한 방법론은 기계 학습 모델이 복잡한 과학적 작업을 처리할 수 있는 능력을 향상시킬 것으로 약속되며, 유한 차원 벡터 공간과 무한 차원 함수 공간 사이의 간격을 메울 것입니다.

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

이제 우리는 트랜스포머를 과학계산 작업에 통합하는 잠재적인 해결책과 발전에 대해 더 깊이 논의해 보도록 합시다.✨✨
