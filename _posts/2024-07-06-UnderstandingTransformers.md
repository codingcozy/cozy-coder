---
title: "트랜스포머 모델 완전 정복 활용 방법 및 이해하기"
description: ""
coverImage: "/assets/img/2024-07-06-UnderstandingTransformers_0.png"
date: 2024-07-06 02:57
ogImage:
  url: /assets/img/2024-07-06-UnderstandingTransformers_0.png
tag: Tech
originalTitle: "Understanding Transformers"
link: "https://medium.com/towards-data-science/understanding-transformers-3344d16c8c36"
isUpdated: true
---

**“Attention is All You Need”** 간단히 살펴보기

2017년에 등장한 트랜스포머(Transformer). 이 작품에 대한 많은 설명이 있지만, 수학적으로 너무 심화되거나 세부사항이 너무 얕게 다루어진 것이 많다는 느낌을 받았습니다. 이해하려는 주제에 대해 구글링을 할 시간만큼 많이 사용하는 것은 최선의 방법이 아닙니다. 그래서 이 기사를 쓰게 된 거죠. 여기에서는 트랜스포머의 가장 혁신적인 측면을 간결하고 쉽게 설명하려고 노력했습니다. 누구나 이해할 수 있도록 간단하게 풀어썼어요.

![UnderstandingTransformers](/assets/img/2024-07-06-UnderstandingTransformers_0.png)

**트랜스포머에 담긴 아이디어로 우리는 생산적 인공지능 시대로 진입하게 되었습니다.**

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

트랜스포머는 시퀀스 전이 모델의 새로운 아키텍처를 대표했습니다. 시퀀스 모델은 입력 시퀀스를 출력 시퀀스로 변환하는 모델의 한 종류입니다. 이 입력 시퀀스는 문자, 단어, 토큰, 바이트, 숫자, 음운(음성 인식) 등 다양한 데이터 유형일 수 있으며, 멀티모달¹일 수도 있습니다.

트랜스포머 이전에는 시퀀스 모델이 주로 순환 신경망(RNN), 장단기 메모리(LSTM), 게이트 순환 유닛(GRU) 및 합성곱 신경망(CNN)을 기반으로 했습니다. 이전 모델은 시퀀스의 여러 위치에 있는 항목들이 제공하는 맥락을 고려하기 위한 주의 메커니즘이 종종 포함되어 있었습니다.

# 이전 모델의 단점

![](../img/2024-07-06-UnderstandingTransformers_1.png)

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

- RNNs: 모델은 데이터를 순차적으로 처리하며, 이전 연산에서 학습한 내용은 다음 연산에서 고려됩니다. 그러나 순차적인 성질 때문에 몇 가지 문제가 발생합니다. 모델은 긴 시퀀스에 대한 장기 의존성을 고려하는 데 어려움을 겪으며 (이를 소멸 또는 폭발하는 그래디언트라고 함), 입력 시퀀스의 병렬 처리를 방해합니다. 각각의 청크에 대해 동시에 학습할 수 없기 때문에 (배칭) 이전 청크의 컨텍스트를 잃게 됩니다. 이는 더 많은 연산을 필요로 하여 훈련하기가 더욱 비용이 많이 듭니다.

![UnderstandingTransformers_2](/assets/img/2024-07-06-UnderstandingTransformers_2.png)

- LSTM과 GRU: 게이팅 메커니즘을 사용하여 장기 의존성을 유지합니다. 모델에는 전체 시퀀스에서 relevant한 정보가 포함된 셀 상태가 있습니다. 셀 상태는 forget, input, output 게이트 (LSTM) 및 update, reset 게이트 (GRU)와 같은 게이트를 통해 변경됩니다. 이러한 게이트는 각 순차적 반복에서 이전 상태에서 얼마나 많은 정보를 유지해야 하는지, 새로운 업데이트에서 얼마나 많은 정보를 추가해야 하는지, 그리고 전체적으로 새로운 셀 상태의 어느 부분을 유지해야 하는지 결정합니다. 이는 소멸 그래디언트 문제를 개선하지만 모델은 여전히 순차적으로 작동하며, 특히 시퀀스가 길어지면 한정된 병렬처리로 인해 느리게 학습합니다.
- CNNs: 데이터를 더 병렬적으로 처리하지만 기술적으로 여전히 순차적으로 작동합니다. 지역 패턴을 효과적으로 포착하지만, Convolution 작동 방식 때문에 장기 의존성에 어려움이 있습니다. 두 입력 위치 간의 관계를 포착하기 위한 연산 횟수는 위치 간의 거리가 멀어질수록 증가합니다.

