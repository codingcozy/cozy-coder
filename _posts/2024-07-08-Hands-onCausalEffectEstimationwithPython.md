---
title: "파이썬을 사용한 인과 효과 추정 실습 방법"
description: ""
coverImage: "/assets/img/2024-07-08-Hands-onCausalEffectEstimationwithPython_0.png"
date: 2024-07-08 00:02
ogImage: 
  url: /assets/img/2024-07-08-Hands-onCausalEffectEstimationwithPython_0.png
tag: Tech
originalTitle: "Hands-on Causal Effect Estimation with Python"
link: "https://medium.com/causality-in-data-science/hands-on-causal-effect-estimation-with-python-aac40ca2cae0"
isUpdated: true
---





![Hands-onCausalEffectEstimationwithPython](/assets/img/2024-07-08-Hands-onCausalEffectEstimationwithPython_0.png)

단순히 것들이 인과 관계가 있는 것으로 아는 것만으로 충분하지는 않습니다. 예를 들어 점점 심해지는 기상 현상을 생각해보세요. 일요일에 약간 비가 올 것으로 예상만 하는 일기 예보는 별 도움이 되지 않을 겁니다. 특히 강가에 살고 있는 분들처럼 홍수 두려움이 있는 경우에는 더 그렇습니다. 이와 같은 유형의 더 많은 예시가 있습니다. 각각은 종종 원인의 효과를 알고 싶어하고 단순히 인과 관계 자체만을 알고 싶어하는 것을 보여줍니다.

이 튜토리얼에서는 지난 블로그 글을 통해 습득한 이론적 지식을 활용하여 여러분의 프로젝트에서 인과 관계를 추정할 수 있는 계산적 방법에 대해 소개하겠습니다.

계속 읽기 전에 아래 개념들에 익숙하신지 확인해 주세요:

<div class="content-ad"></div>

친구들 안녕하세요! 오늘은 카지노 추정에 대한 이론적인 내용을 함께 알아보려고 해요. 이미 잘 알고 계신 분들은 tigramite 패키지를 사용한 카지노 효과 추정을 살펴보는 두 번째 부분으로 건너뛰셔도 좋아요.

### 카지노 추정을 위한 큰 그림: 카지노 발견부터 카지노 효과 추정까지

이번에는 원인과 결과 사이의 인과효과 추정에 대한 이론적인 내용을 다뤄볼 거에요. 만약 이미 잘 알고 계시다면 tigramite 패키지를 사용한 카지노 효과 추정을 보여주는 두 번째 부분으로 건너뛰어도 괜찮아요.

<div class="content-ad"></div>

입문에서 이미 근본적인 질문에 qualitatively뿐만 아니라 quantitatively로도 대답해야 한다는 것을 확립했습니다. 다시 말해, 우리는 단순히 존재/비존재를 나타내는 인과 관계에서 이분법을 사용하고 싶어하지 않습니다. 대신, 특정 가능한 효과의 실제 크기를 얻고 싶어합니다. 인과 발견에서는 데이터로부터 인과 그래프를 추정하는 작업이 있는데, 이는 단지 인과 효과 추정을 바로 제공해주지 않습니다. 그럼에도 불구하고, 이들은 그것들로 가는 길에서의 근본적인 부분입니다. 조금 혼란스러워 보일 수 있나요? 걱정하지 마세요. 그렇게 어렵지 않습니다. 인과 효과 추정을 두 단계 프로세스로 상상해보세요. 먼저 인과 발견 방법(또는 사용 가능한 전문가 지식)을 사용하여 인과 그래프를 식별한 다음 발견된 효과의 크기를 결정하기 위해 발견된 그래프를 사용합니다. 그게 전부입니다! 물론 학습 없이 그래프를 이미 질적으로 알고 계시다면 더욱 좋습니다.

좋아요, 이제 우리가 큰 그림을 이해했으니 조금의 작은 수학적 형식을 살펴볼 차례입니다. 이는 우리가 최종적으로 인과 효과를 추정하는 목표를 신뢰할 수 있게 달성하는 데 도움이 될 것입니다.

## Pearl의 인과 효과 프레임워크:

시스템과 그것을 설명하는 인과 그래프가 주어졌다고 가정해 봅시다. Pearl의 프레임워크에서의 표준 문제는 알려진 인과 그래픽 모델을 기반으로 변수 X가 Y에 미치는 인과 효과를 추정하는 것입니다. Pearl의 프레임워크에서 X = x로 설정할 때의 Y에 대한 인과 효과는 개입 분포 p(Y|do(X=x))의 함수입니다.

<div class="content-ad"></div>

만약 이 표기에 익숙하지 않다면, 이 간단한 소개를 참고해주세요. 이 시점에서 do-표기법에 대한 간단한 요약이 적당할 것입니다. 분포 정의 내에서 "do()"를 발견하면, 이는 우리가 시스템에 개입하여 그 변수 중 하나를 특정 값으로 설정한다는 것을 의미합니다. 이러한 개입은 동일한 값에 대한 조건부와 근본적으로 다릅니다. 이를 명확히 하기 위해, 실제적인 측면에서 두 개념을 고려해보세요. 조건부는 시스템에서 일부 데이터를 샘플링한 후, 이제 X = x인 데이터만 선택한다는 것을 의미합니다. 결과적으로, X가 x가 아닌 값일 때의 모든 값들을 샘플에서 무시합니다.

개입을 수행하는 것은 근본적으로 다릅니다. 여기서는 시스템에 개입하거나 조작하고 X의 모든 인스턴스 값을 x로 설정합니다. 결과적으로, 시스템에서 종속 변수의 분포는 조건부와 개입시에는 매우 다를 것입니다.

## SCMs

이 배경을 고려하면, Pearl의 프레임워크의 기초는 무작위 변수들 사이의 근본적 (하지만 알려지지 않은) 구조적 인과 모델(SCM) 가정입니다. 예를 들어,

<div class="content-ad"></div>

![Hands-on Causal Effect Estimation with Python - Image 1](/assets/img/2024-07-08-Hands-onCausalEffectEstimationwithPython_1.png)

With f() being assignment functions, through which the value of a random variable (left-hand side of the equation) gets determined by its parents and a noise variable in the graph. The graph corresponding to the SCM above can be depicted as:

![Hands-on Causal Effect Estimation with Python - Image 2](/assets/img/2024-07-08-Hands-onCausalEffectEstimationwithPython_2.png)

So how can we interpret p(Y|do(X=x)) in such a case? Intuitively, it is the (interventional) probability distribution of Y in the intervened SCM where the assignment equation of X is replaced such that the SCM becomes:

<div class="content-ad"></div>

상단의 이미지는 "The Elements of Causal Inference"에서 설명된 "완벽한" 개입입니다. 더욱 진보된 그리고 매우 흥미로운 종류의 개입이 더 있음을 유의하십시오. 현재의 간단한 경우에 대해서는 상기한 것이 충분할 것입니다.

## 평균 처리 효과

그렇다면 이러한 실험의 개념을 따른다면 인과 효과에 대한 표준 개념은 어떻게 보일까요? 널리 사용되는 평균 처리 효과는 X에 대한 개입 후 Y의 분포 간의 차이로 정의됩니다. 간단히 말해서 X에 대한 한 값으로의 개입 후 Y의 분포와 X에 대한 다른 값으로의 개입 후 Y의 분포의 차이입니다.

<div class="content-ad"></div>

![Hands-onCausalEffectEstimationwithPython](/assets/img/2024-07-08-Hands-onCausalEffectEstimationwithPython_4.png)

정확히 말하자면, 이것은 개입적 분포를 기반으로 인과 효과를 형식화하는 한 가지 방법에 불과합니다. CATE(조건부 평균 처리 효과), 이변 treatment 효과 및 개별 처리 효과와 같이 더 많은 유형의 효과를 발견할 수 있습니다. 그러나 ATE(Average Treatment Effect)는 고전적이면서도 가장 간단한 유형이기 때문에 이 기사의 나머지 부분에서는 이것을 사용할 것입니다. 시작하기에 좋은 방법이죠!

## 조정 세트

실험을 실시할 수 있는 것은 항상 아닙니다. 우리는 이미 여러분 중 한 명과 같은 처음 게시물 중 하나에서 이 사실을 강조했습니다. 이는 인과 추론이 다루는 근본적인 문제 중 하나이기 때문입니다. 여기서 Pearl의 이론이 큰 도움이 되는 이유는 이론이 의도된 변수 V 사이에서 X가 Y에 미치는 인과 효과가 식별 가능한지(개입적 대상 쿼리를 작성할 수 있는지)를 특징화하기 위한 기준을 적용하는 데 순수한 그래프 지식을 활용할 수 있기 때문입니다.

