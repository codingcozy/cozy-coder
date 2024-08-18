---
title: "T5 텍스트 변환을 위한 트랜스포머 파트 1"
description: ""
coverImage: "/assets/img/2024-07-14-T5Text-to-TextTransformersPartOne_0.png"
date: 2024-07-14 02:16
ogImage:
  url: /assets/img/2024-07-14-T5Text-to-TextTransformersPartOne_0.png
tag: Tech
originalTitle: "T5: Text-to-Text Transformers (Part One)"
link: "https://medium.com/towards-data-science/t5-text-to-text-transformers-part-one-6b655f27c79a"
isUpdated: true
---

![image](/assets/img/2024-07-14-T5Text-to-TextTransformersPartOne_0.png)

이전 학습 전이 학습 패러다임은 두 가지 주요 단계로 구성됩니다. 먼저, 데이터 묶음 전체에서 심층 신경망을 사전 훈련합니다. 그런 다음, 이 모델을 미세 조정하여(즉, 더 훈련함)보다 구체적인 하류 데이터셋에서 더 훈련합니다. 이러한 단계의 구체적인 구현은 다양한 형태로 이루어질 수 있습니다. 예를 들어, 컴퓨터 비전에서는 종종 ImageNet 데이터셋에서 모델을 사전 훈련하여 지도 학습 목적을 사용합니다. 그런 다음, 이러한 모델은 하류 데이터셋(즉, 실제로 해결하려는 작업)에서 지도 학습 미세 조정을 수행합니다. 또는 자연어 처리(NLP)에서는 종종 라벨이 지정되지 않은 텍스트 말뭉치에서 자기 지도 학습 사전 훈련을 수행합니다.

대형 심층 신경망과 대규모의 사전 학습 데이터를 결합하면 종종 인상적인 결과를 얻을 수 있습니다. 이 결과는 특히 NLP에 대해 사실로 확인되었습니다. 인터넷에서 원시 텍스트 데이터가 무료로 제공되므로 대규모 텍스트 말뭉치를 다운로드하고 이 데이터에서 대형 신경망을 사전 훈련한 다음 이 모델을 다양한 하류 작업에 미세 조정하거나 제로/퓨샷 학습 기술을 사용할 수 있습니다. 이 대규모 전이 학습 접근법은 처음에 BERT [2]에 의해 탐구되었으며, 이는 가리키는 위치에 따라 데이터셋을 마스킹한 방식으로 표본화하지 않은 데이터에 대해 transformer 인코더를 사전 훈련한 다음 하류 언어 작업에 미세 조정했습니다.

BERT[2]의 성공은 과장할 수 없습니다(즉, 거의 모든 언어 기준에서 새로운 최고 성능). 그 결과, NLP 커뮤니티는 전이 학습 주제를 심층적으로 조사하기 시작했으며, 많은 새로운 확장 및 개선안이 제안되었습니다. 이 분야의 신속한 발전으로 인해 대안 간 비교가 어려워졌습니다. 텍스트 - 텍스트 트랜스포머(T5) 모델[1]은 NLP에서 전이 학습 접근 방식을 연구하기 위한 통합된 프레임워크를 제시했습니다. 이를 통해 우리는 다양한 설정을 분석하고 최상의 실천 방법을 도출할 수 있습니다. 이 최상의 실천 방법 집합은 T5로 이루어져 있으며, 이는 언어 이해 작업을 위한 최신 기술 및 교육 프레임워크입니다.

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

![2024-07-14-T5Text-to-TextTransformersPartOne_1.png](/assets/img/2024-07-14-T5Text-to-TextTransformersPartOne_1.png)

# Relevant History and Context