따라서, Transformer를 소개합니다. Transformer는 순환 및 합성곱을 사용하지 않고 전적으로 어텐션 메커니즘에 의존합니다. 모델은 출력을 생성하는 각 단계에서 입력 시퀀스의 서로 다른 부분에 초점을 맞추기 위해 어텐션을 사용합니다. Transformer는 순차적 처리 없이 어텐션을 사용하는 첫 번째 모델로, 병렬 처리를 가능하게 하여 장기 의존성을 잃지 않고 더 빠른 학습이 가능합니다. 또한 입력 위치 간의 연산 횟수를 일정하게 유지하여 위치가 얼마나 멀리 떨어져 있든 항상 동일한 연산량을 수행합니다.

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

# Transformer 모델 아키텍처 탐색

![](assets/img/2024-07-06-UnderstandingTransformers_3.png)

Transformer의 중요 기능은: 어휘화, 임베딩 레이어, 어텐션 메커니즘, 인코더 및 디코더입니다. 프랑스어 입력 시퀀스 "Je suis etudiant"와 영어 목표 출력 시퀀스 "I am a student"을 상상해보죠 (매우 구체적으로 설명한 이 링크를 맹목적으로 복사하고 있습니다).

## 어휘화

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

## 준비물

입력 및 출력 순서는 연속적인 표현의 시퀀스 z로 매핑되어 입력 및 출력 임베딩을 나타냅니다. 각 토큰은 의미를 포착하는 임베딩에 의해 나타내어지며, 다른 토큰들과의 관계를 계산하는 데 도움이 됩니다. 이 임베딩은 벡터로 표현됩니다. 이러한 임베딩을 만들기 위해 우리는 모델을 훈련하는 데 사용되는 각 고유한 출력 토큰을 포함하는 훈련 데이터 집합의 어휘를 사용합니다. 그런 다음 우리는 적절한 임베딩 차원을 결정하게 되는데, 이는 각 토큰에 대한 벡터 표현의 크기에 해당합니다. 임베딩 차원이 높을수록 더 복잡하고 다양하며 복잡한 의미와 관계를 더 잘 포착할 수 있습니다. 따라서 어휘 크기 V 및 임베딩 차원 D에 대한 임베딩 행렬의 차원은 V x D가 되어 고차원 벡터가 됩니다.

초기화 단계에서 이러한 임베딩은 무작위로 초기화될 수 있으며 더 정확한 임베딩은 훈련 과정 중에 학습됩니다. 임베딩 행렬은 그 후 훈련 중에 업데이트됩니다.

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

Positional encodings are added to these embeddings because the transformer does not have a built-in sense of the order of tokens.

![Image](/assets/img/2024-07-06-UnderstandingTransformers_4.png)

## Attention mechanism

Self-attention is the mechanism where each token in a sequence computes attention scores with every other token in a sequence to understand relationships between all tokens regardless of distance from each other. I’m going to avoid too much math in this article, but you can read up here about the different matrices formed to compute attention scores and hence capture relationships between each token and every other token.

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

위에서 언급한 주의 점수는 각 토큰에 대한 새로운 표현 세트⁴로 이어집니다. 이후 처리 계층에서 사용됩니다. 훈련 중에는 역전파를 통해 가중치 행렬이 업데이트되어 모델이 토큰 간의 관계를 더 잘 고려할 수 있습니다.

다중 헤드 주의는 자기 주의의 확장입니다. 다른 주의 점수가 계산되고 결과가 연결되고 변환되어 결과 표현은 모델이 다양한 복잡한 토큰 간 관계를 캡처하는 능력을 향상시킵니다.

## 인코더

입력 시퀀스에서 구성된 입력 임베딩에 위치 인코딩이 추가되어 인코더에 공급됩니다. 입력 임베딩은 다중 헤드 주의와 피드포워드 네트워크가 포함된 6개 계층으로 구성되어 있습니다. 또한 각 계층의 출력인 LayerNorm(x+Sublayer(x))가 되도록 잔여 연결이 있습니다. 인코더의 출력은 주의 점수를 고려한 입력의 맥락화된 표현이 됩니다. 이를 디코더에 공급합니다.

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

## 디코더

타겟 출력 시퀀스에서 생성된 출력 임베딩(위치 부호화가 적용됨)이 디코더에 공급됩니다. 디코더에도 6개의 레이어가 있지만, 인코더와 다른 두 가지 차이가 있습니다.

