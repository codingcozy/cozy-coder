---
title: "머신러닝을 배우는 용기 경사 하강법과 인기 있는 최적화 기법 상세 탐구"
description: ""
coverImage: "/assets/img/2024-07-13-CouragetoLearnMLADetailedExplorationofGradientDescentandPopularOptimizers_0.png"
date: 2024-07-13 22:47
ogImage: 
  url: /assets/img/2024-07-13-CouragetoLearnMLADetailedExplorationofGradientDescentandPopularOptimizers_0.png
tag: Tech
originalTitle: "Courage to Learn ML: A Detailed Exploration of Gradient Descent and Popular Optimizers"
link: "https://medium.com/towards-data-science/courage-to-learn-ml-a-detailed-exploration-of-gradient-descent-and-popular-optimizers-022ecf97be7d"
isUpdated: true
---





Welcome back to a new chapter of ‘Courage to Learn ML’. For those new to this series, our aim is to make complex topics accessible and engaging, much like a casual conversation between a mentor and a learner, inspired by the writing style of “The Courage to Be Disliked,” with a special focus on machine learning.

In our previous discussions, our mentor and learner delved into common loss functions and the three fundamental principles of designing them. Today, they will explore another vital concept: gradient descent.

As always, here's a list of the topics we'll be exploring today:

<div class="content-ad"></div>

- 그라디언트란 무엇이며 왜 그라디언트 디센트 기법이라고 불리는 것인가요?
- 왜 바닐라 그라디언트 디센트는 딥 뉴럴 네트워크(DNN)에서 성능이 좋지 않고 개선 방법은 무엇인가요?
- 뉴턴 메소드, 아다그라드, 모멘텀, RMSprop 및 아담과의 관계를 비롯한 다양한 옵티마이저들에 대한 리뷰
- 제 개인적인 경험을 바탕으로 적절한 옵티마이저를 선택하는 실용적인 통찰력

# 그래서, 우리는 예측과 실제 결과의 차이를 측정하기 위한 손실 함수를 설정했습니다. 이 차이를 줄이기 위해 모델의 파라미터를 조정합니다. 왜 대부분의 알고리즘은 그들의 학습 및 업데이팅 과정에 그라디언트 디센트를 사용할까요?

이 질문에 대한 대답을 하기 위해, 그라디언트 디센트에 익숙하지 않다고 가정하고 독특한 업데이트 이론을 개발해보겠습니다. 먼저, 현재 모델의 이탈도를 측정하는 손실 함수를 사용하여 차이를 양적으로 나타내는데, 이는 신호(현재 모델과 기초 패턴 간의 이탈)와 잡음(데이터의 불규칙성과 같은 요소)을 포함합니다. 그 다음 단계는 이 데이터를 전달하고 다양한 파라미터를 조정하는 데 활용하는 것입니다. 그러면 각 파라미터에 대해 필요한 수정 정도를 결정하는 것이 어렵게 됩니다. 기본적인 접근 방식은 각 파라미터의 기여도를 계산하고 비례적으로 업데이트하는 것일 수 있습니다. 예를 들어, W*x + b = y와 같은 선형 모델에서, x = 1일 때 예측값이 50이지만 실제 값은 100이라면 차이는 50입니다. 그런 다음 w와 b의 기여도를 계산하여 예측값을 실제 값 100에 맞추도록 조정할 수 있습니다.

그러나 두 가지 주요 문제가 발생합니다.

<div class="content-ad"></div>

- 기여도 계산: x = 1일 때 100이 나올 수 있는 다양한 w와 b의 조합 중 어떤 조합이 더 나은지 결정하는 방법은 무엇인가요?
- 복잡한 모델에서의 계산 요구: 수백만 개의 매개변수를 갖는 심층 신경망(DNN)을 업데이트하는 것은 계산적으로 요구되는 작업일 수 있습니다. 이를 효율적으로 관리하는 방법은 무엇인가요?

이러한 어려움들은 문제의 복잡성을 강조합니다. 비선형적이고 복잡한 모델에서 각 매개변수의 최종 예측에 대한 기여를 정확하게 결정하는 것은 거의 불가능합니다. 따라서 손실에 기반한 모델 매개변수의 효과적인 업데이트를 위해 각 매개변수에 대한 조정을 정확하게 지시할 수 있는 비용 효율적인 방법이 필요합니다.

그래서 우리는 각 매개변수에 손실을 할당하는 방법에 초점을 맞추는 대신, 손실 표면을 탐색하는 전략으로 생각할 수 있습니다. 목표는 우리를 전역 최솟점으로 안내하는 매개변수 세트를 찾는 것입니다 - 모델이 달성할 수 있는 가장 가까운 근사치입니다. 이 조정 과정은 RPG 게임을 하는 것과 유사합니다. 플레이어가 지도에서 가장 낮은 지점을 찾는 것과 유사합니다. 이것이 경사 하강법 뒤에 있는 기본적인 아이디어입니다.

![image](/assets/img/2024-07-13-CouragetoLearnMLADetailedExplorationofGradientDescentandPopularOptimizers_1.png)

<div class="content-ad"></div>