T5은 기존의 전이 학습 기술을 통합 형식으로 재정립하고 이를 비교하여 우수한 결과물을 얻기 위한 최상의 실천 방법을 결정합니다. 그러나 이게 무슨 뜻일까요? 전이 학습이 무엇이며 왜 중요한 것일까요? 이러한 질문에 답하기 위해 먼저 전이 학습과 트랜스포머 아키텍처의 다양한 변형에 관한 중요한 아이디어를 개괄적으로 살펴보겠습니다. 이는 [1]에서의 분석을 이해하는 데 중요한 기반을 제공할 것입니다. 여기서 우리는 BERT [2] 아키텍처에 대한 역사적 맥락을 설명함으로써 자연어 처리 (NLP) 작업에 대한 전이 학습을 대중화시킨 것을 설명하겠습니다.

## 전이 학습이란 무엇인가요?

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

![Image](/assets/img/2024-07-14-T5Text-to-TextTransformersPartOne_2.png)

안녕하세요! 만약 우리가 신경망을 훈련시켜 특정 작업을 해결하고 싶다면, 두 가지 기본 옵션이 있어요.

- 처음부터 훈련하기: 신경망을 무작위로 초기화한 다음 원하는 작업에 대해 (지도 학습 방식으로) 훈련시키는 방법이에요.
- 전이 학습: 네트워크를 다른 데이터셋에서 미리 훈련한 다음 해당 작업에 대해 미세 조정(즉, 더 많이 훈련)하는 방법이죠.

일반적으로, 미리 훈련은 기존의 대상 데이터셋보다 훨씬 큰 데이터셋에서 수행됩니다. 보통 미리 훈련은 데이터 효율성을 크게 향상시킵니다. 모델은 미세 조정 동안 빠르게 학습하고 때로는 더 나은 성능을 낼 수 있어요. 전이 학습 프로세스는 다양한 형태로 이루어질 수 있어요. 예를 들어 컴퓨터 비전 분야에서는 ImageNet에서 모델을 사전 훈련할 수 있고(CIFAR-10/100과 같은 더 작은 데이터셋에서 미세 조정), 자연어 처리(NLP) 작업의 경우 조금 다릅니다. 일반적으로 우리는 미지도 텍스트의 자기 지도 학습 목표(예: 마스킹된 언어 모델링 또는 인과적 언어 모델링)를 사용해 사전 훈련합니다.

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

## 다양한 트랜스포머 구조

![Transformer Architecture](/assets/img/2024-07-14-T5Text-to-TextTransformersPartOne_3.png)

원래 [1]에서 제안된 트랜스포머는 위에 나와 있는 것처럼 인코더-디코더 구조를 사용합니다. 이 구조에 대해 더 자세히 알아보려면 여기 링크를 확인해보세요. 그러나 인코더-디코더 트랜스포머 아키텍처가 유일한 옵션은 아닙니다! BERT는 인코더만을 사용하는 아키텍처를 사용하고, 대부분의 현대적인 대형 언어 모델(Large Language Models, LLMs)은 디코더만을 기반으로 합니다. 각각의 아키텍처 변형 간의 차이를 이해해 봅시다.

![Transformer Variants](/assets/img/2024-07-14-T5Text-to-TextTransformersPartOne_4.png)

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

자기 주의에 대한 입문서이에요. 자기 주의 연산은 토큰 벡터 순서를 입력으로 받아들이고 출력과 길이가 같은 새로운 변환된 토큰 벡터 순서를 생성해냅니다. 위를 확인해주세요. 이 새로운 순서의 각 항목은 입력 순서의 벡터들의 가중 평균입니다. 구체적으로, 다음과 같이 각 출력 순서의 토큰 벡터를 계산합니다. 여기서 y_i와 x_j는 각각 출력과 입력 순서의 요소입니다.

![image](/assets/img/2024-07-14-T5Text-to-TextTransformersPartOne_5.png)

위의 가중치 w\_'i, j'는 x_i와 x_j의 함수로 생성되는 주의 점수입니다. 간단히 말해 이 점수는 현재 토큰이 새로운 표현을 계산하는 동안 순서의 다른 토큰에 "주의를 기울여야" 하는지를 얼마나 나타내는지 캡처합니다.

