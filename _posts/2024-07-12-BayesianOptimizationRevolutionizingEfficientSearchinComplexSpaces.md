---
title: "베이즈 최적화 복잡한 공간에서 효율적인 탐색 혁명 방법"
description: ""
coverImage: "/assets/img/2024-07-12-BayesianOptimizationRevolutionizingEfficientSearchinComplexSpaces_0.png"
date: 2024-07-12 23:27
ogImage:
  url: /assets/img/2024-07-12-BayesianOptimizationRevolutionizingEfficientSearchinComplexSpaces_0.png
tag: Tech
originalTitle: "Bayesian Optimization: Revolutionizing Efficient Search in Complex Spaces"
link: "https://medium.com/aimonks/bayesian-optimization-revolutionizing-efficient-search-in-complex-spaces-3e2cc476d2cd"
isUpdated: true
---

## 소개

최적화의 영역에서 성능 향상을 위한 탐구가 컴퓨터 및 자원 효율성의 제약과 상호 작용할 때, 베이지안 최적화는 희망의 촛불로 등장합니다. 이 강력한 프레임워크는 특히 목적 함수가 복잡하고 평가 비용이 높은 분야에서 자리를 잡았습니다. 그 전문성은 기계 학습 모델의 하이퍼파라미터 튜닝, 공학 분야의 설계 최적화, 그리고 목적 함수의 불투명한 특성이나 평가 비용의 제한으로 인해 전통적인 최적화 방법이 실패하는 상황에서 두드러집니다.

![image](/assets/img/2024-07-12-BayesianOptimizationRevolutionizingEfficientSearchinComplexSpaces_0.png)

## 배경

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

베이지안 최적화는 평가하기 비용이 많이 드는 목적 함수를 최적화하는 강력한 전략입니다. 특히 명시적인 공식이 없는 블랙박스 함수가 있는 상황에서 유용하며, 함수의 평가가 시간이나 자원 면에서 비용이 많이 듭니다. 이로 인해 베이지안 최적화는 기계 학습 모델의 하이퍼파라미터 튜닝, 시뮬레이션이나 실험이 시간이 많이 걸리는 복잡한 시스템의 최적화, 목적 함수가 간단한 해석적 형태를 갖지 않는 상황에 매우 적합합니다.

베이지안 최적화의 핵심 아이디어는 확률 모델을 사용하여 테스트되지 않은 지점에서 함수의 성능을 추측하는 것입니다. 이것이 일반적으로 작동하는 방식의 단계별 분해입니다:

- 사전 선택: 고려 중인 함수의 행동에 대한 신념을 나타내는 사전 분포를 선택하여 프로세스를 시작합니다. 가우시안 프로세스(GPs)는 복잡한 함수를 모델링하는 데 유연하며 예측의 불확실성을 제공할 수 있어 사전 분포로 일반적으로 사용됩니다.
- 함수 관찰: 처음에는 함수를 몇 지점에서 관찰하여 입력과 해당 함수의 출력을 포함합니다. 이러한 관찰은 확률 모델을 업데이트하는 데 사용되어 관찰된 내용에 기초하여 함수에 대한 신념을 정제합니다. 확률 모델의 예측 평균 및 불확실성(분산)을 고려하여 탐색(높은 불확실성 영역 테스트)과 개발(현재 가장 좋은 관찰에 대한 개선 가능성이 있는 영역 테스트)을 균형있게 하는 취득 함수가 사용됩니다. 기본 취득 함수에는 Expected Improvement (EI), Probability of Improvement (PI), Upper Confidence Bound (UCB)가 있습니다. 3단계와 4단계를 반복하여 각 반복에서 업데이트된 모델을 사용하여 관찰할 다음 점을 선택하고, 반복 사이의 개선을 기준으로 한 중지 기준(예: 최대 함수 평가 횟수 또는 수렴 기준)이 충족될 때까지 반복합니다.

베이지안 최적화는 상대적으로 적은 수의 함수 평가로 복잡한 함수의 전역 최적값을 찾는 데 매우 효율적입니다. 그러나 경우에 따라서는 가우시안 프로세스 모델의 업데이트에 대한 계산 비용과 같은 제한이 있을 수 있습니다. 그럼에도 불구하고, 비싼 평가를 처리하는 효율성으로 인해 각 평가가 복잡한 모델의 교육과 검증을 포함할 수 있는 기계 학습 모델의 하이퍼파라미터 튜닝을 포함한 많은 응용 분야에서 가치 있는 도구로 인정받고 있습니다.

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