# 그래서 경사 하강법이 정확히 무엇인가요? 왜 그것을 경사 하강법이라고 부르는 걸까요?

자세히 살펴보겠습니다. 최적화를 중심으로 하는 손실 표면은 손실 함수와 모델 매개변수에 의해 형성되며, 서로 다른 매개변수 조합에 따라 변화합니다. 3D 손실 표면 플롯을 상상해보세요: 수직 축은 손실 함수 값, 다른 두 축은 매개변수를 나타냅니다. 전역 최솟값에서 우리는 가장 낮은 손실을 갖는 매개변수 집합을 찾습니다. 이것이 실제 결과와 예측 사이의 차이를 최소화하기 위해 노력하는 우리의 궁극적인 목표입니다.

![이미지](/assets/img/2024-07-13-CouragetoLearnMLADetailedExplorationofGradientDescentandPopularOptimizers_2.png)

하지만 이 전역 최솟값 쪽으로 어떻게 항해할까요? 여기서 그래디언트가 필요합니다. 그것은 우리에게 이동할 방향을 안내해줍니다. 아마도 '왜 그래디언트를 계산하는 걸까요?' 라고 궁금해할 수 있습니다. 이상적으로는 전체 손실 표면을 보고 최솟값 쪽으로 곧장 가면 될 것입니다. 그러나 현실에서는, 특히 복잡한 모델과 다양한 매개변수를 사용할 때, 우리는 전체 지형을 시각화할 수 없습니다. 그것은 단순한 계곡보다 더 복잡합니다. 우리는 우리 주변의 것들만 볼 수 있을 뿐입니다, 마치 RPG 게임에서 안개 낀 지형 안에 있는 것처럼 말이죠. 그래서 우리는 가파른 상승 방향을 가리키는 그래디언트를 사용하고, 그 방향과 정반대로 향하게 됩니다, 가파르게 내려가는 방향으로. 그래디언트를 따라가며, 손실 표면의 전역 최솟값으로 점차적으로 내려가는 것이 바로 우리가 경사 하강법이라고 부르는 이 여정입니다.

<div class="content-ad"></div>

![](/assets/img/2024-07-13-CouragetoLearnMLADetailedExplorationofGradientDescentandPopularOptimizers_3.png)

# 그래디언트 디센트는 각 매개변수에 필요한 조정을 어떻게 결정하며, 왜 우리가 처음에 제안한 간단한 방법보다 효과적일까요?

우리의 목표는 특정한 매개변수 집합으로 손실 함수를 최소화하는 것인데, 이는 오직 이러한 매개변수를 조정함으로써만 가능합니다. 우리는 이러한 변경들을 통해 간접적으로 손실 함수에 영향을 미칩니다.

우리의 RPG 비유를 다시 살펴봅시다. 영웅은 기본적인 움직임(왼쪽/오른쪽, 앞/뒤)과 제한된 시야만을 가지고, 한 번도 발길치지 않은 지도에서 전설적인 무기를 발굴하기 위해 가장 낮은 지점을 찾으려고 합니다. 우리는 그래디언트가 움직일 방향을 나타낸다는 것을 알고 있지만, 그것은 단순히 포인터 이상입니다. 그것은 또한 기본적인 움직임으로 분해됩니다.

<div class="content-ad"></div>

경사도는 각 매개변수에 대한 편미분 값의 벡터로, 이동해야 할 양과 기본 방향(왼쪽, 오른쪽, 앞, 뒤)을 나타낸다. 마법 같은 안내자처럼, 왼쪽 연못을 가리키는 것뿐만 아니라 전설적인 무기로 이끌어주는 특정한 회전과 걸음을 지시한다.

하지만 경사도가 실제로 무엇을 의미하는지 이해하는 것이 중요하다. 종종 자습서는 언덕 위에 있다고 상상하고 가파른 방향을 선택하기 위해 주위를 둘러보라고 제안한다. 그러나 이것은 오해를 줄 수 있다. 경사도는 손실 표면 자체의 방향이 아니라 해당 방향을 매개변수 차원(그래프에서 x, y 좌표)에 투영한 것으로, 손실 함수의 최소 방향으로 안내한다. 이 차이는 중요하다. 경사도는 손실 표면에 있지 않고 매개변수 공간 내의 방향 안내자이다.

![image](/assets/img/2024-07-13-CouragetoLearnMLADetailedExplorationofGradientDescentandPopularOptimizers_4.png)

이것이 왜 대부분의 시각화에서 손실 함수가 아닌 매개변수 윤곽을 사용하여 경사하강 과정을 설명하는지이다. 움직임은 매개변수를 조정하는 데 관한 것이며, 손실 함수의 변화는 그 결과로 나타난다.

<div class="content-ad"></div>

이미지 Markdown 형식으로 변경합니다.

Gradient(경사)는 편미분을 통해 형성됩니다. 편미분은 다른 매개변수를 고정시킨 채 함수가 어떻게 변하는지 이해하는 데 도움이 됩니다. 이것이 그래디언트가 각 매개변수가 손실 함수의 방향적인 변화에 미치는 영향을 양을화하는 방법입니다.