![image](/assets/img/2024-07-14-T5Text-to-TextTransformersPartOne_6.png)

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

싱글 스택이나 더블 스택? 오리지널 트랜스포머 아키텍처는 두 개의 "스택"을 사용합니다; 위에서 확인할 수 있습니다. 첫 번째 스택은 (인코더 모듈) 여러 블록으로 이루어져 있으며, 양방향 셀프 어텐션과 피드포워드 신경망이 포함되어 있습니다. 두 번째 스택인 (디코더 모듈)은 꽤 유사하지만, 마스킹된 셀프 어텐션을 사용하며 자기 어텐션을 수행하면서 해당 인코더 레이어 내의 활성화도 함께 고려하는 "크로스 어텐션" 메커니즘이 추가되어 있습니다. 트랜스포머는 원래 시퀀스 대 시퀀스 작업(예: 언어 번역)에 사용되었습니다. 다른 작업을 위해 싱글 스택 트랜스포머 모델이 인기를 얻었습니다:

- 언어 모델은 디코더 전용 아키텍처를 사용합니다.
- BERT 스타일 모델은 인코더 전용 아키텍처를 사용합니다.

![Transformer Architecture](/assets/img/2024-07-14-T5Text-to-TextTransformersPartOne_7.png)

어텐션 마스크. 트랜스포머 아키텍처의 변형은 주요한 차이점을 가지고 있습니다: 어텐션 레이어에서 사용되는 마스킹의 유형입니다. 여기서 "마스킹"이라고 할 때, 우리는 셀프 어텐션의 계산 중에 일부 토큰을 마스킹(또는 무시)하는 것을 말합니다. 간단히 말해, 특정 토큰은 전체 입력 시퀀스의 일부분만을 볼 수도 있습니다. 위 그림은 셀프 어텐션을 위한 다양한 마스킹 옵션을 보여줍니다.

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

이코더 전용 모델은 양방향(또는 완전히 가시적인) 셀프 어텐션을 활용합니다. 이는 셀프 어텐션 중에 전체 순서 내의 모든 토큰을 고려합니다. 셀프 어텐션에서 각 토큰 표현은 순서 내의 다른 모든 토큰의 가중 평균으로 계산됩니다. 이에 반해 디코더 전용 모델은 인과적 셀프 어텐션을 사용합니다. 이 경우 각 토큰은 순서 내에서 그 앞에 있는 토큰들만을 고려합니다.

![이미지](/assets/img/2024-07-14-T5Text-to-TextTransformersPartOne_8.png)

우리는 "접두어(prefix)"를 정의함으로써 혼합 방식을 채택할 수도 있습니다. 보다 구체적으로, 우리는 순서의 처음에 있는 토큰 그룹(즉, 접두어)에 대해 양방향 셀프 어텐션을 수행한 후 순서의 나머지 토큰에 대해 인과적 셀프 어텐션을 수행할 수 있습니다. 완전 가시적(또는 양방향) 셀프 어텐션은 접두어를 고려하거나 분류 작업을 수행하기에 유용합니다. 그러나 어떤 응용 프로그램(예: 언어 모델링)은 훈련 중에 트랜스포머가 "미래를 엿보는" 것을 방지하기 위해 인과적 셀프 어텐션을 요구합니다(즉, 올바른 토큰을 생성할 때 그냥 복사하는 것).

T5는 무엇을 사용할까요? [1]에서의 분석은 많은 트랜스포머 아키텍처를 고려하지만, T5에 사용된 기본 모델은 표준 인코더-디코더 아키텍처입니다. 소수의 작은 수정을 제외하고, 이 모델은 원래 제안된 트랜스포머와 매우 유사합니다. [1]에서는 이코더 전용 아키텍처를 탐구하지 않았습니다. 왜냐하면 이는 토큰 또는 시퀀스 수준 분류를 위해 설계되었기 때문이며, 번역 또는 요약과 같은 창조적 작업에는 사용되지 않기 때문입니다. T5는 많은 언어 이해 과제를 해결하기 위한 통합된 접근 방식을 찾으려는 것입니다(전이 학습 기반).

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