<div class="content-ad"></div>

한 이미지 태그를 Markdown 형식으로 변경해 주세요.

"q"가 관측 분포의 일부 함수인 경우, estimand가 됩니다. 그러므로 인과 효과가 식별 가능하다면, 우리는 Pearl의 이론과 do-계산법과 같은 그의 잘 개발된 방법을 사용하여 관측 데이터에서 개입 없이 개입 분포를 얻을 수 있습니다.
이를 달성하는 한 가지 방법은 backdoor 기준 또는 더 일반적으로 일반화된 조정 기준을 충족하는 유효한 조정 세트를 통해 가능합니다. 우리는 시작할 때 간단히 유지하고, 여기서 적절한 (가능한 경우 비어있을 수 있는) 조정 변수 Z가 우리에게 X=x 설정에 대한 개입 분포를 관측 분포의 용어로 표현할 수 있게 해주는 backdoor 조정을 다룰 것입니다. 비어 있지 않은 Z에 대해, backdoor 조정을 위한 공식은 다음과 같습니다:

![Backdoor Adjustment Formula](/assets/img/2024-07-08-Hands-onCausalEffectEstimationwithPython_6.png)

<div class="content-ad"></div>

당신이 바로 가져야 할 변수들을 조정하고, 우리가 유효한 조정 집합이라고 부르는 것을 표준화하면, 우리는 우리 자신이 "진짜 개입"을 하지 않더라도 개입 분포가 어떻게 될지 계산할 수 있습니다. 마치 마술 같지만 굉장히 논리적이고 확실하게 증명된 것입니다. 다만, 이는 인과 그래프를 알고 있다는 가정에 기반하며, 거기서 조정 집합을 결정할 수 있습니다.

Perkovic 등 (2018)의 일반화된 후문 기준에 따르면, 조정 집합 Z가 유효한지 여부는 다음 두 가지 조건이 모두 적용될 때만 유효합니다:

![그림](/assets/img/2024-07-08-Hands-onCausalEffectEstimationwithPython_7.png)

여기서 "forb"는 X와 Y 사이에 위치한 특정 중개자 노드 M이나 Y의 하속인 금지된 노드를 의미합니다. 두 번째 조건은 가짜 연관의 샌틀링을 방지하고 d-분리에 기반합니다.

<div class="content-ad"></div>

## 인과 효과 추정을 위해 조정 세트 사용하기

이제 본론으로 들어가 봅시다. 유효한 조정 세트를 갖추고, 그 결과로 인한 개입분포의 요소화를 한 상태에서, 개입 X=x 하에서 Y의 기댓값은 다음과 같습니다:

![image not available](/assets/img/2024-07-08-Hands-onCausalEffectEstimationwithPython_8.png)

우측의 기댓값들은 모두 개입문이 없는 기댓값입니다. 그러므로 우리는 이들을 우리의 관측 데이터 (X, Y, Z)를 사용하여 추정할 수 있습니다. 내부 기댓값을 추정하기 위해 우리는 통계적 학습 모델을 사용합니다:

<div class="content-ad"></div>

이는 X와 Z의 값을 고려할 때 Y의 편향되지 않은 추정치를 제공하는 모델을 의미합니다. 선형 경우에는 이는 기본 선형 회귀 모델인 sklearn에서 확인 가능하며, 더 정교한 확장이 물론 가능하며 때로는 필수적입니다.

거의 다 왔어요. 마지막 단계는 훈련된 모델 f_hat을 모든 관찰된 Z의 값 z_k에 대해 평가하여(여기서 z_k는 조정 세트 Z에 포함된 각 변수의 관찰 값이므로 벡터입니다) 평균을 내는 것입니다:

![image](/assets/img/2024-07-08-Hands-onCausalEffectEstimationwithPython_10.png)

<div class="content-ad"></div>

I mean, when you've worked out the expected value of your intervention, the next step is to find the average treatment effect. Try out different values for x and x`, take the expected values, and then subtract them. 

In a linear scenario, things get pretty exciting. You can determine the average treatment effect of an intervention on X by looking at the regression coefficient for X in a multivariate linear regression of Y on X and Z. It's like magic!