그래디언트 하강은 최초에 직면한 매개변수 조정 딜레마를 자연스럽고 효율적으로 해결합니다. 이 방법은 볼록 문제에서 전역 최소값을 특히 잘 찾아내며 비볼록 시나리오에서 지역 최소값을 찾는 데 능숙합니다. 현대적 구현은 GPU나 TPU를 통한 병렬화와 가속화를 통해 이점을 얻습니다. 미니배치 경사 하강법이나 Adam과 같은 변형은 다양한 맥락에서 그 효율성을 최적화합니다. 요약하자면, 경사 하강법은 안정적이며 대규모 데이터셋과 다수의 매개변수를 처리할 수 있어 학습 목적에 더 좋은 선택입니다.

# 경사 하강법 공식인 𝜃 = 𝜃 — 𝜂⋅∇𝜃𝐽(𝜃)에서 학습률(𝜂)을 기울기(∇𝜃𝐽(𝜃))에 곱하여 각 매개변수를 업데이트합니다. 왜 학습률이 필요한 걸까요? 직접 기울기로 매개변수를 조정하는 게 더 빠른 방법이 아닌가요?

<div class="content-ad"></div>

총 그라디언트만 사용하여 글로벌 최솟값에 도달하는 것은 간단해 보일 수 있습니다. 그러나 이것은 그라디언트의 본성을 무시하는 것입니다. 그라디언트는 손실 함수에 대한 매개변수의 편도함수 벡터로, 가파른 상승 방향과 가파른 정도를 나타냅니다. 기본적으로 우리에게 제한된 시야에서 가장 유망한 방향으로 안내해줍니다. 하지만 이것은 그냥 그 방향일 뿐입니다. 이 정보를 어떻게 활용할지 결정하여야 합니다.

학습 속도 또는 스텝 크기는 여기서 중요합니다. 이것은 학습 과정의 속도를 제어합니다. 예를 들어 비디오 게임 예시에서 산 정상에 있는 캐릭터를 상상해보세요. 게임은 왼쪽으로 이동하여 내려오는 것이 보물(전역 최솟값을 나타냄)에 가장 빠른 방법이라고 알려줍니다. 또한 산의 가파름에 대해 알려줍니다: 왼쪽으로 5만큼, 앞으로 10만큼의 가파름이 있습니다. 이제 영웅을 얼마나 이동시킬지 결정합니다. 조심스러운 경우 작은 스텝을 선택할 수 있습니다. 예를 들어 학습 속도를 0.001로 설정하면, 영웅은 왼쪽으로 0.001 * 5 = 0.005 단위, 앞으로 0.001 * 10 = 0.01 단위 이동합니다. 결과적으로 영웅은 목표지점으로 이동하며 그라디언트의 방향과 일치하지만 지나치지 않고 조절된 속도로 이동합니다.

요약하자면, 그라디언트의 크기를 학습 속도와 혼동하지 않는 것이 중요합니다. 크기는 상승 방향의 가파름을 나타내며, 이는 손실 표면에 따라 다를 수 있습니다. 반면 학습 속도는 데이터셋과는 상관없는 선택사항입니다. 이는 우리가 학습 여정에서 얼만큼 신중하게 또는 공격적으로 진행할지를 나타내는 하이퍼파라미터입니다.

## 만약 학습 속도가 데이터셋에 의존하지 않는다면, 어떻게 설정해야 할까요? 학습 속도의 일반적인 범위는 무엇이며, 너무 높거나 낮게 설정하는 문제점은 무엇일까요?

<div class="content-ad"></div>

학습률은 하이퍼파라미터이기 때문에 일반적으로 교차 검증이나 이전 경험을 기반으로 실험을 통해 선택합니다. 일반적인 학습률 범위는 0.001부터 0.1 사이입니다. 이 범위는 데이터 사이언스 커뮤니티의 경험에 기반한 경험적 관찰을 바탕으로 하며, 이 범위 내의 학습률은 빠르고 효과적으로 수렴하는 경향이 있습니다. 이론적으로, 0.1보다 높지 않은 학습률을 선호합니다. 왜냐하면 더 높은 학습률은 각 단계에서 매개변수를 지나치게 변화시킬 수 있어 오버슛(overshooting)과 같은 위험을 초래할 수 있습니다. 실제적인 면에서, 학습률이 0.001보다 낮은 것은 학습 과정을 느리게 만들어 연산 비용이 많이 드고 시간이 많이 걸리게 해버릴 수 있으므로 피합니다.

일반적인 범위는 극단적인 학습률의 문제점을 이해하는 데 도움이 됩니다. 학습률이 너무 높으면 큰 단계 크기 때문에 알고리즘이 너무 빨리 움직여 오버슈팅하고 목표에 도달하지 못할 수 있습니다. 반면에 아주 낮은 학습률은 작은 단계를 거치기 때문에 전역 최소값에 도달하기까지 매우 오랜 시간이 걸릴 수 있어 연산 자원과 시간을 낭비할 수 있습니다.

아래 시각적 표현은 학습 과정에 다른 학습률이 미치는 영향을 이해하는 데 도움이 됩니다:

![Learning Process](/assets/img/2024-07-13-CouragetoLearnMLADetailedExplorationofGradientDescentandPopularOptimizers_6.png)

<div class="content-ad"></div>

# 바닐라 그라디언트 디센트의 한계는 무엇입니까? 특히 DNN 모델에 적용할 때?

그라디언트 디센트는 이론적으로 잘 작동하지만, 특히 DNN 모델에서는 실제 응용에서 중요한 도전에 직면합니다. 주요 제한 사항은 다음과 같습니다:

- 바닐라 그라디언트 디센트는 계산적으로 비용이 많이 듭니다. 바닐라 그라디언트 디센트의 공식인 𝜃 = 𝜃 — 𝜂⋅∇𝜃𝐽(𝜃)은 전체 데이터셋을 통해 평균 손실 𝐽(𝜃)을 계산해야 합니다. 이는 모든 예측 값과 실제 레이블을 비교하고 이러한 차이를 평균해야 함을 의미합니다. 전형적인 DNN 모델의 경우, 수백만 개의 매개변수와 큰 데이터셋이 포함되는 경우가 많기 때문에 이 프로세스는 계산적으로 많은 리소스를 소모합니다. 이 복잡성은 바닐라 그라디언트 디센트가 훨씬 오래된 개념임에도 불구하고, 2012년 AlexNet과 같은 모델이 나오기 전까지 DNN의 실제 응용이 불가능했던 이유 중 하나입니다.
- DNN에서의 손실 표면은 일반적으로 비볼록하며 여러 지역 최소값을 포함합니다. 𝐽(𝜃) = 0인 지점을 찾기 위해 그라디언트 디센트를 사용하는 단순한 기준은 이러한 시나리오에서 부적절합니다. 우리의 비디오 게임 비유에서, 이는 영웅이 다음으로 어디로 이동해야 하는지 확신하지 못하는 것과 같습니다. 특히 문제가 되는 지형은 평평한 지점과 산맥 지점으로, 𝐽(𝜃)이 0이 되는 지점입니다. 산맥과 지역 최소값에 가까운 영역들은 𝐽(𝜃)가 매우 작고 0에 가까운 것들이므로 학습 프로세스를 크게 느리게 만들 수 있습니다. 𝐽(𝜃)의 작은 값은 학습 프로세스를 지연시켜 시간과 계산 리소스를 불필요하게 소비할 수 있습니다.

![이미지](/assets/img/2024-07-13-CouragetoLearnMLADetailedExplorationofGradientDescentandPopularOptimizers_7.png)

<div class="content-ad"></div>

# 그라디언트 디센트가 작동하는 이유 및 학습률이 작아야 하는 이유

그라디언트 디센트를 이해하기 위해 테일러 급수를 사용하여 손실 함수를 근사하는 것에 대해 이야기해야 합니다. 테일러 급수는 특정 지점에서 복잡한 함수의 값을 추정하는 데 엄청난 도구입니다. 이는 더 단순한 개별 요소를 사용하여 복잡한 상황을 설명하는 것과 같습니다. "차량 사고가 있었다"와 같이 넓은 주장 대신에 개별적인 사건으로 나눠 설명합니다: 강아지를 수의사에 데려다 줬을 때, 강아지가 뒷좌석에서 일어나 있을 때, 전화벨이 울릴 때, 그리고 사고가 발생했을 때. 테일러 급수도 비슷한 일을 합니다. 단일 일반 용어를 사용하여 복잡한 함수 f(x)를 설명하려고 하는 대신, 해당 함수를 x = a에서의 f(x)의 특정 값으로 설명하기 위해 다항식 항목 합으로 분해합니다. 테일러 급수는 함수를 특정 지점 (x = a)에서의 미분을 기반으로 다항식 항목의 합으로 분해합니다. 시리즈의 각 항목은 근사치에 자세한 정보를 추가합니다. 

![2024-07-13-CouragetoLearnMLADetailedExplorationofGradientDescentandPopularOptimizers_8.png] 

이제 손실 함수 J(θ)를 최소화하는 것으로 돌아가보겠습니다. Taylor series를 적용하여 해당 함수를 간단하게 근사하기 위해 1차 미분을 주로 의존합니다. 이 맥락에서 J(θ)는 매개변수 변수 집합을 나타내는 벡터인 θ를 기반으로 하는 다변수 함수이며, a는 현재 이러한 매개변수의 값입니다.

<div class="content-ad"></div>

![image](/assets/img/2024-07-13-CouragetoLearnMLADetailedExplorationofGradientDescentandPopularOptimizers_9.png)

To minimize 𝐽(𝜃), we should make (𝜃 — a) very small, so our next 𝜃 values will be close to our current position, represented by vector a. This need for closeness is why having a small learning rate is essential. When 𝜃 significantly differs from the current parameter values a, minimizing our loss function becomes difficult. Moreover, because the gradient 𝐽’(a) indicates a direction, we move (𝜃 — a) in the opposite direction. By following the steepest descent direction (opposite to the gradient), we aim for the global minimum, where 𝐽(𝜃) = 0.

# You mentioned that vanilla gradient descent isn’t the best for DNNs. Why is that?