## BERT: NLP을 위한 전이 학습

이른 시절에 NLP에서는 주로 인과적 언어 모델링 목적으로 사전 훈련된 순환 신경망을 사용한 전이 학습이 이루어졌습니다. 그러나 모든 것이 BERT [2]의 제안으로 변화했습니다. BERT는 자기 지도 목적을 사용하여 사전 훈련된 트랜스포머 기반 모델 [6]입니다. BERT는 대량의 미지의 텍스트로 사전 훈련할 수 있으며, 그런 다음 문장 (물론 문장 내 개별 토큰도)을 놀랍도록 높은 정확도로 분류하기 위해 세밀하게 조정할 수 있습니다. BERT가 제안된 당시, 대부분의 NLP 작업에서 새로운 최고 수준을 세우며, NLP에서 전이 학습을 강화시키면서 go-to 접근 방식으로 자리 잡았습니다.

![BERT Image](/assets/img/2024-07-14-T5Text-to-TextTransformersPartOne_9.png)

좀 더 구체적으로 설명하자면, BERT는 사전 훈련 중 "소음 제거" 목적인 masked language modeling (MLM)에 의존합니다; 위에서 설명한 바와 같습니다. 이것이 약간 복잡하게 들릴 수 있지만, 아이디어는 간단합니다:

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

- 입력 시퀀스에서 토큰 몇 개를 특별한 [MASK] 토큰으로 대체하여 가립니다.
- 손상된/수정된 시퀀스를 BERT로 처리합니다.
- BERT를 훈련하여 가려진 토큰을 정확하게 예측합니다.

이 구현은 조금 더 복잡합니다. 우리는 무작위로 토큰의 15%를 선택한 다음, [MASK] 토큰(90% 확률)이나 무작위 토큰(10% 확률)으로 대체합니다. 이 목적을 충분히 큰 사전 훈련 말뭉치에 적용함으로써 BERT는 전이 학습에 매우 효과적인 모델이 되도록 여러 가지 일반적인 언어학적 지식을 학습할 수 있습니다.

T5와 BERT는 어떻게 관련이 있을까요? BERT의 제안은 전이 학습이 자연어 처리 문제를 해결하는 데 유용한 접근법임을 보여줬습니다. 많은 사람들이 빠르게 BERT를 사용해 새로운 기술을 시도하고 개선안을 제안하기 시작했습니다. 결과적으로, 이 분야는 BERT와 유사한 모델을 사용한 전이 학습을 수행하는 다양한 옵션으로 넘쳐났습니다. T5 [1]은 이 연구 라인을 이어가지만, 이러한 다양한 제안을 통합된 프레임워크로 분석하여 자연어 처리에 대한 전이 학습의 모범 사례를 훨씬 명확하게 보여줍니다. 최종 T5 모델은 모든 이러한 모범 사례를 사용하여 최고 성능에 도달하기 위해 훈련됩니다.

T5와 LLMs는 어떤 관련이 있을까요? 현재, 우리는 작성 기반 AI 공간에서 대대적인 혁명을 보고 있습니다. LLMs(디코더만 있는 트랜스포머 아키텍처를 기반으로 한 언어 생성 모델)이 언어 모델 사전 훈련을 통해 언어 작업을 해결하고 제로/퓌샷 학습을 수행하는 방식을 사용합니다. LLMs는 훌륭하지만 T5는 비교적 독자적인 도구 및 연구 분야에 속합니다. 특히, T5는 출력을 생성하기 전에 입력을 명시적으로 처리하는 모델에 주로 초점을 맞추고 있습니다. 게다가 T5는 각 대상 작업에서의 사전 훈련을 계속한 후 파인튜닝하는 전이 학습 접근 방식을 채택합니다.

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