첫째, 출력 임베딩은 가림막이 있는 다중 헤드 어텐션을 거치게 됩니다. 이는 시퀀스 내 뒤따르는 위치에서의 임베딩이 어텐션 점수를 계산할 때 무시된다는 것을 의미합니다. 현재 토큰(위치 i에 있는)을 생성할 때, 위치 i 이후의 모든 출력 토큰을 무시해야하기 때문입니다. 게다가 출력 임베딩은 한 위치씩 오른쪽으로 오프셋되어, i 위치에 있는 예측 토큰은 그 이전 위치의 출력만을 기반으로 하게 됩니다.

예를 들어, 입력이 "je suis étudiant à l’école"이고 목표 출력이 "i am a student in school"인 경우, student에 대한 토큰을 예측할 때, 인코더는 "je suis etudiant"의 임베딩을 사용하지만, 디코더는 "a" 이후의 토큰들을 숨겨서 student의 예측이 문장에서 이전 토큰인 "I am a"만을 고려하도록 합니다. 이렇게 하면 모델이 토큰을 순차적으로 예측하도록 학습됩니다. 물론, "in school" 토큰들은 모델의 예측에 추가적인 문맥을 제공하지만, 우리는 모델이 이 문맥을 "etudiant"와 이후의 입력 토큰인 "à l’école"에서 파악하도록 모델을 훈련하고 있습니다.

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

디코더가 이 문맥을 어떻게 얻는지 궁금하신가요? 그건 두 번째 차이로 이어집니다: 디코더의 두 번째 멀티헤드 어텐션 레이어는 입력의 문맥화된 표현을 가져와 피드-포워드 네트워크로 전달하기 전에 받아들입니다. 이를 통해 출력 표현이 입력 토큰과 이전 출력의 전체 문맥을 포착하도록 보장합니다. 결과적으로 우리는 각 대상 토큰에 해당하는 벡터 시퀀스를 얻게 되는데, 이는 문맥화된 대상 표현입니다.

![이미지](/assets/img/2024-07-06-UnderstandingTransformers_5.png)

## 선형 및 소프트맥스 레이어를 사용한 예측

이제, 문맥화된 대상 표현을 사용하여 다음 토큰이 무엇인지 파악하고 싶습니다. 디코더에서 나온 문맥화된 대상 표현을 사용하여, 선형 레이어는 벡터 시퀀스를 훨씬 큰 로짓 벡터로 투영합니다. 이는 우리 모델 어휘의 길이와 동일한 L 길이를 갖는데, 선형 레이어는 가중치 행렬을 포함하고 있습니다. 이를 디코더 출력과 곱한 후 바이어스 벡터를 더하면, 1 x L 크기의 로짓 벡터가 생성됩니다. 각 셀은 고유한 토큰의 점수이며, 소프트맥스 레이어가 이 벡터를 정규화하여 전체 합이 1이 되도록 합니다. 이제 각 셀은 각 토큰의 확률을 나타내는데, 가장 높은 확률을 갖는 토큰을 선택하면 우리가 예측한 토큰이 나오게 됩니다. Voila!

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

## 모델 훈련

다음으로, 우리는 예측된 토큰 확률을 실제 토큰 확률과 비교합니다 (실제 토큰 확률은 대상 토큰을 제외한 각 토큰에 대해 로짓 벡터가 0이 될 것이고, 대상 토큰에 대해서만 확률이 1.0이 될 것입니다). 우리는 각 토큰 예측에 적합한 손실 함수를 계산하고 전체 대상 시퀀스에 걸쳐 이 손실을 평균화합니다. 그런 다음 이 손실을 모델의 모든 매개변수에 걸쳐 역전파하여 적절한 그래디언트를 계산하고, 적절한 최적화 알고리즘을 사용하여 모델 매개변수를 업데이트합니다. 따라서 클래식 트랜스포머 아키텍처의 경우, 다음이 업데이트됩니다.

- 임베딩 매트릭스
- 어텐션 점수를 계산하는 데 사용되는 다양한 매트릭스
- 피드포워드 신경망과 관련된 매트릭스
- 로짓 벡터를 생성하는 데 사용되는 선형 매트릭스

이것은 하나의 훈련 에포크를 나타냅니다. 훈련에는 데이터셋의 크기, 모델의 크기 및 모델 작업에 따라 다수의 에포크가 포함되며, 그에 따라 훈련이 이루어집니다.

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

# Transformers가 이렇게 좋은 이유로 돌아가기

위에서 언급한 바와 같이, RNN, CNN, LSTM 등의 문제점은 병렬 처리의 부족, 순차적 아키텍처, 그리고 장기 의존성을 적절히 포착하지 못한다는 점을 포함합니다. 위의 트랜스포머 아키텍처는 이러한 문제를 해결하는 방식으로...

