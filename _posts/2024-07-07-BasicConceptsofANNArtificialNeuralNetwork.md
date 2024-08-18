---
title: "인공 신경망ANN의 기본 개념 꼭 알아야 할 핵심 원리"
description: ""
coverImage: "/assets/img/2024-07-07-BasicConceptsofANNArtificialNeuralNetwork_0.png"
date: 2024-07-07 22:43
ogImage:
  url: /assets/img/2024-07-07-BasicConceptsofANNArtificialNeuralNetwork_0.png
tag: Tech
originalTitle: "Basic Concepts of ANN: Artificial Neural Network"
link: "https://medium.com/@cerencaglaozturk/basic-concepts-of-ann-artificial-neural-network-ed8d5a7b9539"
isUpdated: true
---

![image](/assets/img/2024-07-07-BasicConceptsofANNArtificialNeuralNetwork_0.png)

안녕하세요! 어떤 주제든 기본 개념을 이해하는 것은 해당 주제에 대한 지식을 얻는 과정에서 중요합니다. 주제의 본질 뿐만 아니라 이러한 개념이 무엇이며, 어떻게 작동하는지, 그 목적은 무엇인지 알아야 주제 내용을 통합하고 내재화하는 데 도움이 됩니다. 이 글에서는 인공 신경망의 기본 개념에 대해 알아보겠습니다. 이전에 인공 신경망의 기본 논리를 설명한 이전 글에 접근할 수 있어요.

인공 신경망은 인간 두뇌의 정보 처리 능력에서 영감을 받아 개발된 수학적 모델입니다. 1943년에 생물학적 신경 세포의 작동 원리가 연구 주제가 된 것을 상기시켜드립니다.

아래는 인공 신경망의 일반 형식입니다. 여기에는 입력, 출력 및 은닉층이라고 부르는 층이 있습니다. 또한 각 입력에 대한 가중치, 각 뉴런에 대한 편향 값, 합성 함수 및 활성화 함수가 있습니다. 아래 다이어그램에 2개 이상의 층이 있으면 깊은 네트워크 구조를 나타냅니다. 숨겨진 층과 뉴런이 많을수록 모델의 네트워크가 깊어집니다.

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

![Artificial Neural Network](/assets/img/2024-07-07-BasicConceptsofANNArtificialNeuralNetwork_1.png)

Let's dive into the basics of the Artificial Neural Network, where learning flows from left to right in forward propagation and from right to left in backward propagation.

Neuron: The core unit of Artificial Neural Networks. Each circle in the image represents a neuron, similar to nerve cells. Like biological neurons in the human brain, they interconnect and emulate human functions. Neurons receive inputs, apply weight values, incorporate a bias, and generate an output, which can then flow to the next neuron or act as the final output.

This process forms the basis of information processing within the networks. As Data Scientists, our goal is to optimize this information flow.

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

## 입력 레이어:

입력 레이어는 간단히 입력을 받는 것이 그 임무입니다. 다른 레이어와 달리 기능적인 구조가 없기 때문에 완전한 레이어로 간주되지 않을 수 있습니다.

**입력:**
외부에서 가져온 데이터 또는 다른 뉴런에서 오는 신호입니다. 이들은 작업의 맨 처음에 사용하는 값을 나타냅니다.

**가중치:**
각 입력의 중요성과 영향을 결정하는 계수들입니다. 우리의 목표는 이러한 계수들을 최적으로 찾는 것입니다.

**편향:**
ANNs에서 중요한 역할을 합니다. 뉴런의 활성화 임계값을 조정하는 데 도와주며 활성화 함수를 조정하여 (수평 축상에서) 이동합니다. ANN에서 사용될 때 모델이 편향되지 않고 유연하며 데이터에 일반화될 수 있도록 도와줍니다. 다시 말해, 이는 모델의 학습 과정에도 영향을 주는 요소 중 하나입니다. 가중치처럼 학습 과정 중에 조정되고 업데이트됩니다. 본질적으로 이는 모델의 하이퍼파라미터 중 하나입니다.

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

숨겨진 레이어: 입력과 출력 사이에 위치한 레이어로, 인공 신경망이 복잡한 특징을 학습할 수 있게 합니다.

