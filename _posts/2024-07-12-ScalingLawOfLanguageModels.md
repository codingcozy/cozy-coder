---
title: "언어 모델 확장 법칙 이해하기 2024 최신 분석"
description: ""
coverImage: "/assets/img/2024-07-12-ScalingLawOfLanguageModels_0.png"
date: 2024-07-12 23:12
ogImage:
  url: /assets/img/2024-07-12-ScalingLawOfLanguageModels_0.png
tag: Tech
originalTitle: "Scaling Law Of Language Models"
link: "https://medium.com/towards-data-science/scaling-law-of-language-models-5759de7f830c"
isUpdated: true
---

![ScalingLawOfLanguageModels_0](/assets/img/2024-07-12-ScalingLawOfLanguageModels_0.png)

안녕하세요! 인공 지능의 세계는 혁명을 목격하고 있습니다. 그 중심에는 BERT, GPT-3, PaLM과 같은 거대한 언어 모델들이 하루하루 강력해지고 있는 것으로 보입니다. 이러한 AI 거인들은 자연어 처리에서 가능한 범위를 넓히고 있습니다. 하지만 그들의 엄청난 성장을 이끄는 것이 무엇인지 궁금한 적이 있었나요?

이 글에서 저희는 언어 모델의 스케일링의 핵심에 대한 매혹적인 여정을 떠날 것입니다. 이 모델들이 작동하는 비밀 요리를 발견할 것입니다. 모델 크기, 훈련 데이터, 그리고 컴퓨팅 파워라는 세 가지 중요한 요소가 유독 순수하게 혼합됩니다. 이러한 요소들이 어떻게 상호 작용하고 확장되는지를 이해함으로써, 우리는 과거, 현재, 그리고 미래의 AI 언어 모델에 대한 귀중한 통찰력을 얻을 것입니다.

그러니 함께 뛰어 들어가서, 언어 모델을 새로운 성능과 능력의 높은 곳으로 추진하는 스케일링 법칙을 이해해 보도록 하겠습니다.

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

이 게시물은 다음 섹션으로 구성되어 있어요:

- 소개

- 최근 언어 모델 발전 개요

- 언어 모델 확장의 핵심 요소

2. 거듭제곱 분포: 간단한 복습

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

### - 파워 법칙 관계 이해하기

### - 파워 법칙 시각화하기

### 3. 언어 모델에서의 스케일링 법칙 행동

- 모델 크기와 성능
- 데이터셋 크기와 성능
- 계산 자원과 성능

### 4. 스케일링 요인들 사이의 상호작용

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

# 6 FLOP Rules

5. Chinchilla Paper: A Game-Changer

- Essential Discoveries and Consequences
- Chinchilla Predictive Formula

# Final Reflections

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

**- 스케일링 법칙을 이해하는 중요성**

**7. 참고 자료**

# 소개

요즘 몇 년간 언어 모델 개발에서 급속한 스케일링이 일어나고 있다는 것을 이미 알고 있을 것입니다. 아래 이미지에서 볼 수 있듯이, 언어 모델은 2018년 BERT-base의 1억 90백만 개의 파라미터에서 2022년 PaLM의 5400억 개의 파라미터로 스케일이 조정되었습니다. 각 모델은 크기뿐만 아니라 (즉, 파라미터 수), 훈련 토큰 수와 훈련 연산량 (부동 소수점 연산 또는 FLOPs로 표시됨)도 증가했습니다.

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

![Scaling Law of Language Models](/assets/img/2024-07-12-ScalingLawOfLanguageModels_1.png)

오직 자연스러운 질문이 있습니다. "이 세 가지 요소 사이의 관골은 무엇인가요?" 모델 크기와 훈련 데이터는 모델 성능(즉, 테스트 손실)에 동일하게 기여합니까? 어떤 요인이 더 중요한가요? 테스트 손실을 10% 줄이려면 모델 크기를 키워야 하나요 아니면 훈련 데이터를 늘려야 하나요? 어느 정도까지?

이러한 질문에 대한 답은 LLMs의 확장 법칙 행동에 있습니다. 그러나 답변에 들어가기 전에, 우리는 거듭된 법칙 분포를 다시 살펴보겠습니다.

# 거듭된 법칙 분포: 간단한 리뷰

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

![2024-07-12-ScalingLawOfLanguageModels_2.png](/assets/img/2024-07-12-ScalingLawOfLanguageModels_2.png)

![2024-07-12-ScalingLawOfLanguageModels_3.png](/assets/img/2024-07-12-ScalingLawOfLanguageModels_3.png)