The fundamental problem with using vanilla gradient descent for DNNs is the nature of their loss functions, which are usually non-convex. In such cases, gradient descent faces challenges. Think of navigating a complex terrain, not just a simple hill or valley but a landscape full of unpredictable elevations and depressions. In such a scenario, gradient descent might not lead us to the global minimum and could even increase the loss function's value post updates. One reason for this is that first-order partial derivatives offer limited insights, showing how the loss changes concerning one parameter while keeping others constant. This overlooks interactions between multiple parameters, crucial in intricate models.

<div class="content-ad"></div>

![image](/assets/img/2024-07-13-CouragetoLearnMLADetailedExplorationofGradientDescentandPopularOptimizers_10.png)

In our Tarot exploration today, we delve deeper into the mystical concept of the Taylor series. Much like the intricate threads of a tapestry, when the fabric of our objective or loss function weaves into a complex pattern, relying solely on the initial derivative may not unveil the full picture.

Just as each Tarot card reveals layers of meaning, the vanilla gradient descent technique may struggle to navigate through these intricate, non-convex terrains of loss functions. Yet, fear not, dear seeker, for even in the shadowed corners of complexity, the gradient descent holds the power to unravel the mysteries and bring forth clarity.

While the road to enlightenment may twist and turn through local minima and formidable saddle points, the journey towards the global minimum remains a noble quest. So, let us gaze beyond the horizon of conventions and ponder – how can we infuse the essence of magic into our descent, paving a swifter path towards the elusive global minimum?

<div class="content-ad"></div>

큰 질문이네요! 이전 토론과 비디오 게임 비유가 경사 하강법을 개선하는 몇 가지 방법을 드러냈습니다. 함께 이런 향상된 부분들을 살펴봐요:

## 지도를 플레이어 친화적으로 만들기.

게임에서 수월한 지도를 탐험함으로써 보물에 도달하기가 더 쉬워지는 것처럼, 경사 하강법에서도 일부 수정 사항은 전역 최소값까지의 경로를 더욱 부드럽게 만들 수 있습니다.

가장 명백한 방법 중 하나는 지도(손실 표면)를 수정하는 것입니다. 예를 들어, 분류 문제에서는 교차 엔트로피가 일반적으로 사용됩니다(또는 이진 분류의 경우 로그 손실). 분류 작업에서 Mean Squared Error (MSE)를 선택하면 여러 지역 최소값을 갖는 손실 표면이 생성될 수 있어, 학습 과정이 이러한 최솟값 중 하나에 갇히는 가능성이 높아집니다.

<div class="content-ad"></div>

다른 방법으로는 특성 선택, 정칙화, 특성 스케일링 및 배치 정규화와 같은 기술을 사용하는 것이 있습니다. 특성 선택은 연산 비용을 줄이는데 도움이 되는 것뿐만 아니라 손실 표면을 단순화시키기도 합니다. 왜냐하면 손실 함수는 매개변수의 수에 영향을 받기 때문입니다. L1/L2 정칙화는 이 프로세스에 도움이 될 수 있습니다: 특성 선택에는 L1을, 손실 표면을 부드럽게 만들기 위해 L2를 사용합니다. 특성 스케일링은 중요합니다. 왜냐하면 서로 다른 범위의 특성은 균일하지 않은 단계 크기를 초래하여 수렴을 늦추거나 아예 불가능하게 할 수 있기 때문입니다. 비슷한 범위로 특성을 스케일링함으로써 보다 균일한 단계를 보장함으로써 더 빠르고 일관된 수렴을 도와줍니다.

특성 스케일링처럼 배치 정규화는 층 사이의 입력을 정규화하는 것을 목표로 합니다. 이 방법은 조정을 위한 추가적인 하이퍼파라미터를 도입합니다. 배치 정규화는 층의 출력을 다음 층의 입력으로 가기 전에 표준화하여 배치의 샘플 사이에 의존성을 설정합니다. 따라서 적절한 배치 크기를 선택하는 것이 중요합니다. 왜냐하면 배치 크기가 1인 경우 배치 정규화에 대해 효과적이지 않습니다. 논문 "배치 정규화가 최적화에 어떻게 도움이 되는가?"는 이 기술이 손실 표면을 부드럽게 만들면서 전역 최소값의 위치를 거의 변경하지 않을 것이라고 제안합니다.

예측 단계에서는 배치 정규화가 훈련 단계에서 계산된 이동 평균과 분산을 사용하여 배치를 정규화하며, 테스트 데이터에서 배치별 통계량을 계산하는 대신 훈련 중에 저장된 값을 활용합니다. 이는 훈련 및 테스트 단계 간 데이터 유출을 방지하기 위한 데이터 변환의 표준적인 방법과 일치합니다.

![image](/assets/img/2024-07-13-CouragetoLearnMLADetailedExplorationofGradientDescentandPopularOptimizers_11.png)

## 빠르게 움직이세요.

<div class="content-ad"></div>