## 기타 유용한 링크

- transformer 아키텍처 [링크]
- self-attention [링크]
- BERT 모델 [링크]
- 언어 모델 기초 [링크]

# T5: 통합 텍스트 전용 Transformer

T5의 공헌은 새로운 아키텍처나 교육 방법론이 아닙니다. 오히려 [1]에서 수행된 연구는 기존 기술에 완전히 기반을 두고 있습니다. T5는 NLP의 전이 학습 파이프라인의 모든 측면을 고려합니다. 이는 다른 (미분류된) 데이터 세트, 사전 훈련 목표, 벤치마크 및 세부 튜닝 방법 등을 포함합니다. 그러나 이러한 모든 측면은 통합된 텍스트-텍스트 형식을 통해 연구됩니다. T5의 목표는 i) 전이 학습 설정을 분석하고 ii) 가장 효과적인 접근 방식을 결정하는 것입니다.

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

## 텍스트 대 텍스트 프레임워크

T5는 모든 텍스트 처리 문제를 "텍스트 대 텍스트" 형식으로 변환합니다(즉, 입력으로 텍스트를 사용하고 출력으로 텍스트를 생성합니다). 이 일반적인 구조는 제로/퓨 샷 학습을 이용하는 LLMs에 의해서도 활용되며, 공통된 방식으로 다양한 작업을 모델링하고 해결할 수 있게 합니다. 고려하는 모든 작업에 동일한 모델, 목적, 훈련 과정 및 디코딩 프로세스를 적용할 수 있습니다! 우리는 단순히 프롬프팅 접근법을 채택하고 언어 모델에게 답변을 텍스트 형식으로 생성하도록 요청합니다.

![이미지](/assets/img/2024-07-14-T5Text-to-TextTransformersPartOne_10.png)

조금 더 구체적으로 만들기 위해, T5가 해결하는 모든 작업은 다음과 같이 텍스트 대 텍스트 형식으로 변환할 수 있습니다:

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

- 주어진 형식을 사용하면 요약이나 번역과 같은 작업을 쉽게 수행할 수 있습니다. (예: 대상은 자연스럽게 시퀀스입니다.) 게다가 모델이 올바른 클래스와 관련된 텍스트를 생성하도록 모델을 훈련시킴으로써 분류를 수행할 수도 있습니다. 이 프로세스는 회귀와 같은 문제의 경우 약간 복잡해지지만(실수값을 반올림하여 가장 가까운 소수점으로 처리하고 분류 문제로 취급하는 등) 대부분의 언어 작업에 대해서는 잘 작동하는 경향이 있습니다. 위의 그림에서 예제를 확인할 수 있습니다.

T5는 해결하는 각 작업에 대해 파인튜닝됩니다. 이는 소수의 샘플 학습을 사용하는 LLMs 및 여러 작업을 동시에 해결하도록 하는 NLP Decathlon [3]과 대조적입니다.

## T5는 어떻게 학습되었습니까?

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

[1]에서 수행된 모든 분석은 위에서 설명한 통합된 텍스트 대 텍스트 프레임워크를 사용합니다. 이 프레임워크를 사용하면 다양한 언어 이해 작업을 공유 형식으로 변환할 수 있습니다. 또한, T5의 분석은 동일한 기본 트랜스포머 아키텍처와 사전 훈련 데이터셋을 사용합니다.

![이미지](/assets/img/2024-07-14-T5Text-to-TextTransformersPartOne_11.png)

모델입니다. 이전에 논의되었듯이, 트랜스포머 아키텍처는 원래 [6]에서 제안된 대로 인코더와 디코더 모듈을 모두 포함하고 있습니다. 최근의 언어 모델링 연구에서는 인코더 또는 디코더만 사용하는 아키텍처 변형을 탐구했습니다. 예를 들어, BERT는 인코더만 사용합니다 [2], 대부분의 (대형) 언어 모델은 디코더만 사용합니다. T5는 원래 트랜스포머와 밀접한 유사성을 갖는 인코더-디코더 아키텍처를 사용합니다. 주요 차이점은 다음과 같습니다:

- 각 어텐션 및 피드포워드 변환 직전에 LayerNorm이 적용됩니다 (즉, 잔여 경로 외부에)
- LayerNorm에 추가적인 바이어스가 사용되지 않습니다 (즉, 여기를 참조하십시오; 우리는 스케일만 사용하고 추가적인 바이어스를 제거합니다)
- 어텐션 가중치, 피드포워드 네트워크, 스킵 연결 등 네트워크 전체에 드롭아웃이 적용됩니다.

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

위의 그림에 나와 있는 수정 사항은요. 이 모델을 사용하여 T5는 여러 다양한 전이 학습 설정을 테스트하여 최상의 실천법 집합을 도출할 수 있습니다.

T5는 Colossal Clean Crawled Corpus (C4)에서 사전 훈련되었습니다. 이는 "비교적 깨끗한" 영어 텍스트의 750GB 코퍼스입니다. 이 내용은 [1]에서 작성되었습니다. 이전 연구에서 다양한 사전 훈련 데이터셋이 제안되었지만, [1]의 저자들은 공개적으로 사용 가능하지 않은 이전 데이터셋, 제한된 필터링 규칙 집합 사용, 제한된 범위(예: Creative Commons에서만 가져온 데이터) 또는 기계 번역을 위해 병렬 데이터에만 초점을 맞춘 데이터셋 때문에 자체 데이터셋을 구성하기로 선택했습니다.

![이미지](/assets/img/2024-07-14-T5Text-to-TextTransformersPartOne_12.png)

특히, C4는 나중에 Gopher와 Chinchilla의 사전 훈련에 사용된 MassiveText 데이터셋의 하위 집합으로 사용되었습니다. 해당 데이터셋의 크기 측정치를 위한 위의 표를 참조하면, 현대적인 LLMs를 훈련시키는 데 사용된 사전 훈련 데이터셋과 C4의 크기 사이의 관계를 더 잘 이해할 수 있습니다. LLMs에서는 충분히 큰 데이터셋 위에서 디코더만 있는 모델을 사전 훈련하는 것이 성공에 중요하다는 것을 확인했습니다. T5와 같은 다양한 아키텍처의 트랜스포머에도 똑같은 이야기가 적용됩니다. 대규모, 라벨이 붙지 않은 데이터셋을 통한 철저한 사전 훈련은 하류 작업 성능을 높이는 데 도움이 됩니다.

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

**실험 설정**
T5는 C4에서 사전 훈련된 후에 각종 하위 작업을 해결하기 위해 미세 조정됩니다. 그러나 이 프레임워크 내에서 사용되는 정확한 설정은 변할 수 있습니다. 즉, 다음을 바꿀 수 있습니다:

- 트랜스포머 아키텍처
- 사전 훈련 설정(예: 작업 또는 데이터 양)
- 미세 조정 설정
- 모델의 크기/규모

이러한 설정을 하나씩 변경하고 결과를 평가함으로써, 우리는 자연어 처리에서 전이 학습을 위한 최상의 방법론을 개발할 수 있습니다. 이를 통해 BERT 이후의 다양한 제안들을 한데 모아 효과적인 언어 이해 모델을 생성하는 단일한 파이프라인으로 정리할 수 있습니다.

# 포인트

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

이 게시물은 T5 모델과 관련된 중요한 배경 정보와 사용되는 기본 실험 프레임워크를 포함한 모든 예비 정보를 다루었습니다. 다음 게시물에서는 [1]에서 수행된 철저한 분석의 세부 내용을 다룰 것이며, 이를 통해 NLP에서의 전이 학습에 대한 모범 사례를 밝힐 것입니다. 지금 당장은 T5에 관련된 주요 포인트를 아래에 설명하겠습니다.