To show the different behaviors of the power law with positive and negative values of k, let's plot two curves. When k is positive, there is a positive relationship between y and x. On the other hand, when k is negative, there is a negative relationship between y and x. Below is the code snippet for plotting the power law curve.

```python
import numpy as np
import matplotlib.pyplot as plt

def plot_power_law(k, x_range=(0.1, 100), num_points=10000):
    """
    Plot the power law function y = x^k for any non-zero k.

    Parameters:
    k (float): The exponent for the power law (can be positive or negative, but not zero).
    x_range (tuple): The range of x values to plot (default is 0.1 to 10).
    num_points (int): Number of points to calculate for a smooth curve.
    """
    if k == 0:
        raise ValueError("k cannot be zero")

    # Generate x values
    x = np.linspace(x_range[0], x_range[1], num_points)

    # Calculate y values
    y = x**k

    # Create the plot
    plt.figure(figsize=(10, 6))
    plt.plot(x, y, 'b-', label=f'y = x^{k}')
    plt.title(f'Power Law: y = x^{k}')
    plt.xlabel('x')
    plt.ylabel('y')
    plt.grid(True)
    plt.legend()

    plt.show()
```

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

그리고 긍정적인 k 값을 가지고 아래와 같이 plot 해보세요:

```js
plot_power_law(2)  # 이 코드는 y = x^2를 plot합니다
```

![Plot for Positive Exponent](/assets/img/2024-07-12-ScalingLawOfLanguageModels_4.png)

음수 지수를 선택하면 관계가 감소할 것입니다:

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

```python
plot_power_law(-0.5)  # This will plot y = x^(-0.5)
```

![Scaling Law of Language Models](/assets/img/2024-07-12-ScalingLawOfLanguageModels_5.png)

Above plots are in linear scales on both x-axis and y-axis. If we plot them in logarithmic scale, they will form a line as shown in equation 2. Let's now connect all these points and illustrate how the power law is connected to the test loss of LLMs.

# Scaling Law Behavior in Language Models

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

언어 모델의 스케일링 법칙 행동은 모델 성능과 모델 크기, 데이터셋 크기, 계산 자원과 같은 다양한 요소들 간의 관찰된 관계를 의미합니다. 이러한 관계는 모델이 확장됨에 따라 예측 가능한 패턴을 따릅니다. 스케일링 법칙 행동에서 주요한 요인들은 다음과 같습니다:

- 모델 크기: 모델의 매개변수 수가 증가함에 따라 성능은 일반적으로 거듭 제곱 법칙을 따라 향상됩니다.
- 데이터셋 크기: 더 큰 학습 데이터셋은 일반적으로 더 나은 성능을 이끌어내며, 이 또한 거듭 제곱 법칙과 관련이 있습니다.
- 계산 자원: 학습에 사용된 계산 자원(플롭스) 양은 성능 향상과 관련이 있습니다.

아래의 세 개의 그래프는 LLMs에서의 스케일링 법칙을 나타냅니다.

![Scaling Law of Language Models](/assets/img/2024-07-12-ScalingLawOfLanguageModels_6.png)

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

세 개의 그래프 모두 로그-로그 공간에 있으며 선형이므로 테스트 손실이 컴퓨팅, 데이터셋 크기 및 모델 매개변수 각각과의 멱법칙 관계를 보여줍니다. 또한 이러한 그래프들은 언어 모델링 성능이 모델 크기, 데이터셋 크기, 그리고 훈련에 사용된 컴퓨팅 양을 증가시킬수록 개선된다는 것을 보여줍니다.

지금까지 우리는 이 세 가지 요소와 테스트 손실 간의 개별적인 관계를 보았습니다. 이제 몇 가지 질문이 있습니다: 이 세 가지 요소 사이의 관계는 무엇인가요? 이러한 요소들이 테스트 손실에 어떻게 기여하는가요? 모두 동등하게 기여하나요? 아니면 한 가지가 다른 것보다 중요한가요?

# 스케일링 요소들의 상호 작용

간단히 말해, 모델의 매개변수와 각 훈련 예제마다 약 6개의 부동 소수점 연산이 필요합니다. 따라서 세 가지 요소 간의 관계는 다음과 같습니다:

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

![Image](/assets/img/2024-07-12-ScalingLawOfLanguageModels_7.png)

Hey there! Let's dive into the reason why we need approximately 6 flops for each parameter and training example. When we look into a parameter w during training, here's why:

- In the forward pass, we need 2 flops to multiply w with the input node and add it to the output node in the language model's computational graph. (1 flop for multiplication, 1 flop for addition)
- When calculating the gradient of the loss with respect to w, we need another 2 flops.
- Lastly, 2 flops are needed to update the parameter w with the gradient of the loss.

Isn't it fascinating how these operations come together during the training process? 🌟

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

만약 이 문제에 대해 더 자세한 설명을 원한다면, 이 게시물을 확인해보세요.

α 값이 약 6이라면, 우리는 언어 모델을 훈련하기 위해 필요한 계산량을 추정할 수 있습니다. 이때 모델의 크기와 사용된 훈련 데이터 양을 알아야 합니다.

다음으로, 모델 크기와 훈련 데이터 양이 모델 성능에 어떻게 기여하는지에 대한 질문에 대답해보겠습니다.

# The Chinchilla Paper: A Game-Changer

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

2022년에 DeepMind에서 발표된 논문인 Chinchilla에 대해 이야기해볼게요. 이 논문에서 저자들은 현재의 대형 언어 모델이 모델 크기를 확장하면서 훈련 데이터를 일정하게 유지하는 데 주안점을 두어 충분히 훈련되지 않았다는 사실을 발견했습니다!

사실 저자들은 70백만 개에서 160억 개 이상의 매개변수를 가진 400개 이상의 언어 모델을 50억 개부터 5000억 개까지의 토큰에 대해 훈련시켰고, 연산 최적 훈련을 위해 모델 크기와 훈련 토큰 수 모두를 동등하게 확장해야 한다고 결론 내렸다는 걸 알아두세요.

그들은 모델 크기와 훈련 데이터를 모델 성능에 연결하는 다음과 같은 경험적 예측 공식을 제안했습니다.

\[ N은 매개변수 수(즉, 모델 크기)를 의미하고, D는 훈련 토큰을 나타냅니다. \( L(N,D)는 N개의 매개변수를 가진 모델 및 D 토큰에 대해 훈련된 모델 성능이나 테스트 손실을 가리킵니다. E은 모델이 완벽하게 훈련을 받았을 때 달성할 수 있는 최소 손실인 불가피한 손실을 나타내는 상수로, 모델이 훈련받는 작업의 본질적 난이도와 데이터의 잡음을 설명합니다. \]

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

상수 A와 B 및 지수 α와 β는 실험을 통해 경험적으로 결정되며 데이터를 맞추기 위해 사용됩니다. 구체적으로, α≈0.50이고 β≈0.50임을 발견했습니다. 이는 논문의 주요 발견을 강화하며 모델 크기가 두 배로 증가할 때마다 훈련 토큰 수도 두 배로 증가해야 컴퓨팅 최적의 훈련을 달성함을 나타냅니다.

# 마무리

언어 모델의 확장 법칙은 이러한 강력한 AI 시스템의 개발과 최적화에 대한 중요한 통찰을 제공합니다. 우리가 살펴본대로, 모델 크기, 훈련 데이터 및 계산 리소스 간의 관계는 예측 가능한 거듭 제곱 법칙 패턴을 따릅니다. 이러한 법칙은 AI 연구원들과 엔지니어들에게 중요한 함의가 있습니다:

- 균형있는 확장: 치칠라 연구 결과는 최적의 성능을 위해 모델 크기와 훈련 데이터를 모두 동등하게 확장하는 중요성을 강조합니다. 이전에는 모델 크기만을 증가시키는 데 중점을 두었던 관행에 도전합니다.
- 자원 할당: 이러한 관계를 이해하면 계산 리소스의 효율적인 할당이 가능해져 더 비용 효율적이고 환경 친화적인 AI 개발을 이끌 수 있습니다.
- 성능 예측: 이러한 법칙은 연구자들이 사용 가능한 리소스를 기반으로 모델의 성능에 대한 교육된 예측을 할 수 있게 해줍니다. 이는 현실적인 목표와 기대치를 설정하는 데 도움이 됩니다.

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

AI 분야의 급속한 진보 속에서 이러한 확장 법칙을 염두에 두는 것이 모델 개발, 자원 할당 및 연구 방향에 대한 신중한 결정을 내리는 데 중요할 것입니다. 이러한 관계를 이해하고 활용함으로써, 우리는 미래에 더 효율적이고 강력하며 책임감 있는 언어 모델 개발을 위해 노력할 수 있습니다.

## 참고문헌

- 대규모 언어 모델을 최적으로 학습하는 방법

2. 신경망 언어 모델의 확장 법칙