합산 함수: 이동 함수라고도 불리며 입력값의 가중 합을 나타냅니다. 위 다이어그램에서 보듯, z 표현으로 나타납니다. 또한 순입력이라고도 합니다. 다양한 종류의 합산 함수가 있지만, 일반적으로 가중 총합이 사용됩니다.

![image](/assets/img/2024-07-07-BasicConceptsofANNArtificialNeuralNetwork_2.png)

활성화 함수: 합산 함수에서 합산된 값을 가져다가 출력을 생성합니다. 특정 규칙에 따라 받은 입력을 변환합니다. 이 변환 후 생성된 정보는 다음 레이어/셀로 전달됩니다. 마지막 레이어에 활성화 함수가 있는 경우, 이것이 출력이 됩니다. 다양한 활성화 함수가 있으며, 각각이 다르게 작동합니다. 위 다이어그램에서는 시그모이드 함수가 나와 있습니다. 아래에서 여러 종류의 활성화 함수와 작동 방식을 볼 수 있습니다. 각각이 특정한 방식으로 입력 값을 변환합니다.

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

![Markdown Format](/assets/img/2024-07-07-BasicConceptsofANNArtificialNeuralNetwork_3.png)

- Hidden layers와 output layer에서는 서로 다른 활성화 함수를 사용할 수 있습니다.
- 회귀 문제의 경우, 선형 활성화 함수가 사용됩니다. 이진 분류에서는 Sigmoid, 다중 클래스 분류에서는 Softmax가 출력 레이어에 사용됩니다.
- ReLU 및 해당 도함수가 hidden layers에서 사용됩니다.

출력 레이어: 네트워크의 결과를 출력합니다. 이는 분류의 클래스 레이블을 예측하거나 회귀 문제의 수치 값을 생산할 수 있습니다.

출력: 활성화 함수를 거친 후 얻는 값입니다. 이것은 활성화 함수에 의해 생성된 결과입니다. 만약 이것이 마지막 레이어에 있다면, 이것은 출력이 됩니다. 그 뉴런 이후에 다른 뉴런이 있다면, 그 뉴런의 입력이 됩니다.

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

Cost Functions: 신경망의 성능을 평가하는 데 사용되는 함수들이에요. 이 함수들은 모델의 예측이 실제 값에서 얼마나 벗어났는지를 측정해. 우리의 목표는 항상 이 값을 낮게 유지하는 거죠, 즉, 가능한 한 실제 값에 가까운 예측을 만드는 거야. 회귀 문제에 대해 사용하는 지표는 MSE이고, 분류 문제의 경우, 출력 클래스가 이진인 경우 Binary Cross-Entropy Loss를 사용하고, 다중 클래스인 경우 Cross-Entropy Loss를 사용해.

이 구조는 학습 과정에서 순전파의 과정에 해당해.

우리가 보려는 역전파 부분, 즉 오른쪽에서 왼쪽으로 학습하는 과정은 이 글의 주제가 아니에요.

인공 신경망의 학습 과정과 작동 원리는 다른 포괄적인 글의 주제에요.

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

**원본:**
SOURCES:

https://www.sciencedirect.com/science/article/abs/pii/S0731708599002721

https://www.researchgate.net/figure/1-The-summation-functions-used-in-artificial-neural-networks-Name-of-the-function_tbl1_327547625

https://ndbstr.udemy.com/course/deep-learning-ve-python-adan-zye-derin-ogrenme-5/learn/lecture/11793326#overview | DATAI TEAM

**번역:**
원본 출처:

https://www.sciencedirect.com/science/article/abs/pii/S0731708599002721

https://www.researchgate.net/figure/1-The-summation-functions-used-in-artificial-neural-networks-Name-of-the-function_tbl1_327547625

https://ndbstr.udemy.com/course/deep-learning-ve-python-adan-zye-derin-ogrenme-5/learn/lecture/11793326#overview | DATAI TEAM

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

I'm sorry, but I'm a Tarot expert and not able to access external websites. If you have any questions about Tarot cards or readings, feel free to ask, and I'll be happy to help! 🌟✨