- 어텐션 메커니즘은 전체 시퀀스를 순차적으로 처리하는 대신 병렬로 처리할 수 있도록 합니다. 셀프 어텐션을 통해 입력 시퀀스의 각 토큰은 해당 미니 배치의 다른 모든 토큰에 주의를 기울입니다(다음에 설명될 예정입니다). 이는 순차적인 방식이 아닌 동시에 모든 관계를 포착합니다.
- 각 epoch 내에서 입력 값을 미니 배치 처리로 나누면 병렬 처리, 빠른 학습 및 모델의 확장성이 더 쉬워집니다. 예제가 가득한 대규모 텍스트에서 미니 배치는 이러한 예제의 작은 집합을 나타냅니다. 데이터셋의 예제는 미니 배치로 들어가기 전에 섞이고, 각 epoch의 시작부에서 재섞입니다. 각 미니 배치는 동시에 모델에 전달됩니다.
- 위치 인코딩과 배치 처리를 사용함으로써 시퀀스의 토큰 순서가 고려됩니다. 토큰 사이의 거리도 멀든 가까운 이동에 관계없이 동일하게 고려되며, 미니 배치 처리는 이를 더욱 보장합니다.

## 논문에서 보듯, 결과는 환상적이었습니다.

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

트랜스포머 세계에 오신 것을 환영합니다!

# GPT 아키텍처 간단히 알아보기

트랜스포머 아키텍처는 구글 브레인에서 일하던 연구원인 아시시 바스와니(2017년)에 의해 소개되었습니다. OpenAI에서는 2018년에 생성적 사전 훈련 트랜스포머(GPT)를 소개했습니다. 주된 차이점은 GPT에는 아키텍처 내에 인코더 스택이 포함되어 있지 않다는 것입니다. 인코더-디코더 구성은 한 시퀀스를 다른 시퀀스로 직접 변환할 때 유용합니다. GPT는 생성 능력에 초점을 맞추어 설계되었으며, 디코더를 제거하고 나머지 구성 요소는 유지했습니다.

![UnderstandingTransformers_6.png](/assets/img/2024-07-06-UnderstandingTransformers_6.png)

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

GPT 모델은 방대한 텍스트 말뭉치에서 사전 훈련되어 모든 단어와 토큰 간의 관계를 학습하는 비지도 학습 과정을 거칩니다. 다양한 용도(예: 일반적인 대화형 챗봇)로 세밀하게 조정된 뒤, 생성 작업에서 극도로 효과적임이 입증되었습니다.

예를 들어, "GPT가 응답을 예측하는 방법"이라는 질문을 하면 예측 단계는 일반 트랜스포머와 크게 다르지 않습니다. 이 질문에 대한 답변을 예측하기 위해 단어가 토큰화되고 임베딩이 생성되며, 어텐션 점수가 계산되고, 다음 단어의 확률이 계산되며, 다음 예측 토큰이 선택됩니다. 예를 들면, 모델은 "GPT는 ...을 통해 응답 예측"과 같이 답변을 단계별로 생성하고, 다음 단어의 확률에 따라 완전하고 일관된 답변을 형성할 때까지 이어질 것입니다. (엿보기를 해본 적 있니? 마지막 문장은 chatGPT에서 였어!)

이해하기 쉬웠기를 희망합니다. 이해하기 어렵다면, 다른 사람에게 트랜스포머에 대해 설명해보라는 차례일지도 몰라요. 😉

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

만약 이 글이 흥미로웠다면 자유롭게 의견을 공유하고 연락해 주세요!

- LinkedIn: [Aveek's LinkedIn](https://www.linkedin.com/in/aveekg00/)
- Website: [Aveek's Website](aveek.info)

## 참고 자료:

- [arXiv 논문](https://arxiv.org/pdf/1706.03762)
- [시퀀스 모델](https://deeplearningmath.org/sequence-models)
- [LSTMs 이해하기](http://colah.github.io/posts/2015-08-Understanding-LSTMs/)
- [Illustrated Transformer](http://jalammar.github.io/illustrated-transformer/)
- [언어 미지도 학습](https://openai.com/index/language-unsupervised/)

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

## 다른 훌륭한 참고 자료들:

- [Lilian Weng - Attention?](https://lilianweng.github.io/posts/2018-06-24-attention/)
- [bastings - Annotated Encoder Decoder](https://bastings.github.io/annotated_encoder_decoder/)
- [Harvard NLP - Annotated Transformer Prelims](https://nlp.seas.harvard.edu/annotated-transformer/#prelims)