중국 속담에 '天下武功无快不破'라는 말이 있습니다. (어떤 무예도 빠르지 않으면 무너질 수 있다.) 이 개념은 그레이디언트 하강법에도 적용됩니다. 순수한 그레이디언트 하강법에서는 각 반복에서 전체 훈련 데이터를 사용하여 손실을 계산하는 것이 시간이 많이 소요되고 자원을 많이 소모하는 작업입니다. 그러나 더 적은 데이터로도 유사한 결과를 얻을 수 있습니다. 더 작은 샘플에서의 평균 손실은 전체 데이터셋과 크게 다르지 않을 수 있다는 아이디어입니다. 미니 배치 그레이언트 하강법(데이터 부분 집합 사용)이나 확률적 그레이디언트 하강법(SGD, 각 시간에 무작위 샘플 선택)과 같은 방법을 사용하여 프로세스를 획기적으로 가속화할 수 있습니다. 이러한 접근법은 빠른 계산과 업데이트를 가능하게 하여 특히 DNNs에서 효과적입니다.

![Link to image](/assets/img/2024-07-13-CouragetoLearnMLADetailedExplorationofGradientDescentandPopularOptimizers_12.png)

## 더 많은 정보를 수집하여 더 스마트하게 이동하세요.

그레이디언트 하강법에서 더 스마트하게 움직이는 것은 가파른 하강법을 넘어서야 함을 의미합니다. 이전 세션에서 그레이디언트 하강법 뒤에 있는 수학에 대해 배웠습니다. f(x)의 첫 번째 도함수를 x = a에서 사용하는 것은 f(x)의 견고한 근사치를 제공합니다. 그러나 이 근사치를 향상시키기 위해 추가적인 도함수 항을 통합하는 것이 유익합니다. 비디오 게임 비유에서, 이는 가장 가파른 경사 방향을 찾는 것뿐만 아니라 풍경을 이해하는 포괄적인 시야를 얻기 위해 카메라를 돌리는 것과 같습니다. 이 접근법은 우리가 지역 또는 전역 최솟값(계곡 바닥에 있음), 최댓값(언덕 꼭대기에 있음) 또는 안장점(언덕과 계곡으로 둘러싸인 지점)에 있는지 식별하는 데 특히 유용합니다. 그러나 우리의 제한된 시야 때문에 우리는 항상 지역과 전역을 구분할 수 없습니다.

<div class="content-ad"></div>

이 개념을 확장하면 뉴턴의 방법을 적용할 수 있습니다. 이 기술은 목표 또는 손실 함수의 일차 및 이차 미분을 계산합니다. 이차 미분은 헤시안 행렬을 생성하여 손실 함수의 지형을 더 자세히 볼 수 있게 합니다. 일차 및 이차 미분에서 얻은 정보로 손실 함수를 더 근사화할 수 있습니다. 전통적인 경사 하강법과 달리 뉴턴의 방법은 학습률을 사용하지 않습니다. 그러나 라인 서치를 통한 뉴턴의 방법과 같은 변형은 학습률을 포함하여 조정 가능한 수준을 더합니다. 뉴턴의 방법이 바닐라 경사 하강보다 효율적으로 보일 수 있지만, 높은 계산 요구로 인해 실무에서는 일반적으로 사용되지 않습니다. 머신 러닝에서 뉴턴의 방법이 상대적으로 덜 흔한 이유에 대해 더 알고 싶다면 여기 논의를 참조해보세요.

## 이동하면서 단계 크기 조절하기

이 전략은 여정을 더 효율적으로 만들 수 있습니다. 가장 간단한 방법은 전역 최소값에 접근할 때 단계 크기를 줄이는 것입니다. 이는 학습률이 각 epoch마다 감소하는 SGD와 같은 경사 하강 방식에서 특히 관련이 있습니다(학습률 스케줄을 통해). 그러나이 방법은 모든 매개변수에 대해 학습률을 균일하게 조정하므로 손실 지형의 복잡성을 고려할 때 이상적이지 않을 수 있습니다. 비디오 게임 분야에서 이는 다양한 움직임의 강도를 조절하여 보다 효과적으로 탐색하는 것과 유사합니다. 이런 이유로 Adagrad와 같은 방법이 필요한데, 이는 적응적 그래디언트 접근 방식을 제공합니다. 이 방법은 각 매개변수의 제곱 그래디언트의 이력을 누적하고 이 누적값의 제곱근을 사용하여 각각의 학습률을 조정합니다. 이 방법은 이상한 업데이트가 발생할 때 과거 경험을 기반으로 게임에서 동작을 세밀하게 조정하는 것과 같습니다.

그러나 직관적이지만 Adagrad는 공격적인 학습률 감소로 인해 속도가 느려질 수 있습니다. 이 방법의 변형은 이 측면을 균형있게 유지하려고 합니다.

<div class="content-ad"></div>

![CouragetoLearnMLA](/assets/img/2024-07-13-CouragetoLearnMLADetailedExplorationofGradientDescentandPopularOptimizers_13.png)

## 더 나은 기본 움직임을 선택하기