## 철학적 근간

베이지안 최적화의 핵심은 사전 지식과 관측된 데이터를 결합하여 미지에 대한 정보를 판단하는 데 체계적으로 개선할 수 있다는 철학적 입장을 보여주는 것입니다. 이 방법론은 베이지안 통계를 활용하여 최적화 지형의 위험한 영역을 탐색하며, 각 평가가 한정된 단서를 가지고 숨겨진 보물을 찾는 것과 유사할 수 있습니다.

## 베이지안 최적화의 메커니즘

과정은 일반적으로 가우시안 프로세스(GP)로 모델링된 목적 함수에 대한 사전 믿음으로 시작합니다. 이 선택은 우발적이지 않습니다. GP는 복잡한 함수의 세세한 점을 잡아내고 미개척 지역의 불확실성을 양적화하는 데 뛰어납니다. 함수의 초기 관측치는 이러한 믿음을 업데이트하는 데 사실적인 기반을 제공하며, 베이즈 이론을 사용하여 사전 지식과 새로운 데이터를 결합하여 함수 지형의 정교한 지도를 제공하는 사후 분포를 얻게 됩니다.

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

베이지안 최적화의 진정한 예술은 탐색 전략에 있습니다. 확률론적 모델을 갖춘 이 방법은 획기적으로 탐색과 개척의 목표를 조심스럽게 균형을 이루도록 하는 취득 함수를 활용합니다. 이 섬세한 균형은 최적화자가 불확실성이 높은 지역(탐색)과 향상의 약속이 있는 영역(개척)을 조사하는 것을 가능하게 하며 최적화 공간을 효율적으로 탐험할 수 있도록 계산되고 우아하게 만듭니다.

## 취득 함수의 무용

취득 함수의 선택은 최적화자의 행동에서 중요한 역할을 합니다. 예상 향상(EI), 개선 확률(PI), 그리고 상한 신뢰 구간(UCB)과 같은 옵션들은 탐색 대 개척의 중요성을 우선시하는 다른 전략을 제공합니다. 이러한 함수들은 가우시안 프로세스 모델에 의해 그려진 확률적 지형을 고려하여 이끄는 나침반 역할을 합니다.

## 베이지안 최적화의 영향

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

베이지안 최적화의 영향력이 가장 두드러지게 나타나는 분야는 머신러닝이며, 하이퍼파라미터 튜닝의 사실상 표준이 되었습니다. 이 방법은 적은 평가로 최적 구성을 식별하는 능력이 탁월하며, 복잡한 모델을 훈련하는 계산 비용을 고려할 때 매우 유용합니다. 머신러닝 이외에도 이 방법은 약물 발견 분야에서 유망한 화합물을 신속하게 식별하고, 시스템 및 소재의 설계를 간소화하는 엔지니어링 분야에서 사용됩니다.

## 도전과 한계

강점에도 불구하고, 베이지안 최적화에는 도전 과제가 있습니다. GP 모델을 업데이트하는 계산 오버헤드는 관측치 수가 늘어남에 따라 부담스러워질 수 있어, 확장성이 제한될 수 있습니다. 또한 선택한 사전 및 획득 함수에 의존하는 이 방법은 주관성을 도입하며, 이 최적화 도구를 효과적으로 활용하기 위해 전문 지식의 중요성을 강조합니다.

## 코드

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

베이지안 최적화를 처음부터 구현하려면 목적 함수를 정의하고, 가우시안 프로세스와 같은 대리 모델을 선택하고, 획득 함수를 선택하고, 관측된 데이터를 기반으로 모델을 반복적으로 업데이트하는 여러 구성 요소가 필요합니다. 여기에서는 간단하면서도 설명적인 목적 함수를 최적화하기 위해 합성 데이터 세트를 사용할 것입니다. 베이지안 최적화를 구현하는데 도움이 되는 파이썬 라이브러리인 scikit-optimize (skopt)을 활용하여 최적화 과정을 시각화하는 플롯 생성을 포함하여 프로세스를 설명하겠습니다.

먼저, 환경에 scikit-optimize이 설치되어 있는지 확인하세요. 그렇지 않은 경우 다음 명령어를 사용하여 설치할 수 있습니다:

```js
pip install scikit-optimize
```