전이 학습은 강력합니다. 전이 학습이란 깊은 학습 모델을 미리 학습시킨 다음 이 모델을 하류 목표 데이터셋(즉, 실제로 해결하려는 작업)에 대해 미세 조정(또는 추가 학습)하는 과정을 말합니다. 충분히 크고 일치하는(즉, 하류 작업과 유사한) 데이터셋 상에서 수행된다면 미리 학습은 극도로 효과적입니다. 모델은 미세 조정 중에 빠르게 학습하고 더 높은 정확도에 도달할 수 있습니다. 이 기술은 도메인을 넘어(예: 컴퓨터 비전 및 NLP) 효과적이지만 미리 학습이나 미세 조정에 사용되는 구체적인 접근 방식은 다를 수 있습니다.

BERT 이후의 다음은 무엇일까요? BERT의 제안은 NLP 작업에 대한 전이 학습의 사용을 대중화한 거대한 발전이었습니다. 사실, BERT는 거의 모든 고려된 작업에서 새로운 최첨단 성능을 세웠습니다. 이러한 성공으로 인해 연구 커뮤니티는 BERT의 접근 방식을 채택하고 발전시켰습니다. T5는 BERT의 제안 이후에 이어지는 모든 후속 작업 및 분석을 통합하려고 시도하며, 가장 효과적인 전이 학습 접근 방식을 더 명확하게 제시합니다.

일반적인 작업 공식. 많은 다양한 전이 학습 접근 방식이 연구될 수 있는 통일된 프레임워크를 만들기 위해 T5는 일반적인 텍스트 대 텍스트 프레임워크를 제안했습니다. LLMs에 사용되는 프롬프팅 및 퓨샷 학습 기술과 유사하게, 이 텍스트 대 텍스트 프레임워크는 어떤 언어 작업이든 텍스트 입력과 출력으로 재구성할 수 있습니다. 구체적으로, 이는 작업별 접두어를 텍스트 입력에 추가하고(즉, T5가 어떤 작업을 해결하려는지 알 수 있도록) T5의 디코더 모듈을 사용하여 원하는 대상(예: 레이블, 회귀 값 또는 텍스트 시퀀스)에 해당하는 텍스트를 생성함으로써 이루어집니다.

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

## 마무리 말씀

이 기사를 읽어 주셔서 감사합니다. 저는 Rebuy의 AI 이사인 Cameron R. Wolfe입니다. 저는 딥러닝의 경험적 및 이론적 기초를 연구합니다. 미디엄에서 다른 글도 읽어보실 수 있습니다! 만약 좋아하셨다면 트위터에서 제 계정을 팔로우하거나 Deep (Learning) Focus 뉴스레터를 구독해보세요. 거기에서 인기 논문들에 대한 이해 가능한 개괄을 통해 AI 연구 주제에 대한 더 깊은 이해를 돕고 있습니다.

## 참고 문헌

[1] Raffel, Colin 등. "통합 텍스트-텍스트 트랜스포머를 사용한 전이 학습의 한계 탐구." 기계 학습 연구 지 21.1 (2020): 5485-5551.

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

[2] Devlin, J., et al. “Bert: Pre-training of deep bidirectional transformers for language understanding.” arXiv preprint arXiv:1810.04805 (2018).

[3] McCann, B., et al. “The natural language decathlon: Multitask learning as question answering.” arXiv preprint arXiv:1806.08730 (2018).

[4] Rae, J. W., et al. “Scaling language models: Methods, analysis & insights from training gopher.” arXiv preprint arXiv:2112.11446 (2021).

[5] Hoffmann, J., et al. “Training compute-optimal large language models.” arXiv preprint arXiv:2203.15556 (2022).

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

**[6] Vaswani, Ashish, et al. “Attention is all you need.” Advances in neural information processing systems 30 (2017).**