When dealing with multivariate interventions in X (when X isn't just one variable but several), this approach really comes in handy.

<div class="content-ad"></div>


![Image](/assets/img/2024-07-08-Hands-onCausalEffectEstimationwithPython_12.png)

from the regression model:

![Image](/assets/img/2024-07-08-Hands-onCausalEffectEstimationwithPython_13.png)

# Part 2: Estimating Causal Effects with Python


<div class="content-ad"></div>

## 시작하기:

이제 인과효과 추정을 수행할 때 무슨 일이 벌어지는지 조금 더 명확해지겠죠. 이제 코딩 예제를 실제로 해봅시다!

인과효과 추정을 수행하기 위해 Tigramite 패키지를 사용할 것입니다. 시계열 데이터를 사용한 풀스택 인과 추론의 핵심이 되는 Tigramite은 저 package를 사용하여 최신 기술 솔루션을 제공하며 package 의존성이 낮아 계속해서 유지보수 및 발전되고 있습니다 (자세한 내용: [https://github.com/jakobrunge/tigramite](https://github.com/jakobrunge/tigramite)).

터미널을 통해 사용하려는 디렉토리 내에서 다음 명령어를 사용하여 설치할 수 있습니다:

<div class="content-ad"></div>

```bash
pip install tigramite
```

Let's move on and download the necessary packages for the tutorial:

```python
# Imports
import numpy as np
import matplotlib
from matplotlib import pyplot as plt
%matplotlib inline     
from scipy.stats import gaussian_kde
import tigramite
from tigramite import data_processing as pp
from tigramite.toymodels import structural_causal_processes as toys
from tigramite import plotting as tp
from tigramite.models import Models
from tigramite.causal_effects import CausalEffects
import sklearn
from sklearn.linear_model import LinearRegression
```

To estimate basic causal effects in Tigramite, all you need are a few additional packages alongside tigramite itself. Those include numpy, matplotlib, and sklearn.

<div class="content-ad"></div>

지금까지 잘 진행되고 있어요. 인과 추정을 시작하기 전에 먼저 우리의 인과 추론 노력의 결과를 비교할 수 있는 참값 데이터를 생성해봅시다.

이를 위해 조금 더 복잡한 toy SCM에서 혼합 링크를 사용하여 일부 데이터를 생성합니다. 가우시안 단위 분산 노이즈 (noises = None)를 사용하세요.

```python
def lin_f(x): return x
coeff = .5
links_coeffs = {
                0: [], 
                1: [((0, 0), coeff, lin_f), ((5, 0), coeff, lin_f)], 
                2: [((1, 0), coeff, lin_f), ((5, 0), coeff, lin_f)],
                3: [((1, 0), coeff, lin_f), ((2, 0), coeff, lin_f), ((6, 0), coeff, lin_f), ((7, 0), coeff, lin_f)],
                4: [((5, 0), coeff, lin_f), ((7, 0), coeff, lin_f)], 
                5: [],
                6: [],
                7: [],
                }
T = 10000
data, nonstat = toys.structural_causal_process(links_coeffs, T=T, noises=None, seed=7)
# 시계열 7번은 관측되지 않은 혼탁변수
data = data[:, [0,1,2,3,4,5,6]]
dataframe = pp.DataFrame(data)
```

우리는 인과 효과를 추정하기 위해 개입을 평가해야하기 때문에 x1=x2=0으로 두 가지 개입 데이터도 생성합니다.

<div class="content-ad"></div>

```python
intervention2 = 0.*np.ones(T)
intervention_data2, nonstat = toys.structural_causal_process(links_coeffs, T=T, noises=None, seed=7,
                                            intervention={X[0][0]:intervention2, X[1][0]:intervention2}, 
                                            intervention_type='hard',)
# Time series no 7 is unobserved confounder
intervention_data2 = intervention_data2[:, [0,1,2,3,4,5,6]]
tp.plot_timeseries(pp.DataFrame(intervention_data2)); plt.show()
```

그리고 x’1=x’2=1

```python
T = 10000
intervention1 = np.ones(T)
intervention_data1, nonstat = toys.structural_causal_process(
                                       links_coeffs, T=T, 
                                       noises=None, 
                                       seed=7,
                                       intervention={X[0][0]:intervention1, X[1][0]:intervention1}, 
                                       intervention_type='hard',)

# Time series no 7 is unobserved confounder
intervention_data1 = intervention_data1[:, [0,1,2,3,4,5,6]]
tp.plot_timeseries(pp.DataFrame(intervention_data1)); plt.show()
```

자, 이제 필요한 데이터가 준비되었습니다. 효과를 추정해 보겠습니다.


<div class="content-ad"></div>

진정한 평균 처리 효과는 두 가지 서로 다른 데이터 프레임의 Y 값 차이의 평균입니다. 저희는 세 줄의 코드를 사용하여 계산하여 원인 효과 0.75가 나왔어요.

```python
true_effect = (intervention_data1[:, Y[0][0]] - intervention_data2[:, Y[0][0]]).mean()
print("True effect = %.2f" % true_effect)
```

## "CausalEffects"에 대한 인수

Tigramite의 CausalEffects 클래스를 사용하여 인과 효과 측정을 수행하려면 다음과 같은 인수를 지정해야해요.

<div class="content-ad"></div>

- 그래프는 dtype='`U3`'의 문자열 배열로, 그래프 유형에 따라 다른 모양을 갖습니다:
- graph_type='dag'는 그래프가 (N, N) 형태인 논-시계열 DAG를 나타내며, 여기서 N은 노드 수를 나타냅니다. 엣지는 들어오는 엣지를 나타내는 ← 또는 나가는 엣지를 나타내는 →가 될 수 있습니다.
- graph_type='admg'는 이 게시물에서 자세히 다루지 않는 비-시계열 ADMG(비순환-방향성 혼합 그래프)를 나타냅니다. 더 자세한 내용은 GitHub를 참조하십시오.
- graph_type='stationary_dag'는 그래프가 (N, N, tau_max + 1) 형태인 정상시계열 DAG를 나타냅니다. 여기서 tau_max는 최대 시차를 의미하며, +1은 동시적 관계를 나타냅니다. (그래프 어휘가 생소하다면 과거 블로그 게시물에서 시계열 DAG에 대해 살펴보세요.)
- hidden_variables는 숨겨진 변수를 포함하는 [(k, -tau), ...] 형식의 튜플 목록입니다. 시계열 그래프의 경우, 변수 k의 선택된 숨겨진 시점 Xt-k를 나타내므로 (k, -tau)가 됩니다.

그래프를 구축하기 위해 numpy 배열에 모든 엣지를 제공합니다. 따라서 CausalEffects를 사용하려면 이미 CausalDiscovery를 수행해야 합니다. 즉, 이전 블로그 게시물에서 설명한 PCMCI를 사용하거나 전문지식을 통해 그래프를 알아야 합니다. 인과 관계 링크를 식별했다면 위에서 지정한 SCM의 기반이 되는 그래프를 나타내는 배열을 지정할 수 있습니다.

```js
# 장난감 모델의 그래프
graph = np.array([['', '-->', '', '', '', '', ''],
                  ['<--', '', '-->', '-->', '', '<--', ''],
                  ['', '<--', '', '-->', '', '<--', ''],
                  ['', '<--', '<--', '', '<->', '', '<--'],
                  ['', '', '', '<->', '', '<--', ''],
                  ['', '-->', '-->', '', '-->', '', ''],
                  ['', '', '', '-->', '', '', '']], dtype='<U3')
``` 

이 결과는 다음과 같은 그래프를 얻게 됩니다:

<div class="content-ad"></div>

```python
tp.plot_graph(graph=그래프,
               var_names=변수_이름,
               save_name='예시.pdf',
               figsize=(8, 6))
plt.show()
```

![이미지](/assets/img/2024-07-08-Hands-onCausalEffectEstimationwithPython_14.png)

그래프, 그래프_타입, 그리고 숨겨진_변수가 정의된 후에 Tigramite의 CausalEffects 함수를 실행할 수 있습니다 (참고: 생성된 데이터는 아직 필요하지 않음).

```python
# 위 배열에서의 위치 지정: X_1 = 첫 번째 하위 배열; X_2 = 두 번째 하위 배열
X = [(0,0), (1,0)] 
Y = [(3,0)]

# CausalEffects 실행
causal_effects = CausalEffects(graph, graph_type='admg', X=X, Y=Y, S=None, hidden_variables=None, verbosity=1)

# 그래프 플로팅을 위한 부분입니다.
var_names = ['$X_1$', '$X_2$', '$M$', '$Y$', '$Z_1$', '$Z_2$', '$Z_3$']
```

<div class="content-ad"></div>

우리는 이후에 get_optimal_set() 함수를 사용하여 결과적인 CausalEffects 객체를 얻을 수 있습니다. (참고: 우리는 어떤 조정 집합이 아니라, 최적의 조정 집합을 사용합니다. 즉, 노출과 결과 사이의 모든 역문 경로를 막아 최소의 점근적 평가자 분산을 초래하는 공변량 집합).

```js
opt = causal_effects.get_optimal_set()
print("Oset = ", [(var_names[v[0]], v[1]) for v in opt])
```

결과는 다음과 같습니다: Oset = [('$Z_1$', 0), ('$Z_2$', 0), ('$Z_3$', 0)]

그래서 이 경우, 최적의 조정 집합은 Z3, Z2 그리고 Z1로 구성되어 있습니다.

<div class="content-ad"></div>

이제 우리는 최적 조정 세트를 활용한 시각적 일반화된 백도어 조정 기준을 사용하여 평균 처리 효과를 추정합니다. 이를 위해 fit_total_effect 및 predict_total_effect 함수를 사용합니다. 기본적으로 우리는 지정된 최적 조정 세트로 인과 효과 객체를 가져와서 (예: sklearn estimators) 추정기를 선택하고, 이론적 설명에서 정의한 유효한 조정 로직에 따라 DataFrame에 맞추게 됩니다. 따라서 귀하가 원하는 모델을 선택할 수 있습니다. 물론, 예상하는 함수 종속성의 종류에 따라 일부 추정기가 다른 것보다 유용할 수 있습니다. 예를 들어, 고도로 중첩된 인공신경망(ANNs)을 사용하여 매핑 함수의 복잡성을 임의로 증가시킬 수 있습니다. 이 단계에서는 편향과 분산을 균형있게 유지하는 것이 올바른 추정기를 선택하는 데 필수적입니다. 다행히도 통계/머신러닝 분야는 우리가 이 결정을 내릴 수 있도록 도와주는 방대한 문헌과 방법을 제공했습니다. 선형 회귀로 꽤 간단하게 시작하는 것이 최악의 생각이 아닐 수 있습니다:

```python
causal_effects.fit_total_effect(
        dataframe=dataframe, 
        estimator=LinearRegression(),
        adjustment_set='optimal',
        conditional_estimator=None,  
        data_transform=None,
        mask_type=None,
        )
```

추정기를 적합한 후에는 데이터에서 인과 효과 모델을 학습했습니다. 이제 해당 모델을 사용하여 이전에 나타낸 두 개의 개입의 효과를 예측할 수 있습니다. predict_total_effect 함수는 intervention_data, conditions_data 및 pred_params를 인수로 사용합니다.

- pred_params는 sklearn 예측 함수로 전달되는 선택적 매개변수입니다.
- intervention_data는 (1, len(X)) 모양의 개입 데이터로, 단일 값 X=x의 개입을 예측하거나 (T_x, len(X)) 모양의 개입으로 양 T_x 길이의 값 범위의 효과를 예측합니다.
- conditions_data는 조건부 인과 효과를 추정하는 데 사용되며 여기서 더 이상 다루지 않습니다.

<div class="content-ad"></div>

다시 말해, X = 1 및 X = 0으로 설정하여 각각 0과 1의 배열을 생성하여 intervention_data를 만듭니다. 그런 다음 이를 인과 효과 모델에 입력으로 제공하여 이 개입값을 기반으로 한 y의 예상 값을 얻습니다.

```python
intervention_data = 1.*np.ones((1, 2))
y1 = causal_effects.predict_total_effect(intervention_data=intervention_data)
print("y1 = ", y1)

intervention_data = 0.*np.ones((1, 2))
y2 = causal_effects.predict_total_effect(intervention_data=intervention_data)
print("y2 = ", y2)
```

그런 다음 X를 0에서 1로 증가시키는 인과 효과의 예상 값을 최종적으로 계산하기 위해 이 둘 간의 차이를 단순히 계산합니다.

```python
beta = (y1 - y2)
print("Causal effect = %.2f" % (beta))
```

<div class="content-ad"></div>

여기 원인 효과의 예상치가 있습니다:

![Causal Effect](/assets/img/2024-07-08-Hands-onCausalEffectEstimationwithPython_15.png)

진정한 효과 값인 0.75에 매우 근접한 것을 볼 수 있습니다. 이제 numpy 타일을 사용하여 -2부터 2까지의 개입 값 범위에서 효과를 계산하여이 분석을 더 진화시킬 수 있습니다:

```python
intervention_data = np.tile(np.linspace(-2, 2, 20).reshape(20, 1), (1, 2))
estimated_causal_effects = causal_effects.predict_total_effect( 
        intervention_data=intervention_data,
#         conditions_data=conditions_data,
        )
plt.scatter(intervention_data[:,0], estimated_causal_effects[:,0], label="Estimated")
# plt.plot(intervention_values, true_causal_effects, 'k-', label="True")
# plt.title(r"NRMSE = %.2f" % (np.abs(estimated_causal_effects - true_causal_effects).mean()/true_causal_effects.std()))
plt.xlabel('Intervention / X-value range')
plt.ylabel('Causal effect')
plt.legend()
plt.show()
```

이렇게 추가 분석을 통해 더 많은 진전을 이룰 수 있습니다. 좋은 작업하셨어요!

<div class="content-ad"></div>

![Hands-on Causal Effect Estimation with Python](/assets/img/2024-07-08-Hands-onCausalEffectEstimationwithPython_16.png)

여기서 보는 것처럼, 우리는 선형 추정자를 사용했기 때문에 거의 진짜 인과 효과에 가까운 0.74의 기울기를 가진 직선을 출력합니다. 우리가 선택한 추정자에 따라, 이 플롯은 매우 다르게 보일 수 있으며, 일부 실제 현상의 본질적인 복잡성을 드러낼 수 있습니다. 비선형 효과에 대한 자세한 내용은 여기를 참조하세요.

## 정리

오, 기쁜 날! 단순히 데이터에서 패턴을 학습하는 평범한 알고리즘 대신 더 신중한 방식을 택하여, 인과 추론에 기반을 둔 머신 러닝 모델을 만들었습니다. 특히 이전 글들을 모두 따라오셨다면, 이것은 통계적 학습과 인과 추론 사이의 경계에서 발전하는 이 분야의 연구를 탐구하기 위한 필수적인 기본 지식을 습득한 것입니다. 당신이 한 일은 당신의 기술 세트에 가치 있는 자산을 추가한 것입니다. 데이터를 통해 단순한 패턴을 배우는 대신, 더 견고하고 통찰력 있는 모델로 이어지는 더 신중한 방법을 선택했습니다.

<div class="content-ad"></div>

이 여정에서 함께 해 주셔서 감사드리며, 앞으로 이 플랫폼에 게시할 최신 방법들을 살펴보시고 머무르시기를 바랍니다.

독자 여러분, 감사합니다!

저자 소개:

Kenneth Styppa는 독일 항공우주 연구소 데이터 과학 연구소의 인과 추론 그룹 소속이다. UC 버클리 및 제플린 대학에서 정보 시스템 및 기업가 정신 배경을 쌓았으며, 기계 학습과 관련된 창업 및 연구 프로젝트에 참여했다. Jakob과 협력하며 BMW, QuantumBlack와 같은 기업에서 데이터 과학자로 일한 Kenneth는 현재 하이델베르크 대학에서 응용 수학 및 컴퓨터 과학 석사 학위를 취득 중이다. 더 많은 정보: [Kenneth Styppa LinkedIn](https://www.linkedin.com/in/kenneth-styppa-546779159/)

<div class="content-ad"></div>

**Jakob Runge** 교수님은 **드레스덴 공대**의 데이터 과학 교수입니다. 그의 인과 추론 그룹은 지구 시스템 과학 및 다른 많은 분야에 응용 가능한 인과 추론 이론, 방법 및 쉽게 접근 가능한 도구를 개발하고 있습니다. Jakob은 **훔볼트 대학 베를린**에서 물리학 박사 학위를 받았으며 인과 추론의 여정은 **포츠담 기후 변화 연구소**에서 시작되었습니다. 이 그룹의 방법은 **https://github.com/jakobrunge/tigramite.git**에서 공개되어 있습니다. 그룹에 대해 더 알고 싶다면 **www.climateinformaticslab.com**을 방문해주세요.