목적 함수

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

# Synthetic Example Optimization using Bayesian Optimization

To demonstrate the effectiveness of Bayesian optimization, we will optimize a simple yet commonly used objective function: a 1-D sine wave function. This function introduces variability and complexity, making it a suitable candidate for showcasing Bayesian optimization.

# Complete Code

```python
import numpy as np
import matplotlib.pyplot as plt
from skopt import gp_minimize
from skopt.plots import plot_convergence, plot_objective, plot_evaluations
from skopt.space import Real
from skopt.utils import use_named_args

# Define the space of the variable
space  = [Real(-2.0, 2.0, name='x')]

# Define the objective function that we want to minimize
def objective_function(x):
    return np.sin(5 * x[0]) * (1 - np.tanh(x[0] ** 2))

# Use decorator to convert the function to take named arguments
@use_named_args(space)
def objective(x):
    return objective_function([x])

# Perform Bayesian optimization using Gaussian Process
res_gp = gp_minimize(objective, space, n_calls=20, random_state=0)

# Plotting the convergence
plot_convergence(res_gp)
plt.show()

# Plot evaluations
plot_evaluations(res_gp)
plt.show()

# Plot objective with points sampled
plot_objective(res_gp)
plt.show()

print(f"Optimal value (x): {res_gp.x[0]}, Minimum value: {res_gp.fun}")
```

This code snippet demonstrates the optimization process using Bayesian optimization for a 1-D sine wave function. Ensure necessary libraries are installed for the code to execute successfully. Enjoy the optimization journey! 🌟

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

- Space Definition: 저희 목적 함수의 정의 영역은 -2에서 2 사이의 1차원 공간으로 정의됩니다.
- Objective Function: 우리의 목적 함수는 삼각함수로 정의되어 있으며, 비선형성과 지역 최솟값을 도입하기 위해 하이퍼볼릭 탄젠트로 변조되었습니다.
- Optimization: 저희는 scikit-optimize의 gp_minimize를 사용하여 베이지안 최적화를 수행하며, 목적 함수, 정의된 영역 공간, 호출 횟수(반복 횟수), 그리고 재현성을 위한 랜덤 시드를 지정합니다.
- Plotting: 총 세 개의 그래프를 생성합니다:
  - 수렴 플롯은 발견된 최솟값이 반복에 따라 어떻게 변화하는지 보여줍니다.
  - 평가 플롯은 최적화 중 샘플링된 지점들을 시각화합니다.
  - 목적 플롯은 목적 함수의 2차원 시각화를 제공하며, 샘플링된 지점에 대한 랜드스케이프와 알고리즘이 초점을 맞춘 곳을 보여줍니다.
- Result: 마지막으로, 우리는 발견된 최적 x 값과 목적 함수의 최솟값을 출력합니다.

![Plot Image](/assets/img/2024-07-12-BayesianOptimizationRevolutionizingEfficientSearchinComplexSpaces_1.png)

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

이미지 Markdown 포맷으로 변경해 드리겠습니다.

![이미지 2](/assets/img/2024-07-12-BayesianOptimizationRevolutionizingEfficientSearchinComplexSpaces_2.png)

![이미지 3](/assets/img/2024-07-12-BayesianOptimizationRevolutionizingEfficientSearchinComplexSpaces_3.png)

```js
최적값 (x): -0.28918578105156434, 최소값: -0.909429787044548
```

이 코드 조각은 Bayesian 최적화를 합성 데이터세트에 적용하는 전 과정을 안내해주어, 복잡한 목적 함수를 효율적으로 탐색하여 최적 솔루션을 찾는 능력을 보여줍니다.

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

## 결론

베이지안 최적화는 최적화 영역에서 효율성의 모범 사례로 떠오르며, 통계 원리와 전략적 탐색을 섬세하게 엮어 복잡한 공간에서 해결책을 찾아내고 있습니다. 이론적 우아함과 실용적 유틸리티의 조화는 현대 최적화 도전 과제의 본질을 소화하며, 데이터에 근거한 길잡이로 확률에 의해 이끌리며 나아가는 길을 제공합니다. 컴퓨팅적으로 실행 가능한 범위를 끊임없이 넓히면서, 베이지안 최적화는 이전에 상상조차 못 했던 노력의 경제성으로 고차원 공간의 바늘을 찾을 수 있도록 하는 중요한 동료로 남아 있습니다.