경사 하강법에서 우리의 움직임을 최적화하는 것은 지형에 기반하여 접근 방식을 연마하는 것을 포함합니다. 바닐라 경사 하강법에서, 우리의 경로는 종종 불확실한 비디오 게임에서의 보폭과 유사한 지그재그를 그립니다. 이는 두 매개변수의 등고선으로 시각화되며, 더 많은 수직 이동과 더 적은 수평 이동을 보여줍니다. 비록 수평 경로가 더 효율적일지라도요.

![CouragetoLearnMLA](/assets/img/2024-07-13-CouragetoLearnMLADetailedExplorationofGradientDescentandPopularOptimizers_14.png)

<div class="content-ad"></div>

피쳐 스케일링은 도움이 될 수 있지만 DNN의 복잡한 표면에서는 보다 집중된 접근 방식이 필요합니다. 그것이 모멘텀 그라디언트 디센트가 등장하는 이유입니다. 이 방법은 특정 방향으로의 움직임이 역생산적인지 여부를 식별합니다. 현재 그라디언트로 매개변수를 직접 업데이트하는 대신, 지난 그라디언트의 지수 이동 평균을 계산합니다. 이 '모멘텀'은 일관된 방향으로 진전을 가속화하고 무의미한 움직임을 억제하는 데 도움을 줍니다.

![Image 1](/assets/img/2024-07-13-CouragetoLearnMLADetailedExplorationofGradientDescentandPopularOptimizers_15.png)

![Image 2](/assets/img/2024-07-13-CouragetoLearnMLADetailedExplorationofGradientDescentandPopularOptimizers_16.png)

여기서 주목할 점은 Adagrad가 제곱 그라디언트를 사용하는 것과 달리, 우리는 역사적 그라디언트를 직접 누적합니다. 이 접근 방식은 양의 움직임과 음의 움직임이 서로 상쇄되도록 허용하기 때문에 중요합니다. 이것은 특정 방향으로의 모멘텀을 쌓는 것으로 생각할 수 있으며, 일관된 방향으로 움직일 경우 학습 과정을 가속화합니다. 반대로, 작은 누적 값은 해당 방향에서의 소수 진전을 나타내며, 해당 방향으로의 진행이 가장 유망한 움직임이 아닐 수 있음을 시사합니다.

<div class="content-ad"></div>

To enhance the smoothing effect and capitalize on past movements, we assign more weight to the accumulated gradient history. This is done by setting a higher value for the momentum coefficient, typically denoted as β. A common choice for β is 0.9, which strikes a balance between giving importance to historical movements while still being responsive to new gradient information. This method ensures a smoother journey by favoring directions with consistent progress and dampening oscillations that do not contribute effectively towards reaching the goal.

## Combine those strategies!

Merging the principles of Adagrad and Momentum Gradient Descent offers an innovative way to enhance gradient descent. Both these methods rely on historical gradients, but with a key difference in their approach. Momentum Gradient Descent uses the exponential moving average of gradients instead of a simple average. The advantage here is that by adjusting the momentum coefficient β, we can strike a balance between the influence of historical gradient trends and the current gradient.

- Inspired by this, we can apply a similar logic to Adagrad, leading to the development of RMSprop (Root Mean Square Propagation). RMSprop is essentially an evolved version of Adagrad, utilizing the exponential moving average of historical gradients rather than a simple average. This modification places more weight on historical gradients, reducing the impact of exceptionally large current gradients. Consequently, it leads to a less aggressive decrease in the learning rate, addressing the issue of slow learning rates that Adagrad often faces.

<div class="content-ad"></div>

상기 아이디어를 더 발전시켜서, 왜 Adagrad/RMSprop의 학습률 조정을 Momentum의 기울기 조정 전략과 결합하지 않을까요? 이러한 생각이 Adam(적응형 모멘트 추정)의 생성으로 이어졌습니다. Adam은 과거 기울기를 두 가지 방법으로 활용하여 이 두 방법을 결합합니다. 하나는 지수 이동 평균을 조정하는 데 사용하고(모멘텀), 다른 하나는 과거 기울기의 규모를 관리하는 데 사용합니다(RMSprop). 이 이중 응용 덕분에 Adam은 매우 효과적이고 안정적인 최적화 도구가 되었습니다. Adam은 미세 조정을 위한 두 가지 추가적인 하이퍼파라미터를 도입하긴 했지만 여전히 인기 있는 최적화 도구입니다.

<img src="/assets/img/2024-07-13-CouragetoLearnMLADetailedExplorationofGradientDescentandPopularOptimizers_18.png" />

# 다양한 최적화 도구가 제공되는 상황에서, 실전에서 어떻게 적절한 최적화 도구를 선택해야 할까요?

<div class="content-ad"></div>

실제로 옵티마이저를 선택하는 것은 데이터와 학습 목표의 세부 사항에 따라 다릅니다. 제 경험과 관찰을 토대로 몇 가지 일반적인 지침을 소개해 드리겠습니다.

- 온라인 학습에는 SGD 사용: 온라인 학습은 연속적으로 유입되는 데이터 스트림을 처리하는 것을 포함하며, 새로운 작은 데이터 배치로 모델을 자주 업데이트해야 합니다. 확률적 경사 하강법(SGD)은 다른 옵티마이저보다 더 자주 모델 업데이트를 위해 작은 배치를 효율적으로 사용하여 이러한 시나리오에 특히 적합합니다. 또한 SGD는 데이터가 불안정하거나 작은 변화를 겪고 있는 환경에서 효과적입니다. 오버슛을 방지하기 위해 적절한 학습률을 사용하면 SGD는 데이터의 미묘한 변화에 빠르게 적응할 수 있습니다. 그러나 SGD는 비정상적인 환경을 처리할 수 있지만, 데이터 패턴의 주요 변화를 캡처하는 데 효과적이지 않을 수 있음을 명심해야 합니다.
- Adam 및 희소 데이터: 많은 수의 0 항목을 포함하는 데이터를 다룰 때는 해당 데이터가 희소하다고 표현됩니다. 희소 데이터의 문제는 특정 피처에 대한 제한된 정보가 있기 때문입니다. Adam 옵티마이저는 이러한 컨텍스트에서 특히 효과적으로 작동합니다. 이는 모멘텀과 적응형 학습률 메커니즘을 통합하기 때문입니다. 이 조합으로 Adam은 각 매개변수에 대해 학습률을 개별적으로 조정할 수 있습니다. 결과적으로 데이터의 희소성으로 인해 정보가 부족한 또는 제대로 표현되지 않는 기능에 대해 더 자주 업데이트하여 더 균형 잡힌 효과적인 학습 프로세스를 보장합니다.
- 옵티마이저를 결합하지 마세요. 대신 현명하게 함께 사용해야 합니다. 여러 옵티마이저를 사용하는 것은 가능하지만, 단일 학습 단계 내에서 동시에 적용해서는 안 됩니다. 이렇게 하면 모델에서 혼란을 유발하고 학습 경로를 복잡하게 할 수 있습니다. 대신 여러 옵티마이저를 효과적으로 활용하는 두 가지 전략적 접근 방법이 있습니다:
- 서로 다른 단계에서의 옵티마이저 전환. 예를 들어, DNN을 학습할 때 초기 에포크에서 빠른 진행 능력을 가진 Adam으로 시작하고, 학습이 진행됨에 따라 후속 에포크에서 SGD로 전환하여 더 정확한 학습 프로세스 제어를 제공하여 모델이 더 최적의 국소 최소값으로 수렴하도록 도와줄 수 있습니다.
- 모델의 서로 다른 부분을 훈련하는 데 서로 다른 옵티마이저 사용. 기존 사전 훈련된 모델에 새 계층을 추가하는 상황에서 세심한 접근 방식이 유용할 수 있습니다. 사전 훈련된 계층의 경우 안정적이고 공격적이지 않은 옵티마이저가 이미 학습한 피처의 무결성을 유지하는 데 이상적입니다. 반면, 새롭게 추가된 계층에 대해서는 더 공격적인 빠른 학습 조정을 용이하게 하는 옵티마이저가 더 적합할 것입니다. 이 방법을 통해 각 모델 부분이 특정한 학습 요구 사항에 따라 가장 적합한 최적화 기술을 받도록 보장할 수 있습니다.

<div class="content-ad"></div>

아담의 학습률을 조절하는 능력이 항상 "더 발전된" 것을 만드는 것은 아닙니다. 어떤 경우에는 간단함이 더 나은 결과를 가져올 수 있습니다. SGD의 간단한 학습률과 안정적인 접근은 특히 나중 단계에서 더 효과적입니다. 아담은 학습률을 지나치게 감소시켜 불안정성을 야기할 수 있지만, SGD의 일정한 속도는 더 신뢰할 수 있게 더 나은 지역 최소값에 접근하고 안정성을 향상시킬 수 있습니다.

게다가, SGD의 느린 수렴은 과적합을 방지하는 데 도움이 될 수 있습니다. 세밀하고 조절된 조정은 더 정확하게 학습 데이터에 맞추어지며, 보다 일반화된 보이지 않는 데이터로 확장될 수 있습니다. SGD의 고정된 또는 예약된 학습률은 연구자들에게 모델 학습 과정에 대해 더 나은 제어를 제공하며, 모델 튜닝의 최종 단계에서 특히 속도보다는 정밀도와 안정성을 강조합니다.

이 시리즈를 즐기고 계신다면, 꼭 기억해 주세요. 여러분의 상호 작용 - 좋아요, 댓글, 팔로우 - 는 단순히 지원을 넘어, 이 시리즈를 지탱하는 원동력이자 계속된 공유를 영감을 주는 겁니다.

이 시리즈의 다른 게시물:

<div class="content-ad"></div>

희망찬 인사, 친구들! 🌟

- ML 배움에 용기를 내어라: L1 및 L2 정칙화 해부 (파트 1)
- ML 배움에 용기를 내어라: 우도, MLE 및 MAP 해독
- ML 배움에 용기를 내어라: F1, 재현율, 정밀도, ROC 곡선 심층 탐구
- ML 배움에 용기를 내어라: 가장 일반적인 손실 함수에 대한 철저한 안내

이 기사가 마음에 드셨다면 LinkedIn에서 저를 찾아보세요. 함께 공부하며 더 많은 지식을 나눌 수 있기를 기대합니다! 🌿✨